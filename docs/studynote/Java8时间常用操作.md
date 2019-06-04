<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">Java8时间常用操作</h1>

> 前言：时间操作在开发中经常被使用到，最近项目是用Java8开发，因此总结一下时间操作常用方法。

## 1.常用方法总结

### 1.1.获取当前时间

```java
// 当前日期：2019-03-16
LocalDate date = LocalDate.now();
// 当前时间：22:13:03.450
LocalTime time = LocalTime.now();
// 当前日期和时间：2019-03-16T22:13:03.450
LocalDateTime dateTime = LocalDateTime.now();
```

### 1.2.月份的第一天和最后一天

```java
 LocalDate now = LocalDate.now();
// 本月第一天： 2019-03-01
LocalDate with = now.with(TemporalAdjusters.firstDayOfMonth());
// 本月最后一天：2019-03-31
LocalDate with1 = now.with(TemporalAdjusters.lastDayOfMonth());
```

### 1.3.下一月的第一天和下一年的第一天 

```java
LocalDate now = LocalDate.now();
// 下一个月的第一天：2019-04-01
LocalDate with2 = now.with(TemporalAdjusters.firstDayOfNextMonth());
// 下一年的第一天：2020-01-01
LocalDate with3 = now.with(TemporalAdjusters.firstDayOfNextYear());
```

### 1.4.获取当前年第一个周一的日期

```java
LocalDate now = LocalDate.now();
// 获取当前年的第一天
LocalDate with4 = now.with(TemporalAdjusters.firstDayOfYear());
// 获取当前年的第一个周一所在的日期：2019-01-07
LocalDate.parse(with4.toString()).with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY));
```

### 1.5.指定时间的后几天日期

```java
LocalDate now = LocalDate.now();
// 当前时间的后两天： 2019-03-18
LocalDate date1 = now.plusDays(2);
// 当前时间的后两月：2019-05-16
LocalDate date2 = now.plusMonths(2);
```

### 1.6.计算两个日期相距

```java
// 实例化一个时间
LocalDate of = LocalDate.of(2014, 11, 07);
// 相距多少年
long until = now.until(of, ChronoUnit.YEARS);
// 相距多少月
long until2 = now.until(of, ChronoUnit.MONTHS);
// 相距多少天
long until3 = now.until(of, ChronoUnit.DAYS);
```

### 1.7.判断年月的周期性

```java
LocalDate nowDate = LocalDate.of(2019, 03, 17);
LocalDate birthDayTime = LocalDate.of(1995, 03, 16);
MonthDay birthday = MonthDay.of(birthDayTime.getMonth(), birthDayTime.getDayOfMonth());
MonthDay today = MonthDay.from(nowDate);
if(birthday.equals(today)){
    System.out.println("今天是我的生日");
}else{
    System.out.println("今天不是我的生日");
}
```

### 1.8.一周后的日期

```java
LocalDate today = LocalDate.now();  
LocalDate nextWeek = today.plus(1, ChronoUnit.WEEKS); 
```

### 1.9.一年前或一年后的日期

```java
LocalDate today = LocalDate.now();  
LocalDate preYear = today.minus(1, ChronoUnit.YEARS);  
LocalDate nextYear = today.plus(1, ChronoUnit.YEARS); 
```

### 1.10.检查闰年

```java
LocalDate today = LocalDate.now();  
if (today.isLeapYear()) {  
    System.out.println("今年是闰年！");
} else {
    System.out.println("今年不是闰年！");
}
```

### 1.11.信用卡到期

```java
YearMonth currentYearMonth = YearMonth.now();
// 月份有多少天
currentYearMonth.lengthOfMonth();
// 信用卡到期日期
YearMonth creditCardExpiry = YearMonth.of(2018, Month.FEBRUARY);  
```

## 2.附录

### 2.1.date转为LocalTime

```java
// date转为LocalTime
Date from = Date.from(instant);
Instant instant2 = from.toInstant();
instant2.atZone(zone).toLocalTime();
```

### 2.2.将LocalDateTime转为date

```java
// 将LocalDateTime转为date
ZoneId zone = ZoneId.systemDefault();
Instant instant = now.atZone(zone).toInstant();
Date from = Date.from(instant);
```

> 说明：目前项目中就使用了这些，如果你有其他常用的操作，请留言，持续补充
