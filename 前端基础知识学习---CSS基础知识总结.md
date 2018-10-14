# 一、CSS基础知识总结

## 1.CSS基础知识脑图总结



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

