# 编译

## configure

`./configure`是用来检测你的安装平台的目标特征的。比如它会检测你是不是有安装CC或GCC，并不是需要CC或GCC，它是个shell脚本。

一般用来生成 `Makefile`，为下一步的编译做准备。

你可以通过在 `configure` 后加上参数来对安装进行控制，比如代码:

`./configure --prefix=/usr/local/xxx`

意思是将该软件安装在 `/usr/local/xxx` 下面，同时一些软件的配置文件你可以通过指定` --sys-config= `参数进行设定。
有一些软件还可以加上 `--with`、`--enable`、`--without`、`--disable `等等参数对编译加以控制，
可以通过 `./configure --help` 查看详细的帮助说明。

## make 

make 的作用是开始进行源代码编译，以及一些功能的提供，这些功能由他的 `Makefile` 设置文件提供相关的功能，
比如` make install `一般表示进行安装，make uninstall 是卸载，不加参数就是默认的进行源代码编译。
如果 在 make 过程中出现 error ，你就要记下错误代码（注意不仅仅是最后一行）。

make 是 Linux 开发套件里面自动化编译的一个控制程序，他通过借助 Makefile 里面编写的编译规范进行自动化的调用 gcc 、ld 以及运行某些需要的程序进行编译的程序。
一般情况下，他所使用的 Makefile ，由 configure 这个设置脚本根据给定的参数和系统环境生成。

## make install 

安装（当然有些软件需要先运行 make check 或 make test来进行一些测试），这一步一般需要你有 root 权限（sudo make install）