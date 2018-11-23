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

* `equals(final CharSequence cs1, final CharSequence cs2)`:判断两个字符串是否相等；
* `equalsIgnoreCase(final CharSequence str1, final CharSequence str2)`：判断两个字符串在忽略大小写下是否相等；
* `compare(final String str1, final String str2)`：比较两个字符串长度的大小；
* ` compareIgnoreCase(final String str1, final String str2)`忽略大小写下比较两个字符串的大小；
* `equalsAny(final CharSequence string, final CharSequence... searchStrings)`:比较多字符串中是否存在相同的字符串；
* `equalsAnyIgnoreCase(final CharSequence string, final CharSequence...searchStrings)`：忽略大小写是否存在相同字符

### 字符串的比较代码示例

```java
StringUtils.equals("abc", null)  = false
StringUtils.equals("abc", "abc") = true
StringUtils.equals("abc", "ABC") = false
tringUtils.equalsIgnoreCase("abc", "ABC") = true
StringUtils.compare("abc", "abc") = 0
StringUtils.compare("a", "b")     > 0
StringUtils.compare("b", "a")     < 0
StringUtils.compare("a", "B")     < 0
StringUtils.compare("ab", "abc")  < 0
StringUtils.compareIgnoreCase("a", "B")     < 0
StringUtils.compareIgnoreCase("A", "b")     < 0
StringUtils.compareIgnoreCase("ab", "ABC")  < 0
StringUtils.equalsAny("abc", "abc", "def") = true
StringUtils.equalsAny("abc", "ABC", "DEF") = false
StringUtils.equalsAnyIgnoreCase("abc", "abc", "def") = true
StringUtils.equalsAnyIgnoreCase("abc", "ABC", "DEF") = true
```

## 1.7.字符串的查找

* `indexOf(final CharSequence seq, final int searchChar)`：查找指定字符在字符串中首次出现的位置的索引值；
* `indexOf(final CharSequence seq, final int searchChar, final int startPos)`：从指定位置` final int startPos`开始查找；
* `indexOfIgnoreCase(final CharSequence str, final CharSequence searchStr)`：忽略大小写的查找字符串首次出现位置
* `indexOfIgnoreCase(final CharSequence str, final CharSequence searchStr, int startPos)`：同``indexOf`类似；
* `lastIndexOf(final CharSequence seq, final int searchChar)`：从尾部开始查找；
* `lastIndexOf(final CharSequence seq, final int searchChar, final int startPos) `：同理从尾部开始查找指定位置前的字符

### 字符串的查找示例代码

```java
StringUtils.indexOf("aabaabaa", 'b') = 2
StringUtils.indexOf("aabaabaa", "")   = 0
StringUtils.indexOf("aabaabaa", "b", 3)  = 5
StringUtils.ordinalIndexOf("abababab", "abab", 3) = 4
StringUtils.lastIndexOf("aabaabaa", 'b') = 5
StringUtils.lastIndexOf("aabaabaa", 'a', 0)  = 0
```

## 1.8.是否包含字符串

* `contains(final CharSequence seq, final int searchChar`：是否包含指定字符串；
* `containsIgnoreCase(final CharSequence str, final CharSequence searchStr`：忽略大小写下检查是否包含指定字符串
* `containsWhitespace(final CharSequence seq)`：是否包含空白字符串；
* ` containsAny(final CharSequence cs, final char... searchChars)`：判断字符窜是否包含后面多个中的任意一个；
* `containsOnly(final CharSequence cs, final char... valid)`：判断前面的字符是否全部是后面的字符中的；
* `containsNone(final CharSequence cs, final char... searchChars)`：判断前面字符是否不含其后字符的任意一个字符

### 是否包含字符串示例代码

```java
StringUtils.contains("abc", 'a') = true
StringUtils.contains("abc", 'z') = false
StringUtils.containsIgnoreCase("abc", "A") = true
StringUtils.containsIgnoreCase("abc", "Z") = false
StringUtils.containsWhitespace("abc")      = false
StringUtils.containsWhitespace("a  bc")    = true
StringUtils.containsAny("zzabyycdxx",['z','a']) = true
StringUtils.containsAny("zzabyycdxx",['b','y']) = true
StringUtils.containsAny("zzabyycdxx",['z','y']) = true
StringUtils.containsAny("aba", ['z'])           = false
StringUtils.containsOnly("abab", 'abc') = true
StringUtils.containsOnly("ab1", 'abc')  = false
StringUtils.containsOnly("abz", 'abc')  = false
StringUtils.containsNone("abab", 'xyz') = true
StringUtils.containsNone("ab1", 'xyz')  = true
StringUtils.containsNone("abz", 'xyz')  = false
```

## 1.9.截取字符串

* `substring(final String str, int start)`：指定开始为止截取字符串；
* `substring(final String str, int start)`:指定开始和结束为止截取字符串；
* `left(final String str, final int len)`：得到一个字符串最左边的`len`个字符；
* `right(final String str, final int len)`：得到一个字符串最右边的`len`个字符；
* `mid(final String str, int pos, final int len)`：得到中间的指定区域的字符，是左包含；
* `substringBefore(final String str, final String separator)`：得到一个字符串中第一个分割符之前的字符串；
* `substringAfter(final String str, final String separator)`：同理，得到一个字符串第一个分割符之后的字符串；

### 截取字符串示例代码

```java
StringUtils.substring("abc", 2)  = "c"
StringUtils.substring("abc", 4)  = ""
StringUtils.substring("abc", -2) = "bc"
StringUtils.substring("abc", -4) = "abc"
StringUtils.substring("abc", 2, 2)   = ""
StringUtils.substring("abc", -2, -1) = "b"
StringUtils.substring("abc", -4, 2)  = "ab"
StringUtils.left("abc", 2)   = "ab"
StringUtils.left("abc", 4)   = "abc"
StringUtils.right("abc", 2)   = "bc"
StringUtils.right("abc", 4)   = "abc"
StringUtils.mid("abc", 2, 4)   = "c"
StringUtils.mid("abc", 4, 2)   = ""
StringUtils.mid("abc", -2, 2)  = "ab"
StringUtils.substringBefore("abcba", "b") = "a"
StringUtils.substringBefore("abc", "c")   = "ab"
StringUtils.substringBefore("abc", "d")   = "abc"
StringUtils.substringAfter("abcba", "b") = "cba"
StringUtils.substringAfter("abc", "c")   = ""
StringUtils.substringAfter("abc", "d")   = ""
```

## 1.10.字符串分割

* `String[] split(final String str)`：按照空格分割字符串，返回数组；
* `String[] split(final String str, final char separatorChar)`：指定分割符分割字符串；
* `String[] split(final String str, final String separatorChars, final int max)`：指定分割符，指定最大分割的数组大小;
* ` String[] splitByCharacterTypeCamelCase(final String str)`：根据字符串类型分割字符串；

### 字符串分割示例代码

```java
StringUtils.split("abc  def") = ["abc", "def"]
StringUtils.split(" abc ")    = ["abc"]
StringUtils.split("a..b.c", '.')   = ["a", "b", "c"]
StringUtils.split("a:b:c", '.')    = ["a:b:c"]
StringUtils.split("a b c", ' ')    = ["a", "b", "c"]
StringUtils.split("ab:cd:ef", ":", 3); = [ab, cd, ef]
StringUtils.split("ab:cd:ef", ":", 2); = [ab:cd:ef]
StringUtils.split("ab:cd:ef", ":", 1); = [ab, cd:ef]
StringUtils.splitByCharacterTypeCamelCase("number5")    = ["number", "5"]
StringUtils.splitByCharacterTypeCamelCase("fooBar")     = ["foo", "Bar"]
StringUtils.splitByCharacterTypeCamelCase("foo200Bar")  = ["foo", "200", "Bar"]
StringUtils.splitByCharacterTypeCamelCase("ASFRules")   = ["ASF", "Rules"]
```

## 1.11.字符串连接（数组转字符）

* `join(final T... elements)`；普通的连接字符串
* `join(final Object[] array, final char separator)`：指定连接符，连接字符串；

### 字符串连接示例代码

```java
StringUtils.join(["a", "b", "c"]) = "abc"
StringUtils.join([null, "", "a"]) = "a"
StringUtils.join(["a", "b", "c"], ';')  = "a;b;c"
StringUtils.join(["a", "b", "c"], null) = "abc"
StringUtils.join([null, "", "a"], ';')  = ";;a"
```

## 1.12.删除和替换字符串

* `removeStart(final String str, final String remove)`：删除字符串中的指定的前缀字符串；
* ` removeStartIgnoreCase(final String str, final String remove)`：同`removeStart`类似，只是忽略的大小写；
* `removeEnd(final String str, final String remove)`：同`removeStart`相反；
* `deleteWhitespace(final String str)`：删除字符串中的空格；
* `remove(final String str, final String remove)`：从字符串中移除指定字符串；
* `removeAll(final String text, final String regex)`：移除符合正则表达式的所有字符串；
* `replaceOnce(final String text, final String searchString, final String replacement)`：将字符串中指定字符替换一个；
* `replaceAll(final String text, final String regex, final String replacement)`：将字符串中指定字符替换所有；

### 删除和替换字符串示例代码

```java
StringUtils.removeStart("www.domain.com", "www.")   = "domain.com"
StringUtils.removeStart("domain.com", "www.")       = "domain.com"
StringUtils.removeStart("www.domain.com", "domain") = "www.domain.com"
StringUtils.removeStart("abc", "")    = "abc"
StringUtils.deleteWhitespace("abc")        = "abc"
StringUtils.deleteWhitespace("   ab  c  ") = "abc"
StringUtils.removeEnd("www.domain.com", ".com.")  = "www.domain.com"
StringUtils.removeEnd("www.domain.com", ".com")   = "www.domain"
StringUtils.removeEnd("www.domain.com", "domain") = "www.domain.com"
StringUtils.remove("queued", "ue") = "qd"
StringUtils.remove("queued", "zz") = "queued"
StringUtils.removeAll("any", ".*")  = ""
StringUtils.removeAll("any", ".+")  = ""
StringUtils.removeAll("abc", ".?")  = ""
StringUtils.removeAll("A&lt;__&gt;\n&lt;__&gt;B", "&lt;.*&gt;")      = "A\nB"
StringUtils.removeAll("A&lt;__&gt;\n&lt;__&gt;B", "(?s)&lt;.*&gt;")  = "AB"
StringUtils.removeAll("ABCabc123abc", "[a-z]")     = "ABC123"
StringUtils.replaceOnce("aba", "a", null)  = "aba"
StringUtils.replaceOnce("aba", "a", "")    = "ba"
StringUtils.replaceOnce("aba", "a", "z")   = "zba"
StringUtils.replaceAll("ABCabc123", "[^A-Z0-9]+", "_")  = "ABC_123"
StringUtils.replaceAll("ABCabc123", "[^A-Z0-9]+", "")   = "ABC123"
StringUtils.replaceAll("abc", "", "ZZ")  = "ZZaZZbZZcZZ"
```

## 1.13.重复字符串

* `repeat(final String str, final int repeat)`：生成指定重复次数的重复字符串；

### 重复字符串生成示例代码

```java
StringUtils.repeat("a", 3)  = "aaa"
StringUtils.repeat("ab", 2) = "abab"
StringUtils.repeat("a", -2) = ""
```

## 1.14.字符串出现次数

* `countMatches(final CharSequence str, final CharSequence sub)`：获取指定字符串在原字符串中出现的次数；

### 字符串出现次数示例代码

```java
StringUtils.countMatches("abba", "a")   = 2
StringUtils.countMatches("abba", "ab")  = 1
```

# 二、StringUtils其他常用方法

## 2.1.判断是否是空格或者数字

* `isNumericSpace(final CharSequence cs)`

```java
StringUtils.isNumericSpace("12 3") = true
StringUtils.isNumericSpace("12.3") = false
```

## 2.2.判断是否是空格

* `isWhitespace(final CharSequence cs)`

```java
StringUtils.isWhitespace("")     = true
StringUtils.isWhitespace("  ")   = true
StringUtils.isWhitespace("abc")  = false
```

## 2.3.判断是否全是小写

* `isAllLowerCase(final CharSequence cs)`

```java
StringUtils.isAllLowerCase("abc")  = true
StringUtils.isAllLowerCase("abC")  = false
```

## 2.4.判断是否全是大写

* `isAllUpperCase(final CharSequence cs)`

```java
StringUtils.isAllUpperCase("ABC")  = true
StringUtils.isAllUpperCase("aBC")  = false
```

## 2.5.反转字符串

* `reverse(final String str)`

```java
StringUtils.reverse("bat") = "tab"
```

## 2.6.字符串显示省略号

* `abbreviate(final String str, final int maxWidth)`

```java
StringUtils.abbreviate("abcdefg", 6) = "abc..."
StringUtils.abbreviate("abcdefg", 7) = "abcdefg"
StringUtils.abbreviate("abcdefg", 8) = "abcdefg"
StringUtils.abbreviate("abcdefg", 4) = "a..."
```

## 2.7.判断字符串是否以什么开头

* `startsWith(final CharSequence str, final CharSequence prefix)`

```java
StringUtils.startsWith("abcdef", null)  = false
StringUtils.startsWith("abcdef", "abc") = true
StringUtils.startsWith("ABCDEF", "abc") = false
```

## 2.7.判断字符串是否以什么结尾

* `endsWith(final CharSequence str, final CharSequence suffix) `

```java
StringUtils.endsWith("ABCDEF", "def") = false
StringUtils.endsWith("ABCDEF", "cde") = false
StringUtils.endsWith("ABCDEF", "")    = true
```
