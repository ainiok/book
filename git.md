# Git

## 安装
    
   - windows
   
   - Linux
   
```
yum -y install zlib-devel openssl-devel cpio expat-devel gettext-devel curl-devel perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker

https://mirrors.edge.kernel.org/pub/software/scm/git/  #到这里可以查看各个版本

wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.19.1.tar.gz

tar -zxvf  && cd git-2.19.1 && ./configure --prefix=/usr/bin 

./configure
make && make install

git config --global user.name "Your Name"
git config --global user.email "email@example.com"

ssh-keygen -t rsa -C "XXX@outlook.com"

ssh -T -v git@github.com
```
