## Install

### 一、通过rpm 安装
>  版本：mysql 5.7

1、检查是否已经安装mysql服务

`rpm -qa | grep mysql `

2、如果已安装则删除 

`rpm -e --nodeps  服务名 `

3、下载 `mysql57-community-release-el7-8.noarch.rpm`的 YUM 源

`wget http://repo.mysql.com/mysql57-community-release-el7-8.noarch.rpm`

4、安装

`rpm -ivh mysql57-community-release-el7-8.noarch.rpm`

6、安装 MySQL，如果出现提示，输入Y

`yum install mysql-server`

安装完毕后，运行mysql，然后在  /var/log/mysqld.log 文件中会自动生成一个随机的密码，我们需要先取得这个随机密码，以用于登录 MySQL 服务端：


```
service mysqld start
grep "password" /var/log/mysqld.log
```

会返回如下内容

`[Note] A temporary password is generated for root@localhost: Do/mqrXtY2Cz`

**设置service**
在`/usr/lib/systemd/system` 目录创建`mysql.service`文件,内容如下
```
# systemd service file for MySQL forking server

[Unit]
Description=MySQL Server
Documentation=man:mysqld(8)
Documentation=http://dev.mysql.com/doc/refman/en/using-systemd.html
After=network.target
After=syslog.target

[Service]
User=mysql
Group=mysql

Type=forking
TimeoutSec=5min
PermissionsStartOnly=true
ExecStart=/etc/init.d/mysqld start
ExecStop=/etc/init.d/mysqld start
ExecRestart=/etc/init.d/mysqld restart
ExecReload=/etc/init.d/mysqld reload
Restart=on-failure
KillMode=process
GuessMainPID=no
RemainAfterExit=yes
```


### 源码安装

> 参考github  https://github.com/ainiok/PhpEnv/blob/master/envinstall.sh