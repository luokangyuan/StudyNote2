# CSS3学习笔记

## 前言

* css的全称是什么？——`casccading style sheets`
* 样式表的组成？——`选择器+声明块`
* 浏览器杜宇编译css的顺序？——`div ul li #test从右往左`

## 选择器

### 基本选择器

* 通配符选择器——`* {margin:0;}`
* 元素选择器——`body {background: #eee}`
* 类选择器——`.list {list-style: square}`
* ID选择器——`#list {width: 500ox}`
* 后代选择器——`.list li {margin-top: 10px}`

#### 子元素选择器

也可以叫后代直接选择器，此类选择器只能匹配到`直接后代`，不能匹配到深层次的后代元素

```css
#wrap > .inner {color: pink;}
```

`Html实例`

```html
<div id="wrap">
    <div>1
        <div>1-1</div>
    </div>
    <div>2</div>
    <div>3</div>
    <div>4</div>
    <div>5</div>
</div>
```

`CSS实例`

```css
#wrap > div{
    color: brown;
    border: 5px solid;
}
```

> 注意：选择的是#wrap下的所有直接后代div,但是color是可继承的，所以1-1颜色也会改变，border却不会。

#### 相邻兄弟选择器

它只会匹配`紧跟`着的兄弟元素

```css
#wrap > #first + .inner{color: #A52A2A;}
```

`Html实例`

```ht
<div id="wrap">
<div class="inner">1</div>
<div id="first">2</div>
<div class="inner">3</div>
<div class="inner">4</div>
<div class="inner">5</div>
</div>
```

> 注意：改变的只是3号div，如果3号之前存在一个`<div></div>`,那么将不会改变

#### 通用兄弟选择器

它会匹配所有的兄弟元素（不需要紧跟）

```css
#wrap #first ~div{border: 1px solid cornflowerblue;}
```

> 注意：HTML结构和相邻兄弟选择器一样，改变的是3,4,5号div

### 属性选择器

#### 存在和值属性选择器

`html结构`

```html
<div id="wrap">
    <div name="zhangsan">1</div>
    <div name="li luo">2</div>
    <div name="li">3</div>
</div>
```

`[attr]`：该选择器选择包含attr属性的所有元素，无论attr的值是什么

```css
div[name]{border: 1px solid blueviolet;} /*选中的是所有div*/
```

`[attr = val]`：该选择器`仅`选择attr属性被赋值val的`所有元素`

```css
div[name = "li"]{border: 1px solid coral;} /*选中的是2,3号div*/
```

`[attr ~= val]`：表示带有以attr命名的属性的元素，并且该属性是以一个空格作为分割的值列表，其中至少一个为val

```css
div[name ~= "luo"]{border: 1px solid coral;} /*选中的是2号div*/
```

#### 子串值属性选择器

`html结构`

```html
<div id="wrap">
    <div name="luo-zhangsan">1</div>
    <div name="li luo">2</div>
    <div name="luo-li">3</div>
    <div name="luoliluo">4</div>
</div>
```

`[attr |= val]`：选择的是attr属性的值是val（包括val）或者以val-开头的元素

```css
div[name |= "luo"]{border: 1px solid coral;} /*匹配的只有1,3号元素*/
```

`[attr ^= val]`：选择的是attr属性的值以val开头（包括val）的元素

```css
div[name ^= "luo"]{border: 1px solid coral;} /*匹配的只有1,4号元素*/
```

`[attr $= val]`：选择的是attr属性的值以val结尾（包括val）的元素

```css
div[name $= "luo"]{border: 1px solid coral;} /*匹配的只有2,4号元素*/
```

`[attr *= val]`：选择的是attr属性的值中包含字符串val的元素

```css
div[name *= "luo"]{border: 1px solid coral;} /*匹配的只有1,2,3,4号元素*/
```

### 伪类与伪元素选择器

#### 链接伪类

`:link`：表示作为超链接，并指向一个为访问的地址的所有锚

`:visited`：表示作为超链接，并指向一个已访问的地址的所有锚

```css
a{text-decoration: none;}
a:link{color: deeppink;}
#test :link{background: pink;}
```
`:target`：代表一个特殊元素，它的id是URL的片段标识符

`:target实例-选项卡,html结构如下`

```html
<body>
    <a href="#div1">div1</a>
    <a href="#div2">div2</a>
    <a href="#div3">div3</a>
    <div id="div1">
        div1
    </div>
    <div id="div2">
        div2
    </div>
    <div id="div3">
        div3
    </div>
</body>
```

`css结构如下`

```css
*{
    margin: 0;
    padding: 0;
}
a{
    text-decoration: none;
    color: deeppink;
}
div{
    width: 200px;
    height: 200px;
    background: pink;
    display: none;
    text-align: center;
    font: 50px/200px "微软雅黑";
}
:target{
    display: block;
}
```


> 注意：`:link`，`:visited`，`:target`是作用与链接元素的

#### 动态伪类

`:hover`：表示悬浮到元素上

`:active`：表示匹配被用户激活的元素（点击按住）

由于a标签的：link和：visited可以覆盖了所有的a标签的状态，所以当：link,:visited,:hover,:active同时出现在a标签身上时，：link和：visited不能放在最后

> 注意：:hover和:active基本可以作用于所有的元素

#### 表单相关伪类

`:enabled`：匹配可编辑的表单
`:disable`：匹配被禁用的表单
`:checked`：匹配被选中的表单
`:focus	`	：匹配获焦的表单

`实例如下`

```css
input:enabled{
    background: deeppink;
}
input:disabled{
    background: blue;
}
input:checked{
    width: 200px;
    height: 200px;
}
input:focus{
    background: darkcyan;
}
```

`html结构如下`

```html
<input type="text" />
<input type="text" disabled="disabled" />
<input type="checkbox" />
```

`小例子-自定义单选按钮`

```html
<label>
    <input type="radio" name="mj" />
    <span></span>
</label>
<label>
    <input type="radio" name="mj" />
    <span></span>
</label>
<label>
    <input type="radio" name="mj" />
    <span></span>
</label>
```

`css代码如下`

```css
*{
    margin: 0;
    padding: 0;
}
label{
    position: relative;
    float: left;
    width: 100px;
    height: 100px;
    border: 2px solid;
    overflow: hidden;
    border-radius: 50%;
}
label > span{
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
}
input{
    position: absolute;
    left: -50px;
    top: -50px;
}
input:checked + span{
    background: pink;
}
```

#### 结构性伪类选择器

`:nth-child(index)系列`

* :first-child
* :last-child
* nth-last-child(index):表示从后面开始计数
* only-child(相当于:first-child:last-child或者:noth-child(1):nth-last-child(1))

`:nth-child(index)实例`

```css
/*找到#warp底下的所有li子元素,并且选中第一个子元素，并且这个子元素必须是li*/
#wrap li:nth-child(1){
    color: deeppink;
}
```

> 注意：1.index的值从1开始计数；2.index可以为变量n(只能是n)；3.index可以为even或者odd

`:nth-of-type系列`

* :first-of-type
* last-of-type
* nth-last-type(index):表示从后面开始计数
* only-of-type

```css
#wrap li:nth-of-type(1){
    color: deeppink;
}
```

> 注意：nth-child(index)和nth-of-type(index)的区别：前者找某某下的第一个适配元素，后者找某某下的出现第一次的适配元素。

`nth-child和nth-of-type的区别（坑）`

`html结构如下`

```html
<div id="warp">
    <div class="inner">div</div>
    <span class="inner">span</span>
    <p class="inner">p</p>
    <h1 class="inner">h1</h1>
    <h2 class="inner">h2</h2>
</div>
```

使用nth-child如下，这个没什么问题，选中的是#warp下的class为.inner的第一个元素div

```css
#warp .inner:nth-child(1){
    color: deeppink;
}
```

使用nth-of-type如下，选中的却是所有元素，这是因为`nth-of-type是以元素为中心`

```css
#warp .inner:nth-of-type(1){
    color: deeppink;
}
```

* not

```css
div > a:not(:last-of-type){
    border-right: 1px solid red;
}
```

* enpty(内容必须是空的，有空格都不行)

#### 伪元素选择器

伪元素包含这几种，`::after`，`::before`，`::firstLetter`，`::firstLine`，`::selection`

`::after实例如下`

```css
#warp::after{
    content: "";
    display: block;
    width: 200px;
    height: 200px;
    background: deeppink;
}
```

`::firstLetter实例如下`:将第一个字改变

```html
<div>我是谁？</div>
```

```css
div::first-letter{
    color: deeppink;
    font-size: 24px;
    font-weight: bold;
}
```

`::firstLine实例如下`：将第一行改变

```html
<div>
    我是谁？<br>
    我来自哪里？<br>
</div>
```

```css
div::first-line{
    color: deeppink;
    font-size: 24px;
    font-weight: bold;
}
```

`::selection实例如下`：改变鼠标选中时的状态

```html
<div>我是谁？我来自哪里？</div>
```

```css
div::selection{
    color: deeppink;
    background: white;
}
```

## 自定义字体

`实例如下`

```css
@font-face {
    font-family:;
    src: url();
}
```

## 新增UI方案

### 文本新增样式

#### `opacity`：改变透明度

```css
#warp{
    width: 300px;
    height: 300px;
    margin: 100px auto;
    background: pink;
    opacity: 0.1;
}
#inner{
    width: 100px;
    height: 100px;
    background: deeppink;
}
```

```html
<div id="warp">
    <div id="inner">
        inner
    </div>
</div>
```

#### `新增颜色模式rgba`

```css
#warp{
    width: 300px;
    height: 300px;
    margin: 100px auto;
    background: rgba(0,0,0,.8);
}
```

> 说明：rgba其实就是rgb颜色加一个透明度

#### `实例，背景透明，文字不透明`

```css
#warp{
    width: 300px;
    height: 300px;
    margin: 100px auto;
    background: rgba(0,0,0,0.8);
    color: #FFFFFF;
    font-size: 30px;
    line-height: 300px;
    text-align: center;
}
```

> 如果是文字透明，背景不透明，将color换成rgba,background使用#形式的颜色模式

#### `文字阴影`

text-shadow用来为文字添加阴影，而且可以添加多层，阴影之间用逗号隔开（多个阴影时，第一个在最上面）

```css
h1{
    text-align: center;
    font: 100px/200px "微软雅黑";
    text-shadow: gray 10px 10px 10px;
}
```

#### `实例-浮雕文字`

```css
h1{
    text-align: center;
    font: 100px/200px "微软雅黑";
    color: white;
    text-shadow: black 1px 1px 10px;
}
```

#### `实例-文字模糊效果`

```css
h1{
    text-align: center;
    font: 100px/200px "微软雅黑";
    color: black;
    transition: 1s;
}
h1:hover{
    color: rgba(0,0,0,0);
    text-shadow: black 0 0 100px;
}
```

#### `实例-模糊背景`

```css
#warp{
    height: 100px;
    background: rgba(0,0,0,.5);
    position: relative;
}
#warp #bg{
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
    background: url(img/2_1.jpg) no-repeat;
    background-size: 100% 100%;
    z-index: -1;
    filter: blur(10px);/*元素模糊*/
}
img{
    margin: 24px 0 0 24px;
}
```

```html
<div id="warp">
    <img src="img/2_1.jpg" width="64" height="64" />
    <div id="bg"></div>
</div>
```

#### `文字描边`

```css
h1{
    font: 100px/200px "微软雅黑";
    text-align: center;
    color: white;
    -webkit-text-stroke: pink 4px;
}
```

#### `文字排版`

实例，省略过长内容显示为...

```css
div{
    width: 200px;
    height: 100px;
    border: 1px solid;
    margin: 0 auto;
    white-space: nowrap;/*不换行*/
    overflow: hidden;/*省略溢出内容*/
    text-overflow: ellipsis;
}
```

> 注意：这个的使用的前提是：不能让元素的大小靠内容撑大，也就是不能使用`display: inline`;属性

### 盒模型新增样式

#### `盒模型阴影`

```css
box-shadow: inset 10px 10px 10px 0px black ;
```

> 说明：box-shadow较text-shadow多了两个参数，第一个是阴影方向，第五个是阴影大小

```css
#warp{
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: auto;
    width: 100px;
    height: 100px;
    background: pink;/*以上所有样式就是让盒子水平和垂直居中*/
    text-align: center;
    line-height: 100px;
    transition: 1s;
}
#warp:hover{
    box-shadow:  10px 10px  10px 0px black ;
}
```

#### `倒影-webkit-box-reflect`

```css
img{
    vertical-align: middle;
    -webkit-box-reflect: right;
}
```

`resize`:允许你控制一个元素的可调整性，需要overflow：auto配合使用

```css
#warp{
    display: inline-block;
    width: 200px;
    height: 200px;
    background: pink;
    resize: both;
    overflow: auto;
}
```

#### `box-sizing`

box-sizing 属性允许您以特定的方式定义匹配某个区域的特定元素， 

```css
#warp > div{
    margin: 10px;
    width:130px ;
    height: 130px;
    background: deeppink;
    float: left;
    border: 1px solid;
    box-sizing: border-box;
}
```

在上面的css代码中使用 `float: left`;如果要使用 `border: 1px solid`;就必须添加` box-sizing: border-box`;才不会改变布局

### 新增UI样式

#### `圆角`

```css
#warp{
    position: absolute;
    height: 70px;
    width: 200px;
    border: 1px solid;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: auto;/*以上都是让元素水平和垂直居中的方法*/
    border-radius: 30px;
}
```

> border-radius: 30px;这种是简写方式，border-radius: 30px 20px 10px 40px; 是分别对应四角的写法

> 注意：`圆角最好使用px值，不要使用百分比`

#### `圆角实例-旋转的风车`

```css
*{
    margin: 0;
    padding: 0;
}
html,body{
    height: 100%;
    overflow: hidden;/*这两个是禁止滚动条*/
}
#warp{
    position: absolute;
    height: 300px;
    width: 300px;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: auto;/*以上都是让元素水平和垂直居中的方法*/
    transition: 2s;
}
#warp > div{
    margin: 10px;
    width:130px ;
    height: 130px;
    background: deeppink;
    float: left;
    border: 1px solid;
    box-sizing: border-box;
}
#warp > div:nth-child(1),#warp > div:nth-child(4){
    border-radius: 0 60%;
}
#warp > div:nth-child(2),#warp > div:nth-child(3){
    border-radius: 60% 0;
}
#warp:hover{
    transform: rotate(120deg);/*旋转函数*/
}
```

```html
<div id="warp">
    <div></div>
    <div></div>
    <div></div>
    <div></div>
</div>
```

#### `边框图片`

```css
#warp{
    position: absolute;
    height: 200px;
    width: 200px;
    border: 1px solid;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    margin: auto;/*以上都是让元素水平和垂直居中的方法*/
    border: 50px solid;
    border-image-source: url(img/border-image.png);
    border-image-slice: 33.3333%;
    border-image-repeat: round;
    border-image-width: 20px;
}
```

```html
<div id="warp"></div>
```

#### `线性渐变`

`双颜色值的线性渐变`

```css
background-image:linear-gradient(red,blue);
```

`多颜色值的线性渐变`

```css
background-image:linear-gradient(red,blue,pink,black);
```

`改变渐变方向`

```css
background-image:linear-gradient(to top left,red,blue);
```

`使用角度`

```css
background-image:linear-gradient(0deg,red,blue);
```

`控制颜色节点的分布`

```css
background-image:linear-gradient(90deg,red 10%,orange 15%,yellow 20%,green 30%,blue 50%,indigo 70%,violet 80%);
```

`透明度的渐变`

```css
background-image:linear-gradient(90deg,rgba(255,0,0,0) 50%,rgba(255,0,0,0.5),rgba(255,0,0,1) 60%);
```

`重复渐变`

```css
background: repeating-linear-gradient(90deg,red 10%,blue 30%);
```

#### 渐变实例-发廊灯

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			*{
				margin: 0;
				padding: 0;
			}
			html,body{
				height: 100%;
				overflow: hidden;
			}
			#warp{
				height: 300px;
				width: 40px;
				border: 1px solid;
				margin: 100px auto;
				overflow: hidden;
			}
			#warp > .inner{
				height: 600px;/*这是是颜色所在的高度。远比能看见的要高*/
				background: repeating-linear-gradient(135deg,black 0px,black 10px,white 10px,white 20px);
			}
			#warp:hover .inner{
				margin-top: -300px;
			}
			
		</style>
	</head>
	<body>
		<div id="warp">
			<div class="inner"></div>
		</div>
	</body>
	<script type="text/javascript">
		var inner  = document.querySelector("#warp > .inner");/*获取元素*/
		var flag = 0;/*循环结束后归零*/
		setInterval(function(){
			flag++;
			if(flag == 300){
				flag = 0;
			}
			inner.style.marginTop = -flag+"px";
			
		},1000/60)/*设置定时器循环*/
	</script>
</html>
```

#### 渐变实例-光斑动画

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			*{
				margin: 0;
				padding: 0;
			}
			html,body{
				height: 100%;
				overflow: hidden;
				background: black;
				text-align: center;/*配合h1的display: inline-block;属性让h1这个元素居中*/
			}
			h1{
				display: inline-block;
				color: rgba(255,255,255,.3);
				font: bold 80px "微软雅黑";
				background: linear-gradient(120deg,rgba(255,255,255,0) 100px,rgba(255,255,255,1) 180px,rgba(255,255,255,0) 260px);
				background-repeat: no-repeat;
				-webkit-background-clip: text;
			}
		</style>
	</head>
	<body>
		<h1>码酱博客-专注与总结分享</h1>
	</body>
	<script type="text/javascript">
		var h1  = document.querySelector("h1");/*获取元素*/
		var flag = -160;/*循环结束后归零*/
		setInterval(function(){
			flag+=10;
			if(flag == 900){
				flag = -160;
			}
			h1.style.backgroundPosition = flag+"px";
			
		},30)/*设置定时器循环*/
	</script>
</html>
```

#### 径向渐变

`双颜色值的径向渐变`

```css
background-image:radial-gradient(red,blue);
```

`多颜色值的径向渐变`

```css
background-image: radial-gradient(red,blue,pink,black);
```

`不均匀分布`

```css
background-image:radial-gradient(red 50%,blue 70%);
```

`改变渐变形状`

```css
background-image:radial-gradient(ellipse,red,blue);
```

`渐变形状的尺寸大小`

```css
background-image:radial-gradient(farthest-corner  ellipse,red,blue);
```

`改变圆心`

```css
background-image:radial-gradient(closest-corner  circle at 10px 10px,red,blue);
```

`重复渐变`

```css
background-image:repeating-radial-gradient(closest-corner  circle,red 30%,blue 50%);
```

## 过渡transition

 CSS transition 提供了一种在更改CSS属性时控制动画速度的方法。 其可以让属性变化成为一个持续一段时间的过程，而不是立即生效的。比如，将一个元素的颜色从白色改为黑色，通常这个改变是立即生效的，使用 CSS transitions 后该元素的颜色将逐渐从白色变为黑色，按照一定的曲线速率变化。这个过程可以自定义

 transition是一个简写属性，用于 transition-property,transition-duration,transition-timing-function, 和transition-delay。 

> 注意：在transition属性中，各个值的书写顺序是很重要的：第一个可以解析为时间的值会被赋值给transition-duration，第二个可以解析为时间的值会被赋值给transition-delay


`transition`分为一下属性：`transition-property `，`transition-duration`，`transition-timing-function`，`transition-delay`

### transition-property

指定应用过渡属性的名称，默认值为 all，表示所有可被动画的属性都表现出过渡动,可以指定`多个 property `

> 注意：不是所有的属性都可以添加动画过渡的，过渡时间必须带单位s秒

那些属性可以添加动画过渡，[参看这个连接](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_animated_properties)

默认值：

*  none： 没有过渡动画。
*  all ：所有可被动画的属性都表现出过渡动画。

### transition-duration

属性以秒或毫秒为单位指定过渡动画所需的时间。默认值为 `0s` (`一定要带单位`)，表示不出现过渡动画。

可以指定多个时长，每个时长会被应用到由 transition-property 指定的对应属性上。如果指定的时长个数小于属性个数，那么时长列表会重复。如果时长列表更长，那么该列表会被裁减。两种情况下，属性列表都保持不变。

默认值：

* 属性值： 以毫秒或秒为单位的数值` <time> 类型`。表示过渡属性从旧的值转变到新的值所需要的时间。


> 如果时长是 0s ，表示不会呈现过渡动画，属性会瞬间完成转变。不接受负值。一定要加单位(不能写0 一定要写0s  1s,0s,1s)！

### transition-timing-function

CSS属性受到 transition的影响，会产生不断变化的中间值，而 CSS transition-timing-function 属性用来描述这个中间值是怎样计算的。实质上，通过这个函数会建立一条加速度曲线，因此在整个transition变化过程中，变化速度可以不断改变

属性值：

* ease：（加速然后减速）默认值，ease函数等同于贝塞尔曲线(0.25, 0.1, 0.25, 1.0)
* linear：（匀速），linear 函数等同于贝塞尔曲线(0.0, 0.0, 1.0, 1.0)
* ease-in：(加速)，ease-in 函数等同于贝塞尔曲线(0.42, 0, 1.0, 1.0)
* ease-out：（减速），ease-out 函数等同于贝塞尔曲线(0, 0, 0.58, 1.0)
* ease-in-out：（加速然后减速），ease-in-out 函数等同于贝塞尔曲线(0.42, 0, 0.58, 1.0)
* cubic-bezier： 贝塞尔曲线
* step-start：等同于steps(1,start)
* step-end：等同于steps(1,end)
  * `steps(<integer>,[,[start|end]]?)`
  * 第一个参数：必须为正整数，指定函数的步数
  * 第二个参数：指定每一步的值发生变化的时间点（默认值end）

### transition-delay

规定了在过渡效果开始作用之前需要等待的时间。默认值：0s;

你可以指定多个延迟时间，每个延迟将会分别作用于你所指定的相符合的css属性。如果指定的时长个数小于属性个数，那么时长列表会重复。如果时长列表更长，那么该列表会被裁减。两种情况下，属性列表都保持不变

 值以秒（s）或毫秒（ms）为单位，表明动画过渡效果将在何时开始。`取值为正时会延迟一段时间来响应过渡效果；取值为负时会导致过渡立即开始`

### 当属性值的列表长度不一致时

```css
transition-property: background,width,height;
transition-duration: 3s,2s;
transition-delay:3s,2s;
transition-timing-function:linear;
```

`实际效果如下`

```css
transition-property: background,width,height;
transition-duration: 3s,2s,3s;
transition-delay:3s,2s,3s;
transition-timing-function:linear,ease,ease;
```

> 说明：
>
> 1.超出的情况下是会被全部截掉的
> 2.不够的时候，关于时间的会重复列表，transition-timing-function的时候使用的是默认值ease

## 2D变换（变形）transform

transform 属性允许你修改CSS视觉格式模型的坐标空间，transform 属性 , `只对 block 级元素生效`！

* 旋转（rotate）
* 平移（translate）
* 倾斜（skew）
* 缩放（scale）
* 基点的变换
* 矩阵（matrix）
  * 旋转
  * 平移
  * 倾斜
  * 缩放

### `旋转（rotate）`

```css
transform:rotate(angle);   
```

正值:顺时针旋转  rotate(360deg),  负值:逆时针旋转  rotate(-360deg)

>  只能设单值。正数表示顺时针旋转，负数表示逆时针旋转

### `平移（translate）`

X方向平移:`transform:  translateX(tx)`，Y方向平移:`transform:  translateY(ty) `
二维平移：transform:  translate(tx, ty)； 如果ty没有指定，它的值默认为0。

> 说明：可设单值，可设双值，正数表示XY轴正向位移，负数为反向位移。设单值表示只X轴位移，Y轴坐标不变，

```css
transform: translate(100px,200px);// 斜着移动
transform: translate(100px,0);
```

### `倾斜（skew）`

X方向倾斜:transform:  skewX(angle)

```css
transform:  skewX(45deg) // 参数值以deg为单位 代表与y轴之间的角度
```

 Y方向倾斜:transform:  skewY(angle)

```css
transform:  skewY(45deg) //  参数值以deg为单位 代表与x轴之间的角度
```

 二维倾斜:transform:  skew(ax[, ay]);  如果ay未提供，在Y轴上没有倾斜

```css
transform:skew(45deg,15deg);//  第一个参数代表与y轴之间的角度, 第二个参数代表与x轴之间的角度
```

### `缩放（scale）`

X方向缩放:transform:  scaleX(sx); 

```css
transform: scaleX(2);
```

Y方向缩放:transform:  scaleY(sy);

```css
transform: scaleY(.5);
```

二维缩放 :transform:  scale(sx[, sy]);  (如果sy 未指定，默认认为和sx的值相同)  

```css
transform: scale(2,.5);
```

要缩小请设0.01～0.99之间的值，要放大请设超过1的值。单值时表示只X轴,Y轴上缩放粒度一样，如transform: scale(2);等价于transform: scale(2,2);

> 注意：以上的变换都是基于中心原点变换，改变基点，使用`transform-origin: 100% 100%;`属性

### `基点变换transform-origin`

transform-origin CSS属性让你更改一个元素变形的基点。

### `矩阵（matrix）`

在 2D变换 中，矩阵变换函数 matrix() 接受 6个值，语法形式如下：` transform: matrix(a, b, c, d, e, f);   `

* 对某一元素应用旋转变换 rotate(θ)，使用矩阵实现 `matrix(cosθ, sinθ, -sinθ, cosθ, 0, 0)`
* 对某一元素应用旋转变换 translate(X, Y)，使用矩阵实现： `matrix(1, 0, 0, 1, X, Y)`
* 对某一元素应用倾斜变换 skew(α, β)，使用矩阵变换函数 `matrix(1, tanβ, tanα,1, 0, 0)。`
* 对某一元素应用缩放变换 scale(scaleX, scaleY)，使用矩阵变换函数 `matrix(scaleX, 0, 0, scaleY, 0, 0)`

## 3D变形

在浏览器中，X轴是从左到右，Y轴是从上到下，Z轴是从里到外

### `3D缩放`

`transform: scale3d(scaleX,scaleY,scaleZ);`或者`transform: scaleZ(number)`

如果只设置scaleZ(number)，你会发现元素并没有被扩大或压缩，scaleZ(number)需要和translateZ(length)配合使用，number乘以length得到的值，是元素沿Z轴移动的距离，从而使得感觉被扩大或压缩 

### `3D旋转`

CSS3中的3D旋转主要包括四个功能函数：`rotateX(angle)`、 `rotateY(angle)`、` rotateZ(angle)`、`rotate3d(x,y,z,angle)`

x, y, z分别接受一个数值(number),用来计算矢量方向(direction vector)，矢量方向是三维空间中的一条线, 从坐标系原点到x, y, z值确定的那个点，元素围绕这条线旋转angle指定的值

### `3D平移`

`transform: translateZ(length)`是3D Transformaton特有的，其他两个2D中就有

>  translateZ  它不能是百分比值; 那样的移动是没有意义的。

`transform: translate3d(translateX,translateY,translateZ)`;

> translateZ  它不能是百分比值; 那样的移动是没有意义的。

## 景深（perspective）

## 动画（Animation）

css3动画就是使元素从一种样式逐渐变化为另一种样式的效果，animation属性是一个简写属性形式: （可以用来描述可动画的属性） [可动画属性的列表]( https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties )

> 在每个动画定义中，顺序很重要：可以被解析为 <time>的第一个值被分配给animation-duration, 第二个分配给 animation-delay。

### 关键帧（@keyframes）

语法如下：

```css
 @keyframes animiationName{
     keyframes-selector{
         css-style;
     }
}
```

>  animiationName:必写项，定义动画的名称, keyframes-selector：必写项，动画持续时间的百分比，  from：0%
> to：100%， css-style：css声明

示例如下：

```css
#wran{
    animation-name :move
}
@keyframes move{
    from{
        transform:rotate(0deg);
    }
    to{
        transform:rotate(360deg);
    }
}
```

### `animation-name`

animation-name属性指定应用的一系列动画，每个名称代表一个由@keyframes定义的动画序列

### `animation-duration`

animation-duration属性指定一个动画周期的时长。默认值为0s，表示无动画。一个动画周期的时长，单位为秒(s)或者毫秒(ms)，无单位值无效。

> 注意：负值无效，浏览器会忽略该声明，但是一些早起的带前缀的声明会将负值当作0s

### `animation-timing-function`

animation-timing-function属性定义CSS动画在每一动画周期中执行的节奏。 对于关键帧动画来说，timing function作用于一个关键帧周期而非整个动画周期，即从关键帧开始，到关键帧结束。动画的默认效果：`由慢变快再变慢`

* linear:线性过渡，等同于贝塞尔曲线（0,0,1,1）
* ease:平滑过渡，等同于贝塞尔曲线（0.25,0.1,0.25,1.0）
* ease-in:由慢到快，等同于贝塞尔曲线（0.42,0,1,1）
* ease-out:由快到慢，等同于贝塞尔曲线（0,0,0.58,1）
* ease-in-out:由慢到快再到慢，等同于贝塞尔曲线（0.42,0,0.58,1）
* cubic-bezier(1,1,2,3)
* steps(n,[start|end])
  * 传入一到两个参数，第一个参数意思是把动画分成 n 等分，然后动画就会平均地运行。
  *  第二个参数 start 表示从动画的开头开始运行，相反，end 就表示从动画的结尾开始运行， 默认值为 end。

### ` animation-delay`

定义动画开始前等待的时间,以秒或毫秒计(`属于动画外的范畴`),值为time, 从动画样式应用到元素上到元素开始执行动画的时间差。该值可用单位为秒(s)和毫秒(ms)。如果未设置单位，定义无效

### ` animation-iteration-count`

定义了动画执行的次数（`属于动画内的范畴`）,值为`infinite`表示 无限循环播放动画.`<number>`表示动画播放的次数 `不可为负值. `

### `animation-direction`

定义了动画执行的方向

* normal： 每个循环内动画向前循环，换言之，每个动画循环结束，动画重置到起点重新开始，默认属性。
*  alternate：动画交替反向运行，反向运行时，动画按步后退，同时，带时间功能的函数也反向， 比如，ease-in 在反向时成为ease-out。计数取决于开始时是奇数迭代还是偶数迭       代
* reverse：反向运行动画，每周期结束动画由尾到头运行。
* alternate-reverse：反向交替， 反向开始交替

### `animation-fill-mode`

属于动画外的范畴，定义动画在`动画外的状态`

### `animation-play-state`

定义了动画执行的运行和暂停， `running`表示当前动画正在运行。`paused`表示当前动画以被停止。

## 布局扩展

### 老版本布局

CSS3 弹性盒子(Flexible Box 或 Flexbox)，是一种用于在页面上布置元素的布局模式，使得当页面布局必须适应不同的屏幕尺寸和不同的显示设备时，元素可预测地运行/列。对于许多应用程序，弹性盒子模型提供了对块模型的改进，因为它不使用浮动，flex容器的边缘也不会与其内容的边缘折叠。 老版本的我们通常称之为box， 新版本的我们通常称之为flex

> `注意`：项目永远在主轴的正方向排列

#### 老版容器的布局方向

* -webkit-box-orient: horizontal; x轴
* -webkit-box-orient: vertical; y轴

```css
display: -webkit-box;
/*-webkit-box-orient属性控制的主轴是那个*/
-webkit-box-orient: vertical;
```

> `注意`：项目永远在主轴的正方向排列

#### 老版容器的排列方向

* -webkit-box-direction: normal;
* -webkit-box-direction: reverse;

` -webkit-box-direction`属性本质上改变了主轴的方向

```css
display: -webkit-box;
-webkit-box-orient: vertical;
/*-webkit-box-direction控制主轴的方向*/
-webkit-box-direction: reverse;
```

#### 老版本富裕空间管理（主轴上）

```css
 -webkit-box-pack:start; 不会给项目区分配空间，只是确定富裕空间的位置 
```

* `start`：表示富裕空间在右边
* `end`：表示富裕空间在左边
* `center`：表示富裕空间在两边
* ` justify`：表示富裕空间在项目之间

#### 老版本富裕空间管理（侧轴上）

```css
 -webkit-box-align:center; /*不会给项目区分配空间，只是确定富裕空间的位置*/
```

* `start`：侧轴为x轴，富裕空间位于右边，侧轴为y轴，富裕空间位于下边
* `end`：侧轴为x轴，富裕空间位于左边，侧轴为y轴，富裕空间位于上边
* `center` ：富裕空间位于两边

### 新版本flex布局

#### 新版本的布局方向

* flex-direction: row;
* flex-direction: column;

```css
display: flex;
/*flex-direction决定主轴方向*/
flex-direction: row;
```

#### 新版本的排列方向

* flex-direction:row-reverse;
* flex-direction:column-reverse;

```css
display: flex;
/*flex-direction既控制主轴是那个，也控制主轴方向*/
flex-direction: row-reverse
```

#### 新版本富裕空间管理（主轴上）

```css
justify-content: flex-start; /*更强大的富裕空间的管理（主轴）*/
```

* `flex-start`：富裕空间在`主轴的正方向`
* `flex-end`：富裕空间在`主轴的反方向`
* `center`：富裕空间在主轴的两边
* `space-between`：富裕空间在项目之间
* `space-around(box 没有的)`：富裕空间在项目两边

#### 新版本富裕空间管理（侧轴上）

```css
align-items: stretch;
```

*  `flex-start`：富裕空间在`侧轴的正方向`
* `flex-end`：富裕空间在`侧轴的反方向`
* `center`：富裕空间在`侧轴的两边`
* `baseline`(box 没有的)：按照基线对齐
* ` stretch`(box 没有的)：等高布局

`有关布局的HTML结构如下`

```html
<div id="warp">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
    <div class="item">5</div>
</div>
```

> 说明：id="warp"为容器，`<div class="item">1</div>`为项目

### flex布局总结

* 明确什么是容器，什么是项目，什么是主轴，什么是侧轴
* 项目永远排列在主轴上
* flex布局分为两个版本：`-webkit-box-`和`flex`

#### 老版本

##### 容器

* 容器的布局方向：`-webkit-box-orient:horizontal/vertical`,控制主轴是哪一根，
  * horizontal：x轴，vertical  ：y轴
* 容器的排列方向：`-webkit-box-direction：normal/reverse`控制主轴的方向，
  * normal：从左往右（正方向）reverse：从右往左（反方向）
* 富裕空间的管理：`只决定富裕空间的位置，不会给项目区分配空间`
  * 主轴：`-webkit-box-pack`
    * 主轴是x轴
      * start：在右边
      	 end:	在左边
      * center：在两边
      * justify：在项目之间
    * 主轴是y轴
      * start：在下边
      * end：在上边
      * center：在两边
      * justify：在项目之间
  * 侧轴：`-webkit-box-algin`
    * 侧轴是x轴
      * start：在右边
      * end：  在左边
      * center：在两边
    * 侧轴是y轴
      * start：在下边
      	 end：   在上边	
      * center：在两边

##### 项目

* 弹性空间管理：`-webkit-box-flex`：弹性因子（默认值为0）

#### 新版本

##### 容器

容器的布局方向和容器的排列方向使用一个属性`flex-direction`,控制主轴是哪一根，控制主轴的方向

* row;		从左往右的x轴	
* row-reverse;从右往左的x轴
	 column;		从上往下的y轴
* column-reverse;从下往上的y轴

富裕空间的管理：只决定富裕空间的位置，不会给项目区分配空间

* 主轴`justify-content`
  * flex-start：		在主轴的正方向
  	 flex-end:		在主轴的反方向
  	 center：			在两边
  	 space-between：	在项目之间
  * space-around：  在项目两边
* 侧轴`align-items`
  * flex-start：在侧轴的正方向
  * flex-end：    在侧轴的反方向
  	 center：		在两边
  * baseline    基线对齐
  	 stretch		等高布局（项目没有高度）

### 新版flex布局详解

#### 容器

* flex-wrap：控制的是侧轴的方向

```css
align-items: flex-start;/*对单行单列的富裕空间管理*/
flex-wrap: wrap-reverse;/*当容器的宽度小于项目的宽度时控制是否换行显示，也就是控制侧轴的方向*/
align-content: flex-start;/*对多行多列富裕空间的管理*/
```

> `align-content` 属性定义弹性容器的侧轴方向上有额外空间时，如何排布每一行/列。当弹性容器只有一行/列时无作用

align-content值如下：

* `flex-start`：所有行/列从侧轴起点开始填充。第一行/列的侧轴起点边和容器的侧轴起点边对齐。 接下来的每一行/列紧跟前一行/列。
* `flex-end`：所有弹性元素从侧轴末尾开始填充。最后一个弹性元素的侧轴终点和容器的侧轴终点对齐。同时所有后续元素与前一个对齐。
* `center`：所有行/列朝向容器的中心填充。每行/列互相紧挨，相对于容器居中对齐。 容器的侧轴起点边和第一行/列的距离相等于容器的侧轴终点边和最后一行/列的距离。
* `space-between`：所有行/列在容器中平均分布。相邻两行/列间距相等。 容器的侧轴起点边和终点边分别与第一行/列和最后一行/列的边对齐。
* `space-around`：所有行/列在容器中平均分布，相邻两行/列间距相等。容器的侧轴起点边和终点边分别与第一行/列和最后一行/列的距离是相邻两行/列间距的一半。
* `stretch`：拉伸所有行/列来填满剩余空间。剩余空间平均的分配给每一行/列       

`flex-flow 属性`

flex-flow 属性是设置“flex-direction”和“flex-wrap”的简写,默认值：row nowrap    不可继承

`控制主轴和侧轴的位置以及方向`

#### 项目

`order 属性`

order 属性规定了弹性容器中的可伸缩项目在布局时的顺序。元素按照 order 属性的值的增序进行布局。拥有相同 order 属性值的元素按照它们在源代码中出现的顺序进行布局,`order越大越后`

```css
#warp > .item:nth-child(1){
    order: 5;
}
#warp > .item:nth-child(2){
    order: 3;
}
```

`align-self 属性`

align-self 会对齐当前 flex 行中的 flex 元素，并覆盖 align-items 的值. 如果任何 flex 元素的侧轴方向 margin 值设置为 auto，则会忽略 align-self。

* auto：设置为父元素的 align-items 值，如果该元素没有父元素的话，就设置为 stretch。
* flex-start：flex 元素会对齐到 cross-axis 的首端。
* flex-end：flex 元素会对齐到 cross-axis 的尾端。
* center： flex 元素会对齐到 cross-axis 的中间，如果该元素的 cross-size 的尺寸大于 flex 容器，将在两个方向均等溢出。
* baseline：所有的 flex 元素会沿着基线对齐，
* stretch：flex 元素将会基于容器的宽和高，按照自身 margin box 的 cross-size 拉伸

```css
#warp > .item:nth-child(2){
    order: 3;
    align-self: flex-end;
}
#warp > .item:nth-child(3){
    order: 2;
    align-self: center;
}
```

`flex-shrink属性`

 flex-grow 属性定义弹性盒子项（flex item）的拉伸因子。

*   可用空间 = (容器大小 - 所有相邻项目flex-basis的总和)
* 可扩展空间 = (可用空间/所有相邻项目flex-grow的总和)
* 每项伸缩大小 = (伸缩基准值 + (可扩展空间 x flex-grow值))

 flex-shrink 属性指定了 flex 元素的收缩因子  默认值为1

*  计算收缩因子与基准值乘的总和  
* 计算收缩因数： 收缩因数=（项目的收缩因子*项目基准值）/第一步计算总和    
* 移除空间的计算：移除空间= 项目收缩因数 x 负溢出的空间   

`flex-basis属性`

flex-basis 指定了 flex 元素在主轴方向上的初始大小，默认值 ：auto  不可继承

>  注意：  在flex简写属性中 flex-basis的默认值为0

 `flex实例等分布局`

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			*{
				margin: 0;
				padding: 0;
			}
			#warp{
				width:500px;
				height: 300px;
				border: 1px solid;
				margin: 100px auto;
				display: flex;
			}
			#warp > .item{
				height: 50px;
				background: pink;
				text-align: center;
				line-height: 50px;
                flex-shrink: 1;
				flex-grow: 1;/*将富裕空间等分，并没有将项目等分*/
				flex-basis: 0;
			}
			
			
		</style>
	</head>
	<body>
		<div id="warp">
			<div class="item">1</div>
			<div class="item">22</div>
			<div class="item">333</div>
			<div class="item">4444</div>
			<div class="item">55555</div>
		</div>
	</body>
</html>
```

`flex简写属性语法糖`

```css
flex-shrink: 1;
flex-grow: 1;/*将富裕空间等分，并没有将项目等分*/
flex-basis: 0;
/*上述三个属性和下列属性语法一样*/
flex: 1;
```

`等分布局实例，天猫导航栏`

```html
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			*{
				margin: 0;
				padding: 0;
			}
			a{
				text-decoration: none;
				color: gray;
				display: block;/*设置a标签不仅仅是可以点击文字，还可以点击div块*/
			}
			#nav .row{
				display: flex;
			}
			#nav >.row > .item{
				flex: 1;
				text-align: center;
			}
			#nav > .row > .item > a:before{
				content: "";
				display: block;
				width: 86px;
				height: 86px;
				margin: 0 auto;
			}
			#nav >.row:nth-child(1) > .item:nth-child(1) > a:before{
				background: url(../img/01.png) no-repeat;
			}
			#nav >.row:nth-child(1) > .item:nth-child(2) > a:before{
				background: url(../img/02.png) no-repeat;
			}
			#nav >.row:nth-child(1) > .item:nth-child(3) > a:before{
				background: url(../img/03.png) no-repeat;
			}
			#nav >.row:nth-child(1) > .item:nth-child(4) > a:before{
				background: url(../img/04.png) no-repeat;
			}
			#nav >.row:nth-child(1) > .item:nth-child(5) > a:before{
				background: url(../img/05.png) no-repeat;
			}
			#nav >.row:nth-child(2) > .item:nth-child(1) > a:before{
				background: url(../img/06.png) no-repeat;
			}
			#nav >.row:nth-child(2) > .item:nth-child(2) > a:before{
				background: url(../img/07.png) no-repeat;
			}
			#nav >.row:nth-child(2) > .item:nth-child(3) > a:before{
				background: url(../img/08.png) no-repeat;
			}
			#nav >.row:nth-child(2) > .item:nth-child(4) > a:before{
				background: url(../img/09.png) no-repeat;
			}
			#nav >.row:nth-child(2) > .item:nth-child(5) > a:before{
				background: url(../img/10.png) no-repeat;
			}
		</style>
	</head>
	<body>
		<div id="nav">
			<div class="row">
				<div class="item">
					<a href="javascript;">天猫</a>
				</div>
				<div class="item">
					<a href="javascript;">聚划算</a>
				</div>
				<div class="item">
					<a href="javascript;">天猫国际</a>
				</div>
				<div class="item">
					<a href="javascript;">外卖</a>
				</div>
				<div class="item">
					<a href="javascript;">天猫超时</a>
				</div>
			</div>
			<div class="row">
				<div class="item">
					<a href="javascript;">充值中心</a>
				</div>
				<div class="item">
					<a href="javascript;">天猫旅行</a>
				</div>
				<div class="item">
					<a href="javascript;">领金币</a>
				</div>
				<div class="item">
					<a href="javascript;">拍卖</a>
				</div>
				<div class="item">
					<a href="javascript;">分类</a>
				</div>
			</div>
		</div>
	</body>
</html>
```

