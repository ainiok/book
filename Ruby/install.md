## Ruby


```bash
wget https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.5.tar.gz
tar -zxvf ruby-2.6.5.tar.gz

cd ruby-2.6.5 

./configure

make && make install 

```

### 配置

Ruby 的默认源使用的是 cocoapods.org，国内访问这个网址有时候会有问题，网上的一种解决方案是将远替换成 ruby-china 的,替换方式如下：

```bash
gem source -r https://rubygems.org/
gem source -a https://gems.ruby-china.com/
```

要想验证是否替换成功了，可以执行：

```bash
gem sources -l  
```

结果如下

```bash
*** CURRENT SOURCES ***

https://gems.ruby-china.com/
```