# 0x00服务器安装JDK
### 下载JDK
下载地址：``http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html``
解压安装 ``tar -zxvf jdk-8u211-linux-x64.tar.gz``

### 配置
在usr/lib目录下创建一个jdk文件夹 将解压出来文件夹移进去
```bash
cd /usr/lib
mkdir jdk
mv jdk-8u211-linux-x64 /usr/lib/jdk
```
配置环境变量``vim /etc/profile``
打开后在末尾加上
```BASH
#set java env
export JAVA_HOME=/usr/lib/jdk/jdk1.8.0_171
export JRE_HOME=${JAVA_HOME}/jre    
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib    
export PATH=${JAVA_HOME}/bin:$PATH
```
执行立即生效``source /etc/profile``

### 配置软链接 
```bash
sudo update-alternatives --install /usr/bin/java  java  /usr/lib/jdk/jdk1.8.0_211/bin/java 300   
sudo update-alternatives --install /usr/bin/javac  javac  /usr/lib/jdk/jdk1.8.0_211/bin/javac 300 
```

### 检查是否配置成功
```bash
root@iZwz9d1cy0175pjnvgalocZ:~# java -version
java version "1.8.0_211"
Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.211-b12, mixed mode)
root@iZwz9d1cy0175pjnvgalocZ:~#
```
# 0x01安装tomcat
### 下载tomcat
进入官方：https://tomcat.apache.org/download-80.cgi 安装tar.gz包

### 配置
在usr目录下创建一个tomcat目录，将安装包解压移动到里面
``mv apache-tomcat-8.5.9 /usr/tomcat/``
给tomcat目录权限 ``chmod 755 -R tomcat``
编辑bin目录下的startup.sh和shutdown.sh 在``exec "$PRGDIR"/"$EXECUTABLE" start "$@"``上面加自己java路径和tomcat路径
```bash
#set java environment
export JAVA_HOME=/usr/lib/jdk/jdk1.8.0_211
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

#tomcat
export TOMCAT_HOME=/usr/tomcat/apache-tomcat-8.5.40
```
``./startup.sh``开启
``./shutdown.sh``关闭
### 服务器防火墙安全组
这个坑踩了很多次，使用的是阿里云服务，需要把开放的端口加入到安全组中。
在服务器防火墙添加规则，将端口8080加入既可以访问
#  0x02Javaweb工程部署
自己可以把工程打包成war包或者直接由out出来的文件夹放到webapps文件夹中既可以，怎么打包就不细说了
war包：http://nosystemsafe.cn:8080/html_war/
文件夹形式：http://nosystemsafe.cn:8080/html_war_exploded/
(图片可能存在显示问题，不要紧不要紧~!)
