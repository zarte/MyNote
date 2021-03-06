[TOC]
##安装 jdk
```
CentOS6.4 64位系统安装jdk
1. CentOS操作安装好了以后，系统自带了openJDK，先查看相关的安装信息：
$rpm -qa | grep java
tzdata-java-2013b-1.el6.noarch
java-1.6.0-openjdk-1.6.0.0-1.61.1.11.11.el6_4.x86_64
java-1.7.0-openjdk-1.7.0.19-2.3.9.1.el6_4.x86_64

2. 可以用java -version命令查看系统自带jdk的版本信息
$ java -version
java version "1.7.0_19"
OpenJDK Runtime Environment (rhel-2.3.9.1.el6_4-x86_64)
OpenJDK 64-Bit Server VM (build 23.7-b01, mixed mode)

3. 卸载系统自带openJDK
$ rpm -e --nodeps java-1.7.0-openjdk-1.7.0.19-2.3.9.1.el6_4.x86_64
$ rpm -e --nodeps java-1.6.0-openjdk-1.6.0.0-1.61.1.11.11.el6_4.x86_64
$ rpm -e --nodeps tzdata-java-2013b-1.el6.noarch

4. 安装自己的JDK
JDK下载地址：http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html

下载后上传到linux服务器中
$ rpm -ivh jdk-7u17-linux-x64.rpm

也可以下载源码安装
#tar zxvf jdk-7u17-linux-x64.tar.gz -C /usr/
#cd /usr/
#mv jdk1.7.17/ java
默认情况下，jdk将会安装在/usr/java 目录下

5. 配置环境变量
$vim /etc/profile
在profile文件末尾加入如下内容：
JAVA_HOME=/usr/java/jdk1.7.0_17
JRE_HOME=/usr/java/jdk1.7.0_17/jre
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
export JAVA_HOME JRE_HOME PATH CLASSPATH
 
6. 使配置文件立即生效
$source /etc/profile

7. 验证是否安装成功
依次输入java, java -version, javac可以查看到jdk的安装信息，说明安装成功

8. 查看JAVA_HOME
$echo JAVA_HOME

附：windows环境下安装JDK之后环境变量是这样配置的：

JAVA_HOME:当前jdk路径
PATH:%JAVA_HOME%/bin;%JAVA_HOME%/jre/bin
CLASSPATH:.;%JAVA_HOME%/lib/dt.jar;%JAVA_HOME%/lib/tools.jar
```
##centos关闭防火墙
- CentOS 6:
1） 永久性生效，重启后不会复原
开启： chkconfig iptables on
关闭： chkconfig iptables off
2） 即时生效，重启后复原
开启： service iptables start
关闭： service iptables stop

- CentOS 7:
systemctl start firewalld.service#启动firewall
systemctl stop firewalld.service#停止firewall
systemctl disable firewalld.service#禁止firewall开机启动

查询TCP连接情况：
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
查询端口占用情况：
netstat   -anp   |   grep  portno（例如：netstat –apn | grep 80）

