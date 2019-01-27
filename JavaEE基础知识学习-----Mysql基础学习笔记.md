# 一、Mysql基础学习笔记

## 1.1.基本的sql语句

### select语句

```sql
SELECT DISTINCT USER
	.id AS userId,
	USER.user_name AS NAME,
	USER.age AS age,
	USER.gender AS gender 
FROM
	tbl_user USER 
WHERE
	USER.id < 11 
ORDER BY
	USER.id DESC
```

使用`select`关键字查询，`DISTINCT`关键字去重，`WHERE`关键字拼接条件，`ORDER BY`关键字定排序字段和方式；

### Update语句

```sql

```



### 常用的命令

```sql
mysql -h localhost -P 3306 -u root -p  # 连接数据库，连接本地可以 mysql -u root -p

show databases; # 查看数据库

use test; # 切换数据库

show tables; # 查看当前数据库的表

show tables from test; # 在当前数据库下查看其他库存在的数据表

select database(); # 查看当前库是那个

select version(); # 查看数据库版本

desc test; # 查看表结构
```

## 1.2.sql中查询

### mysql中的加号

在`mysql`中，加号仅仅是一种运算符，不能作为字符串的连接方式，例如，

```sql
select "hello" + "word"; # 结果：0

select "Hello" + 100; # 结果：100

select "1" + 99; # 结果：100

select null + 12; # 结果： null ,只有加号两边存在一个null，结果就为null;
```

### 字符串的拼接

使用`concat`函数，这是一个可以传入多个参数的可变参数函数，使用方法如下：

```sql
SELECT
	concat ( "hello", "：","word" ); # 结果：hello：word
```

### IFNULL函数，当值为空时

```sql
SELECT 
	IFNULL(USER.age,0) AS age
FROM
	tbl_user USER 
```

查询的时候，使用IFNULL函数，可以将查询结果为null的字段赋上我们需要的默认值。

### 条件查询

在开始的的基本sql语句中，我们的查询语句中，就使用了条件查询的关键字`where`	, 在mysql中，条件的查询还存在一下关键字。

* 条件运算符：`>`（大于）、` <（`小于）、 `=`（等于） 、`!=`（不等于）、` <>（`不等于）、` >= `（大于等于）、`<=`（小于等于） 
* `like`（模糊查询）、 `between and` 、`in`、` is null`
* `&&` 、`||`、` ! `、`and`、 `or `、`not`




