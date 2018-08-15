# 内部网络搭建gitbook
   由于jenkins主机我没有权限登录，所以这次就使用webhook来完成

## 环境
有一个gitlab可访问的域名， 内网都是虚拟域名，配置方法可以查看 (计算机网络/域名解析)

## 在域名的目录下 创建一个 index.php

```
<?php

/**
 * Created by PhpStorm.
 * User: xiaojin
 * Date: 2018/8/9
 * Time: 14:23
 */
function writeLog($content)
{
	file_put_contents("gitlab-webhook_log.txt", $content, FILE_APPEND);
}
//gitlab webhook 自动部署脚本
//项目存放物理路径,第一次clone时,必须保证该目录为空
$savePath = "/var/www/book/xybook";
$gitPath = "http://xiaojin@gitlab.com/xiaojin/xybook.git";//代码仓库
$email = "your email";//用户仓库邮箱
$name = "your name";//仓库用户名,一般和邮箱一致即可
$token = 'ye2hQ!z9NU^aAcxce5D3*LytvZpexldd'; // webhook 里设置的Secret Token
$vaild_ip = ['200.200.115.29', '192.168.0.23']; //允许访问的IP， gitlab的地址， 内网地址和外网地址
$client_ip = $_SERVER['SERVER_ADDR'];
$client_token = $_SERVER['HTTP_X_GITLAB_TOKEN'];
if ($_SERVER['REQUEST_METHOD'] !== 'POST'){ // webhook 发送的是post请求
	writeLog('No allow method');
    die('No allow method');
}
if ($client_token != $token){
	writeLog('invalid token');
    die('invalid token');
}
if (!in_array($client_ip, $vaild_ip)){
    writeLog('No permission');
    writeLog($client_ip.'在'.time().'时发送了POST请求');
    die('No permission');
}
$requestBody = file_get_contents("php://input");
if (empty($requestBody)) {
	writeLog('request null');
    die('send fail');
}
//解析Git服务器通知过来的JSON信息
$content = json_decode($requestBody, true);
//若是主分支且提交数大于0
if ($content['ref'] == 'refs/heads/master' && $content['total_commits_count'] > 0) {

    $res_log .= PHP_EOL . "pull start --------" . PHP_EOL;
    $res_log .= shell_exec("cd {$savePath} && git pull {$gitPath} && git fetch --all && git reset --hard origin/master && git pull");//拉取代码
    $res_log .= '-------------------------' . PHP_EOL;
    $res_log .= $content['user_name'] . ' 在' . date('Y-m-d H:i:s') . '向' . $content['repository']['name'] . '项目的' . $content['ref'] . '分支push了' . $content['total_commits_count'] . '个commit：';
    $res_log .= "pull end --------" . PHP_EOL;
    shell_exec("cd {$savePath} && gitbook install && gitbook build");
    writeLog($res_log);
}
```

- shell_exec()

1. 首先查看启动PHP进程的用户， `ps -ef |grep php ` centos下默认是nobody, nobody 默认没有任何权限

添加一个用户组
```angular2html
groupadd www
useradd -g www -s /bin/bash www
```

加完以后 `cat /etc/passwd ` 就可以看到了

2. 修改PHP启动用户， 在 PHP安装目录下` /path/to/php/etc/php-fpm.d/www.conf  ` (这里有所不同，每个人安装的方式都不一样，配置也不有可能不同)

编辑里面的  `user = www; group = www;`  重启 php-fpm

3. 添加刚才的用户到 sudoers里面
```angular2html
vim /etc/sudoers


www   ALL=(ALL)       ALL //新添
%www        ALL=(ALL)       NOPASSWD: ALL  //设置 www 组下面的用户使用sudo不需要输入密码

```










# 强制拉取
```
git fetch --all
git reset --hard origin/master
git pull
```