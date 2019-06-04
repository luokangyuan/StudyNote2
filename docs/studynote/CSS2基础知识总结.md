<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Css.svg"/>
</p>
<h1 align="center">CSS基础知识总结</h1>

## 1.CSS基础知识脑图总结

![CSS初级知识](http://image.luokangyuan.com/2018-10-15-155344.png)

## 2.简介

CSS 指层叠样式表 (Cascading Style Sheets) ，简单讲就是定义如何显示 HTML 元素 ；

## 3.基本语法

* CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明 
* 选择器通常是您需要改变样式的 HTML 元素 
* 每条声明由一个属性和一个值组成 
* 属性是您希望设置的样式属性，每个属性有一个值。属性和值被冒号分开 

> 说明：选择器+大括号＋键值对+用分号隔开 

## 4.常用选择器

首先，我们需要引入外部的CSS样式文件，外部样式表可以极大提高工作效率 ，引入方法如下：

```html
<link rel="stylesheet" type="text/css" href="../css/index.css" >
```

### 4.1.基本选择器

**通用选择器：`*`**：选中页面所有的元素

```css
* {font-size: 20px;}
```

```html
<div id="card">
    <div>
        <span>1</span>
    </div>
    <div>
        <span>2</span>
    </div>
</div>
```

> 说明：html页面的所有元素的文字大小都会被改变；

**元素选择器**：选中html页面元素

```css
span {font-size: 22px;}
```

```html
<div id="card">
    <div>
        <span>1</span>
        <h2>h2</h2>
    </div>
    <div>
        <span>2</span>
    </div>
</div>
```

> 说明：html页面中span元素的文字大小会被改变，h2的文字大小不会被改变；

**ID选择器**：选中对应ID属性值的html元素

```css
#card {border: 1px solid #FF2A68;}
```

> 说明：html元素和上述一样，最外层的div将会被添加一个边框；

**后代选择器：空格**

```html
<div id="card">
    <div class="photo">
        <span>1</span>
        <h2>h2</h2>
    </div>
    <div class="photo">
        <span>2</span>
    </div>
    <div class="default">
        <span class="photo">3</span>
    </div>
</div>
```

```css
#card .photo {background: #FF2A68;}
```

> 说明：上述所有的class = "photo"的元素都会被选中；

**直接后代元素选择器：`>`**

```css
#card > .photo {background: aqua;}
```

> 说明：上述的3号photo的元素不会被选中，因为不属于直接后代元素；

**相邻兄弟选择器：`+`**

```html
<div class="default">1</div>
<div class="photo">3</div>
<div class="photo">5</div>
<div class="nav">6</div>
<div class="photo">7</div>
```

```css
.default + .photo {background: bisque;}
```

> 说明：上述的只有3号元素会被选中，只有它才是default的相邻photo兄弟元素；

**通用兄弟元素：`～`**

```html
<div class="default">1</div>
<div class="photo">3</div>
<div class="photo">5</div>
<div class="nav">6</div>
<div class="photo">7</div>
```

```css
.default ~ .photo {background: bisque;}
```

> 说明：其中的3，5，7号元素都会被选中，他们都是default的相邻兄弟元素

### 4.2.属性选择器

**存在属性选择器：`[attr]`**

```html
<form class="userInfo">
     <label>
         用户名： <input type="text" name="userName" autocomplete="off" placeholder="用户名">
     </label>
     <label>
         密码：<input type="password" name="password" autocomplete="off" placeholder="密码">
     </label>
     <label>
         性别：<input type="text" name="sex">
     </label>
</form>
```

```css
.userInfo [name] {border: 1px solid #FF2A68;}
```

> 说明：上述的html元素中的三个input框都包含了name属性，所以都将被选中；

**存在和属性值选择器：`[attr=val]`**

同样的是上述的html元素布局，我们使用存在和值属性选择器如下：

```css
.userInfo [name="userName"] {border: 1px solid #FF2A68;}
```

> 说明：这样只会选中name="userName"的input元素；

**存在属性同事包含属性值选择器：`[attr ~=val]`**

```html
<a href="#" title="Hello Word">HelloWord</a>
<a href="#" title="Hello People">HelloPeople</a>
<a href="#" title="Hello Girl">HelloGirl</a>
<a href="#" title="My name is solo">My name is solo</a>
```

```css
a[title ~= "Hello"] {color: #FF2A68;}
```

> 说明：前面三个a元素的title属性值都包含了Hello值，所以都会被选中；

**子串值属性选择器选择属性值等于val或者以val-开头的元素：`[attr|=val]`**

```html
<a href="#" title="HelloPeople">HelloPeople</a>
<a href="#" title="Hello-Word">HelloWord</a>
<a href="#" title="Hello">HelloWord</a>
```

```css
a[title |= "Hello"]{color: aqua}
```

> 说明：后两个a标签将会被选中；

**选中存在属性，并且属性值以val 开始的元素：`[attr ^= val]`**

```html
<a href="#" title="Hello Word">HelloWord</a>
<a href="#" title="Hello">Hello</a>
<a href="#" title="HelloPeople">HelloPeople</a>
<a href="#" title="Hello-Word">HelloWord</a>
```

```css
a[title ^= "Hello"]{color: aqua}
```

> 说明；上述列举的四种情况都满足，都将会被选中；

**选中存在属性，并且属性值以val结束的元素：`[attr $= val]`**

同样是上述的元素，修改为如下，将只会有第一个和最后一个被选中。

```css
a[title $= "ord"]{color: aqua}
```

**选中存在属性，并且属性值中包含val的元素：`[attr *= val]`**

同样的是上述的元素，修为如下，就只有第三个元素将会被选中。

```css
a[title *= "p"]{color: aqua}
```

### 4.3.伪类选择器和伪元素选择器

伪类选择器包括 **伪类选择器**、 **动态伪类** 、**表单伪类** 和**结构性伪类**  ，

**链接伪类**

*  link：表示作为超链接，并指向一个未访问的地址的所有锚 
*  visited：表示作为超链接，并指向一个已访问的地址的所有锚 

**动态伪类**

* hover：悬浮到目标元素上 
* active：点击下去 

**表单伪类**

* enabled：可用 
* disabled：不可用 
* checked：选中 

**结构性伪类**

* :nth-child(index)系列：找到某下的第一个适配元素 
* :nth-of-type(index)系列：找到某一下第一次出现的适配元素 

> 说明：在 CSS 定义中，`a:hover` 必须被置于` a:link` 和 `a:visited` 之后，才是有效的；
>
> `a:active `必须被置于` a:hover` 之后，才是有效的；
>
> 伪类名称对大小写不敏感

```html
<div class="card">
    <span class="a1">我被.a1:nth-child(1)选中</span>
    <span class="a2">我被.a2:nth-of-type(1)选中</span>
    <span class="a1">我不被.a1:nth-child(1)选中</span>
    <a class="baidu" href="#">百度</a>
    <span class="baidu">我是伪类选择器</span>
    <label class="a1">
        <input type="text" disabled value="我会被disabled选中">
    </label>
    <label class="a2">
        <input type="text"  value="我会被enabled选中">
    </label>
    <label class="a1">
        <input type="checkbox" checked><span>羽毛球</span>
    </label>
    <label class="a3">
        <input type="checkbox" ><span>篮球</span>
    </label>

</div>
```

```css
a:visited{color: blanchedalmond}
a:link{color: aqua}
.baidu:hover{color: coral}
.baidu a:active{color: blueviolet}
input:disabled{border: 1px solid #FF2A68}
input:enabled{border: 1px solid #52EDC7}
input:checked + span{color: #FF2A68}
.a1:nth-child(1){color: #5856D6}
.a2:nth-of-type(1){color: #5AD427}
```



### 4.4.选择器优先级

!important > 行内样式（写到元素上）> id选择器 > class选择器 > 标签（*） > 通配符 > 继承 > 浏览器默认属性

## 5.其他基础知识

## 5.1.浮动

简单讲：**浮动元素会脱离文档流并向左/向右浮动，直到碰到父元素或者另一个浮动元素**，**脱离文档的意思就是说浮动不会影响普通元素的布局**

## 5.2.高度坍塌

**浮动元素脱离了文档流，并不占据文档流的位置，自然父元素也就不能被撑开，所以没了高度**。这就是高度坍塌

## 5.3.清除浮动的方式

清除浮动主要有两种方式，分别是**clear清除浮动**和**BFC清除浮动**；

**clear清除浮动**

clear属性不允许被清除浮动的元素的左边/右边挨着浮动元素，底层原理是在被清除浮动的元素上边或者下边添加足够的清除空间。

> 说明：我们是通过在别的元素上清除浮动来实现撑开高度的， 而不是在浮动元素，不要在浮动元素上清除浮动

```css
// 方式一：不支持IE6/7
.clearfix:after {
    display: table;
    content: " ";
    clear: both;
}
// 方式二：引入了zoom以支持IE6/7
.clearfix:after {
    display: table;
    content: " ";
    clear: both;
}
.clearfix{
    *zoom: 1;
}
// 全浏览器通用的clearfix方案【推荐】
// 引入了zoom以支持IE6/7
// 同时加入:before以解决现代浏览器上边距折叠的问题
.clearfix:before,
.clearfix:after {
    display: table;
    content: " ";
}
.clearfix:after {
    clear: both;
}
.clearfix{
    *zoom: 1;
}
```

**BFC清除浮动**

还没完全理解

## 5.4.定位

* 元素按照其在 HTML 中的位置顺序决定排布的过程。
* HTML的布局机制就是用文档流模型的，即块元素（block）独占一行，内联元素（inline），不独占一行
* 使用margin是用来隔开元素与元素的间距
* padding是用来隔开元素与内容的间隔
* 只要不是float和绝对定位方式布局的，都在文档流里面

**position属性说明**

* static，默认值。位置设置为static的元素，它始终会处于文档流给予的位置
* inherit，规定应该从父元素继承 position 属性的值
* absolute，生成绝对定位的元素，**相对于距该元素最近的已定位的祖先元素**进行定位
* relative，生成相对定位的元素，**相对于该元素在文档中的初始位置进行定位**

## 5.5.盒子模型

在一个文档中，每一个元素都被抽象成一个盒子，每一个盒子又包括四部分(从内到外):内容(content)，内填充(padding)，边框(border)，外边距(margin)；
