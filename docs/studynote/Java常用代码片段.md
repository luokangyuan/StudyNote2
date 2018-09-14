# 一、Java中常用判空

## 1.1.字符串判空

```java
if(StringUtils.isNotBlank(str)){
    logger.info("字符串不为空");
}
```

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

### 2.3.1.判断两个Date是否是同一天

```java
DateUtils.isSameDay(new Date(), new Date());
```

### 2.3.2.年份加减

```java
DateUtils.addYears(new Date(), -3);
```

### 2.3.3.月份加减

```java
DateUtils.addMonths(new Date(), 2);
```

### 2.3.4.日加减

```java
DateUtils.addDays(new Date(),1);
```

### 2.3.5.周加减

```java
DateUtils.addWeeks(new Date(),1);
```

### 2.3.6.小时加减

```java
DateUtils.addHours(new Date(),5);
```

### 2.3.7.分钟加减

```java
DateUtils.addMinutes(new Date(),5);
```

### 2.3.8.秒加减

```java
DateUtils.addSeconds(new Date(),5);
```

### 2.3.9.毫秒加减

```java
DateUtils.addMilliseconds(new Date(),5);
```

### 2.3.10.给年份设置

```java
 DateUtils.setYears(new Date(), 2019);
```

### 2.3.11.给月份设置

```java
DateUtils.setMonths(new Date(),0);
```

> 说明：给月份设置的时候参数为0表示设置为1月；

### 2.3.12.给日设置

```java
DateUtils.setDays(new Date(),1);
```







