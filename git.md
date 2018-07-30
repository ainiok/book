# Git

## 安装
    
   - windows
   
   - Linux
   ```
yum -y install zlib-devel openssl-devel cpio expat-devel gettext-devel curl-devel perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker

https://mirrors.edge.kernel.org/pub/software/scm/git/

wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.18.0.tar.gz

tar -zxvf 

./configure
make && make install

git config --global user.name "Your Name"
git config --global user.email "email@example.com"

ssh-keygen -t rsa -C "XXX@outlook.com"

ssh -T -v git@github.com
```
