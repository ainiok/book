# Jenkins
> Jenkins 是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

# 安装 (centOS7)
* [下载地址](https://jenkins.io/download/)

   ### yum
   
   1. 拉取库的配置到本地文件
   `wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo `
   LTS 版本有点点不同
   `wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo`
   
   2. 导入公钥
   `rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key`
   
   3. 安装
   `yum install jenkins`
   
   查找安装路径
   `rpm -ql jenkins`
   
   jenkins相关目录释义：
   
   + /usr/lib/jenkins/：jenkins安装目录，war包会放在这里。
   + /etc/sysconfig/jenkins：jenkins配置文件，"**端口**"，"JENKINS_HOME"等都可以在这里配置。
   + /var/lib/jenkins/：默认的JENKINS_HOME。
   + /var/log/jenkins/jenkins.log：jenkins日志文件。
   
   
   ### 下载java包 (.war)
   
   ```
   mkdir -p /usr/local/jenkins/ && \
   wget -c -O /usr/local/jenkins/jenkins.war http://mirrors.jenkins.io/war-stable/latest/jenkins.war &&\
   nohup java -jar /usr/local/jenkins/jenkins.war &
   ```

# 配置

> jenkins 默认端口是8008



## Nginx反向代理

* [官网介绍](https://wiki.jenkins.io/display/JENKINS/Jenkins+behind+an+NGinX+reverse+proxy)