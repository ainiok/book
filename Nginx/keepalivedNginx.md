## keepalived + nginx 高可用

机器上安装了 nginx, 没有安装的[传送门](https://github.com/ainiok/build-script)

### 环境

- CenntOS 7 
- Nginx-1.14.2
- keepalived-1.4.5

|     VIP     |      IP      |  节点名称 |    主从   |
| ----------- |:------------:|:--------:|:---------|
| 10.113.2.88 | 10.113.2.82  |  node-3  |  master  | 
| 10.113.2.88 | 10.113.2.83  |  node-4  |  backup  |


 ### 安装
 
> 以下操作每个节点都要执行

一、安装基础依赖包
 
 ```
yum -y install libnl libnl-devel libnfnetlink-devel psmisc
```

二、 安装keepalived

> 以下步骤可以创建一个 keepalived.sh 脚本，然后复制以下内容，执行脚本即可一键执行

```
wget http://www.keepalived.org/software/keepalived-1.4.5.tar.gz

tar -zxf keepalived-1.4.5.tar.gz

cd keepalived-1.4.5

./configure --prefix=/usr/local/keepalived

make && make install

cp keepalived/etc/init.d/keepalived /etc/rc.d/init.d/

cp /usr/local/keepalived/etc/sysconfig/keepalived /etc/sysconfig/

mkdir /etc/keepalived/

cp /usr/local/keepalived/etc/keepalived/keepalived.conf /etc/keepalived/

cp /usr/local/keepalived/sbin/keepalived /usr/sbin/

echo "/etc/init.d/keepalived start" >> /etc/rc.local

chmod +x /etc/rc.d/init.d/keepalived

chkconfig keepalived on

```

三、配置

先备份配置文件

```
cp /etc/keepalived/keepalived.conf cp /etc/keepalived/keepalived.conf.bak

```

修改配置文件 keepalived.conf

```
! Configuration File for keepalived

global_defs {
   router_id node-3
}

vrrp_script check_nginx
{
    script "/etc/keepalived/check_nginx.sh"
    interval 2   # 2s执行一次
}

vrrp_instance VI_1 {
    state MASTER     # 从节点改成 BACKUP
    interface ens18    # 绑定的网卡
    virtual_router_id 66   # 同一实例下virtual_router_id要相同
    priority 100     # master 的权重要高于 backup
    advert_int 1     # 主从心跳检测时间,单位s
    authentication { 
        auth_type PASS
        auth_pass 1111 
    }
    virtual_ipaddress {
        10.113.2.88    # 设置VIP,可以多个，一行一个
    }
    track_script {
         check_nginx  # vrrp_script 一定要写在前面，不然不会执行，踩过坑
    }
}

```

check_nginx.sh

```
#!/bin/bash -x

ACTIVE=`ps -C nginx --no-header | wc -l`
if [ $ACTIVE -eq 0 ]; then
     /usr/local/nginx/sbin/nginx
    sleep 2
    if [ `ps -C nginx --no-header |wc -l` -eq 0 ]; then
        killall keepalived
        exit 1
    else
        exit 0
    fi
else
    exit 0
fi
```

四、 启动服务

`systemctl restart keepalived `
