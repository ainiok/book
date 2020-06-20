## redis 通配符 批量删除key

dis 中 DEL指令支持多个key作为参数进行删除 但不支持通配符，无法通过通配符批量删除key，不过我们可以借助 Linux 的管道和 xargs 指令来完成这个动作。


比如要删除所有以user开头的key 可以这样实现：

```
[root@~]# redis-cli keys "base_info#*"
 1) "base_info#1"
 2) "base_info#2"
 3) "base_info#3"
 4) "base_info#4"
 5) "base_info#5"
 [root@~]# redis-cli keys "base_info#*" |xargs redis-cli del
 (integer) 5
```


这样就删除成功了