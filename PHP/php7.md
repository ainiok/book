## php7

1.Opcache
记得启用Zend Opcache, 因为PHP7即使不启用Opcache速度也比PHP-5.6启用了Opcache快, 所以之前测试时期就发生了有人一直没有启用Opcache的事情. 启用Opcache非常简单, 在php.ini配置文件中加入:
```
extension=opcache
opcache.enable=1
opcache.enable_cli=1"
```

2. 