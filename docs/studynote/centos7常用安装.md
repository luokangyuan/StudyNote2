# 一、centos7软件安装

## 1.1.jdk1.8安装

### 下载安装包

* 官网下载：[https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* 网盘下载：[https://pan.baidu.com/s/1JyZgHxiwQi-Fm9Zv_uzpLg](https://pan.baidu.com/s/1JyZgHxiwQi-Fm9Zv_uzpLg)  `6s3w`

### 解压

```bash
tar -zxvf jdk-8u212-linux-x64.tar.gz 
```

### 配置环境变量

```bash
vim /etc/profile
```

```bash
# jdk配置
JAVA_HOME=/opt/soft/jdk/jdk1.8.0_212
JRE_HOME=/opt/soft/jdk/jdk1.8.0_212/jre
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
export JAVA_HOME JRE_HOME PATH CLASSPATH
```

### 配置生效

```bash
source /etc/profile
```

### 安装成功

```bash
java -version
```

```java
openjdk version "1.8.0_181"
OpenJDK Runtime Environment (build 1.8.0_181-b13)
OpenJDK 64-Bit Server VM (build 25.181-b13, mixed mode)
```

## 1.2.RocketMQ 4.3.2安装

### 下载安装包

```bash
 wget http://mirror.bit.edu.cn/apache/rocketmq/4.3.2/rocketmq-all-4.3.2-bin-release.zip
```

```bash
unzip rocketmq-all-4.3.2-bin-release.zip
```

```bash
mv rocketmq-all-4.3.2-bin-release rocketmq
```

### 增加环境配置

```bash
# rocketMq配置
ROCKETMQ_HOME=/opt/soft/rocketmq
PATH=$ROCKETMQ_HOME/bin:$PATH
export ROCKETMQ_HOME PATH
```

```bash
source /etc/profile
```

### 修改mq启动的内存

```bash
root@localhost bin]# vim runbroker.sh 
```

```bash
JAVA_OPT="${JAVA_OPT} -server -Xms4g -Xmx4g -Xmn2g"
```

### 启动NameServer

```bash
sh mqnamesrv  -n 172.25.8.178:9876 &
```

### 修改broker.conf

```bash
# 集群名字
brokerClusterName = mj

# 注册中心的IP地址
namesvAddr=172.25.8.178:9876

# 节点的地址
brokerIP1=127.0.0.1
# 节点名字
brokerName = mj-broker-master

# brokerId = 0 表示此broker是master 
brokerId = 0
deleteWhen = 04
fileReservedTime = 48
brokerRole = ASYNC_MASTER
flushDiskType = ASYNC_FLUSH
autoCreateTopicEnable = true
```

### 启动Broker

```bash
sh mqbroker -c ../conf/broker.conf
```

### docker安装RocketMQ控制台

```bash
docker pull styletang/rocketmq-console-ng
```

```bash
docker run -e "JAVA_OPTS=-Drocketmq.namesrv.addr=172.25.8.178:9876 -Dcom.rocketmq.sendMessageWithVIPChannel=false" -p 8083:8080 -t styletang/rocketmq-console-ng
```

```bash
http://172.25.8.178:8083/#/
```

> 说明：172.25.8.178是我虚拟机的IP地址

## 1.3.docker安装

###  安装GCC

```bash
 yum -y install gcc
```

```bash
yum -y install gcc-c++
```

### 安装软件包

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 设置stable镜像仓库

```bash
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 更新软件包

```bash
yum makecache fast
```

### 安装docker社区版本

```bash
yum -y install docker-ce
```

### 启动docker

```bash
systemctl start docker
```

## 1.4.Maven安装

### 下载安装包

[apache-maven-3.6.0-bin.tar.gz](http://mirrors.hust.edu.cn/apache/maven/maven-3/3.6.0/binaries/)

### 解压

```bash
[root@localhost apache-maven-3.6.0]# tar -zxvf apache-maven-3.6.0-bin.tar.gz
```

### 配置环境变量

```bash
vim /etc/profile
```

```bash
# maven配置
M2_HOME=/opt/soft/apache-maven-3.6.0
PATH=$PATH:$JAVA_HOME/bin:$M2_HOME/bin
export M2_HOME PATH
```

### 配置生效

```bash
source /etc/profile
```

