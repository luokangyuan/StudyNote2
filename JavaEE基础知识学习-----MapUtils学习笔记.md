# 一、MapUtils学习笔记

官方文档传送门：[MapUtils](http://commons.apache.org/proper/commons-collections/apidocs/org/apache/commons/collections4/MapUtils.html)

本篇笔记的基础示例数据代码如下：

```java
HashMap<String, Object> map = new HashMap<>();
map.put("name","zhangsan");
map.put("sex",true);
map.put("age",34);
map.put("money",null);
```

## 1.1.MapUtils常用方法

### 获取Map中指定key的value

使用`getString(final Map map, final Object key)`方法，当然，也可使用`getString( Map map, Object key, String defaultValue )`方法，当我们`get`属性值时候发生了转换异常的就会报错，为了避免这种报错，可以使用默认值的方法解决。

```java
String name = MapUtils.getString(map, "name");
```

当我们获取`Integer`类型的时候，当类型不能转换时候报`java.lang.NullPointerException`如下：

```java
int money = MapUtils.getInteger(map,"money");
```

就可以使用`getInteger( Map map, Object key, Integer defaultValue )`方法解决。

```java
int money = MapUtils.getInteger(map,"money",23);
```

`MapUtils`中其他的`get`属性值的方法还有如下这些，使用方法和`MapUtils.getString`一样，这里就不再一一举例说明，

* `Object getObject(final Map map, final Object key)`：获取Object类型的值。
* `String getString(final Map map, final Object key)`：获取String类型的值。
* `Boolean getBoolean(final Map map, final Object key)`：获取Boolean类型的值。
* `Number getNumber(final Map map, final Object key)`：获取Number类型的值。
* `Byte getByte(final Map map, final Object key)`：获取Byte类型的值。
* `Short getShort(final Map map, final Object key)`：获取Short类型的值。
* `Integer getInteger(final Map map, final Object key)`：获取Integer类型的值。
* `Long getLong(final Map map, final Object key)`：获取Long类型的值。
* `Float getFloat(final Map map, final Object key)`：获取Float类型的值。
* `Double getDouble(final Map map, final Object key)`：获取Double类型的值。
* `Map getMap(final Map map, final Object key)`：获取Map类型的值。

> 说明：每一个获取值的方法都有一个带有获取失败使用默认值的方法。

### Map判空

使用`MapUtils.isEmpty`方法和` MapUtils.isNotEmpty`方法对Map进行空判断。

```java
boolean empty = MapUtils.isEmpty(map);
boolean notEmpty = MapUtils.isNotEmpty(map);
```

### 将二维数组放入Map中

使用`MapUtils.putAll`方法。

```java
String[][] user = {{"names","zhangfsan"},{"sexs","1f"}};
Map map1 = MapUtils.putAll(map, user);
```

MapUtils类的常用方法基本就是这些，后面设计的`MultiMap`、`LazyMap`、`BidiMap`会在后面的博客中仔细说明，我们在平时的开发中应该多使用开源的工具类，简洁高效我们的代码。


