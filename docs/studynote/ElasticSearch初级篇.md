<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Elasticsearch.svg"/>
</p>
<h1 align="center">ElasticSearch学习笔记</h1>


> 撸一门技术，必先登其官网，扒其皮，喝其血

**官网地址：**[https://www.elastic.co/products/elasticsearch](https://www.elastic.co/products/elasticsearch)

**官方中文文档地址：**[https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html](https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html)

## 1.1.ElasticSearch简介

`ElasticSearch`是一个分布式搜索服务，提供的是一组`Restful API`，底层基于`Lucene`，采用多`shard`（分片）的方式保证数据安全，并且提供自动resharding的功能。是目前全文搜索引擎的首选，可以快速的存储、搜索和分析海量数据，`Springboot`通过整合`Spring Data ElasticSearch`为我们提供了非常方便的检索功能支持。

## 1.2.ElasticSearch原始安装

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

### ElasticSearch插件安装

**插件地址：**[https://github.com/zt1115798334/elasticsearch-head-master](https://github.com/zt1115798334/elasticsearch-head-master)

#### Nodejs安装

```bash
tar -zxvf node-v10.15.1-linux-x64.tar.gz -C /opt/module/
```

```bash
vi /etc/profile
export NODE_HOME=/opt/module/node-v10.15.1-linux-x64
export PATH=$PATH:$NODE_HOME/bin
source /etc/profile 
```

#### elasticsearch-head-master安装

```bash
[root@hadoop102 sortware]# unzip elasticsearch-head-master.zip -d /opt/module/
```

```bash
[root@hadoop102 elasticsearch-head-master]# npm install grunt --save
```

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

```bash
npm install -g grunt-cli
```

```bash
vim Gruntfile.js
```

```json
options: {
    hostname:'0.0.0.0',
    port: 9100,
    base: '.',
    keepalive: true
}
```

```bash
# 检查head根目录下是否存在base文件夹 没有：将 _site下的base文件夹及其内容复制到head根目录下
mkdir base
cp base/* ../base/
```

```bash
[root@hadoop102 module]# chown -R luokangyuan:luokangyuan elasticsearch-head-master/
```

```bash
[luokangyuan@hadoop102 elasticsearch-head-master]$ grunt server -d
```

```bash
npm install grunt-contrib-clean -registry=https://registry.npm.taobao.org
npm install grunt-contrib-concat -registry=https://registry.npm.taobao.org
npm install grunt-contrib-watch -registry=https://registry.npm.taobao.org 
npm install grunt-contrib-connect -registry=https://registry.npm.taobao.org
npm install grunt-contrib-copy -registry=https://registry.npm.taobao.org 
npm install grunt-contrib-jasmine -registry=https://registry.npm.taobao.org
```

```bash
http://192.168.1.8:9100/
```

```bash
vi elasticsearch.yml
http.cors.enabled: true
http.cors.allow-origin: "*"
```

## 1.3.ElasticSearch的docker安装

### 启动Docker

```bash
[root@localhost /]# systemctl start docker
```

### 搜索镜像

``` bash
[root@localhost /]# docker search elasticsearch
```

### 使用镜像加速器下载

```bash
[root@localhost /]# docker pull registry.docker-cn.com/library/elasticsearch
```

### 检查是否安装成功

```bash
[root@localhost /]# docker images
```

### 启动ElasticSearch

```bash
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -p 9200:9200 -p 9300:9300 --name ES01 5acf0e8da90b
```

### 检查是否启动

```bash
[root@localhost /]# docker ps
```

### 访问测试

```bash
http://192.168.1.14:9200/
```

## 1.4.ElasticSearch核心概念

### Cluster集群

集群就是包含了多个节点，每一个节点属于哪一个集群是通过一个集群名称配置。

### Node节点

集群中的一个节点，节点也存在名称，默认是随机分配一个名称，默认节点会加入到一个`elasticsearch`集群中。

### Index索引

索引包含的是一大推相似结构的文档数据，例如我们的商品索引，订单索引等，类比于我们的数据库。

### Type类型

每一个索引里面可以有一个或者多个`type`，`type`是`index`中的一个逻辑数据分类，比如我的博客系统，一个索引，可以定义用户数据`type`，可以定义文章数据`type`，也可以定义评论数据`type`，类比数据库的表。

### Document文档

文档是`ElasticSearch`中最小的数据单元，一条`Document`可以是一条文章数据，一条用户数据，一条评论数据，通常使用`JSON`数据结构来表示，每个`index`下的`type`中，存储多个`document`，类别数据库中的行。

### Field字段

`Field`是`ElasticSearch`中的最小单位，一个`document`里面粗在多个`Field`字段，每个`Field`就是一个数据字段，类比数据库中的列。

### mapping映射

数据如何存储在索引上，需要一个约束配置，例如数据类型，是否存储，查询的时候是否分词等等，类比数据库汇总的约束。

### ElasticSearch和数据库对别

| 关系型数据库Mysql | 非关系型数据库ElasticSearch |
| ----------------- | --------------------------- |
| 数据库Database    | 索引Index                   |
| 表Table           | 类型Type                    |
| 数据行Row         | 文档Document                |
| 数据列Column      | 字段Field                   |
| 约束Schema        | 映射Mapping                 |

### ElasticSearch核心概念图解

![WX20190223-173048@2x](http://image.luokangyuan.com/2019-02-23-093122.png)

## 1.5.Springboot集成ElasticSearch

 `Springboot`默认使用`Spring  Data Elasticsearch`模块进行操作，同时也存在另外一个操作`ElasticSearch`的模块，那就是`jest`。

### 使用Jest与ElasticSearch进行交互

**Jest的GitHub地址：**[https://github.com/searchbox-io/Jest](https://github.com/searchbox-io/Jest)

**Jest文档地址：**[https://github.com/searchbox-io/Jest/tree/master/jest](https://github.com/searchbox-io/Jest/tree/master/jest)

**第一步：增加POM 文件**

```xml
<dependency>
    <groupId>io.searchbox</groupId>
    <artifactId>jest</artifactId>
    <version>5.3.4</version>
</dependency>
```

**第二步：增加ElasticSearch配置项**

```yaml
spring:
  elasticsearch:
    jest:
      uris: http://192.168.1.9:9200/
```

**第三步：使用JestClient进行交互**

```java
public class Users {
    // 标示主键字段
    @JestId
    private Integer id;
    private Integer code;
    private String name;
    private String sex;
    private String age;
    private String notes;
}
```

```java
@Autowired
JestClient jestClient;

@Test
public void contextLoads() {
    // 给es中保存一份文档
    Users users = new Users();
    users.setId(2);
    users.setCode(123456);
    users.setAge("87");
    users.setName("鲁班七号");
    users.setSex("男");
    users.setNotes("王者峡谷人见人想揍的小鲁班");

    // 构建一个王者荣耀的索引和英雄角色类型
    Index build = new Index.Builder(users).index("wzry").type("yxjs").build();

    try {
        jestClient.execute(build);
    } catch (IOException e) {
        e.printStackTrace();
    }
}

@Test
public void testSeach(){
    // 测试搜索es中满足条件的数据
    String json = "{\n" +
        "    \"query\" : {\n" +
        "        \"match\" : {\n" +
        "            \"notes\" : \"峡谷小人\"\n" +
        "        }\n" +
        "    }\n" +
        "}";
    Search build = new Search.Builder(json).addIndex("wzry").addType("yxjs").build();
    try {
        SearchResult execute = jestClient.execute(build);
        System.out.println(execute.getJsonString());
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

**最后测试**

```properties
http://192.168.1.9:9200/wzry/yxjs/2
```

### 使用Spring  Data Elasticsearch

**官方文档地址：**[https://docs.spring.io/spring-data/elasticsearch/docs/3.1.5.RELEASE/reference/html/](https://docs.spring.io/spring-data/elasticsearch/docs/3.1.5.RELEASE/reference/html/)

**第一步：增加POM文件**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

**第二步：增加配置项**

```yaml
spring:
  data:
    elasticsearch:
      cluster-name: elasticsearch
      cluster-nodes: 192.168.1.9:9300
      repositories:
        enabled: true
```

**第三步：进行数据交互**

```java
@Document(indexName = "study", type = "book")
public class Book {
    private Integer id;
    private String name;
    private String notes;
}
```

```java
public interface BookRepository extends ElasticsearchRepository<Book, Integer> {
}
```

```java
@Autowired
BookRepository bookRepository;

@Test
public void testSpringDataEs(){
    Book book = new Book();
    book.setId(11);
    book.setName("一个陌生女人的来信");
    book.setNotes("还不错");
    bookRepository.index(book);
}
```

> 注意：如果启动报错，可能是spring data elasticsearch和elasticsearch存在版本对应关系

版本对应参考官方文档：[https://github.com/spring-projects/spring-data-elasticsearch/blob/master/README.md](https://github.com/spring-projects/spring-data-elasticsearch/blob/master/README.md)

| spring data elasticsearch | elasticsearch |
| ------------------------- | ------------- |
| 3.2.x                     | 6.5.0         |
| 3.1.x                     | 6.2.2         |
| 3.0.x                     | 5.5.0         |
| 2.1.x                     | 2.4.0         |
| 2.0.x                     | 2.2.0         |
| 1.3.x                     | 1.5.2         |

**版本不适配解决方法**

* 查看`spring data elasticsearch`的版本号
* 安装对应版本的`elasticsearch`即可
* 当然也可以根据安装的`elasticsearch`版本改变`Springboot`版本

**解决办法示例：**

```bash
# 安装对应版本的elasticsearch
[root@localhost /]# docker pull registry.docker-cn.com/library/elasticsearch:2.4
```

```bash
# 启动对应版本的elasticsearch
[root@localhost /]# docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -p 9201:9200 -p 9301:9300 --name ES14  01e5bee1e059
```

**我个人本次测试环境：**

* Springboot：`1.5.19`
* elasticsearch：`2.4`

## 附录一：使用的Linux命令

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

## 附录二：CentOs7安装jdk1.8

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

## 附录三：CentOs7安装docker

![CentOs7安装步骤](http://image.luokangyuan.com/2019-02-23-075620.png)

## 附录四：常用docker命令

**删除所有容器**

```bash
docker rm `docker ps -a -q`
```

**查看存在的镜像**

```bash
docker images
```

**查看所有启动的容器**

```bash
docker ps -a
```

**停止容器**

```bash
docker stop
```

**搜索仓库**

```bash
docker search elasticsearch
```

**拉取仓库**

```bash
 docker pull registry.docker-cn.com/library/elasticsearch
```
