# Logrotate

## 介绍
 logrotate软件是一个日志管理工具，用于非分隔日志，删除旧的日志文件，并创建新的日志文件，起到“转储作用”，可以为系统节省磁盘空间。
 一般centos系统已经自带安装好了。
 
 logrotate是基于crontab运行的，其脚本是`/etc/cron.daily/logtotate`，日志轮转是系统自发完成的，实际运行时，logrotate会调用配置文件`/etc/logrotate.conf`。可以在`/etc/logrotate.d`目录里放置自定义好的配置文件，用来覆盖logrotate.conf的缺省值。
 
 ## 常用配置文件
 
 ` /etc/logrotate.conf `     主配置文件
 
 ` /etc/logrotate.d/ `      该目录里的所有文件都会自动被读入到logrotate.conf文件中

## /etc/logrotate.conf 文件配置
```
# see "man logrotate" for details
# rotate log files weekly
weekly  #默认每周执行一次日志轮询

# keep 4 weeks worth of backlogs
rotate 4  #默认保留4个日志文件, 0 是没有备份

# create new (empty) log files after rotating old ones
create #自动创建新的日志文件，新的文件和原来的文件具有相同的权限

# use date as a suffix of the rotated file
dateext  #切割后的日志文件以当前日期为结尾，如xxx.log-20180810,如果注释掉,切割出来是按数字递增,即 xxx.log-1这种格式

# uncomment this if you want your log files compressed
#compress 是否通过gzip压缩转储以后的日志文件，如xxx.log-20180810.gz;如果希望日志文件压缩，请取消注释

# RPM packages drop log rotation information into this directory
include /etc/logrotate.d #将logrotate.d目录里的文件加载进来

# no packages own wtmp and btmp -- we'll rotate them here
/var/log/wtmp {
    monthly           #每个月切割一次，取代默认的一周
    create 0664 root utmp  #新建日志权限为0664，属主为root，属组为utmp
	minsize 1M       #文件大小超过1M后才会切割
    rotate 1
}

/var/log/btmp {
    missingok
    monthly
    create 0600 root utmp
    rotate 1
}
```

## 参数介绍
```
compress                     通过gzip 压缩转储以后的日志

nocompress                   不做gzip压缩处理

copytruncate                  用于还在打开中的日志文件，把当前日志备份并截断；是先拷贝再清空的方式，拷贝和清空之间有一个时间差，可能会丢失部分日志数据。

nocopytruncate                备份日志文件不过不截断

create mode owner group        轮转时指定创建新文件的属性，如create 0777 nobody nobody

nocreate                      不建立新的日志文件

delaycompress                 和compress 一起使用时，转储的日志文件到下一次转储时才压缩

nodelaycompress               覆盖 delaycompress 选项，转储同时压缩。

missingok                     如果日志丢失，不报错继续滚动下一个日志

errors address                  转储时的错误信息发送到指定的Email 地址

ifempty                       即使日志文件为空文件也做轮转，这个是logrotate的缺省选项。

notifempty                    当日志文件为空时，不进行轮转

mail address                   把转储的日志文件发送到指定的E-mail 地址

nomail                       转储时不发送日志文件

olddir directory                转储后的日志文件放入指定的目录，必须和当前日志文件在同一个文件系统

noolddir                     转储后的日志文件和当前日志文件放在同一个目录下

sharedscripts                 运行postrotate脚本，作用是在所有日志都轮转后统一执行一次脚本。如果没有配置这个，那么每个日志轮转后都会执行一次脚本

prerotate             在logrotate转储之前需要执行的指令，例如修改文件的属性等动作；必须独立成行

postrotate            在logrotate转储之后需要执行的指令，例如重新启动 (kill -HUP) 某个服务！必须独立成行

daily                 指定转储周期为每天

weekly               指定转储周期为每周

monthly              指定转储周期为每月

rotate count           指定日志文件删除之前转储的次数，0 指没有备份，5 指保留5 个备份

dateext               使用当期日期作为命名格式

dateformat .%s         配合dateext使用，紧跟在下一行出现，定义文件切割后的文件名，必须配合dateext使用，只支持 %Y %m %d %s 这四个参数

size(或minsize) log-size   当日志文件到达指定的大小时才转储，log-size能指定bytes(缺省)及KB (sizek)或MB(sizem).

当日志文件 >= log-size 的时候就转储。 以下为合法格式：（其他格式的单位大小写没有试过）

size = 5 或 size 5 （>= 5 个字节就转储）

size = 100k 或 size 100k

size = 100M 或 size 100M
```



## 使用过的例子

- nginx

```nginx
/var/log/nginx/*log {
    daily
    missingok
    rotate 30
    dateext
    dateformat -%Y%m%d
    copytruncate
    compress
    nodelaycompress
    noolddir
    notifempty
}

```