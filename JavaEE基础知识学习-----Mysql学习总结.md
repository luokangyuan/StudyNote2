# 数据库简介

## 数据库优点

* 持久化数据到本地
* 可以实现结构化查询，方便管理

## 数据库相关概念

* DB：数据库，保存一组有组织的数据的容器
* DBMS：数据库管理系统，又称为数据库软件（产品），用于管理DB中的数据
* SQL:结构化查询语言，用于和DBMS通信的语言

## 数据库存储特点

- 将数据放到表中，表再放到库中
- 一个数据库中可以有多个表，每个表都有一个的名字，用来标识自己。表名具有唯一性。
- 表具有一些特性，这些特性定义了数据在表中如何存储，类似java中 “类”的设计。
- 表由列组成，我们也称为字段。所有表都是由一个或多个列组成的，每一列类似java 中的”属性”
- 表中的数据是按行存储的，每一行类似于java中的“对象”。

## Mysql常用命令

**查看当前所有的数据库**

```sql
show databases;
```

**打开指定的库**

```sql
use 库名
```

**查看当前库的所有表**

```sql
show tables;
```

**查看其它库的所有表**

```sql
show tables from 库名;
```

**创建表**

```sql
create table 表名(
    列名 列类型,
	列名 列类型
    );
```

**查看表结构**

```sql
desc 表名;
```

## Mysql语法规范

* 不区分大小写,但建议关键字大写，表名、列名小写
* 每条命令最好用分号结尾
* 每条命令根据需要，可以进行缩进 或换行
* 注释
  * 单行注释：#注释文字
  * 单行注释：-- 注释文字
  * 多行注释：/* 注释文字  */

## SQL语言分类

* DQL（Data Query Language）：数据查询语言，例如：select 
* DML(Data Manipulate Language):数据操作语言，例如：insert 、update、delete
* DDL（Data Define Languge）：数据定义语言，例如：create、drop、alter
* TCL（Transaction Control Language）：事务控制语言，例如：commit、rollback

# DQL语言学习

## 基础查询

**语法**

```sql
SELECT 要查询的东西
【FROM 表名】;
```

**特点**

* 通过select查询完的结果 ，是一个虚拟的表格，不是真实存在
* 要查询的东西 可以是常量值、可以是表达式、可以是字段、可以是函数

## 条件查询

条件查询：根据条件过滤原始表的数据，查询到想要的数据

**语法**

```sql
select 
	要查询的字段|表达式|常量值|函数
from 
	表
where 
	条件 ;
```

**分类**

* 条件表达式，例如：salary>10000，条件运算符：> < >= <= = != <>
* 逻辑表达式，例如：salary>10000 && salary<20000，逻辑运算符：
  * and（&&）:两个条件如果同时成立，结果为true，否则为false
  * or(||)：两个条件只要有一个成立，结果为true，否则为false
  * not(!)：如果条件成立，则not后为false，否则为true
* 模糊查询，例如：last_name like 'a%'

## 排序查询

**语法**

```sql
select
	要查询的东西
from
	表
where 
	条件
order by 排序的字段|表达式|函数|别名 【asc|desc】
```

## 常见函数

**字符函数**

* concat拼接
* substr截取子串
* upper转换成大写
* lower转换成小写
* trim去前后指定的空格和字符
* ltrim去左边空格
* rtrim去右边空格
* replace替换
* lpad左填充
* rpad右填充
* instr返回子串第一次出现的索引
* length 获取字节个数

**数学函数**

* round 四舍五入
* rand 随机数
* floor向下取整
* ceil向上取整
* mod取余
* truncate截断

**日期函数**

* now当前系统日期+时间
* curdate当前系统日期
* curtime当前系统时间
* str_to_date 将字符转换成日期
* date_format将日期转换成字符

**流程控制函数**

* if 处理双分支
* case语句 处理多分支
  		情况1：处理等值判断
  		情况2：处理条件判断

## 分组函数

* sum 求和
* max 最大值
* min 最小值
* avg 平均值
* count 计数

**分组函数说明**

* 以上五个分组函数都忽略null值，除了count(*)
* sum和avg一般用于处理数值型
* max、min、count可以处理任何数据类型
* 都可以搭配distinct使用，用于统计去重后的结果
* count的参数可以支持：字段、*、常量值，一般放1

## 分组查询

**语法**

```sql
select 查询的字段，分组函数
from 表
group by 分组的字段
```

**分组查询特点**

* 可以按单个字段分组
* 和分组函数一同查询的字段最好是分组后的字段
* 分组筛选
  * 分组前筛选：	原始表		group by的前面		where
  	 分组后筛选：	分组后的结果集	group by的后面		having
* 可以按多个字段分组，字段之间用逗号隔开
* 可以支持排序
* having后可以支持别名

## 多表查询

**语法**

```sql
select 字段，...
from 表1
【inner|left outer|right outer|cross】join 表2 on  连接条件
【inner|left outer|right outer|cross】join 表3 on  连接条件
【where 筛选条件】
【group by 分组字段】
【having 分组后的筛选条件】
【order by 排序的字段或表达式】
```

## 子查询

**子查询含义**

一条查询语句中又嵌套了另一条完整的select语句，其中被嵌套的select语句，称为子查询或内查询
在外面的查询语句，称为主查询或外查询

**子查询特点**

* 子查询都放在小括号内
* 子查询可以放在from后面、select后面、where后面、having后面，但一般放在条件的右侧
* 子查询优先于主查询执行，主查询使用了子查询的执行结果
* 子查询根据查询结果的行数不同分为以下两类
  * 单行子查询：结果集只有一行，一般搭配单行操作符使用：> < = <> >= <= ，非法使用子查询的情况：子查询的结果为一组值或者子查询的结果为空
  * 多行子查询：结果集有多行，一般搭配多行操作符使用：any、all、in、not in，in： 属于子查询结果中的任意一个就行，any和all往往可以用其他查询代替

## 分页查询

**语法**

```sql
select 字段|表达式,...
from 表
【where 条件】
【group by 分组字段】
【having 条件】
【order by 排序的字段】
limit 【起始的条目索引，】条目数;
```

**特点**

* 起始条目索引从0开始
* limit子句放在查询语句的最后
* 公式：select * from  表 limit （page-1）*sizePerPage,sizePerPage

## 联合查询

**语法**

```sql
select 字段|常量|表达式|函数 【from 表】 【where 条件】 union 【all】
select 字段|常量|表达式|函数 【from 表】 【where 条件】 union 【all】
select 字段|常量|表达式|函数 【from 表】 【where 条件】 union  【all】
.....
select 字段|常量|表达式|函数 【from 表】 【where 条件】
```

**特点**

* 多条查询语句的查询的列数必须是一致的
* 多条查询语句的查询的列的类型几乎相同
* union代表去重，union all代表不去重

# DML语言学习

## 插入

**语法**

```sql
insert into 表名(字段名，...)
values(值1，...);
```

**特点**

- 字段类型和值类型一致或兼容，而且一一对应
- 可以为空的字段，可以不用插入值，或用null填充
- 不可以为空的字段，必须插入值
- 字段个数和值的个数必须一致
- 字段可以省略，但默认所有字段，并且顺序和表中的存储顺序一致

## 修改

**修改单表语法**

```sql
update 表名 set 字段=新值,字段=新值
【where 条件】
```

**修改多表语法**

```sql
update 表1 别名1,表2 别名2
set 字段=新值，字段=新值
where 连接条件
and 筛选条件
```

## 删除

**单表删除**

```sql
delete from 表名 【where 筛选条件】
```

**多表删除**

```sql
delete 别名1，别名2
	from 表1 别名1，表2 别名2
	where 连接条件
	and 筛选条件;
```

**使用truncate语句删除**

```sql
truncate table 表名
```

**delelte和truncate区别**

* truncate不能加where条件，而delete可以加where条件
* truncate的效率高一丢丢
* truncate 删除带自增长的列的表后，如果再插入数据，数据从1开始
* delete 删除带自增长列的表后，如果再插入数据，数据从上一次的断点处开始
* truncate删除不能回滚，delete删除可以回滚

# DDL语言学习

## 库和表管理

**库的管理**

```sql
--创建库
create database 库名
--删除库
drop database 库名
```

**表的管理**

```sql
--创建表
CREATE TABLE IF NOT EXISTS stuinfo(
	stuId INT,
	stuName VARCHAR(20),
	gender CHAR,
	bornDate DATETIME);
DESC studentinfo;
--修改表 alter
ALTER TABLE 表名 ADD|MODIFY|DROP|CHANGE COLUMN 字段名 【字段类型】;
--修改字段名
ALTER TABLE studentinfo CHANGE  COLUMN sex gender CHAR;
--修改表名
ALTER TABLE stuinfo RENAME [TO]  studentinfo;
--修改字段类型和列级约束
ALTER TABLE studentinfo MODIFY COLUMN borndate DATE ;
--添加字段
ALTER TABLE studentinfo ADD COLUMN email VARCHAR(20) first;
--删除字段
ALTER TABLE studentinfo DROP COLUMN email;
--删除表
DROP TABLE [IF EXISTS] studentinfo;
```

# 数据库事务

## 概念

通过一组逻辑操作单元（一组DML——sql语句），将数据从一种状态切换到另外一种状态

## 特点

- A：原子性：要么都执行，要么都回滚
- C：一致性：保证数据的状态操作前和操作后保持一致
- I：隔离性：多个事务同时操作相同数据库的同一个数据时，一个事务的执行不受另外一个事务的干扰
- D：持久性：一个事务一旦提交，则数据将持久化到本地，除非其他事务对其进行修改

## 使用事务步骤

* 开启事务
* 编写事务的一组逻辑操作单元（多条sql语句）
* 提交事务或回滚事务

## 事务分类

**隐式事务，没有明显的开启和结束事务的标志**

例如：insert、update、delete语句本身就是一个事务

**显式事务，具有明显的开启和结束事务的标志**

```properties
1、开启事务
	取消自动提交事务的功能	
2、编写事务的一组逻辑操作单元（多条sql语句）
	insert
	update
	delete	
3、提交事务或回滚事务
```

## 关键字

```properties
set autocommit=0;
start transaction;
commit;
rollback;
savepoint  断点
commit to 断点
rollback to 断点
```

## 事务的隔离级别

当多个事务同时操作同一个数据库的相同数据时就会产生事务并发问题，常见的事务并发问题有如下几种

- **脏读**：一个事务读取到了另外一个事务未提交的数据
- **不可重复读**：同一个事务中，多次读取到的数据不一致

**如何避免事务的并发问题**

通过设置事务的隔离级别

- READ UNCOMMITTED
- READ COMMITTED 可以避免脏读
- REPEATABLE READ 可以避免脏读、不可重复读和一部分幻读
- SERIALIZABLE可以避免脏读、不可重复读和幻读

**如何设置事务的隔离级别**

```sql
set session|global  transaction isolation level 隔离级别名;
```

**如何查看事务的隔离级别**

```sql
select @@tx_isolation;
```

# 视图

视图可以理解为一张虚拟的表，视图和表的区别如下

|      | 使用方式 | 占用物理空间              |
| ---- | -------- | ------------------------- |
| 视图 | 完全相同 | 不占用，仅保存的是SQL逻辑 |
| 表   | 完全相同 | 占用                      |

**使用视图的好处**

* sql语句提高重用性，效率高
* 和表实现了分离，提高了安全性

## 视图的创建

```sql
CREATE VIEW  视图名
AS
查询语句;
```

## 视图操作

```sql
--查看视图的数据 
SELECT * FROM my_v4;
SELECT * FROM my_v1 WHERE last_name='Partners';
--插入视图的数据
INSERT INTO my_v4(last_name,department_id) VALUES('虚竹',90);
--修改视图的数据
UPDATE my_v4 SET last_name ='梦姑' WHERE last_name='虚竹';
--删除视图的数据
DELETE FROM my_v4;
```

## 不能更新的视图

* 包含以下关键字的sql语句：分组函数、distinct、group  by、having、union或者union all
* 常量视图
* Select中包含子查询
* join
* from一个不能更新的视图
* where子句的子查询引用了from子句中的表

## 视图删除

```sql
DROP VIEW test_v1,test_v2,test_v3;
```

## 查看视图结构

```sql
DESC test_v7;
SHOW CREATE VIEW test_v7;
```

# 存储过程

存储过程：一组经过预先编译的sql语句的集合

**使用存储过程的好处**

- 提高了sql语句的重用性，减少了开发程序员的压力
- 提高了效率
- 减少了传输次数

## 存储过程分类

- 无返回无参
- 仅仅带in类型，无返回有参
- 仅仅带out类型，有返回无参
- 既带in又带out，有返回有参
- 带inout，有返回有参

注意：in、out、inout都可以在一个存储过程中带多个

## 创建存储过程

```sql
create procedure 存储过程名(in|out|inout 参数名  参数类型,...)
begin
	存储过程体
end
```

**注意**

```sql
CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名  参数类型,...)
BEGIN
	sql语句1;
	sql语句2;

END $
-- 存储过程体中可以有多条sql语句，如果仅仅一条sql语句，则可以省略begin end
-- 参数前面的符号的意思
  -- in:该参数只能作为输入 （该参数不能做返回值）
  -- out：该参数只能作为输出（该参数只能做返回值）
  -- inout：既能做输入又能做输出
```

## 调用存储过程

```sql
call 存储过程名(实参列表)
```