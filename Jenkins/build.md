


Maven Integration



Q:jenkins Error performing command: git ls-remote -h


whereis git



安装 Maven 

 download:  http://maven.apache.org/download.cgi
 
 
 vi /etc/profile
 
 export MAVEN_HOME=/usr/local/maven
 export PATH=$PATH:$MAVEN_HOME/bin
 
 
 source /etc/profile
 
 
 mvn -version
 
 
 安装 Phing  (构建PHP项目)