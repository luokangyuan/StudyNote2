# 一、Mysql基础学习笔记

## 1.1.基本的sql语句

### select语句

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
```

