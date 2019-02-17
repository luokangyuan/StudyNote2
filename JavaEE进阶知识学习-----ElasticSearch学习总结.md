# 一、ElasticSearch学习笔记

## 1.1.ElasticSearch简介



## 1.2.ElasticSearch安装

### 系统环境

* CentOS  7.6.1810
* jdk 1.8.0_201

### 所需安装文件

* elasticsearch-6.6.0.tar.gz    
* jdk-8u201-linux-x64.tar.gz
* elasticsearch-head-master.zip  
* node-v10.15.1-linux-x64.tar.gz

### elasticsearch安装方法

```bash
tar -zxvf elasticsearch-6.6.0.tar.gz -C /opt/module/ # 解压安装包
```

```bash
[root@localhost elasticsearch-6.6.0]# mkdir data # 创建数据文件夹（6.0自带logs文件夹）
```

```bash
vi elasticsearch.yml # 修改配置文件
```

```bash
cluster.name: my-application # 集群名称（多集群时候只需节点名称一直即可）
node.name: node-102 # 节点名称
path.data: /opt/module/elasticsearch-6.6.0/data # 数据路径
path.logs: /opt/module/elasticsearch-6.6.0/logs # 日志路径
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
network.host: 192.168.1.8 # 网络地址
http.port: 9200 # 端口
discovery.zen.ping.unicast.hosts: ["hadoop102"] # 主机名
```

> 注意：node.name可以随便取，但是一个集群中不能重复，注意path.data前不能有空格，冒号后必须有一个空格

 ###  elasticsearch常见问题解决

`问题一：ERROR: bootstrap checks failed`

```bash
su root
vi /etc/security/limits.conf
```

```bash
* soft nofile 65536
* hard nofile 131072
* soft nproc 2048
* hard nproc 4096
```

`问题二：max number of threads [1024] for user [lish] likely too low, increase to at least [2048]`

```bash
vi /etc/security/limits.d/90-nproc.conf
* soft nproc 2048
```

`问题三：max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]`

```bash
vi /etc/sysctl.conf
vm.max_map_count=655360
sysctl -p
```

### elasticsearch启动

> elasticsearch禁止使用root用户启动，需要新建一个testuser用户

```bash
[testuser@hadoop102 elasticsearch-6.6.0]$ ./bin/elasticsearch
```

**访问地址：**http://192.168.1.8:9200/

## 1.3.ElasticSearch核心概念



## 结语，使用的Linux命令

* `mkdir` 创建文件夹
* `pwd` 查看当前所在路径
* `scp -r CentOS-7-x86_64-Minimal-1810.iso  root@192.168.1.8:/opt/sortware` 在当前文件上传文件到指定服务器文件夹
* `cat /etc/centos-release` 查看系统版本
* `rm -rf jdk-8u201-linux-i586.tar.gz` 不提示的递归删除文件或者文件夹
* `tar -zxvf jdk-8u201-linux-x64.tar.gz` 解压
* `hostnamectl`查看主机名
* `whereis sudoers` 查找文件位置
* `ls -l /etc/sudoers` 查看文件权限
* `chmod -v u+w /etc/sudoers` 加入可写权限
* `firewall-cmd --state` 查看防火墙状态
* `systemctl stop firewalld.service` 关闭防火墙



### CentOS 7安装jdk1.8

**1.上传安装文件**

```bash
/opt/sortware/jdk-8u201-linux-x64.tar.gz
```

**2.解压**

```bash
tar -zxvf jdk-8u201-linux-x64.tar.gz
```

**3.重命名**

```bash
mv jdk1.8.0_201 jdk1.8
```

**4.打开系统配置文件**

```bash
vi /etc/profile
```

**5.添加环境变量**

```bash
## Java
export JAVA_HOME=/opt/sortware/jdk1.8
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
```

**6.重启配置文件**

```bash
source /etc/profile
```

**7.查看版本**

```bash
java -version
```

