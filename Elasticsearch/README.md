## 安装

`wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.0.0.rpm`
```
rpm -ivh elasticsearch-5.0.0.rpm
```

## 安装IK 分词插件
```
/usr/share/elasticsearch/bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v5.0.0/elasticsearch-analysis-ik-5.0.0.zip
```

## 修改配置文件

 `vim /etc/elasticsearch/elasticsearch.yml`
 ```
#cluster.name: my-application 
#node.name: node-1
#network.host: 192.168.0.1
#http.port: 9200
```

 只要简单设置以上四项，不是集群的话只要修改 `network.host` 就可以，其余可以使用默认值
 
 ## 句柄数和MMap
 设置最大句柄数为655360（我们的服务器是默认的），sudo sysctl -a | grep fs.file-max 得到，可以通过 GET /_nodes/process 验证
 
 **mmap:**
 `sysctl -w vm.max_map_count=262144`
 
 或者通过修改 `/etc/sysctl.conf` 文件中的 `vm.max_map_count` 永久改变(`sudo sysctl -p /etc/sysctl.conf`生效)。
 
 ## 启动
 `systemctl start elasticsearch`