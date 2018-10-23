## Linux 防火墙

> 从CentOS7(RHEL7)开始，官方的标准防火墙设置软件从iptables变更为firewalld

### iptables

**iptables 语法组成**

`iptables [-t 规则表] 命令选项 规则链 [条件匹配] [跳转动作]`

**1.规则表**

iptables将对于数据包的控制分给4个规则表完成，各自在不同的场合发挥作用。它们分别是“filter表”、“nat表”、“mangle表”、“raw表”。

1.filter表 (默认)

filter表负责数据包的接收或者拒绝，我们最常接触的就是它。
如果直接修改配置文件 `/etc/sysconfig/iptables`

```
 *filter   #如果是net 这就是 *net 
 #在这里编写filter的配置
 COMMIT
```

你也可以使用命令来添加filter规则

```
使用命令往filter表格中添加以下规则：直接丢弃端口80接收到的数据包，默认是filter，filter可省略
iptables -t filter -p tcp --dport 80 -j -DROP
```
**2.nat表**
所谓NAT，就是网络地址转换，主要通过修改数据包中的传输源地址和目的地址来实现。常用于将数据包根据某些规则分发到本地局域网的各台服务器上，实现负载均衡或者IP地址复用扩展。
可以直接修改文件也可以使用命令

```
#把访问10.1.2.2的访问转发到192.168.0.112上（PREROUTING可理解为流入的数据包）
iptables -t nat -A PREROUTING -d 10.1.2.2 -j DNAT --to-destination 192.168.0.112

#把即将要流出本机的数据的源ID地址修改成为10.113.25.6（POSTROUTING可理解为流出的数据包）
iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -j SNAT --to-source 10.113.25.6
```

**3.mangle表**
mangle表主要负责管理各数据包处理的优先级，常用于改善通信质量，其实现方式主要是修改TOS（服务类型）的值。

```
#将80端口的通信服务类型设置为高吞吐量
iptables -t mangle -A PREROUTING -p tcp --dport 80 -j TOS --set-tos Maximize-Throughput
```

**4.raw表**

raw表的功能与mangle表有点类似，都是可以给特定的数据包附加标记，最常见的就是附加NOTRACK标记。也就是不经过防火墙，不对该数据包进行任何跟踪而交由其他机器负责处理。

例如，你发现机器因为大量的DNS解析服务负荷则造成通信质量下降，因此决定使用负载均衡把DNS相关的数据包交给其他服务器来处理，这时候只需要为端口53添加NOTRACK标记就可以了。

```
iptables -t raw -I PREROUTING -p udp --dport 53 -j NOTRACK
iptables -t raw -I OUTPUT -p udp --dport 53 -j NOTRACK
```

**表之间的优先顺序**

> Raw——mangle——nat——filter


**规则链**

|  种类 |  说明 |




