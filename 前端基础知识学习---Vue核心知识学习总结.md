[TOC]

# 一、Vue核心知识

## 1.1.Vue的基本简介

学习一门技术首先登陆其官网，[中文网址](https://cn.vuejs.org/)，[英文网址](https://vuejs.org/)，`vue`是一款渐进式JavaScript框架，作用是为了动态构建用户界面，该框架遵循MVVM模式，编码简洁，体积小，运行效率高；他借鉴了angular的`模板`和`数据绑定技术`，借鉴了react的`组件化`和`虚拟DOM技术`，当然，该技术也存在一个Vue全家桶，例如vue脚手架：`vue-cli`，ajax请求：`vue-resource`，路由：`vue-router`，状态管理：`vuex`，图片懒加载：`vue-lazyload`，移动端UI组件库：`min-ui`，PC端组件库：`element-ui`，页面滑动：`vue-scroller`等等插件；

## 1.2.Vue的基本使用

```html
<div id="app">
    <input type="text" v-model="username">
    <p>Hello {{username}}</p>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    //创建Vue实例
    const vm = new Vue({ // 配置对象
        el:'#app', // element:选择器
        data:{ //数据（Model）
            username:'世界'
        }
    })
</script>
```

`vue的HelloWord编码说明`

*   使用vue首先引入Vue.js，然后创建Vue对象，其中el表示指定的`根element选择器`，data是指初始化数据，双向数据绑定使用`v-model`，显示数据使用语法：`{{xxx}}`；
*   vue的MVVM的体现就是：`model`代表模型，上述代码就是数据对象（data）,`view`代表视图，就是vue中的模板页面，`viewModel`代表是视图模型（vue实例）；

## 1.3.模板语法

所谓的模板就是动态的Html页面，包含了一些JS语法代码，在Vue中使用`双大括号表达式`和`指令`（以v-开头的自定义标签属性）；

`双大括号表达式`

语法是：`{{xxx}}`，作用就是向页面输出数据，可以调用对象的方法，例如`{{msg.toUpperCase()}}`；

`指令：强制数据绑定`

```html
<body>
  <div id="app">
      <p>{{msg}}</p>
      <p>{{msg.toUpperCase()}}</p> 
      <img src="imgSrc" alt="">
  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Word',
            imgSrc: 'http://image.luokangyuan.com/1.jpg'
        }
    })
</script>
</body>
```

上述代码中的img标签的src属性不会获取到data中定义的imgSrc属性的值，这个时候就需要使用指令强制数据绑定，功能就是`指定变化的属性值`，完整写法是：`v-bind:src='imgSrc'`，一般采用简洁写法：`:src='imgSrc'`；正确写法如下：

```html
 <img :src="imgSrc" alt="">
```

`指令：绑定事件监听`

```html
<body>
  <div id="app">
      <button v-on:click = 'test1'>test1</button>
      <button v-on:click = 'test2(msg)'>test2</button>
      <button @click = 'test'>test</button>
  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello Word',
            imgSrc: 'http://image.luokangyuan.com/1.jpg'
        },
        methods: {
            test1() {
                alert(123)
            },
          	test2(content) {
                alert(content)
            },
            test() {
                alert(123)
            }
        }
    })
</script>
</body>
```

绑定事件监听指令的作用就是绑定指定事件名的回调函数，完整写法：`v-on:click='xxx'`或者`v-on:click='xxx(参数)'`再或者`v-on:click.enter='xxx'`，简洁写法就是：`@click='xxx'`，使用`@`符号；

## 1.4.计算属性和监视

`计算属性`

在computed属性对象中定义计算属性的方法，在页面中使用`{{方法名}}`来显示计算的结果；

```html
<body>
  <div id="app">
      姓：<input type="text" placeholder="姓氏"  v-model="firstName"><br>
      名：<input type="text" placeholder="名字" v-model="lastName"><br>
      姓名1（单向）：<input type="text" placeholder="姓名1" v-model="fullName1"><br>
      姓名2（单向）：<input type="text" placeholder="姓名2"><br>
      姓名3（双向）<input type="text" placeholder="姓名3双向">

  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        data: {
            firstName: 'A',
            lastName: 'B'
            //如果将 fullName1写在这个地方，那么改变firstName和lastName的值并不会同时改变fullName1的值
            // 所以需要使用到计算属性，将fullName1写入computed属性对象中，将方法的返回值作为输出值
            // fullName1：'A B' 
        },
        computed: {
            // 这个f方法在初始化会执行，当相关属性发生改变时也会执行
            fullName1() { // 计算属性中的一个方法，方法的返回值作为属性值
                return this.firstName + ' ' + this.lastName
            }
        }
    })
</script>
</body>
```

`计算属性的get和set`

使用计算属性实现上述的双向绑定，代码如下：

```html
<body>
  <div id="app">
      姓：<input type="text" placeholder="姓氏"  v-model="firstName"><br>
      名：<input type="text" placeholder="名字" v-model="lastName"><br>
      姓名3（双向）<input type="text" placeholder="姓名3双向" v-model="fullName3">

  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        data: {
            firstName: 'A',
            lastName: 'B'
            // fullName1：'A B' 
        },
        computed: {
            fullName3: {
                // 回调函数：你定义的，你没有调用，但最终他执行了
                 // 回调函数，当需要读取当前属性值时回调，根据相关的数据计算并返回当前属性的值
                get(){
                    return this.firstName + ' ' + this.lastName
                },
                // 回调函数，监视当前属性的变化，当属性值发生改变时回调，更新相关的属性数据
                set(value){
                    const names = value.split(' ');
                    this.firstName = names[0];
                    this.lastName = names[1];
                }
            }
        }
    })
</script>
</body>
```

>   注意：计算属性存在缓存，多次读取只执行一次getter计算；

`监视`

通过vm对象的`$watch()方法或者watch配置`来监视某一个属性的值是否发生变化，当属性发生变化时，通过执行回调函数来执行相关的功能，下面的代码是使用计算属性完成的同一个功能，

```html
<body>
  <div id="app">
      姓：<input type="text" placeholder="姓氏"  v-model="firstName"><br>
      名：<input type="text" placeholder="名字" v-model="lastName"><br>
      姓名2（单向）：<input type="text" placeholder="姓名2" v-model="fullName2"><br>
      姓名3（双向）<input type="text" placeholder="姓名3双向">

  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
   var vm =  new Vue({
        el: '#app',
        data: {
            firstName: 'A',
            lastName: 'B',
            fullName2: 'A B'
        },
        watch: {
            // 这个方法就是监视firstName，值发生改变是被调用执行函数
            // 函数可以传入两个参数代表新值和改变之前的值，也可以传一个，也可以不传
            firstName: function(value){ 
                this.fullName2 = value+ ' ' + this.lastName
            }
        }
    })
    vm.$watch('lastName',function(value){
        this.fullName2 = this.firstName + ' ' + value
    })
</script>
</body>
```

## 1.5.class和style绑定

在某些页面中，某些元素的样式是动态发生变化的，class和style绑定就是用来实现动态改变样式效果的技术，其中class绑定中，表达式可以是字符串，可以是对象，也可以是数组，实例如下：

```html
<head>
  <meta charset="UTF-8">
  <title>class和style绑定</title>
  <style>
      .aClass{color: red}
      .bClass{color: blue}
  </style>
</head>
<body>
  <div id="app">
      <h1>class绑定</h1>
      <p :class="a">我是字符串</p>
      <p :class="{aClass: isA,bClass: isB}">我是对象</p> <!--class绑定的是对象。当为true才会留下-->
      <h1>style绑定</h1>
      <p :style="{color: activeColor, fontSize: fontSize+'px'}">我是style强制绑定</p>
      <button @click='update'>更新</button>
  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
       data: {
        a: 'aClass',
        isA: true,
        isB: false ,// 以上是绑定class
        activeColor: 'red', // 以下是绑定style
        fontSize : 20
       },
       methods: {
        update(){
            this.a = 'bClass';
            this.isA = false;
            this.isB = true;// 以上是绑定class
            this.activeColor = 'blue';
            this.fontSize = 30;
        }
       }
    })
</script>
</body>
```

## 1.6.条件渲染

在vue中条件渲染使用`v-if`、`v-else`和`v-show`指令，二者不同的地方在于`v-if`是不会生成不应该显示的元素，`v-show`是通过css控制隐藏不应该显示的节点元素，是在页面生成的，当需要频繁的切换时，使用`v-show`比较好，当条件不成立时，`v-is`的所有子节点也不会被解析；

```html
<body>
  <div id="app">
      <p v-if = 'ok'>显示成功</p>
      <p v-else>显示失败</p>
      <p v-show = 'ok'>显示成功-v-show</p>
      <p v-show = '!ok'>显示失败-v-show</p>
      <button @click='ok=!ok'>切换</button>
  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        data: {
            ok: false
        }
    })
</script>
</body>
```

## 1.7.列表渲染

列表的渲染使用的是`v-for`指令，可以渲染数组和对象，注意的是遍历的时候指定唯一的index或者key，另外在做数组的删除和更新操作时使用数组的`变异方法`，有关vue的数组变异方法可以参考官方API；

```html
<body>
  <div id="app">
      <h2>v-for遍历数组</h2>
     <ui>
         <li v-for="(u,index) in users" ::key="index">
             {{index}}===={{u.name}}===={{u.age}}==
             <button @click='deleteUser(index)'>删除</button>==<button @click="updateUser(index,{name: '王八',age: 45})">更新</button>
         </li>
     </ui>
     <h2>v-for遍历对象</h2>
     <ul>
         <li v-for="(value,key) in users[1]" :key="key">
             {{value}}==={{key}}
         </li>
     </ul>
  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        data: {
            users: [ // vue本身只是监视了users的改变，没有监视数组内部数据的改变
                {name: '张三',age: 23},
                {name: '李四',age: 56},
                {name: '王五',age: 76},
                {name: '赵六',age: 87},
                {name: '陈七',age: 34}
            ]
        },
        methods: {
            deleteUser(index){
                this.users.splice(index,1);
            },
            updateUser(index,value){
                // 如果只写 this.users[index] = value这一条语句，只改变了数组内部的数据，如果不调用vue的变异方法，就不会更新页面
                // vue重写了数组中的一系列方法，重写后就是改变数组操作，然后重新渲染页面，也就是实现的数据绑定
                this.users.splice(index,1,value) ;
            }
        }
    })
</script>
</body>
```

`列表渲染-列表过滤和排序`

```html
<body>
  <div id="app">
    <input type="text" v-model="searchName">
     <ui>
         <li v-for="(u,index) in filterUsers" ::key="index">
             {{index}}===={{u.name}}===={{u.age}}
         </li>
     </ui>
     <button @click="setOrderType(1)" >年龄升序</button>
     <button @click="setOrderType(2)">年龄降序</button>
     <button @click="setOrderType(0)">原本排序</button>
  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        data: {
            searchName: '',
            orderType: 0, // 0代表原本，1代表升序，2代表降序
            users: [ // vue本身只是监视了users的改变，没有监视数组内部数据的改变
                {name: '张三',age: 23},
                {name: '李四',age: 56},
                {name: '张五',age: 76},
                {name: '赵六',age: 87},
                {name: '陈七',age: 34}
            ]
        },
        computed: {
            filterUsers() {
                const {searchName,users,orderType} = this;// 取到相关数据（searchName和users）
                let fusers; // 定义最终返回的数组
                fusers = users.filter(u => u.name.indexOf(searchName) !==-1);// 对users进行过滤
                // 对fusers排序
                if(orderType !== 0){
                    fusers.sort(function(u1,u2){ // 如果返回负数p1在前，返回正数p2在前
                        // 1.代表升序,2.代表降序
                        if(orderType == 2){
                            return u2.age - u1.age
                        }else{
                            return u1.age -u2.age
                        }
                    })
                }
                return fusers;
            }
        },
        methods: {
            setOrderType(value){
                this.orderType = value;
            }
        }
    })
</script>
</body>
```

## 1.8.事件处理

在vue中绑定监听使用`@xxx='fun'`,其中点击函数和传参数和不传参数，默认事件的形参是`event`,当需要传参的同时需要默认event时候，使用隐含属性对象`@xxx = fun(123,$event)`；事件有两个修饰符：`.prevent`：阻止事件的默认事件的默认行为和`.stop`：停止事件冒泡；按键修饰符使用`keyup.keyCode = fun()`：keycode是键盘输入对应的ketcode值，可以使用名称，但是存在一部风没有；

```html
<body>
  <div id="app">
      <h2>绑定监听</h2>
     <button @click="test1" >我是按钮一</button>
     <button @click="test2('Hello 码酱')">我是按钮二，我想获取自己传入的值</button>
     <button @click="test3">我是按钮三，我就想获取自己</button>
     <button @click="test4('Hello',$event)">我是按钮四，我想获取获取自己的同时获取传入的值</button>
     <h2>事件修饰符：事件冒泡和事件默认行为</h2>
     <div style="width: 200px; height: 200px; background: red" @click="test5">
         <!-- 使用 @click.stop阻止事件的冒泡-->
          <div style="width: 100px; height: 100px; background: blue" @click.stop="test6"></div>
     </div>
     <!-- 使用@click.prevent阻止事件的默认行为 -->
     <a href="luokangyuan.com" @click.prevent="test7">去码酱博客</a>
     <h2>按键修饰符：使用@keyup.13，其中的数字代表键盘每一个输入对应的keycode</h2>
     <input type="text" @keyup.13="test8">
     <input type="text" @keyup.enter="test8">
  </div>
<script src="js/vue.js" type="text/javascript"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        methods: {
            test1(){
                alert("四川码酱");
            },
            test2(msg){
                alert(msg);
            },
            test3(event){
                alert(event.target.innerHTML);
            },
            test4(msg,event){
                alert(msg+"==="+event.target.innerHTML);
            },
            test5(){
                alert("执行了外面的div的点击事件");
            },
            test6(){
                alert("执行了里面的div的点击事件");
            },
            test7(){
                alert("不去码酱博客")
            },
            test8(event){
                alert(event.target.value);
            }
        }
    })
</script>
</body>
```

## 1.9.表单输入绑定

表单的数据绑定使用`v-model`指令，具体相关编码如下：

```html
<body>
    <div id="app">
        <form action="/xxx" @submit.prevent="handSubmit">
            <span>用户名：</span>
            <input type="text" v-model="userName">
            <br>
            <span>密码：</span>
            <input type="password" v-model="pwd">
            <br>
            <span>性别：</span>
            <input type="radio" id="wman" value="女" v-model="sex">
            <label for="wman">女</label>
            <input type="radio" id="man" value="男" v-model="sex">
            <label for="man">男</label>
            <br>
            <span>爱好：</span>
            <input type="checkbox" id="basket" value="basket" v-model="likes">
            <label for="basket">篮球</label>
            <input type="checkbox" id="footbal" value="foot" v-model="likes">
            <label for="footbal">足球</label>
            <input type="checkbox" id="pingpang" value="pingpang" v-model="likes">
            <label for="pingpang">乒乓球</label>
            <br>
            <span>城市：</span>
            <select name="" id="" v-model="cityId">
                <option value="">未选择</option>
                <option :value="city.id" v-for="(city,index) in allCitys" :key="index">{{city.name}}</option>
            </select>
            <br>
            <span>个人介绍：</span>
            <textarea name="" id="" cols="30" rows="10" v-model="desc"></textarea>
            <br>
            <input type="submit" value="注册">
        </form>
    </div>
    <script src="js/vue.js" type="text/javascript"></script>
    <script type="text/javascript">
        new Vue({
            el: "#app",
            data: {
                userName: '',
                pwd: '',
                sex: '男', // 默认选中性别男
                likes: ['foot'], // 默认选中foot对应的多选框
                allCitys: [{ id: 1, name: "北京" }, { id: 2, name: "成都" }, { id: 3, name: "上海" }, { id: 4, name: "宁波" }],
                cityId: '',// 这里默认是空，则匹配未选择，如果默认选中成都，则写2即可
                desc: ''
            },
            methods: {
                handSubmit() {
                    console.log(this.userName, this.pwd, this.sex, this.likes, this.cityId, this.desc)
                }
            }
        })
    </script>
</body>
```

## 1.10.Vue的生命周期

![](http://image.luokangyuan.com/vue%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE.png)

常用的生命周期方法：`create()/mounted()`:常用于发送Ajax请求启动定时器等异步任务，`beforeDestory()`：常用于做一些收尾工作，例如关闭定时器；

```html
<body>
    <div id="app">
        <button @click= "destoryVm">点击我取消Vue实例</button>
        <p v-show = "isShow">我是四川码酱</p>
    </div>
    <script src="js/vue.js" type="text/javascript"></script>
    <script type="text/javascript">
        new Vue({
            el: "#app",
            data: {
                isShow: true
            },
            beforeCreate() {
                console.log("我是beforeCreate方法，我被执行了");
            },
            created() {
                console.log("我是created方法，我被执行了");
            },
            beforeMount() {
                console.log("我是beforeMount方法，我被执行了");
            },
            mounted(){ // 初始化显示之后立即调用，也是执行一次
                this.intervalId = setInterval(() =>{
                    console.log("=====");
                    this.isShow = !this.isShow;
                }, 1000)
            },
            beforeUpdate() {
                console.log("我是beforeUpdate方法，我被执行了");
            },
            updated() {
                console.log("我是updated方法，我被执行了");
            },
            beforeDestroy() { // 死亡之前回调一次
                // 清除定时器
                clearInterval(this.intervalId)
            },
            destroyed() {
                console.log("我是destroyed方法，我被执行了");
            },
            methods: {
                destoryVm(){
                    this.$destroy();
                }
            }
        })
    </script>
</body>
```

>   说明：`beforeCreate、created、beforeMount、mounted`初始化方法只执行一次，`beforeUpdate、updated`执行多次，`beforeDestroy、destroyed`死亡方法，也执行一次；

## 1.11.过渡和动画

在vue中动画就是操作css的trasition或者animation属性，vue会给目标元素添加和移除指定的class，只不过要遵循一定的命名规则，

```html
 <title>动画和过渡</title>
    <style type="text/css">
        /* 指定显示和隐藏的过渡效果 */

        .fade-enter-active,
        .fade-leave-active {
            transition: opacity 1s;
        }

        /* 指定隐藏的样式 */

        .fade-enter,
        .fade-leave-to {
            opacity: 0;
        }

        /* 指定显示的过滤效果 */

        .move-enter-active {
            transition: all 1s
        }

        /* 指定隐藏的过滤效果 */

        .move-leave-active {
            transition: all 3s
        }

        /* 指定隐藏的样式 */

        .move-enter,
        .move-leave-to {
            opacity: 0;
            transform: translateX(20px)
        }
    </style>
</head>

<body>
    <div id="app">
        <button @click="isshow = !isshow">动画按钮</button>
        <transition name="fade">
            <p v-show="isshow">四川码酱</p>
        </transition>
    </div>
    <div id="app1">
        <button @click="isshow = !isshow">多属性动画按钮</button>
        <transition name="move">
            <p v-show="isshow">四川码酱</p>
        </transition>
    </div>
    <script src="js/vue.js" type="text/javascript"></script>
    <script type="text/javascript">
        new Vue({
            el: "#app",
            data() {
                return {
                    isshow: true
                }
            }
        })
        new Vue({
            el: "#app1",
            data() {
                return {
                    isshow: true
                }
            }
        })
    </script>
</body>
```

## 1.12.过滤器

在vue中允许自定义过滤器，所谓过滤器就是：`对要显示的数据进行特定格式化后在显示，例如时间格式化等`，注意的是：`并没有改变原本的数据，只是产生新的对应数据`；

```html
<body>
    <div id="app">
      <p>当前完整时间为：{{data | dateString}}</p>
      <p>当前日期为：{{data | dateString('YYYY-MM-DD')}}</p>
      <p>当前时间为：{{data | dateString('HH:mm:ss')}}</p>
    </div>
    <script src="js/vue.js" type="text/javascript"></script>
    <script src="https://cdn.bootcss.com/moment.js/2.22.1/moment.js"></script>
    <script type="text/javascript">
    // 自定义过滤器
        Vue.filter('dateString',function(value,format){
            return moment(value).format(format || 'YYYY-MM-DD HH:mm:ss')
        })
        new Vue({
            el: "#app",
            data: {
                data: new Date()
            }
        })
    </script>
</body>
```

## 1.13.内置指令和自定义指令

`常用的内置指令`

*   v:text : 更新元素的 textContent
*   v-html : 更新元素的 innerHTML
*   v-if : 如果为 true, 当前标签才会输出到页面
*   v-else: 如果为 false, 当前标签才会输出到页面
*   v-show : 通过控制 display 样式来控制显示/隐藏
*   v-for : 遍历数组/对象
*   v-on : 绑定事件监听, 一般简写为@
*   v-bind : 强制绑定解析表达式, 可以省略 v-bind
*   v-model : 双向数据绑定
*   ref : 指定唯一标识, vue 对象通过$els 属性访问这个元素对象
*   v-cloak : 防止闪现出现`{{xxx}}`, 与 css 配合: [v-cloak] { display: none }

```html
<title>内置指令</title>
    <style>
        [v-cloak] {
            display: none;
        }
    </style>
</head>

<body>
    <div id="app">
        <p ref="content">四川码酱</p>
        <button @click = "hint">提示</button>
        <p v-cloak>{{msg}}</p>
    </div>
    <script src="js/vue.js" type="text/javascript"></script>
    <script type="text/javascript">
        new Vue({
            el: "#app",
            data: {
                msg: "Hello 四川码酱"
            },
            methods: {
                hint(){
                    alert(this.$refs.content.textContent);
                }
            }
        })
    </script>
</body>
```

>   说明： `v-cloak`指令是为了页面加载数据缓慢时候显示`{{xxx}}`标签而出了一个指令，与CSS搭配使用

`自定义指令`

注册全局指令，方法如下：

```javascript
Vue.directive('my-directive', function(el, binding){
	el.innerHTML = binding.value.toupperCase()
})
```

注册局部指令，方法如下：

```javascript
directives : {
  'my-directive' : {
  bind (el, binding) {
    el.innerHTML = binding.value.toupperCase()
    }
  }
}
```

```html
<body>
    <div id="app">
        <p v-upper-text="msg"></p>
        <p v-lower-text="msg"></p>
    </div>
    <script src="js/vue.js" type="text/javascript"></script>
    <script type="text/javascript">
         // 定义全局指令
         // el：指令属性所在的标签属性
         // binding：包含指令相关信息数据的对象
        Vue.directive('upper-text', function(el,binding){
            el.textContent = binding.value.toUpperCase()
            console.log(el,binding)
        })
        new Vue({
            el: "#app",
            data: {
                msg: "This is my think life"
            },
            directives: { // 使用自定义局部指令
                'lower-text' : function(el,binding){
                    el.textContent = binding.value.toLowerCase()
                }
            }
        })
    </script>
</body>
```

## 1.14.核心知识总结

*   Vue是前端开发库，用于构建用户界面，遵循MVVM模式，编码简洁，体积小，效率高，包含了一些列插件库；
*   基本使用：引入vue.js，创建vue实例对象，其中el代表dom标签选择器，data代表初始化数据对象；
*   `el`：指定dom标签容器的选择器，一般写一个根标签；
*   `data`：对象或者函数类型，指定初始化状态属性数据的对象，页面中可以使用`{{xxx}}`直接访问
*   `methods`：包含多个方法的对象，供页面中的事件指令来回调，回调函数默认有`$event`参数，也可以指定自己的参数，在方法中，访问data中的属性直接使用`this.xxx`；
*   `computed`：计算属性，包含多个方法的对象，对状态属性进行计算处理后返回给页面一个新的数据，使用get和set方法实现属性的计算读取，同时监视数据的变化；
*   `watch`：监视，包含多个属性监视的对象，`xxx.function(value){}`，可以传入两个参数，代表新值和改变前的值，也可以使用`vm.$watch('xxx', function(value){})`的方式添加监听；
*   vue中的过渡和动画，实质就是vue操作css的transition/animation属性；
*   生命周期：常用的钩子函数是`created() / mounted()`: 启动异步任务(启动定时器,发送ajax请求, 绑定监听)和`beforeDestroy()`: 做一些收尾的工作例如清除定时器等；
*   自定义过滤器：使用的是`Vue.filter(filterName,function(value){})`，在页面使用方法：`{{myData | filterName(arg)}}`，参数可传可不传；
*   vue内置指令：`v-for遍历`、`@绑定事件`、`v-model数据双向绑定`、`ref标识标签`；
*   自定义指令：使用`Vue.directive`注册全局指令，使用`directives`注册局部指令；

>   注意：数据在哪个组件，更新数据的行为（方法）就应该定义在那个组件中

# 二、Vue组件化编码方式

## 2.1.使用vue-cli创建模板项目

vue-cli是vue官方提供的脚手架工具，首先确认安装了node和npm，最好安装一个cnpm，使用方法如下：

*   npm install -g vue-cli
*   vue init webpack vue_demo 其中 vue_demo是项目名
*   cd vue_demo
*   npm install或者npm run dev

`项目结构说明`

*   build : webpack 相关的配置文件夹(基本不需要修改)
    *    dev-server.js : 通过 express 启动后台服务器
*   config: webpack 相关的配置文件夹(基本不需要修改)
    *   index.js: 指定的后台服务的端口号和静态资源文件夹
*   src : 源码文件夹
    *   components: vue 组件及其相关资源文件夹
    *   App.vue: 应用根主组件
    *   main.js: 应用入口 js
*   static: 静态资源文件夹
*   .babelrc: babel 的配置文件
*   .eslintignore: eslint 检查忽略的配置
*   .eslintrc.js: eslint 检查的配置
*   .gitignore: git 版本管制忽略的配置
*   index.html: 主页面文件
*   package.json: 应用包配置文件
*   README.md: 应用描述说明的 readme 文件

`简单的使用Vue模板项目`

首先，我们编写了一个HelloWord的组件，

```vue
<template>
    <div class="hello">
        <h1 class="msg">{{ msg }}</h1>
    </div>
</template>

<script>
export default {
    // 配置对象和Vue一致
    data() {
        // data可以 写对象和函数，但是在组件中必须使用函数
        return {
            msg: "Hello Welcome to My Vue"
        };
    }
};
</script>

<style scoped>
    .msg {
        color: red
    }
</style>
```

然后，我们在App.vue中使用我们自己定义的组件

```vue
<template>
  <div id="app">
    <img class="logo" src="./assets/logo.png">
    <!-- 使用组件标签 -->
    <HelloWorld/> 
  </div>
</template>

<script>
// 1.引入需要使用的vue组件（HelloWoed组件）
import HelloWorld from './components/HelloWorld'

export default {
  components: { // 2.映射组件标签
    HelloWorld
  }
}
</script>

<style>
.logo{
    width: 100px;
    height: 100px;
}
</style>
```

我们知道使用Webpack打包后会生成一个js文件，也就是入口文件main.js

```javascript
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

new Vue({
  el: '#app', // 挂载到入口Dom节点（index.html）
  components: { App }, // 映射组件标签
  template: '<App/>' //使用标签
})

```

## 2.2.项目打包发布方式

打包命令：`npm run build`

使用静态服务器：安装命令：`npm install -g serve`，发布命令：`serve dist`，然后直接访问就可以了

使用动态web服务器（Tomcat）:修改webpack.prod.conf.js，加入`output: {publicPath: '/xxx/' //打包文件夹的名称}`，然后重新打包，将dis文件夹的名称改为项目名称，放在tomcat的webapp目录下，访问即可；

## 2.3.组件的定义





# 三、Vue请求方式vue-ajax



# 四、Vue组件库



# 五、Vue路由vue-router



# 六、Vue状态管理vuex







