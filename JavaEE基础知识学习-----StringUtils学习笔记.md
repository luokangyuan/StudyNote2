# 一、StringUtils字符串工具类

## 1.1.判断是否空字符串

* `isBlank(final CharSequence cs)`：判断是空字符串；
* `isNotBlank(final CharSequence cs)`：判断不是空字符串；
* `isEmpty(final CharSequence cs)`：判断是空字符串；
* `isNotEmpty(final CharSequence cs)`：判断不是空字符串；

### 字符串判空示例代码

```java
StringUtils.isBlank(null);      = true
StringUtils.isBlank("");        = true
StringUtils.isBlank(" ");       = true
StringUtils.isBlank("bob");     = false
StringUtils.isBlank("  bob  "); = false
StringUtils.isEmpty(null);      = true
StringUtils.isEmpty("");        = true
StringUtils.isEmpty(" ");       = false
StringUtils.isEmpty("bob");     = false
StringUtils.isEmpty("  bob  "); = false
```

> 说明：`isBlank`和`isEmpty`的区别在于在于空格字符串的判断不同；简言之，`Empty认为空格字符不是空字符串`

## 1.2.判断是否存在空字符串

* `isAnyEmpty(final CharSequence... css)`：判断是否存在空字符串；
* `isNoneEmpty(final CharSequence... css)`判断全部都不是空字符串；
* `isAllEmpty(final CharSequence... css)`：判断全部为空字符串；
* `isAnyBlank(final CharSequence... css)`：判断是否存在空字符串；
* `isNoneBlank(final CharSequence... css)`判断全部都不是空字符串；
* `isAllBlank(final CharSequence... css)`：判断全部为空字符串；

### 判断是否存在空字符串示例代码

```java
StringUtils.isAnyBlank(null)             = true
StringUtils.isAnyBlank(null, "foo")      = true
StringUtils.isAnyBlank(null, null)       = true
StringUtils.isAnyBlank("", "bar")        = true
StringUtils.isAnyBlank("bob", "")        = true
StringUtils.isAnyBlank("  bob  ", null)  = true
StringUtils.isAnyBlank(" ", "bar")       = true
StringUtils.isAnyBlank(new String[] {})  = false
StringUtils.isAnyBlank(new String[]{""}) = true
StringUtils.isAnyBlank("foo", "bar")     = false
```

> 说明：同理，这里的区别也是在于空格字符串的不同对待；

## 1.3.去除字符串前后空格

* `trim(final String str)`:去除字符串前后空格，同时不改变原字符串；
* `trimToNull(final String str)`：去除字符串前后空格，如果去除后是空字符串就转为null；
* `trimToEmpty(final String str)`：去除字符串前后空格，如果去除后是空字符串就转为空字符串(`null转为""`）;
* `strip(final String str)`：这个也可以去除字符串的前后空格，不改变原字符串；
* `stripToNull(String str)`：同`trimToNull(final String str)`类似；
* `stripToEmpty(final String str)`：同`trimToEmpty(final String str)`类似；

### 去除字符串前后空格示例代码

```java
StringUtils.trim(null)                = null
StringUtils.trim("     ")             = ""
StringUtils.trimToNull(null)          = null
StringUtils.trimToNull("     ")       = null
StringUtils.trimToEmpty(null)         = ""
StringUtils.trimToEmpty("     ")      = ""
```

## 1.4.字符串截取

* `truncate(final String str, final int maxWidth)`：截取字符串的前`maxWidth`个字符；
  * 如果`str`的长度小于`maxWidth`就直接返回`str`；
  * 如果`maxWidth`小于0则抛出`IllegalArgumentException`异常；
* `truncate(final String str, final int offset, final int maxWidth)`：截取指定区域的字符串；

### 字符串截取示例代码

```java
StringUtils.truncate(null, 0)       = null
StringUtils.truncate(null, 2)       = null
StringUtils.truncate("", 4)         = ""
StringUtils.truncate("abcdefg", 4)  = "abcd"
StringUtils.truncate(null, 0, 0) = null
StringUtils.truncate(null, 2, 4) = null
StringUtils.truncate("", 0, 10) = ""
StringUtils.truncate("", 2, 10) = ""
StringUtils.truncate("abcdefghij", 0, 3) = "abc"
StringUtils.truncate("abcdefghij", 5, 6) = "fghij"
StringUtils.truncate("abcdefghijklmno", -1, 10) = throws an IllegalArgumentException
StringUtils.truncate("abcdefghijklmno", Integer.MIN_VALUE, 10) = "abcdefghij"
StringUtils.truncate("abcdefghijklmno", 0, Integer.MAX_VALUE) = "abcdefghijklmno"
```

> 说明：更多的示例代码可以参考`StringUtils`源码；

## 1.5.字符串的剥离

* `strip(String str, final String stripChars)`：从`str`的前后剥离`stripChars`字符串；
* `stripStart(final String str, final String stripChars)`：从开始的地方剥离指定字符串；
* `stripEnd(final String str, final String stripChars)`：从结束的地方剥离字符串；
* `String[]   stripAll(final String... strs)`：对数组中的所有字符串进行去除空格；
* `String[] stripAll(final String[] strs, final String stripChars)`：对数组中的每个字符串进行字符串剥离；

### 字符串的剥离示例代码

```java
StringUtils.strip("abcde", "de");  = abc
StringUtils.strip("abcde", "ed");  = abc
StringUtils.stripStart("deabced", "ed");  = abcd
StringUtils.stripStart("deabced", "de");  = abcd
StringUtils.stripEnd("  abcyx", "xyz");   = "  abc"
StringUtils.stripEnd("120.00", ".0");     = "12"
StringUtils.stripAll(["abc", "  abc"]);   = ["abc", "abc"]
StringUtils.stripAll(["yabcz", null], "yz");  = ["abc", null]
```

## 1.6.字符串的比较

* `equals(final CharSequence cs1, final CharSequence cs2)`
* `equalsIgnoreCase(final CharSequence str1, final CharSequence str2)`
* `compare(final String str1, final String str2, final boolean nullIsLess)`
* ` compareIgnoreCase(final String str1, final String str2)`
* `compareIgnoreCase(final String str1, final String str2, final boolean nullIsLess)`
* `equalsAny(final CharSequence string, final CharSequence... searchStrings)`
* `equalsAnyIgnoreCase(final CharSequence string, final CharSequence...searchStrings)`

