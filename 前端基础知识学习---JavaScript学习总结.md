# JavaScript基础部分

## JavaScript简介

### JavaScript实现

JavaScript是一种专门为网页交互设计的脚本语言，有以下三部分组成：

* ECMAScript，提供和核心语言功能；
* 文档对象模型（DOM），提供访问和操作网页内容的方法和接口；
* 浏览器对象模型（BOM），提供与浏览器交互的方法和接口；

## HTML中使用JavaScript

### 标签的位置

`传统位置将JavaScript放在title后面`

```html
<head>
    <meta charset="UTF-8">
    <title></title>
    <script type="text/javascript" src="../js/index.js"></script>
</head>
```

由于解析网页的时候按照从上到写的加载原则，只有等所有的JavaScript文件加载成功后页面才能显示，如果JavaScript文件有一处错误，那么整个页面都就是一片空白，因此，位置应该放置与body内容后面，如下：

```html
<body>
    <!--这里放页面内容-->
    <script type="text/javascript" src="../js/index.js"></script>
</body>
```

## JavaScript基本概念

### JavaScript语法

`严格区分大小写`

在ECMAScript中的一切变量，函数名和操作符都严格区分大小写，例如变量名test和Test是两个不同的变量名。

`标识符`

`标识符`就是指变量，函数，属性的名字，或者函数的参数名，标识符是由满足下列规则组合的一个或者多个字符

* 第一个字符必须是一个字母、下划线（_）或者一个美元符号（$）；
* 其他字符可以是字母、下划线、美元符号或者数字；
* 标识符一般采用驼峰命名法，首字母小写，每一个单词的开头字母大写（规范，不是强制要求）

> 注意：不能将关键字，保留字，true，false和null用作标识符

`注释`

ECMAScript中有单行注释和块级注释，单行注释以两个斜杠开头，块级注释以一个斜杠加一个星号开头（/*），一个星号加一个斜杠结尾。

`语句`

ECMAScript语句以一个分号结尾，虽然不加分号也是能运行的，但是不加分号，是浏览器很会帮我们添加分号，这样依赖降低性能，二来，浏览器有时会加错分号的位置，所以，养成良好的代码编写风格，自己加。

### 关键字和保留字

在ECMAScript中具有特定用途的标识符称之为关键字，这些关键字可表示控制语句的开始或者结束，或者用于描述一个变量等，如下：

* `breake；do；instanceof；typeof；case；else；new；var,catch；finally；return；`
* `void；continue；for；switch；while；default；if,throw；delete；in；try；`
* `function；this；with；debugger；false；true；null`

还存在一些没有特殊用途的标识符，但是以后可能有特殊用途的标识符称之为保留字，如下：

* `class ；enum；extends；super；const；export；`
* `import；implements；le:；private；public；yield；interface；package；protected；static`

> 说明：在ES6中已经将let作为关键字定义变量

## JavaScript数据类型

### 变量

在ES中，变量是`松散类型`的，所谓松散类型就是说变量可以保存任何类型的数据，换句话说，这个变量仅仅是一个用于保存值的占位符，使用关键字`var`，在ES6中推荐使用`let`关键字；

### 数据类型

在ES中定义了六种数据类型，其中包含了五种基本数据类型，分别是：`String，Number，Boolean，Null，Underfined`和一种复杂数据类型`Object`；

### typeof操作符

因为在ES中的数据类型是松散类型的，因此需要一种手段来检测变量的数据类型，这个时候就使用`typeof`，使用该操作符可能返回以下值，分别为：

* `underfined`：表示这个值未定义；
* `boolean`：表示这个值是布尔值；
* `string`：表示这个值是一个字符串；
* `number`：表示这个值是一个数值；
* `object`：表示这个值是一个对象或者null；
* `function`：表示这个值是一个函数；

```javascript
var a = "hello"
console.log(typeof a);
console.log(Number.MAX_VALUE * Number.MAX_VALUE)/*返回的是Infinity无穷大*/
```

### Undefined类型

Undefined类型就只有一个undefined值，在使用var定义变量后，未初始化该变量，那么使用typeof检测，返回的就是undefined，例如：

```javascript
var message;
console.log(message)/*undefined*/
console.log(message == undefined);/*true*/
```

### Null类型

NULL类型也只有一个NULL值，从逻辑角度看，这个NULL值代表的是一个空对象指针，所以当我们使用`typeof null`返回的是Object，如下：

```javascript
var user = null;
console.log(typeof user);/*object*/
```

> 注意：如果定义变量是在将来用于保存对象，那么最好在定义的时候就初始化为NULL

### Boolean类型

Boolean类型有两个值，分别为:true，false；这两个值和数字值没有关系，true并不代表1，如下：

```javascript
var message = false;
var test = true;
```

> 注意：Boolean类型的值是区分大小写的，True和False，或者大小写混写都只是代表标识符，不是Boolean值；

虽然Boolean类型的值只有两个，但是在ES中任何类型的值都可以与这两个Boolean值有着等价的值，要将一个值转换为对应的Boolean值，可以调用转型函数`Boolean()`，如下所示：

```javascript
var test = "Helo";
var testBoolean = Boolean(test);
console.log(typeof testBoolean)/*boolean*/
console.log(testBoolean);/*true*/
```

有关各种数据类型的转换规则如下：

* String类型，非空字符转换为true，空字符串转换为false；
* number类型，任何非零数字值（包括无穷大），转换为true，0和NaN转换为false；
* Object类型，任何对象转换为true，NUL转换为false；

使用这些转换规则可以更方便的使用if流程语句，例如：

```javascript
var message = "Hello Word";
if(message){
    console.log("我执行了。。。")/*该行代码会被执行*/
}
```

### Number类型

ES中Number数据类型包含整数和浮点数值，在JavaScript中可以使用number类型保存正零和负零，正零和负零被认为是相等的。

`浮点数值`

在JavaScript中浮点数值就是数值中必须包含一个小数点，并且小数点后至少有一位数字，虽然小数点前面可以没有整数，但是不建议这种写法，如下：

```javascript
var floatN1 = "1.1";
var floatN2 = "0.1";
var floatN3 = ".1";/*虽有效，但不推荐*/
```

由于保存浮点数值需要的内存空间是整数的两倍，所以，只要可以将浮点数转换为整数，就会转换为整数，如下：

```javascript
var floatN1 = "1.0";
```

当然也可以使用科学计数法，如下：

```javascript
var floatN1 = 3.12e7;
```

> 注意：使用浮点数值进行计算会产生误差的问题，所以和钱相关的就不要使用JavaScript计算了。

`整数数值`

由于内存的限制，JavaScript不可能保存最大的数值，所以就有了JavaScript中的最大值和最小值，如果超出这个值就会返回一个Infinity无穷大和-Infinity负无穷，如下

```javascript
console.log(Number.MIN_VALUE);/*5e-324*/
console.log(Number.MAX_VALUE);/*1.7976931348623157e+308*/
```

### 非数值NAN

NAN,是一个特殊的数值，表示一个本来应该返回数值的操作却没有返回数值的情况，例如任何数值除以非数值就会返回NAN，但是在其他编程语言中就会保存，就是程序的运行。有关非数值NAN需要注意以下几点：

* 任何涉及到NAN的操作都会返回NAN,例如NAN/10;
* 任何数值和NAN都不相等，包括NAN本身；

在JavaScript中针对NAN的特殊性，定义了`isNaN()函数`，这个函数接受一个参数，该参数可以是任何类型，这个函数可以帮我们确定这个参数是否是`不是数值`，这个函数接收一个参数后，会尝试将这个参数转换为数值类型，不能被转换的就会返回false，否则返回true，如下：

```javascript
console.log(isNaN(NaN));/*true*/
console.log(isNaN(10));/*false*/
console.log(isNaN("10"));/*false*/
console.log(isNaN("hello"));/*true*/
			console.log(isNaN(true));/*false*/
```

### 数值转换

在JavaScript中有三个函数可以将非数值转换为数值类型，分别是：`Number()、parseInt()和parseFloat()`,其中Number()函数可以用于任何数据类型，而其他两个用于将字符串转换为数值，三个函数对于同样的输入却有不同的输出，具体的转换规则如下：

`Number()函数的转换规则`

* 如果是Boolean值，则true和false分别转换为1和0；
* 如果是数字值，就只是简单的传入和返回；
* 如果是NULL，返回的是0；
* 如果是undefined，返回的是NAN；
* 如果是字符串，具体转换如下：
  * 如果字符串只包含数字（包括前面的正负号），则将其转换为十进制数值；
  * 如果字符串包含有效的浮点格式，就会转换为对应的浮点数值；
  * 如果字符串包含有效的十六进制格式，就会转换为相同大小的十进制数值；
  * 如果字符串是空字符串，则转换为数值0；
  * 如果字符串不是以上类型的格式，就会转换为NaN；
* 如果是对象，则调用对象的valueOf()方法，然后按照上述的规则转换，如果转换结果为NaN，则调用对象的toString()方法，然后再按照上述规则转换；

```javascript
console.log(Number("hello word"))/*NaN*/
console.log(Number(""))/*0*/
console.log(Number("0000011"))/*11*/
console.log(Number(true))/*1*/
```

`parsenInt()函数转换规则`

* parsenInt()函数在转换数值的时候，注重的是看是否复合数值模式，它会忽略字符串前面的空格，直至找到第一个非空字符串，如果第一个字符串不是数字或者正负号，就返回NaN；
* 对空字符串，返回的是NaN,`Number函数对于空字符串返回的是0`

```javascript
console.log(parseInt("1234hello"));/*1234*/
console.log(parseInt(""));/*NaN*/
console.log(parseInt("0xA"));/*十六进制：10*/
console.log(parseInt("22.5"));/*22*/
console.log(parseInt("070"));/*八进制：70*/
console.log(parseInt("70"));/*十进制：70*/
console.log(parseInt("0xf"));/*十六进制：15*/
```

`parseFloat函数转换规则`

* 从第一个字符开始解析，直到解析到字符串末尾结束，或者遇见一个无效的浮点数字字符位置；
* 字符串中的第一个小数点是有效的，第二个小数点是无效的；
* 如果字符串包含的是一个可解析为整数的数，则会返回整数；

```javascript
console.log(parseFloat("1234hello"));/*1234*/
console.log(parseFloat("0xA"));/*0*/
console.log(parseFloat("22.5"));/*22.5*/
console.log(parseFloat("22.34.45"));/*22.34*/
console.log(parseFloat("0986.6"));/*986.6*/
console.log(parseFloat("3.144e7"));/*31440000*/
```

### String类型

string类型用于表示由零个或者多个16位Unicode字符组成的字符串序列，就是字符串，在JavaScript中字符串可以使用单引号和双引号，但是不可混用。

`转义字符`

* `\n`：表示换行，`\t`：表示制表符，`\b`：表示退格，`\r`：表示回车
* `\\`:表示斜杠，`\'`：表示单引号，`\"`：表示双引号

> 说明：任何字符串可以通过length属性获取字符串的长度，例如：`text.length`

`字符串的特点`

在JavaScript中字符串是不可变的，换句话说，就是字符一旦创建，他的值就不能改变，要改变某一个变量的字符串，就要先销毁原来的字符串，在创建一个新的字符串值填充该变量。

`转换为字符串`

* 调用toString()方法；例如数值，布尔值，对象和字符串值都有toString方法；null和undefined没toString方法；
* 使用String()转型函数，

```javascript
var num = 10;
console.log(num.toString());/*"10"*/
console.log(num.toString(2));/*按照二进制转换："1010"*/
console.log(num.toString(8));/*按照八进制转换："12"*/
console.log(num.toString(10));/*按照十进制转换："10"*/
console.log(num.toString(16));/*按照十六进制转换："a"*/
```

> 注意：调用toString方法可以指定基数，转换成不同的字符串

```javascript
var value;
console.log(String(10));/*"10"*/
console.log(String(true));/*"true"*/
console.log(String(null));/*"null"*/
console.log(String(value));/*"undefined"*/
```

### Object类型

在JavaScript中`对象是一组数据和功能的集合`，对象通过new的方式创建，例如`var u = new Object()`，Object类型是所有它的实例的基础，也就是说，Object具有的任何属性和方法也都存在与具体的对象中，和java中的Object对象一样，Object具有的属性和方法如下：

* constructor：保存着用于创建当前对象的函数，构造函数constructor就是Object()；
* hasOwnProperty（propertyName）：用于检查给定的属性在当前对象实例中是否存在；
* isPrototypeOf(object)：用于检查传入的对象是否是当前对象的原型；
* propertyIsEnumerable(propertyName)：用于检查给定的属性能否使用for-in语句来枚举；
* toLocaleString()：返回对象的字符串表示；
* toString()：返回对象的字符串表示；
* valueOf()：返回对象的字符串、数值或者布尔值。

## JavaScript操作符

### 一元操作符

