## Nginx 配置 upstream实现负载均衡

nginx安装[传送门](https://github.com/ainiok/build-script)

### 环境

- CentOS7
- Nginx-1.14.2
- Apache-2.4.6

> nginx 监听80端口， apache 监听85端口

### 配置

一、修改nginx配置文件

编辑nginx的配置文件

`vim /usr/local/nginx/conf/nginx.conf`

```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    
    # 在http节点下，加入upstream节点
   
    upstream httpd {
        ip_hash;   # 负载均衡方式，更多请参考[2.3.2](https://book.ainiok.com/Nginx/LoadBalance.html)
        server 10.113.2.82:85;
        server 10.113.2.83:85;
    }

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            proxy_pass http://httpd; 
            #root   html;
            #index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

}

```