## Redis


### 主从复制

无论是初次连接还是重新连接，当建立一个从服务器时，从服务器都将从主服务器发送一个SYNC命令。
接到SYNC命令的主服务器将开始执行BGSAVE，并在保存操作执行期间，将所有新执行的命令都保存到一个缓冲区里面。
当BGSAVE执行完毕后，主服务器将执行保存操作所得到的.rdb文件发送给从服务器，从服务器接收这个.rdb文件，并将文件中的数据载入到内存中。
之后主服务器会以Redis命令协议的格式，将写命令缓冲区中积累的所有内容都发送给从服务器。


### Redis如何做持久化的？

bgsave做镜像全量持久化，aof做增量持久化。因为bgsave会耗费较长时间，不够实时。
在停机的时候会导致大量丢失数据，所以需要aof来配合使用。在redis实例重启时，会使用bgsave持久化文件重新构建内存，再使用aof重放近期的操作指令来实现完整恢复重启之前的状态。

如果不要求性能，在每条写指令时都sync一下磁盘，就不会丢失数据。但是在高性能的要求下每次都sync是不现实的，一般都使用定时sync，比如1s1次，这个时候最多就会丢失1s的数据。


### Pipeline有什么好处，为什么要用pipeline？

可以将多次IO往返的时间缩减为一次，前提是pipeline执行的指令之间没有因果相关性。使用redis-benchmark进行压测的时候可以发现影响redis的QPS峰值的一个重要因素是pipeline批次指令的数目