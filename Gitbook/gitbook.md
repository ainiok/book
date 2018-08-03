# Setup and Installation of GitBook

Getting GitBook installed and ready-to-go should only take a few minutes.

### legacy.gitbook.com

[legacy.gitbook.com](https://legacy.gitbook.com) is an easy to use solution to write, publish and host books. It is the easiest solution for publishing your content and collaborating on it.

It integrates well with the [GitBook Editor](https://legacy.gitbook.com/editor).

### 安装

##### 需求

安装GitBook很简单，你的系统只要满足一下两个条件

* NodeJS (v4.0.0 以上)
* Windows, Linux, Unix, or Mac OS X

#####  用NPM安装

安装GitBook的最好方式就是使用**NPM**，只需要运行一下命令

```
$ npm install gitbook-cli -g
```

`gitbook-cli` 是相同的系统上安装和使用多个版本的实用工具,它将自动安装所需版本的GitBook来构建一本书。

##### 新建书籍

GitBook 可以生成一个样板书(**在当前目录**):

```
$ gitbook init
```

如果你想创建到一个新的目录，可以在后面加上目录 `gitbook init ./directory`

> README.md 和 SUMMARY.md 是两个必须文件，README.md 是对书籍的简单介绍

书籍目录结构创建完成以后，就可以使用 gitbook serve 来编译和预览书籍了:

```
$ gitbook serve
```

或使用静态网站构建:

```
$ gitbook build
```
```gitbook serve``` 命令实际上会首先调用 gitbook build 编译书籍，完成以后会打开一个 web 服务器，监听在本地的 4000 端口。

##### Install pre-releases

`gitbook-cli` 可以下载和安装GitBook的其他版本来测试：

```
$ gitbook fetch beta
```

使用 `gitbook ls-remote` 列出可用于安装的版本.

##### Debugging

You can use the options `--log=debug` and `--debug` to get better error messages (with stack trace). For example:

```
$ gitbook build ./ --log=debug --debug
```