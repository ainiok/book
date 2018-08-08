linux 终端颜色修改 
vim ~/.bashrc
```
PS1='[\[\e[32;40m\]\u@\W \t ]$ '
```

设置vim 行号
```
vim   ~/.vimrc

在里面输入 set nu
```

安装 vim 和 unzip

下载软件用于编辑和解压缩文件。运行命令：
```
 yum install -y vim unzip
```

步骤二：安装nginx

1、添加运行nginx服务进程的用户
```
groupadd -r nginx 
useradd -r -g nginx  nginx
```

下载源码包
nginx 1.12.2  PHP7.1.11  mysql

```
wget http://nginx.org/download/nginx-1.12.2.tar.gz

wget http://cn2.php.net/distributions/php-7.1.11.tar.gz

wget https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.22-linux-glibc2.12-x86_64.tar.gz
```

安装nginx 

```
yum groupinstall "Development tools"
yum -y install gcc wget gcc-c++ automake autoconf libtool libxml2-devel libxslt-devel perl-devel perl-ExtUtils-Embed pcre-devel openssl-devel

./configure \
--prefix=/usr/local/nginx \
--sbin-path=/usr/sbin/nginx \
--conf-path=/etc/nginx/nginx.conf \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--pid-path=/var/run/nginx.pid \
--lock-path=/var/run/nginx.lock \
--http-client-body-temp-path=/var/tmp/nginx/client \
--http-proxy-temp-path=/var/tmp/nginx/proxy \
--http-fastcgi-temp-path=/var/tmp/nginx/fcgi \
--http-uwsgi-temp-path=/var/tmp/nginx/uwsgi \
--http-scgi-temp-path=/var/tmp/nginx/scgi \
--user=nginx \
--group=nginx \
--with-pcre \
--with-http_v2_module \
--with-http_ssl_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gunzip_module \
--with-http_gzip_static_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_stub_status_module \
--with-http_auth_request_module \
--with-mail \
--with-mail_ssl_module \
--with-file-aio \
--with-threads \
--with-stream \
--with-stream_ssl_module

make && make install
mkdir -pv /var/tmp/nginx/client

```
添加SysV 启动脚本
```
vim /etc/init.d/nginx

#!/bin/sh 
# 
# nginx - this script starts and stops the nginx daemon 
# 
# chkconfig:   - 85 15 
# description: Nginx is an HTTP(S) server, HTTP(S) reverse \ 
#               proxy and IMAP/POP3 proxy server 
# processname: nginx 
# config:      /etc/nginx/nginx.conf 
# config:      /etc/sysconfig/nginx 
# pidfile:     /var/run/nginx.pid 
# Source function library. 
. /etc/rc.d/init.d/functions
# Source networking configuration. 
. /etc/sysconfig/network
# Check that networking is up. 
[ "$NETWORKING" = "no" ] && exit 0
nginx="/usr/sbin/nginx"
prog=$(basename $nginx)
NGINX_CONF_FILE="/etc/nginx/nginx.conf"
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
lockfile=/var/lock/subsys/nginx
start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    echo -n $"Starting $prog: " 
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo 
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}
stop() {
    echo -n $"Stopping $prog: " 
    killproc $prog -QUIT
    retval=$?
    echo 
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
killall -9 nginx
}
restart() {
    configtest || return $?
    stop
    sleep 1
    start
}
reload() {
    configtest || return $?
    echo -n $"Reloading $prog: " 
    killproc $nginx -HUP
RETVAL=$?
    echo 
}
force_reload() {
    restart
}
configtest() {
$nginx -t -c $NGINX_CONF_FILE
}
rh_status() {
    status $prog
}
rh_status_q() {
    rh_status >/dev/null 2>&1
}
case "$1" in
    start)
        rh_status_q && exit 0
    $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
      echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}" 
        exit 2
esac
```
赋予脚本执行权限
```
chmod +x /etc/init.d/nginx
```
添加至服务管理列表，设置开机启动
```
chkconfig --add nginx
chkconfig  nginx on
```

启动服务
```
service nginx start
```

### **编译安装MySQL**

依次执行以下命令检查系统中是否存在使用 rpm 安装的 MySQL 或者 MariaDB。

```
rpm -qa | grep mysql
rpm -qa | grep mariadb
```
如果已经安装，则运行以下任一个命令删除。
```
rpm -e 软件名    #注意：这里的软件名必须包含软件的版本信息，如rpm -e mariadb-libs-5.5.52-1.el7.x86_64。一般使用此命令即可卸载成功。
rpm -e --nodeps 软件名   #卸载不成功时使用此命令强制卸载
```


依次运行以下命令安装 MySQL。
```
yum install -y libaio-*    #安装依赖
mkdir -p /usr/local/mysql
cd /usr/local/src
tar -xzvf mysql***
mv mysql-5.7.17-linux-glibc2.5-x86_64/* /usr/local/mysql/
```

依次运行以下命令建立 mysql 组和用户，并将 mysql 用户添加到 mysql 组。
```
groupadd mysql
useradd -g mysql -s /sbin/nologin mysql
```
运行命令初始化 MySQL 数据库。
```
/usr/local/mysql/bin/mysqld --initialize-insecure --datadir=/usr/local/mysql/data/ --user=mysql
```
更改 mysql 安装目录的属性
```
chown -R mysql:mysql /usr/local/mysql
```
依次运行以下命令设置开机自启动
```
cd /usr/local/mysql/support-files/
cp mysql.server  /etc/init.d/mysqld
chmod +x /etc/init.d/mysqld

chkconfig --add mysqld
chkconfig  mysqld on
```

设置环境变量
```
vi /root/.bash_profile
PATH=$PATH:$HOME/bin:/usr/local/mysql/bin:/usr/local/mysql/lib
# 再原来的基础上加入mysql 
source /root/.bash_profile  #重新执行文件
```
启动服务

```
service mysqld start
```

修改 MySQL 的 root 用户密码：初始化后 MySQL 为空密码可直接登录，为了保证安全性需要修改 MySQL 的 root 用户密码。

```
mysqladmin -u root password 密码
```

### **安装php-fpm**

Nginx本身不能处理PHP，作为web服务器，当它接收到请求后，不支持对外部程序的直接调用或者解析，必须通过FastCGI进行调用。如果是PHP请求，则交给PHP解释器处理，并把结果返回给客户端。PHP-FPM是支持解析php的一个FastCGI进程管理器。提供了更好管理PHP进程的方式，可以有效控制内存和进程、可以平滑重载PHP配置。

1、安装依赖包
```
yum install -y libmcrypt libmcrypt-devel mhash mhash-devel libxml2 libxml2-devel bzip2 bzip2-devel freetype.x86_64 freetype-devel.x86_64 libjpeg-turbo-devel libcurl-devel libpng libpng-devel
```

编译安装
```
./configure \
 --prefix=/usr/local/php \
 --enable-mysqlnd \
 --with-mysqli=mysqlnd --with-openssl \
 --with-pdo-mysql=mysqlnd \
 --enable-mbstring \
 --with-freetype-dir \
 --with-gd \
 --with-jpeg-dir \
 --with-png-dir \
 --with-zlib --with-libxml-dir=/usr \
 --enable-xml  --enable-sockets \
 --with-mcrypt  --with-config-file-path=/etc \
 --with-config-file-scan-dir=/etc/php.d \
 --enable-maintainer-zts \
 --disable-fileinfo  --with-bz2\
 --enable-fpm\
 --with-curl\
 --enable-pcntl
 
 make && make install
```

添加php和php-fpm配置文件。
```
cp /usr/local/src/php-7.1.11/php.ini-production /etc/php.ini
cd /usr/local/php/etc/
cp php-fpm.conf.default php-fpm.conf

sed -i 's@;pid = run/php-fpm.pid@pid = /usr/local/php/var/run/php-fpm.pid@' php-fpm.conf
```

设置PHP环境变量
```
vi /root/.bash_profile
在$PATH =  ***  后面加上 /usr/local/php/bin

source /root/.bash_profile  #重新执行文件
```
添加php-fpm启动脚本。
```
cp /usr/local/src/php-7.1.11/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
chmod +x /etc/init.d/php-fpm
```

添加php-fpm至服务列表并设置开机自启。
```
chkconfig --add php-fpm    
chkconfig --list php-fpm
 chkconfig php-fpm on
 
cd /usr/local/php/etc/php-fpm.d
cp www.conf.default www.conf

```

启动服务
```
service php-fpm start
```

添加nginx对fastcgi的支持，首先备份默认的配置文件。
```
cp /etc/nginx/nginx.conf /etc/nginx/nginx.confbak
cp /etc/nginx/nginx.conf.default /etc/nginx/nginx.conf
```

编辑/etc/nginx/nginx.conf，在所支持的主页面格式中添加php格式的主页，类似如下：

```
        location / {
            root   /usr/local/nginx/html;
            index  index.php index.html index.htm;
        }
```
取消一下内容前面的注释
```
        location ~ \.php$ {
            root           /usr/local/nginx/html/;
            fastcgi_pass    127.0.0.1:9000;
            fastcgi_index   index.php;
            fastcgi_param  SCRIPT_FILENAME  $doucment_root$fastcgi_script_name;
            include        fastcgi_params;
        }
```
重新载入nginx的配置文件。
```
service nginx reload
```

安装composer

```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```