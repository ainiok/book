# vsftpd 
- 创建一个目录

` mkdir /ftpwww`

- 添加一个禁止shell登陆的用户

`useradd xiao -d /ftpwww/ -s /sbin/nologin`

`passwd xiao`  修改密码

- 修改配置文件

`vim /etc/vsftpd/vsftpd.conf`

```
anonymous_enable=NO   # 禁止匿名用户登陆
chroot_list_enable=YES  # 取消注释
chroot_list_file=/etc/vsftpd/chroot_list #取消注释
local_root=/ftpwww           # 新增
anon_root=/ftpwww            #新增
allow_writeable_chroot=YES   #新增 作用是允许写用户主目录以外的
pasv_enable=YES
pasv_min_port=30000
pasv_max_port=31000
```

`vi /etc/vsftpd/chroot_list`  添加 xiao 用户

- 重新启动

`systemctl restart vsftpd`