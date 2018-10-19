# 服务器记录并查询历史操作记录

> Linux服务器在使用过程中，经常会有除自己之外的其他人员使用。并不是每个人都对Linux服务器特别熟悉，难免会有一些操作导致服务器报错。


history是查询当前连接所操作的命令，通过编写以下内容添加至/etc/profile的原有内容之后，将每个连接的操作都进行记录，并保存在特定位置。

`vim /etc/profile`

添加一下内容

```
history
RQ=`date "+%Y%m%d"`
USER_IP=`who -u am i 2>/dev/null| awk '{print $NF}'|sed -e 's/[()]//g'`
if [ "$USER_IP" = "" ]
then
USER_IP=`hostname`
fi
if [ ! -d /var/log/histories ]
then
mkdir /var/log/histories
chmod 777 /var/log/histories
fi
export HISTSIZE=8192
DT=`date "+%H:%M:%S"`
export HISTFILE="/var/log/histories/history.log"
export PROMPT_COMMAND='{ date "+[%Y-%m-%d %T] ##### $(who am i |awk "{print \$1\" \"\$2\" \"\$5}") #### $(pwd) #### $(history 1 | { read x cmd; echo "$cmd"; })"; } >> $HISTORY_FILE'
chmod 600 /var/log/histories/history.log 2>/dev/null
```

然后保存并退出，执行以下命令，使得编写的配置生效。

`source /etc/profile`

操作审计日志 保存在 `/var/log/histories/history.log`

日志管理[请看这](logrotate.md)