# go-mysql-elasticsearch

- [github](https://github.com/siddontang/go-mysql-elasticsearch)

> 将MySQL数据同步到elasticsearch中

## install 
- Install Go (1.9+) and set your GOPATH, `yum install -y go` 查看版本 `go version`
- `go get github.com/siddontang/go-mysql-elasticsearch` 忽略控制台的消息
- `cd $GOPATH/src/github.com/siddontang/go-mysql-elasticsearch` 这里`$GOPATH` 可以通过`go env GOPATH` 得知
- `make`

> 本来我想做成RPM包，那样就可以rpm安装，但是弄了1天还没弄出来，放弃了，如果有谁知道怎么制作，请联系我

## 添加到systemd.server

复制 go-mysql-elasticsearch 整个目录到 `/usr/local/` 下

` vim /usr/lib/systemd/system/go-mysql-elasticsearch.service`

```
  1 [Unit]
  2 Description=go-mysql-elasticsearch
  3
  4 [Service]
  5 Type=simple
  6 User=root
  7 Environment=GO_AGENT_LOG=/var/log/es-agent/go-mysql-elasticsearch.log
  8 ExecStart=/usr/local/go-mysql-elasticsearch/bin/go-mysql-elasticsearch -config=/etc/river.toml -log_path=${GO_AGENT_LOG} -log_level=info
  9 ExecReload=/usr/bin/kill -s HUP $MAINPID
 10  ExecStop=/usr/bin/kill -s QUIT $MAINPID
 11 PIDFile=/var/run/go-mysql-elasticsearch.pid
 12 KillMode=process
 13 Restart=always
 14 StartLimitInterval=0
 15 StandardOutput=null
 16
 17 [Install]
 18 WantedBy=multi-user.target

```

后面就可以用 `systemctl start|reload|stop go-mysql-elasticsearch` 启动了

## Notice
- MySQL supported version < 8.0
- ES supported version < 6.0
- binlog format must be row.
- binlog row image must be full for MySQL, you may lost some field data if you update PK data in MySQL with minimal or noblob binlog row image. MariaDB only supports full row image.
- Can not alter table format at runtime.
- MySQL table which will be synced should have a PK(primary key), multi columns PK is allowed now, e,g, if the PKs is (a, b), we will use "a:b" as the key. The PK data will be used as "id" in Elasticsearch. And you can also config the id's constituent part with other column.
- You should create the associated mappings in Elasticsearch first, I don't think using the default mapping is a wise decision, you must know how to search accurately.
- mysqldump must exist in the same node with go-mysql-elasticsearch, if not, go-mysql-elasticsearch will try to sync binlog only.
- Don't change too many rows at same time in one SQL.


## my.cnf
```
[mysqld]
log-bin=mysql-bin
gtid-mode=on
enforce-gtid-consistency=true
log-slave-updates=true
binlog-format=ROW
binlog-row-image = full
sync-binlog=1
server-id=1
socket=/usr/local/mysql/mysql.sock
symbolic-links=0
binlog-cache-size=4M
max-binlog-size=1G
max-binlog-cache-size=2G
binlog-checksum=CRC32
wait-timeout=200
interactive-timeout=200
open-files-limit=60000

[mysqld_safe]
log-error=/var/log/mysqld.log
```


## 待完善

- ` master.info ` 这个文件的目录没有自定义