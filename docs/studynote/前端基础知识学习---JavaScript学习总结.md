# JavaScript基础部分

## JavaScript基础知识脑图总结

![JavaScript学习笔记](http://image.luokangyuan.com/2018-10-19-142040.png)

## JavaScript简介

大家都知道Html+CSS+JavaScript是前端入门的三门基本课程，其中Javascript应该是最难的，也是可以解决很多很多问题的，这篇笔记只是介绍JavaScript的基础知识，后面随着学习的深入，将会总结JavaScript的高级知识。接下来，我们需要介绍一下知识：

* JavaScript的基本语法、变量和标识符；
* JavaScript数据类型、类型转换和运算符；
* JavaScript的流程控制语句；
* 对象、函数、数组和文档对象模型；

## JavaScript的基本语法

* JavaScript采用单行注释和多行注释；
* JavaScript严格区分大小写;
* JavaScript每条语句以分号结尾，如果不加，浏览器会帮我们添加，影响性能，压缩的时候还会出错；
* JavaScript会自动忽略多个空格和换行；

## JavaScript变量

* ES的变量是松散类型的，就是定义的时候不区分类型，使用`var`关键字定义变量；
* 使用等号为变量赋值；
* 在最外面定义的是全局变量，在函数内部定义的就是局部变量，如果不加`var`关键字就变成全局变量；
* 变量名不能使用JavaScript中的关键字和保留字；
* 多个变量定义使用逗号隔开，使用分号结尾；

## JavaScript标志符

* 标识符中可以含有数字，字母，下划线和$；
* 标志符不能以数字开头；
* 标志符不能是JavaScript中的关键字和保留字；
* 标志符最好使用驼峰命名方式；

## JavaScript数据类型

在javaScript中存在六种数据类型：五种基本数据类型和一种引用类型（Object）分别为：**String字符串、Number数值、Boolean布尔值、NULL空值、Undefind未定义和Object对象**

> 说明：在JavaScript中使用typeof返回变量的数据类型；

## JavaScript类型转换

**类型转换就是将其他的数据类型转为String、Boolean、Number**，基本每一个类型的转换方式都有很多，详情可以参考脑图总结。

**转为String**：

* 强制类型转换，即调用被转换数据的toString()，不适合null和Undefined类型，他们没有toString()方法；
* 直接调用String函数，例如String(str);会讲null和Undefined转为对应的字符串；
* 隐式类型转换，即在数据后+"";

**转为Number：**

* 调用Number()函数,合法数字字符串正常转换，非法转为NaN,空串或纯空格转为0，true转为1，false转为0，null转为0，Undefind转为NaN；
* 调用parseFloat()和parseInt()函数；

**转为Boolean：**

* 使用Boolean()函数，字符串转布尔除了空串全为true，数值除了0和NaN全为true，null和Undefined全为false，对象转布尔值都是true。
* 隐士转换，使用两个!!，例如

```javascript
let age = 12;
if(!!age){
    console.log("数值转为布尔值")
}
```

## JavaScript运算符

在JavaScript中，运算符包括typeof运算符、算数运算符、一元运算符、逻辑运算符、赋值运算符、关系运算符以及三元运算符和相等运算符。运算符都是一些基本很简单的，可以参考脑图总结，下面列举一些需要注意的：

* 由于undefined衍生自null，所以 null == undefind会返回true，但是 null === undefind 会返回false ；
* NaN不与任何值相等，包括他本身 
* 判断一个值是否是NaN使用isNaN()函数 

## JavaScript对象

对象Object是JavaScript六种数据类型中的引用类型，用来保存不同数据类型的数据，使用`vae obj = {}`或者`var obj = new Object()`方式创建，使用对象名.属性名来获取对象中的属性值，使用**属性名 in 对象**来检测是否含有属性；

## JavaScript数组

数组比较简单，在脑图中也有详细的介绍，下面主要列举数组的一些常用方法

* push()：向数组的末尾添加一个或多个元素，并返回数组的新长度 
* pop()：用来删除数组的最后一个元素，并返回被删除的元素 
* unshift()：用来向数组的开头添加一个或多个元素，并返回数组的新长度 
* shift()：用来删除数组开头的一个元素，并返回被删除的元素 
* slice()：从数组中截取指定的元素 
* splice()：可以用来删除数组中指定的元素，并使用新元素代替 
* 性能最高的数组遍历：for(j = 0,len=arr.length; j < len; j++) {} 

## 文档对象模型DOM

**DOM查询：**

* document.getElementById("id属性值"); 
* document.getElementsByName("name属性值"); 
* 根据元素的class属性查找一组元素。存在多个返回一个：document.querySelector() 
* document.getElementsByTagName("标签名"); 
* document.getElementsByClassName(className)  

**DOM增加：**

* 在指定节点的内部最后添加节点：element.appendChild(Node); 
* 在指定节点内部，指定节点前面添加节点：elelment.insertBefore(newNode,existingNode); 

**DOM删除：**

* 删除当前节点下的子节点，成功返回删除的节点，否则返回null：element.removeChild(Node) 

**DOM节点创建：**

* 创建一个Html元素节点：ar oP = document.createElement("p"); 
* 创建一个文本节点：document.createTextNode(String); 
* 创建一个属性节点：document.createAttribute("class");  

**DOM常用操作：**

* 获取父节点：element.parentNode ；

* 获取所有子节点，包括html节点，文本节点，属性节点：element.chilidNodes ；

* 获取第一个子节点：element.firstChild ；

* 获取当前元素的下一个同级节点，没有返回null：element.nextSibling ；

* 获取当前元素的所有文本，包括Html代码：element.innerHTML

## 浏览器对象模型BOM

**window对象：当前浏览器窗体**

属性如下：

* opener：即谁打开我的，若在A用open打开B则B的opener就是A
* closed：判断子窗体是否关闭

方法如下：

* alert() 弹出框
* confirm() 带确认,取消弹出框
* setInterval() 每隔多少秒调用一次
* clearInterval() 清除setInterval
* setTimeout() 隔多少秒调用一次
* cleartimeout() 清除setTimeout
* open() 打开一个新的窗口`window.open("other.html","_blank","width=200,height=300,top=300");`
  console:console.log()浏览器控制台打印

子对象：doument loation history screen ……

* doument dom操作
* loation 跳转位置`window.location.href="popl.html";`
* history 历史`history.back()//返回到前一页 等于history.go(-1) 参数负几就是返回前几步`
* screen //屏幕
  screen.availHeight //屏幕实际高度
  screen.availWidth //屏幕实际宽度
  screen.height //屏幕高度
  screen.width //屏幕宽度　

## 常见的页面交互事件

**表单元素事件,在表单元素中才有效**

* onchange     框内容改变时

* onsubmit     点击提交按钮时

* onreset     重新点击鼠标按键时

* onselect     文本被选择时

* onblur      元素失去焦点时

* onfocus     当元素获取焦点时


**键盘事件**

* onkeydown    按下键盘按键时
* onkeypress    按下或按住键盘按键时
* onkeyup     放开键盘按键时

**其他事件**

* onclick     鼠标点击一个对象时
* ondblclick    鼠标双击一个对象时
* onmousedown 鼠标被按下时
* onmousemove 鼠标被移动时
* onmouseout    鼠标离开元素时
* onmouseover 鼠标经过元素时
* onmouseup    释放鼠标按键时

