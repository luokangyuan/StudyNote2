# 一、GitHub的基本操作

## 1.1.使用in限制搜索

```java
Blog in:name
```

```java
Blog in:name,description
```

```java
Blog in:name,description,readme
```

## 1.2.根据stars和forks数筛选

```java
springboot stars:>=5000
```

```java
springboot forks:>=5000
```

```java
springboot forks:2000..4000 stars:6000..18000
```

## 1.3.使用awesome搜索

```java
awesome redis
```

## 1.4.高亮显示代码

**使用`#L行号`高亮显示某一行**

```java
https://github.com/codingXiaxw/seckill/blob/master/src/main/java/cn/codingxiaxw/dao/SuccessKilledDao.java#L13
```

**使用`#L行号-L行号`高亮显示一段**

```java
https://github.com/codingXiaxw/seckill/blob/master/src/main/java/cn/codingxiaxw/dao/SuccessKilledDao.java#L13-L25
```

## 1.5.项目使用快捷键`t`内搜索

![image-20190429231322587](http://image.luokangyuan.com/2019-04-29-151328.png)

## 1.6.搜索某地址某语言的活跃用户

```java
location:beijing language:java
```

