<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Elasticsearch.svg"/>
</p>
<h1 align="center">Elasticsearch—（提高篇）</h1>



## 前言

`Elasticsearch`的简单入门请参考之前写的一篇文章[Elasticsearch简单入门篇](http://luokangyuan.com/elasticsearchchu-ji-pian/)，这篇简单介绍啦`Elasticsearch`的基本安装、`Docker`安装方法、基本的概念，以及如何使用`Java`代码实现对`Elasticsearch`的`CRUD`操作等入门知识。

## 1.1.Elastic Stack应用场景

* 网站搜索、代码搜索等（例如生产环境的日志收集 ——格式化分析——全文检索——系统预警）
* 日志管理与分析、应用系统性能分析、安全指标监控等

## 1.2.Elastic Stack技术架构

### Elastic static家族产品

![image-20190802210856808](http://image.luokangyuan.com/2019-08-02-130901.png)

### 高级架构

`Elastic`的技术架构可以简单，也可以高级，它是很具有扩展性的，最简单的技术架构就是使用`Beats`进行数据的收集，`Beats`是一种抽象的称呼，具体的可以是使用`FileBeat`收集数据源为文件的数据或者使用`TopBeat`来收集系统中的监控信息，可以说类似`Linux`系统中的`TOP`命令，当然还有很多的`Beats`的具体实现，再使用`logstash`进行数据的转换和导入到`Elasticsearch`中，最后使用`Kibana`进行数据的操作以及数据的可视化等操作。

当然，在生产环境中，我们的数据可能在不同的地方，例如关系型数据库`Postgre`，或者`MQ`，再或者`Redis`中，我们可以统一使用`Logstash`进行数据的转换，同时，也可以根据数据的热度不同将`ES`集群架构为一种**冷温热**架构，利用`ES`的多节点，将一天以内的数据称谓热数据，读写频繁，就存放在`ES`的**热节点**中，七天以内的数据称之为**温数据**，就是偶尔使用的数据存放在**温节点**中，将极少数会用到的数据存放在**冷节点中**。

![image-20190802212002520](http://image.luokangyuan.com/2019-08-02-132006.png)

## 1.3.ES基本概念回顾

### 文档（Document）

`Elasticsearch`面向文档性，文档就是所有可搜索数据的最小单位。比如，一篇`PDF`中的内容，一部电影的内容，一首歌等，文档会被序列化成`JSON`格式，保存在`Elasticsearch`中，必不可少的是每个文档都会有自己的唯一标识，可以自己指定，也可以由`Elasticsearch`帮你生成。类似数据库的一行数据。

### 元数据（标注文档信息）

```json
"_index" : "user",
"_type" : "_doc",
"_id" : "l0D6UmwBn8Enzbv1XLz0",
"_score" : 1.6943597,
"_source" : {
    "user" : "mj",
    "sex" : "男",
    "age" : "18"
}
```

* `_index`：文档所属的索引名称。
* `_type`：文档所属的类型名。
* `_id`：文档的唯一标识。
* `_version`：文档的版本信息。
* `_score`：文档的相关性打分。
* `_source`：文档的原始`JSON`内容。

### 索引（index）

**索引**是文档的容器，是一类文档的集合，类似关系数据库中的表，索引体现的是一种逻辑空间的概念，每个索引都应该有自己的`Mapping`定义，用于定义包含文档的字段名和字段类型。其中`Shard`（分片）体现的是物理空间的一种概念，就是索引中的数据存放在`Shard`上，因为有啦集群，要保证高空用，当其中一个机器崩溃中，保存在它上的分片数据也能被正常访问，因此，存在啦**分片副本**。

**索引**中有两个重要的概念，`Mapping`和`Setting`。`Mapping`定义的是文档字段和字段类型，`Setting`定义的是数据的不同分布。

### 类型（Type）

* 在7.0之前，一个`index`可以创建多个`Type`。之后就只能一个`index`对应一个`Type`。

### 节点（Node）

一个节点就是一个`Elaseticsearch`实例，本质就是一个`JAVA`进程。每一个节点启动后，默认就是一个`master eligible`节点。就是具备成为`master`资格的节点，你也可以狠心的指定它没有这个资格（`node.master:false`），

第一个节点启动后，他就选自己成为`Master`节点类，每一个节点上都保存了集群状态，但是，只有`Master`才能修改集群状态信息。集群状态信息就比如：

* 所有的节点信息。
* 所有的索引信息，索引对应的`mapping`信息和`setting`信息。
* 分片的路由信息。

### 分片（shard）

* 主分片：用于解决数据的水平扩展问题，通过主分片就数据分布在集群内的不同节点上，主分片在创建索引的时候就指定了，后面就不允许修改，除非重新定义`Index`。
* 副本：用于解决高可用的问题，分片是主分片的拷贝。副本分片数可以动态的调整，增加副本数量可以在一定的程度上提高服务的可用性。关于主分片的理解可以如下图，看是怎样实现高可用的，

![image-20190802231314640](http://image.luokangyuan.com/2019-08-02-151319.png)

```json
"settings" : {
    "index" : {
        // 设置主分片数
        "number_of_shards" : "1",
        "auto_expand_replicas" : "0-1",
        "provided_name" : "kibana_sample_data_logs",
        "creation_date" : "1564753951554",
        // 设置副本分片数
        "number_of_replicas" : "1",
        "uuid" : "VVMLRyw6TZeSfUvvLNYXEw",
        "version" : {
            "created" : "7010099"
        }
    }
}
```

> 小思考：由于分片数是在创建索引的时候就已经设定了，那么设置过大或者过小有什么问题吗？

## 1.4.倒排索引

**正排索引**：就是文档`ID`到文档内容的索引，简单讲，就是根据`ID`找文档。

**倒排索引**：就是根据文档内容找**文档**。

**倒排索引**包含如下信息：

* 单词词典：用于记录**所有文档的单词**，以及单词到倒排列表的关联关系。
* 倒排列表：记录的是单词对应的文档集合，由倒排索引项组成，其中包含
  * 文档ID
  * 单词出现的次数，用于相关性的评分
  * 单词出现的位置
  * 偏移量，用于记录单词的开始位置和结束位置，用于单词的高亮显示

举例说明什么是**正排索引**和**倒排索引**，其中**正排索引**如下：

| 文档ID | 文档内容             |
| ------ | -------------------- |
| 1101   | Elasticsearch Study  |
| 1102   | Elasticsearch Server |
| 1103   | master Elasticsearch |

讲上例`Elasticsearch`单词修改为**倒排索引**，如下：

| 文档ID（Doc ID） | 出现次数（TF） | 位置（Position） | 偏移量（Offset） |
| ---------------- | -------------- | ---------------- | ---------------- |
| 1101             | 1              | 0                | <0,13>           |
| 1102             | 1              | 0                | <0,13>           |
| 1103             | 1              | 1                | <7,20>           |

> `Elasticsearch`中的每一个字段都有自己的倒排索引，也可以指定某些字段不做索引，可以节省存储空间，缺点就是不能被搜索到。

## 1.5.Analyzer分词

`Analysis`：文本分析，就是将文本转换为单词（`term`或者`token`）的过程，其中`Analyzer`就是通过`Analysis`实现的，`Elasticsearch`给我们内置例很多分词器。

* `Standard Analyzer`：默认的分词器，按照词切分，并作大写转小写处理
* `Simple Analyzer`：按照非字母切分（符号被过滤），并作大写转小写处理
* `Stop Anayzer`：停用词（`the`、`is`）切分，并作大写转小写处理
* `Whitespace Anayzer`：空格切分，不做大写转小写处理
* `IK`：中文分词器，需要插件安装
* `ICU`：国际化的分词器，需要插件安装
* `jieba`：时下流行的一个中文分词器。安装方法见附录

> PS：`Elasticsearch`安装插件，`[root@34d02ff9d16c elasticsearch]#  bin/elasticsearch-plugin install  analysis-icu`
>
> 查看已经安装的插件：`bin/elasticsearch-plugin list`

## 1.6.Search API

在`ES`中，我们可以使用`URL Search`和`Request Body Search`进行相关的查询操作。

### URL 查询

#### 使用基本的查询

```json
GET /user/_search?q=2012&df=title&sort=year:desc&from=0&size=10
{
	􏲽profile􏲿: true
}
```

* 使用`q`指定查询的字符串
* 使用`df`指定查询的字段
* 使用`sort`进行排序，使用`from`和`size`指定分页
* 使用`profile`可以查询查询是如何进行查询的

#### 指定所有字段的泛查询

```json
GET /user/_search?q=2012
{
	"profile":"true"
}
```
#### 指定字段的查询

```json

GET /user/_search?q=title:2012&sort=year:desc&from=0&size=10&timeout=1s
{
	"profile":"true"
}
```

#### Term查询

```json
GET /user/_search?q=title:Beautiful Mind
{
	"profile":"true"
}
```

* 上例中的`Beautiful`和`Mind`就是两个`Term`，`Term`是查询中最小的单位。
* `Term`查询是`OR`的关系，在上例中就是`title`字段包含`Beautiful`或者包含`Mind`都会被检索到。

#### Phrase查询

```json
GET /user/_search?q=title:"Beautiful Mind"
{
	"profile":"true"
}
```

* 使用**引号**表示`Phrase`查询
* `Phrase`查询表示的不仅是`And`的关系，即`Title`字段中不仅要包含`Beautiful Mind`，而且。顺序还要一致。

#### 分组查询

```json
GET /user/_search?q=title:(Beautiful Mind)
{
	"profile":"true"
}
```

* 使用**中括号**表示分组查询，一般使用`Term`查询的时候都会带上分组查询。

#### 布尔查询

* 使用 `AND`、`OR`、`NOT`或者`||`、`&&`、`!`
* 还可以使用`+`（表示`must`）,使用`-`（表示`must_not`）
* 需要注意的是必须大写

```json
GET /user/_search?q=title:(Beautiful NOT Mind)
{
	"profile":"true"
}
```

```json
GET /user/_search?q=title:(Beautiful %2BMind)
{
	"profile":"true"
}
```

> PS：`%2B`表示的就是`+`，上例子表示的就是`title`字段中既要包含`Beautiful`，也要包含`Mind`字段

#### 范围查询

```json
GET /user/_search?q=title:beautiful AND age:[2002 TO 2018%7D
{
	"profile":"true"
}
```

* 使用`[ ]`表示闭区间，使用`{ }`表示开区间，例如`age :[* TO 56]`
* 使用算术符表示范围，例如`year :>=2019 && <=1970`

> PS：`URL Search`还有很多查询方式。例如通配符查询，正则插叙，模糊匹配，相似查询，其中通配符查询不建议使用。

### Request Body 查询

将查询的条件参数放在`Request Body`中，调用查询接口，就是`Request Body`查询，

#### 基本 的查询

```json
POST /movies,404_idx/_search?ignore_unavailable=true
{
  "profile": true,
	"query": {
		"match_all": {}
	}
}
```

* 使用`gnore_unavailable=true`可以避免索引`404_idx`不存在导致的报错
* `profile`和`URL Search`查询一样，可以看到查询的执行方式

#### 分页查询

```json
POST /movies/_search
{
  "from":10,
  "size":20,
  "query":{
    "match_all": {}
  }
}
```

#### 排序查询

```json
POST /movies/_search
{
  "sort":[{"order_date":"desc"}],
  "query":{
    "match_all": {}
  }
}

```

#### 过滤要查询的字段

```json
POST /movies/_search
{
  "_source":["order_date"],
  "query":{
    "match_all": {}
  }
}
```

* 如果一个文档中的字段太多，我们不需全部字段显示，就可以使用`_source`指定字段。可以使用通配符。

#### 使用脚本查询

* 将`ES`中的文档字段进行一定的处理后，再根据这个新的字段进行排序，

```json
GET /movies/_search
{
  "script_fields": {
    "new_field": {
      "script": {
        "lang": "painless",
        "source": "doc['name'].value+'是大佬'"
      }
    }
  },
  "query": {
    "match_all": {}
  }
}
```

#### Term查询

```json
POST /movies/_search
{
  "query": {
    "match": {
      "title": "last christmas"
    }
  }
}

POST movies/_search
{
  "query": {
    "match": {
      "title": {
        "query": "last christmas",
        "operator": "and"
      }
    }
  }
}
```

* 使用`match`，表示的就是`OR`的关系
* 使用`operator`，表示查询方式

#### Math_phrase查询

```json
POST movies/_search
{
  "query": {
    "match_phrase": {
      "title":{
        "query": "one love",
         "slop": 4
      }
    }
  }
}
```

* `slop`指定查询的字符中允许出现的字符

## 附录一

### 相关阅读

* 安装`docker` ：[https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
* 安装 `docker-compose` ：[https://docs.docker.com/compose/install](https://docs.docker.com/compose/install)
* `Elasticsearch + Logstash + Kibana`的 `docker-compose `配置 ：[https://github.com/deviantony/docker-elk](https://github.com/deviantony/docker-elk)
* `docker`安装 `Elasticsearch `插件 ：[https://www.elastic.co/cn/blog/elasticsearch-docker-plugin-management](https://www.elastic.co/cn/blog/elasticsearch-docker-plugin-management)
* `Elasticsearch`的中文社区[https://elasticsearch.cn/](https://elasticsearch.cn/)
* `Beats`的产品：[https://www.elastic.co/cn/downloads/beats](https://www.elastic.co/cn/downloads/beats)
* 不错的中文分词器：[https://github.com/fxsjy/jieba](https://github.com/fxsjy/jieba)
* 不错的英文分词器：[https://github.com/nltk/nltk](https://github.com/nltk/nltk)
* `IK`分词器：[https://github.com/medcl/elasticsearch-analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik)
* `THULAC`分词器，清华大学自然语言处理系的分词器[https://github.com/thunlp/THULAC-Python](https://github.com/thunlp/THULAC-Python)
* `ES`发展史：[https://www.cnblogs.com/wangzhen3798/p/10751516.html](https://www.cnblogs.com/wangzhen3798/p/10751516.html)
* ELK6.0部署：：[Elasticsearch+Logstash+Kibana搭建分布式日志平台](https://ken.io/note/elk-deploy-guide?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
* 

## 附录二

### 分片设置

分片设置过小，导致后期水平扩展增加节点无用，同时，会导致单个分片的数据量庞大，重新分配耗时。

分片设置过大，导致资源浪费，影响性能。



## 附录三

### 常用API

```json
// 使用command + / 查看相关命令的信息
// 查看索引相关信息
GET kibana_sample_data_logs

//查看索引的文档总数
GET kibana_sample_data_logs/_count

// 查看前十条文档信息
POST kibana_sample_data_logs/_search

// 查看集群的健康状态
GET _cluster/health

// 查看节点信息
GET _cat/nodes

// 查看分片信息
GET _cat/shards

// 创建一个文档，自动生成文档标识
POST user/_doc 
{
  "user": "mj",
  "sex": "男",
  "age": "18"
}

// 创建一个文档，自己指定文档标识,再次创建会报错
PUT user/_doc/123456?op_type=create
{
   "user": "mj01",
  "sex": "男",
  "age": "19"
}

// 查询具体的文档
GET user/_doc/123456

// 修改文档中现有的内容，先删除，在增加
PUT user/_doc/123456
{
  "user": "张三丰"
}

// 在原有文档上增加内容
POST user/_update/123456/
{
  "doc": {
    "money": "0.01"
  }
}

// 批量操作
GET _mget
{
  "docs":[
    {
      "_index":"user",
      "_id": "123456"
    },
    {
      "_index":"user2",
      "_id": "123456"
    }
    ]
}
```

