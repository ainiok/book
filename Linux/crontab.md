# 定时任务

## anacrontab

系统级别的计划任务及其扩展anacrontab anacrontab就是系统计划任务的扩展文件：在一个指定时间间隔错过后自动执行任务,这个是系统设置好了，清理系统垃圾或者是自动执行某些脚本的系统任务，一般我们做了解就行了，不要更改 配置文件是`/etc/anacontab`

```
# /etc/anacrontab: configuration file for anacron

# See anacron(8) and anacrontab(5) for details.

SHELL=/bin/sh
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
# the maximal random delay added to the base delay of the jobs
RANDOM_DELAY=45
# the jobs will be started during the following hours only
START_HOURS_RANGE=3-22

#period in days   delay in minutes   job-identifier   command
1	5	cron.daily		nice run-parts /etc/cron.daily
7	25	cron.weekly		nice run-parts /etc/cron.weekly
@monthly 45	cron.monthly		nice run-parts /etc/cron.monthly

``` 

SHELL：就是运行计划任务的解释器，默认是bash PATH：执行命令的环境变量 
MAILTO：计划任务的出发者用户  
run-parts是一个脚本，在`/usr/bin/run-parts`，作用是执行一个目录下的所有脚本/程序。 
run-parts /etc/cron.hourly执行目录/etc/cron.hourly/之下的所有脚本/程序。
第一行的意思是：每天开机65分钟后就检查cron.daily文件是否被执行了，如果今天没有被执行就执行它。
第二行的意思是：每隔7天开机后70分钟检查cron.weekly文件是否被执行了,如果一周内没有被执行就执行它。
日志文件位于/var/spool/cron


## crond 

```
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

使用实例：

每1分钟执行一次command: ` * * * * * command`

每小时的第3和15分钟执行: `3,15 * * * * command`

上午8点到11点的第3和第15分钟执行：`3,15 8-11 * * * command`

每隔两天的上午8点到11点的第3和第15分钟执行: `3,15 8-11 */2 * * command`

每个星期一的上午8点到11点的第3和第15分钟执行: `3,15 8-11 * * 1 command`

每月1、10、22日的5 : 30执行：`30 5 1,10,22 * * command`

每周六、周日的1 : 10：`10 1 * * 6,0 command`

