# 一、NoSQL简介

## 1.1NoSOL是什么

Not only sql，代表的不仅仅是sql，**没有声明性查询语言**；没有预定义的模式；**使用键值对，列存储，文档存储，图形数据库的方式存储**；最终一致性，而非ACID属性；CAP定理和高性能，高可用性；

## 1.2.数据库原理CAP+Base

**传统关系型数据库的ACID**


**A (Atomicity) 原子性**
原子性很容易理解，也就是说事务里的所有操作要么全部做完，要么都不做，事务成功的条件是事务里的所有操作都成功，只要有一个操作失败，整个事务就失败，需要回滚。


**C (Consistency) 一致性**
一致性也比较容易理解，也就是说数据库要一直处于一致的状态，事务的运行不会改变数据库原本的一致性约束。


**I (Isolation) 独立性**
所谓的独立性是指并发的事务之间不会互相影响，如果一个事务要访问的数据正在被另外一个事务修改，只要另外一个事务未提交，它所访问的数据就不受未提交事务的影响。

**D (Durability) 持久性**
持久性是指一旦事务提交后，它所做的修改将会永久的保存在数据库上，即使出现宕机也不会丢失。

**CAP理论**

CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性；

**Base理论**

BASE就是为了解决关系数据库强一致性引起的问题而引起的可用性降低而提出的解决方案。BASE是下面三个术语的缩写：**基本可用**（Basically Available）、**软状态**（Soft state）和**最终一致**（Eventually consistent）

# 二、Redis简介

REmote DIctionary Server(远程字典服务器) ,是完全开源免费的，用C语言编写的，遵守BSD协议，是一个高性能的(key/value)分布式内存数据库，基于内存运行并支持持久化的NoSQL数据库，是当前最热门的NoSql数据库之一,也被人们称为**数据结构服务器**;

## 2.1.Redis的特点

Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用 ，Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储 ，Redis支持数据的备份，即master-slave模式的数据备份 。详细资料参考官网[英文官网](http://redis.io/ )和[Redis中文网](http://www.redis.cn/ )。

## 2.2.Redis安装

**第一步：连接虚拟机**

```bash
ssh root@192.168.1.18
```

**第二步：拷贝本机文件到虚拟机中**

```bash
scp redis-5.0.0.tar.gz root@192.168.1.18:/opt
```

**第三步：解压redis**

```bash
tar -zxvf redis-5.0.0.tar.gz
```

**第四步：安装编译redis**

```bash
[root@localhost redis-5.0.0]# make 
[root@localhost redis-5.0.0]# make install
```

**第五步：根目录创建我们自己的redis配置文件夹，存放redis的配置文件备份**

```bash
[root@localhost ~]#  cd /
[root@localhost /]# mkdir myredis
[root@localhost /]# cd opt/
[root@localhost opt]# cd redis-5.0.0
[root@localhost redis-5.0.0]# cp redis.conf /myredis/
```

**第六步：使用vim修改我们的redis配置文件**

```bash
vim redis.conf
daemonize yes
```

**如果提示vim命令不存在，则使用以下命令安装**

```bash
yum -y install vim*
```

**第七步：查看redis服务是否启动**

```bash
[root@localhost myredis]# ps -ef|grep redis
```

**第八步：启动我们的redis服务**

```bash
[root@localhost redis-5.0.0]# cd /usr/local/bin/
[root@localhost bin]# redis-server /myredis/redis.conf
```

**第九步：进入redis**

```bash
[root@localhost bin]# redis-cli -p 6379
```

**第十步：查看redis服务是否被启动**

```bash
127.0.0.1:6379> ping
PONG
```

**第十一步：退出redis**

```bash
127.0.0.1:6379> SHUTDOWN
not connected> exit
```

## 2.3.Redis的其他知识

在redis中，默认16个数据库，类似数组下表从零开始，初始默认使用零号库 ,使用`select命令`切换数据库,使用`dbsize命令`查看当前数据库的key的数量  ，使用`flushdb：清空当前库 `，使用`Flushall；通杀全部库 `，`Redis索引都是从零开始 `。

# 三、Redis五大数据类型

## 3.1.数据类型简介

Redis五大数据类型分别是：`string`（字符串）、`hash`（哈希 ）、`list`（列表） 、`set`（集合）和`zset`(sorted set：有序集合)  ,可以在[Redis常用命令](http://redisdoc.com/ )找到自己忘记的redis命令；

## 3.2.Redis的key

**使用`keys *`命令查看当前库key的数量**

```bash
127.0.0.1:6379> keys *
```

**使用` exists key`命令判断当前库是否存在某一个key**

```bash
127.0.0.1:6379> EXISTS k1
```

**使用`move key db`命令将当前库的key移入到指定库中**

```bash
127.0.0.1:6379> MOVE k1 2
```

**使用`ttl key`命令查看指定key的存活时间，-1代表永不过期，-2代表已经过期**

```bash
127.0.0.1:6379> ttl k2
```

**使用`expipre key second`命令指定key的存活时间**

```bash
127.0.0.1:6379> EXPIRE k2 10
```

> 说明：当key过期后，就已经从内存中移除了，使用`keys * `不能获取到

**使用`type key`命令判断key的数据类型**

```bash
127.0.0.1:6379> type k3
```

## 3.3.String数据类型

string是redis最基本的类型，你可以理解成与Memcached一模一样的类型，**一个key对应一个value**。string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象 。string类型是Redis最基本的数据类型，一个redis中字符串value最多可以是512M。

**常用的String命令**

```bash
# 设值
127.0.0.1:6379> set k5 v5
# 取值
127.0.0.1:6379> get k5
# 删除对应的key
127.0.0.1:6379> del k3
# 在指定的key后面添加value
127.0.0.1:6379> APPEND k5 hello
# 获取指定key的长度
127.0.0.1:6379> STRLEN k5
```

**value数字的加减操作**

```bash
# 将指定的key进行加1操作，注意key的value是数字才可以
127.0.0.1:6379> INCR k6
# 将指定的key进行加指定的数字操作，注意key的value是数字才可以
127.0.0.1:6379> INCRBY k6 3
# 将指定的key进行减1操作，注意key的value是数字才可以
127.0.0.1:6379> DECR k6
# 将指定的key进行减指定的数字操作，注意key的value是数字才可以
127.0.0.1:6379> DECRBY k6 2
```

**key的截取和替换命令**

```bash
127.0.0.1:6379> set k1 hello12345
# 只获取k1的0到4的value
127.0.0.1:6379> GETRANGE k1 0 4
"hello"
# 将k1中的第0位开始替换为word
127.0.0.1:6379> SETRANGE k1 0 word
127.0.0.1:6379> get k1
"wordo12345"
```

**向redis中添加key和value**

```bash
# 增加指定存活时间的key-value
127.0.0.1:6379> SETEX k5 10 v5
# 当key不存在时候才增加对应的key-value,0代表存在，增加不成功，1代表不存在，增加成功
127.0.0.1:6379> SETNX k1 v11
```

**合并设置和取值**

```bash
# mset:同时设置一个或多个 key-value 对
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3
# mget:获取所有(一个或多个)给定 key 的值
127.0.0.1:6379> mget k1 k2 k3
# msetnx:同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在
127.0.0.1:6379> MSETNX k33 v33 k44 v44
```

## 3.4.List数据类型

Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边），**底层实际是个链表**

**List数据的添加命令**

```bash
# 将一个或多个值插入到列表的表头
127.0.0.1:6379> LPUSH list0 1 2 3 4 5
# 返回列表中指定区间的元素
127.0.0.1:6379> LRANGE list0 0 -1
1) "5"
2) "4"
3) "3"
4) "2"
5) "1"
# # 将一个或多个值插入到列表的表尾
127.0.0.1:6379> RPUSH list1 1 2 3 4 5
127.0.0.1:6379> LRANGE list1 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
# 移除并返回列表的头元素
127.0.0.1:6379> lpop list0
# 移除并返回列表的尾元素
127.0.0.1:6379> RPOP list1
```

**常用的操作命令**

```bash
# 返回列表中下标为index的元素
127.0.0.1:6379> LINDEX list0 1
# 返回列表的长度
127.0.0.1:6379> LLEN list1
# 根据参数 count 的值，移除列表中与参数 value 相等的元素
127.0.0.1:6379> LREM list1 3  3
# 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。
127.0.0.1:6379> LTRIM list1 0 0
# 将列表 key 下标为 index 的元素的值设置为 value 
127.0.0.1:6379> LSET list1 0 9
# LINSERT key BEFORE|AFTER pivot value,将值 value 插入到列表 key 当中，位于值 pivot 之前或之后
127.0.0.1:6379> LINSERT list1 after 9 34
```

## 3.5.Set集合数据类型

**常用命令**

```bash
# 将一个或多个 member 元素加入到集合 key 当中，已经存在于集合的 member 元素将被忽略。
127.0.0.1:6379> SADD set0 1 1 2 2 3 3
# 返回集合 key 中的所有成员。
127.0.0.1:6379> SMEMBERS set0
# 判断 member 元素是否集合 key 的成员。1代表存在
127.0.0.1:6379> SISMEMBER set0 2
# 返回集合 key 的基数(集合中元素的数量)。
127.0.0.1:6379> SCARD set0
# 移除集合 key 中的一个或多个 member 元素，不存在的 member 元素会被忽略
127.0.0.1:6379> SREM set0 1
# 如果命令执行时，只提供了 key 参数，那么返回集合中的一个随机元素
127.0.0.1:6379> SRANDMEMBER set0 1
# 移除并返回集合中的一个随机元素。
127.0.0.1:6379> SPOP set0
# 将 member 元素从 source 集合移动到 destination 集合。
127.0.0.1:6379> SMOVE set0 set1 2
# 返回一个集合的全部成员，该集合是所有给定集合之间的差集。
127.0.0.1:6379> SDIFF set0 set1
# 返回一个集合的全部成员，该集合是所有给定集合的交集
127.0.0.1:6379> SINTER set0 set1
# 返回一个集合的全部成员，该集合是所有给定集合的并集
127.0.0.1:6379> SUNION set0 set1
```

## 3.6.Hash数据类型

**常用命令**

```bash
# 将哈希表 user 中的域 id 的值设为 12 。
127.0.0.1:6379> hset user id 12
# 返回哈希表 user 中给定域 id 的值。
127.0.0.1:6379> hget user id
# 同时将多个 field-value (域-值)对设置到哈希表 dept 中
127.0.0.1:6379> HMSET dept id 12 name deptname age 14
# 返回哈希表 key 中，一个或多个给定域的值。
127.0.0.1:6379> HMGET dept id name age
# 返回哈希表 key 中，所有的域和值
127.0.0.1:6379> HGETALL dept
# 删除哈希表 key 中的一个或多个指定域，不存在的域将被忽略。
127.0.0.1:6379> HDEL user id
# 返回哈希表 key 中域的数量
127.0.0.1:6379> HLEN dept
# 查看哈希表 key 中，给定域 field 是否存在。
127.0.0.1:6379> HEXISTS dept id
# 返回哈希表 key 中的所有域。
127.0.0.1:6379> HKEYS dept
# 返回哈希表 key 中所有域的值。
127.0.0.1:6379> HVALS dept
# 为哈希表 dept 中的域 age 的值加上增量 5 。增量可以为负数
127.0.0.1:6379> HINCRBY dept age 5
# 为哈希表 key 中的域 field 加上浮点数增量 increment 。
127.0.0.1:6379> HINCRBYFLOAT dept age 12.3
# 将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在。
127.0.0.1:6379> HSETNX dept sex 1
```

## 3.7.Zset数据类型

在set基础上，加一个score值，之前set是k1 v1 v2 v3，现在zset是k1 score1 v1 score2 v2。

**常用命令**

```bash
# 将一个或多个 member 元素及其 score 值加入到有序集 key 当中。
127.0.0.1:6379> ZADD zset0 99 v1 89 v2 54 v3 56 v4 65 v5
# 返回有序集 key 中，指定区间内的成员,同时返回SCORES
127.0.0.1:6379> ZRANGE zset0 0 -1 WITHSCORES
# 返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员
127.0.0.1:6379> ZRANGEBYSCORE zset0 60 90
# 大于60小于等于90
127.0.0.1:6379> ZRANGEBYSCORE zset0 60 (90
# 大于60 小于90
127.0.0.1:6379> ZRANGEBYSCORE zset0 (60 (90
# 大于等于60小于等于90 的前两条
127.0.0.1:6379> ZRANGEBYSCORE zset0  60 90 limit 2 2
# 移除有序集 key 中的一个或多个成员，不存在的成员将被忽略
127.0.0.1:6379> ZREM zset0 v3
# 返回有序集 key 的基数
127.0.0.1:6379> ZCARD zset0
# 返回有序集 key 中， score 值在 min 和 max 之间(默认包括 score 值等于 min 或 max )的成员的数量。
127.0.0.1:6379> ZCOUNT zset0 60 90
# 返回有序集 key 中成员 member 的排名。其中有序集成员按 score 值递减(从大到小)排序
127.0.0.1:6379> ZREVRANK zset0 v5
```

# 四、Redis配置文件

首先，从前面我们就可以看出，redis的配置文件`redis.conf`位于redis安装包里,但是我们一般都不会修改redis本身的配置文件，而是复制一份修改。

## 4.1.units单位

打开`redis.conf`文件，我们就可以看见redis的units单位，如下若是

```bash
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
```

## 4.2.GENERAL通用配置

* `daemonize yes`配置，默认情况下，ReIIS不作为守护进程运行。
* `loglevel notice`：日志级别，redis有四种日志级别，分别是`debug`、`verbose`、`notice`、`warning`
* `port 6379`：端口配置。
* `tcp-backlog `：设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和=未完成三次握手队列 + 已经完成三次握手队列。在高并发环境下你需要一个高backlog值来避免慢客户端连接问题。注意Linux内核会将这个值减小到/proc/sys/net/core/somaxconn的值，所以需要确认增大somaxconn和tcp_max_syn_backlog两个值
  来达到想要的效果。
* `timeout 0`：在客户端空闲N秒后关闭连接（0禁用）。
* `tcp-keepalive `：单位为秒，如果设置为0，则不会进行Keepalive检测，建议设置成60 。
* `syslog-enabled `：是否把日志输出到syslog中
* `syslog-ident `：指定syslog里的日志标志

## 五、Redis持久化
