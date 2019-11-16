## window10共享文件夹到VMWare

### 添加共享文件夹

> 虚拟机-> 设置-> 选项 -> 共享文件夹 


### 之后的主要命令

```
//查看共享文件夹
vmware-hgfsclient

//下载挂载工具
 yum install open-vm-tools-devel -y

//执行挂载工具，D为分享的文件夹名称[比如这里为www] .host前面有个空格
sudo vmhgfs-fuse .host:/www /mnt/hgfs       // /mnt/hgfs 为挂载路径

//执行完后 查看
ll /mnt/hgfs
```