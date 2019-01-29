# 一、ArrayUtils方法解读

## 1.1.数组的常用判断方法

* `isEmpty(final Object[] array)`：判断数组是否为空；
* `isNotEmpty(final float[] array)`：判断数组是否不为空；
* `isSameLength(final char[] array1, final char[] array2)`：判断两个数组的长度是否相同，要同类型；
* `isSameType(final Object array1, final Object array2)`：判断两个数组的类型是否相同；
* `isSorted(final int[] array)`：判断数组是否以自然顺序排序；

### 数组的常用判断示例代码

```java
String[] arr = {"Hello","Word"};
String[] arr2 = {"34","35","36"};
int[] intarr = {12,13,14};
ArrayUtils.isEmpty(arr);             = false
ArrayUtils.isNotEmpty(arr);          = true
ArrayUtils.isSameLength(arr2,arr);   = false
ArrayUtils.isSameType(intarr,arr);   = false
ArrayUtils.isSorted(intarr);         = true
```

## 1.2.数组的基本操作

* `add(final T[] array, final T element)`：该方法向指定的数组中添加一个元素；
* `remove(final T[] array, final int index)`：移除数组红指定索引位置的元素；
* `addAll(final T[] array1, final T... array2)`：合并两个数组；
* `removeAll(final char[] array, final int... indices)`：移除数组红指定的多个索引位置的元素；
* ` removeElement(final char[] array, final char element)`：从数组中删除第一次出现的指定元素；
* `removeAllOccurences(final char[] array, final char element)`：从数组中移除指定的元素；
* `removeElements(final char[] array, final char... values)`：从数组中移除指定数量的元素，返回新数组；
* `getLength(final Object array)`：获取数组的长度；
* `contains(final Object[] array, final Object objectToFind)`：判断数组中是否包含某一个元素；
* `indexOf(final Object[] array, final Object objectToFind)`：查找数组中是否存在某元素，返回索引位置；
* `lastIndexOf(final Object[] array, final Object objectToFind)`：从尾部开始查找指定元素；
* `insert(final int index, final T[] array, final T... values)`：向数组指定索引位置添加元素；

### 数组的基本操作示例代码

```java
ArrayUtils.add(arr, "你好");
ArrayUtils.remove(arr,1);
ArrayUtils.removeAll([2, 6, 3], 0, 2)    = [6]
ArrayUtils.removeAll([2, 6, 3], 0, 1, 2) = []
ArrayUtils.removeElement(['a', 'b', 'a'], 'a') = ['b', 'a']
ArrayUtils.removeAllOccurences(arr,"Hello");
ArrayUtils.removeElements(arr,1,2,3);
ArrayUtils.addAll([], [])         = []
ArrayUtils.addAll([null], [null]) = [null, null]
ArrayUtils.addAll(arr,arr2);
ArrayUtils.getLength(arr);
ArrayUtils.contains(arr,"word");
ArrayUtils.indexOf(arr,"word");
ArrayUtils.lastIndexOf(arr, "word");
ArrayUtils.insert(0, arr, "谢谢你");
```

> 说明：数组的很多操作都是返回新的数组，不会对原有数组进行改变；

## 1.3.数组的转换操作

* `nullToEmpty(final String[] array)`：将null数组转换为对应类型的空数组；
* `toMap(final Object[] array)`：将二维数组转换为map，一维数组转换抛出异常；
* `reverse(final char[] array)`：反转数组，不返回新数组,可以指定反转的区域；
* `subarray(final char[] array, int startIndexInclusive, int endIndexExclusive)`：数组的截取；包头不包尾；
* `swap(final char[] array, final int offset1, final int offset2)`：交换数组中指定位置的两个元素；
* `toObject(final int[] array)`：将**原始数据类型**的数组转换为**对象类型**的数组；
* `toPrimitive(final Integer[] array)`：将**对象数据类型**的数组转换为**原始数据类型**的数组
* `toStringArray(final Object[] array)`：将`Object`类型的数组转换为`String`类型的数组；

### 数组的转换操作示例代码

```java
ArrayUtils.nullToEmpty((String[])null);
String[][] maparr = {{"name","zhangsan"},{"age","23"},{"money","6700"}};
Map<Object, Object> objectObjectMap = ArrayUtils.toMap(maparr);
String[] rearr = {"1", "2", "3", "4"}; 
ArrayUtils.reverse(rearr, 0, 3); = [3, 2, 1, 4]
String[] subarray = ArrayUtils.subarray(rearr, 1, 2);
ArrayUtils.swap([1, 2, 3], 1, 0) = [2, 1, 3]
ArrayUtils.swap([1, 2, 3], 0, 5) = [1, 2, 3]
ArrayUtils.swap([1, 2, 3], -1, 1) = [2, 1, 3]
int[] intarr = {12,11,14};
Integer[] integers = ArrayUtils.toObject(intarr);
int[] ints = ArrayUtils.toPrimitive(integers);
String[] strings = ArrayUtils.toStringArray(integers);
```

### 简单总结

上面所提到的方法都是平时开发时候经常使用的，可以大大的简化我们对数组的基本操作，需要注意的是方法是否返回新数组，是否是对原有数组进行操作。


