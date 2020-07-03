## windows10下安装scoop

scoop是可用于windows的一款包管理工具,在这记录一下安装过程

> 前提： 打开 Windows PowerShell 

### 1、修改`C:\Windows\System32\drivers\etc\hosts` 文件
由于`raw.githubusercontent.com` 无法连接，所以需要设置本地虚拟域名
可通过 [`IPAddress`](https://www.ipaddress.com/) 查询到 `raw.githubusercontent.com` 的`ip` 是 199.232.68.133
在 hosts文件中添加以下内容

`199.232.28.133  raw.githubusercontent.com`

### 2、打开 `powershell`,然后输入以下代码

```bash
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
``` 

```bash
执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): Y
```


> 如果不执行以上面的指令， 在安装的时候会提示

```bash
PowerShell requires an execution policy in [Unrestricted, RemoteSigned, ByPass] to run Scoop.
For example, to set the execution policy to 'RemoteSigned' please run :
'Set-ExecutionPolicy RemoteSigned -scope CurrentUser'
```

### 3、安装 `Scoop`

执行 `iex (new-object net.webclient).downloadstring('https://raw.githubusercontent.com/lukesampson/scoop/master/bin/install.ps1')`

```bash
C:\WINDOWS\system32> iex (new-object net.webclient).downloadstring('https://raw.githubusercontent.com/lukesampson/scoop/master/bin/install.ps1')
Initializing...
Downloading scoop...
Extracting...
Creating shim...
Downloading main bucket...
Extracting...
Adding ~\scoop\shims to your path.
'lastupdate' has been set to '2020-07-03T14:19:45.6076584+08:00'
Scoop was installed successfully!
Type 'scoop help' for instructions.

```
