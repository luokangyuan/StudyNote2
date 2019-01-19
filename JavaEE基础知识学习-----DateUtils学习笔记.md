# 一、DateUtils常用方法

## 1.1.常用的日期判断

* `isSameDay(final Date date1, final Date date2)`：判断两个时间是否是**同一天**；
* `isSameInstant(final Date date1, final Date date2)`：判断两个时间是否为**同一毫秒**

### 常用的时间判断示例代码

```java
DateUtils.isSameDay(new Date(),new Date());
DateUtils.isSameInstant(new Date(), new Date());
```

## 1.2.日期的基本操作

### String转Date

` parseDate(final String str, final Locale locale, final String... parsePatterns) throws ParseException`

```java
 DateUtils.parseDate("20181223 12:34:23",  Locale.TRADITIONAL_CHINESE,"yyyy-MM-dd HH:mm:ss");
```

### Date转String

可以使用`DateFormatUtils.format()`方法

```java
DateFormatUtils.format(new Date(),"yyyy-MM-dd HH:mm:ss");
```

### 日期加减

`addYears(final Date date, final int amount)`,对日期进行加减年分操作，`amount`为正数表示加，负数表示减。同理，还有`addMonths`加减月份，`addDays`加减日，`addWeeks`加减周，`addHours`加减小时，`addMinutes`加减分钟等方法，具体使用可以参看源码。

```java
Date date = DateUtils.addYears(new Date(), 3);
```

### 日期设置

`setYears(final Date date, final int amount)`，给指定的日期设置指定的年份，同理，`setMonths`设置月份，`setDays`设置日，`setHours`设置小时等等方法。

```java
Date date1 = DateUtils.setYears(new Date(), 2018);
```

### 日期四舍五入

`round(final Date date, final int field)`,将指定的日期中指定的部分四舍五入，四舍五入的 常量有`Calendar.YEAR`和`Calendar.MONTH`、`Calendar.HOUR_OF_DAY`、`Calendar.DAY_OF_MONTH`、`Calendar.HOUR`、`Calendar.MINUTE`等；

```java
/*当前时间：2018-11-25 00:50:57,结果为：2019-01-01 00:00:00*/ 
Date round = DateUtils.round(new Date(), Calendar.YEAR);
```

### 日期截取

`truncate(final Date date, final int field)`和`round`差距在于不会四舍五入，截取的常量字段有`Calendar.YEAR`和`Calendar.MONTH`、`Calendar.HOUR_OF_DAY`、`Calendar.DAY_OF_MONTH`、`Calendar.HOUR`、`Calendar.MINUTE`等。

```java
/*当前时间：2018-11-25 00:58:03 ,结果为：2018-01-01 00:00:00*/
Date truncate = DateUtils.truncate(new Date(), Calendar.YEAR);
```

### 获取指定时间的天数

`getFragmentInDays(final Date date, final int fragment)`获取指定时间的天数，`fragment`可以是`Calendar.YEAR`获取年已经过了多少天，同理，`Calendar.MONTH`月份过去了多少天，当然还有`getFragmentInSeconds`过去多少秒，`getFragmentInMinutes`过去多少分钟，`getFragmentInHours`过去多少小时等。

```java
/*当前时间：2018-11-25 00:58:03 ,结果为：329*/
long fragmentInDays = DateUtils.getFragmentInDays(new Date(), Calendar.YEAR);
```

### 比较日历字段是否相等

`truncatedEquals(final Date date1, final Date date2, final int field)`可以比较年，月，日等日历字段。

```java
boolean b = DateUtils.truncatedEquals(new Date(), new Date(), Calendar.YEAR);
```

