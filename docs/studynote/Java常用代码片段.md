# 一、Java中常用判空

## 1.1.字符串判空

```java
StringUtils.isNotBlank(str)
```

## 1.2.数组判空

```java
ArrayUtils.isNotEmpty(names);
```

## 1.3.集合去重

~~~java

~~~

# 二、日期类型常用代码

## 2.1.Date转String

```java
FastDateFormat.getInstance("yyyy-MM-dd HH:mm:ss").format(new Date());
```

> 说明：也可以使用DateFormatUtils类：`DateFormatUtils.format(new Date(), "yyyy-MM-dd HH:mm:ss");`

## 2.2.String转Date

```java
DateUtils.parseDate("2018-12-23 12:23:59", "yyyy-MM-dd HH:mm:ss");
```

## 2.3.DateUtils类简要说明

```java
// 判断两个Date是否是同一天
DateUtils.isSameDay(new Date(), new Date());
// 年份加减
DateUtils.addYears(new Date(), -3);
// 月份加减
DateUtils.addMonths(new Date(), 2);
// 日加减
DateUtils.addDays(new Date(),1);
// 周加减
DateUtils.addWeeks(new Date(),1);
// 小时加减
DateUtils.addHours(new Date(),5);
// 分钟加减
DateUtils.addMinutes(new Date(),5);
// 秒加减
DateUtils.addSeconds(new Date(),5);
// 毫秒加减
DateUtils.addMilliseconds(new Date(),5);
// 给年份设值
DateUtils.setYears(new Date(), 2019);
// 给月份设值：说明：给月份设置的时候参数为0表示设置为1月；
DateUtils.setMonths(new Date(),0);
// 给日设置
DateUtils.setDays(new Date(),1);
// 给小时设置
DateUtils.setHours(new Date(),13);
// 给分钟设置
DateUtils.setMinutes(new Date(),32);
// 给秒和毫秒设置
DateUtils.setSeconds(new Date(),56);
DateUtils.setMilliseconds(new Date(),998);
// 对指定的的日期进行四舍五入：2019-01-01 00:00:00.000
DateUtils.round(new Date(),Calendar.YEAR);
// 2018-09-01 00:00:00.000
DateUtils.round(new Date(),Calendar.MARCH);
// 2018-09-15 01:00:00.000
DateUtils.round(new Date(),Calendar.HOUR_OF_DAY);
// 2018-09-15 00:00:00.000 
DateUtils.round(new Date(),Calendar.DAY_OF_MONTH);
// 2018-09-15 00:00:00.000
DateUtils.round(new Date(),Calendar.DATE);
// 2018-09-15 01:00:00.000
DateUtils.round(new Date(),Calendar.HOUR);
// 2018-09-15 00:36:00.000
DateUtils.round(new Date(),Calendar.MINUTE);
// 2018-09-15 00:36:30.000
DateUtils.round(new Date(),Calendar.SECOND);
// 从指定的日期开始截取格式化日期:2018-01-01 00:00:00.000
DateUtils.truncate(new Date(),Calendar.YEAR);
// 2018-09-01 00:00:00.000
DateUtils.truncate(new Date(),Calendar.MONTH);
// 2018-09-15 00:00:00.000
DateUtils.truncate(new Date(),Calendar.DATE);
// 2018-09-15 00:00:00.000 
DateUtils.truncate(new Date(),Calendar.HOUR);
// 2018-09-15 00:52:00.000
DateUtils.truncate(new Date(),Calendar.MINUTE);
//2018-09-15 00:52:11.000
DateUtils.truncate(new Date(),Calendar.SECOND);
// 将日期向上取整，当前时间：2018-09-16 21:26:29.917
// 2019-01-01 00:00:00.000
DateUtils.ceiling(new Date(),Calendar.YEAR);
// 2018-10-01 00:00:00.000
DateUtils.ceiling(new Date(),Calendar.MONTH);
// 2018-09-16 22:00:00.000
DateUtils.ceiling(new Date(),Calendar.HOUR_OF_DAY);
// 2018-09-17 00:00:00.000
DateUtils.ceiling(new Date(),Calendar.DAY_OF_MONTH);
// 2018-09-16 22:00:00.000
DateUtils.ceiling(new Date(),Calendar.HOUR);
// 2018-09-16 21:27:00.000
DateUtils.ceiling(new Date(),Calendar.MINUTE);
```

## 2.4.ArraysUtil类

sha