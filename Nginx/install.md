## 安装

安装步骤请参考[github地址](https://github.com/ainiok/build-script/blob/master/deploy.sh)

## 安全设置

一、修改nginx 的 Server 

/src/core/nginx.h  版本号在13行 server 在14行

二、错误页面 nginx/1.4.20

/src/http/ngx_http_header_filter_module.c 的 49行

修改 
/src/http/ngx_http_special_response.c 的 36行