# ES6相关知识学习
## 1. let和const关键字
### 1.1 let关键字
作用：与var类似，用于声明一个变量
特点 在块作用域内有效，不能重复声明，不会预处理，不存在变量提升
应用：循环遍历加监听

```javascript
<body>
    <button>按钮1</button>
    <button>按钮2</button>
    <button>按钮3</button>
    <script type="text/javascript">
        let name = 'zhangsan';
        let btns = document.getElementsByTagName('button');
        for(let i = 0; i<btns.length;i++){
            let btn = btns[i];
            btn.onclick = function(){
                console.log(i);
            }
        }
    </script>
</body>
```
### 1.2 const关键字
作用：定义一个常量。
特点：不能被修改，其他特点和let一样，用于保存不变的数据。

```javascript
const KEY = "Hello";
```
## 2. 变量的解构赋值
* 从对象或数组中提取数据，并赋值给变量（多个）。
* 对象的解构赋值： let {n,a} = {n:'tom',a: 12};
* 数组的解构赋值： let{a,b} = {1,'zhangsan'};
* 变量的解构赋值多用于给多个形参赋值。
	
```javascript
 <script type="text/javascript">
    let obj = {name: 'zhangsan',age: 52};
    //let {name} = obj;//对象的结构赋值,只取一个属性值
    //console.log(name);
    let {name,age} = obj;//对象的结构赋值
    console.log(name,age);
    let arr = [1,3,8,'zhangsan',true];
    //let [a,b] = arr;//a,b可以随便写，代表获取数组中下标为0和1的值。
    let [,,a,b] = arr//代表获取下标为2和3的值
    console.log(a,b)
    //不采用解构赋值
    function test1(obj){
        console.log(obj.name+"===>"+obj.age);
    }
    //给多个形参赋值，采用解构赋值
    function test2({name,age}){
        console.log(obj.name+"===>"+obj.age);
    }
    test1(obj);
    test2(obj);
</script>
```
## 3. 模板字符串
简化字符串的拼接，模板字符串必须使用``包含，注意这个符号不是单引号，是tab键上面的那个键，变化的部分采用${xxx}来定义。
	
```javascript
 <script type="text/javascript">
   //普通的字符串拼接
    let obj = {name: 'zhangsan', age: 56,sex: '男'};
    let str = '我的名字是'+obj.name+',性别：'+obj.sex+',我今年'+obj.age+'岁。'
    console.log(str)
    //使用模板字符串
    let str1 = `我的名字是${obj.name},性别：${obj.sex},我今年${obj.age}岁。`
    console.log(str1);
</script>
```
## 4. 对象的简写方式
省略同名的属性值，省略方法的function，

```javascript
 <script type="text/javascript">
    //普通的写法，对象中的属性值和属性名称一样
    let name = 'zhangsan';
    let age = 56;
    let obj = {
        name:name,
        age: age,
        getName:function(){
            return this.name;
        }
    }
    console.log(obj);
    console.log(obj.getName());
    //ES6的对象简写方式写法
    let username = '张三';
    let userage = 56;
    let obj1 = {
        username,//同名的属性可以省略不写
        userage,//同名的属性可以省略不写
        getName(){//可以省略函数的function
            return this.username;
        }
    }
    console.log(obj1)
    console.log(obj1.getName());
</script>
```
## 5. 箭头函数
* 箭头函数没有自己的this,箭头函数的this不是调用的时候决定的，而是在定义的时候处在的对象就是它的this。
* 箭头函数的this看外层是否有函数，如果有，外层函数的this就是内部箭头函数的 this，没有则this是window。

```javascript
<button id="btn1">测试按钮1</button>
<button id="btn2">测试按钮1</button>

<script type="text/javascript">
    let test1 = function(){
        console.log('我是普通函数');
    }
    let test2 = () => console.log('我是箭头函数')
    test2();
    //形参情况
    //1.没有形参的情况下
    let test3 = () => console.log('我是没有形参的箭头函数');
    test3();
    //2.只有一个形参的情况下,可以省略()
    let test4 = (a) =>  console.log('我是有一个形参的箭头函数'+a);
    let test5 = a =>  console.log('我是有一个形参的箭头函数'+a);
    test4('zhangsan');
    test5('zhangsan');
    //3.两个或者两个以上的形参
    let test6 = (x,y) => console.log('我是有两个参数的箭头函数'+x+"==="+y);
    test6(45,85);
    //函数体的情况
    //1.函数体只有一条语句或者表达式的时候，可以省略大括号,会自动返回执行结果。
    let test7 = (x,y) => x+y;
    console.log(test7(54,54));//打印结果为108
    //2.函数体不止一条语句或者函数体的情况，不可以省略大括号
    let test8 = (x,y) =>{
        console.log(x,y);
        return x+y;
    }
    console.log(test8(54,54));
    //箭头函数的this,结果为obj
    let btn1 = document.getElementById('btn1');
    let obj = {
        name:'箭头函数',
        getName(){
            btn1.onclick =() =>{
                console.log(this)
            }
        }
    }
    obj.getName();
    //箭头函数的外面还是箭头函数，this就是window
    let btn2 = document.getElementById('btn2');
    let obj1 = {
        name:'箭头函数',
        getName:()=>{
            btn2.onclick =() =>{
                console.log(this)
            }
        }
    }
    obj1.getName();
</script>
```
## 6. 三点运算符

```javascript
 <script type="text/javascript">
    //...运算符只能放在最后，前面可以使用占位符。
    function test (a,...value){
        console.log(a)//输出结果为2
        console.log(value);
        value.forEach(function(item,index){
            console.log(item,index);
        })

    }
    test(2,54,87,76)
    let arr = [1,6];
    let arr2 = [2,3,4,5];
    arr = [1,...arr2,6];
    console.log(arr);
    console.log(...arr);//直接遍历arr数组，拿到值。
</script>
```
输出结果为：

![](https://i.imgur.com/V2ZIe5k.png)
## 7. 形参默认值

```javascript
<script type="text/javascript">
   //定义一个点的坐标的构造函数
    function Point(x = 0,y = 5){//使用形参默认值
        this.x = x;
        this.y = y;
    }
    let point = new Point(23,35);
    console.log(point);
    let point1 = new Point();//new一个构造函数的时候，如果没有传参数，就使用形参默认值
    console.log(point1);
</script>
```
输出结果为;

![](https://i.imgur.com/wyfPfxT.png)
## 8. promise对象原理
Promise对象，代表了未来某个将要发生的事件（通常是一个异步操作）；
有了Promise对象。我们可以将异步操作以同步的流程表达出来，避免了层层嵌套的回调函数，在ES6中，Promise是一个构造函数，用来生成promise实例。
### 8.1 使用步骤：

```javascript
<script type="text/javascript">
   //创建promise对象
let promise = new Promise((resolve,reject) =>{
    //初始化promise的状态为pending初始化状态
    console.log('1111')
    //执行异步操作，通常是发送AJAX请求，或者开启定时器
    setTimeout(()=>{
        console.log('3333');
        //根据异步任务的返回结果去修改promise的状态
        //异步任务执行成功,
        //通常异步操作得到的数据是在回调函数中操作，就涉及到如何把数据传到回调函数中，使用形参注入的方式
        resolve('成功的数据');//自动修改promise的状态为fullfilled状态，就会调用成功的回调函数
        //reject();//自动修改promise的状态为reject状态
    },2000)
})
console.log('222');
promise
        .then((data)=>{//成功的回调函数
            console.log('成功的回调函数被调用！！',data)
        },(error)=>{//失败的成功回调函数
            console.log('失败的回调函数被调用！！')
        })
</script>
```
### 8.2 promise的三种状态
pending:初始化状态，fullfilled:成功状态，rejected:失败状态
## 9. Symbol属性
* ES6中添加一种原始的数据类型symbol（已经存在的原始数据类型有：string，number，boolean,null,underfined,对象）
* symbol属性值对应的值是唯一的，解决了命名冲突的问题。
* symbol值不能与其他数据进行计算，包括字符串拼接。
* for in,for of遍历时不会遍历symbol属性。


Symbol 值通过Symbol函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。
在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。
### 9.1 使用场景：

```javascript
 <script type="text/javascript">
    //调用Symbol函数得到symbol值
    let symbol = Symbol();
    let obj = {
        name:'zhangsan',
        age: 56
    };
    obj[symbol] = 'hello world';//在对象的内部，使用 Symbol 值定义属性时，Symbol 值必须放在方括号之中。
    console.log(obj);
    let symbol1 = Symbol('one');
    let symbol2 = Symbol('two');
    console.log(symbol1 == symbol2);//false
    console.log(symbol1,symbol2)//肉眼看见是相同的
    // 定义常量
    const Person_Key = Symbol('person_key');
</script>
```
## 10. iterator接口机制
首先在JavaScript中，存在数组和对象的数据结构，ES6又增加了map和Set,这四种数据集合，当然还可以彼此使用，例如数组的成员是Map，Map的成员是对象。
iterator是一种接口机制，为各种不同的数据结构提供统一的访问机制。
为各种数据结构提供一种统一的，简单的访问接口。使得数据结构的成员能够按某种次序排列，在ES6中创造一种新的遍历方式for...of循环。
### 10.1 原理：
* 创建一个指针对象（遍历器对象），指向数据结构的起始位置。
* 第一次调用指针对象的next()方法，可以将指针指向数据结构的第一个成员。
* 第二次调用指针对象的next()方法，指针就指向数据结构的第二个成员。
* 不断的调用next()方法，直到它指向数据结构的结束位置。


### 10.2 模拟指针对象
```javascript
 //模拟指针对象
  function myIterator(arr){
      let nextIndex = 0;//记录指针的位置
      return{//遍历器对象
          nxet: function(){
              return nextIndex < arr.length ? {vlaue:arr[nextIndex++],done:false}:{value:undefined,done:true}
          }
      }
  }
  //准备一个数据
    let arr = [4,5,58,'hello world'];
    let iterator = myIterator(arr);
    console.log(iterator.nxet());
    console.log(iterator.nxet());
    console.log(iterator.nxet());
    console.log(iterator.nxet());
    console.log(iterator.nxet());
    console.log(iterator.nxet());
```

* 每一次调用next()方法，都会返回数据结构的当前成员信息，具体来说，就是返回一个包含value和done两个属性的对象，其中，value表示当前成员的值，done属性是一个布尔值，遍历遍历是否结束。
* 一种数据结构只要部署了Iterator接口，我们就称这种数据结构是‘可遍历的（iterable）’。
* ES6规定，默认的Iterator接口部署在数据结构的Symbol.iterator属性，或者说一个数据结构只要只要有symbol.iterator属性，就可以认为是‘可遍历的’。
* symbol.iterator属性本身是一个函数，就是当前数据结构默认的遍历器生成函数，执行这个函数，就会返回一个遍历器
	
```javascript
<script type="text/javascript">
  const obj = {
      [Symbol.iterator] : function () {
          return {
              next: function () {
                  return {
                      value: 1,
                      done: true
                  }
              }
          }
      }
  }
</script>
```
* 上述代码中，对象obj是可以遍历的，因为它具有Symbol.iterator属性，执行这个属性，就会返回一个遍历对象，该对象的根本特征就具有next方法，每一次调用next方法，都会返回一个代表当前成员的信息对象，具有value和done两个属性。
* 在ES6的有些数据结构原声就具备了Iterator接口（比如数组），即不用做任何的处理，就可以被for...of循环遍历，原因就在于这些数据结构原生就已经部署了Symbol.iterator属性，另外一些数据结构（比如对象）就没有部署Symbol.iterator属性，原生具备Iterator接口的数据结构如下：

    * Array
    * Map
    * Set
    * String
    * TypedArray
    * 函数的arguments对象
    * NodeLise对象


### 10.3 可以使用for...of的例子
```javascript
 // for...of的使用-数组
let arr1 = [5,8,98,'Hello word'];
for(let i of arr1){
    console.log(i);
}
// for...of的使用-字符串
let str = 'Hello word';
for(let i of str){
    console.log(i);
}
// for...of的使用-arguments
function test(){
    for(let i of arguments){//arguments是一个伪数组，不能使用foreach遍历。
        console.log(i);
    }
}
test(1,3,5,7,8,'Hello word');
```
> 注意：使用三点运算符，解构赋值，默认会调用Iterator接口。
## 11. Generator函数
* Generator函数是ES6提供的一种异步编程解决方案，语法行为与传统函数完全不一样。
* Generator函数可以理解为一个状态机，封装了多个内部状态。
* 执行Generator函数会返回一个遍历器对象，也就是说，Generator函数除了一个状态机，还是一个遍历器生成函数。返回的遍历器对象可以一次遍历Generator函数内部的每一个状态。
* 形式上，Generator函数就是一个普通的函数，有两个特征，其一是：function关键字与函数名之间有一个星号，其二是：函数体内部使用yield表达式，定义不同的内部状态。

```javascript
function* myGenerator() {
        yield 'a';
        yield 'b';
        yield 'c';
        return 'ending';

    }
 let gen = myGenerator();
```
上述代码就定义了一个Generator函数myGenerator，在他的内部就有三个yield表达式，即函数就存在三个状态，但是，当我门调用函数的时候，函数并不会执行，返回的也不是函数的运行结果，而是一个指向内部状态的指针对象，也叫遍历器对象（iterator object）。
下一步就是调用遍历器的next方法，是的指针下移一个状态，也就是说，每一次的调用next方法，就遍历一个状态，
	
```javascript
<script type="text/javascript">
    function* myGenerator() {
        yield 'a';
        yield 'b';
        yield 'c';
        return 'ending';

    }
    let gen = myGenerator();
    console.log(gen.next());// {value: "a", done: false}
    console.log(gen.next());// {value: "b", done: false}
    console.log(gen.next());// {value: "c", done: false}
    console.log(gen.next());// {value: "ending", done: true}
	console.log(gen.next());// {value: undefined, done: true}
</script>
```
### 11.1 总结：
调用Generator函数返回遍历器对象，代表Generator函数的内部指针，以后每次调用遍历器对象的next方法，就会返回一个有着value和done属性的对象。value属性表示当前的内部状态值，是yield表达式后面的那个表达式的值。done属性是一个布尔值，表示是否遍历结束。
## 11.2 yield表达式
由于Generator函数返回的是一个遍历器对象，只有调用next方法才会遍历下一个内部状态，所以它其实提供的就是一种可以暂停执行的函数，yield表达式就是暂停的标志。
遍历器对象的next方法的运行逻辑如下：

* 遇到yield表达式，就暂停执行后面的操作，并将紧跟在后面yield后面的那个表达式的值，作为返回对象的value的属性值。
* 下一次调用next的方法时，再继续往下执行，直到遇到一个yield表达式。
* 如果没有遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回对象的value的属性值。
* 如果该函数没有return语句，则返回的对象的value属性值为undefined。

> 需要注意的是：yield表达式后面的表达式，只有当调用next方法，内部指针指向该语句才会执行。
	
```javascript
function* myGenerator1(){
    yield 5+5;
    yield 5*5;
    return 5*5+5*5;
}
let gen1 = myGenerator1();
console.log(gen1.next());
console.log(gen1.next());
console.log(gen1.next());
console.log(gen1.next());
```
如果Generator函数不使用yield表达式，就变成了一个单纯的暂缓执行函数。

```javascript
function* test(){
    console.log("我被执行了！！");
}
let gen3 = test()
gen3.next();
```
上述代码中test函数就是一个普通的函数，但它是一个Generator函数，就变成了只有调用next方法，才会执行text函数。
> yield表达式只有在Generator函数中使用，其他地方使用就会报错。如果，yield表达式使用在另一个表达式中，就必须放在圆括号里面，如下所示


```javascript
function* demo(){
	console.log('hello==>'+(yield));
	console.log('hello=>='+(yield 123));
}
let gendemo = demo();
console.log(gendemo.next());
console.log(gendemo.next());
console.log(gendemo.next());
```
### 11.3 next方法的参数
yield表达式本身没有返回值，或者说总是返回undefined，next方法可以带一个参数，该参数就会被当成上一个yield表达式的返回值。

```javascript
function* foo(x){
    let y = 2* (yield (x+1));
    let z = yield (y / 3);
    return (x + y + z);
}
let g = foo(5);
console.log('输入结果为1：',g.next());// 输入结果为： {value: 6, done: false}
console.log('输入结果为2：',g.next(12));// 输入结果为： {value: 8, done: false}
console.log('输入结果为3：',g.next(13));// 输入结果为： {value: 42, done: true}
```
上述代码中，第一次调用next方法，返回x+1的值6，第二次调用调用next方法，next方法中传入参数为12，就说上一次yield表达式的值为12，因此y等于24，返回的是y / 3的值8，第三次调用next方法，传入参数13，因此 z 等于13，这个时候x等于5，y等于24，z等于13，返回42。
### 11.4 for of遍历Generator函数
下面是一个利用 Generator 函数和for...of循环，实现斐波那契数列的例子。
	
```javascript
 function* fibonacci(){
        let [prev,curr] = [0,1];
        for(;;){
            [prev,curr] = [curr,prev+curr];
            yield  curr;
        }
    }
    for(let n of fibonacci()){
        if(n > 1000) break;
        console.log(n);
    }
```
### 11.5 Generator函数的小练习

```javascript
// 需求就是先一部发送发送1号请求，根据1号请求返回的数据中的comurl值作为2号请求的url
function getDept(url){
    $.get(url,function(data){
        let url = "http://localhost:3000"+data.comurl;
        sx.next(url);
    })
}
function* show(){
   let url =  yield getDept('http://localhost:3300/dept?id=3');
    yield getDept(url)
}
let sx = show();
sx.next();//1
```
分析：执行到1语句的时候，就会调用show方法中的第一个yield，然后执行getDept方法，在将返回的url做为next的参数，在调用show方法中的第二个yield，再执行getDept方法，发送第二次请求。

``` javascript
// 需求就是先一部发送发送1号请求，根据1号请求返回的数据中的comurl值作为2号请求的url
function getDept(url){
    $.get(url,function(data){
        console.log(data);// 请求后返回的数据
        let url = "http://localhost:3000"+data.comurl;
        sx.next(url);
    })
}
function* show(){
   let url =  yield getDept('http://localhost:3300/dept?id=3');
    yield getDept(url)
}
let sx = show();
sx.next();
```
### 11.6 遍历对象调用return方法，
遍历对象调用return方法后，返回的值的value就是return方法的参数值，并且 Generator函数的遍历就终止了，返回的done的值为true。以后调用next方法，done的属性值就一直是true，例如

```javascript
 function* hello(){
        yield '1'
        yield '2'
        yield '3'
        yield '4'
        yield '5'
    }
    let bubu = hello();
    console.log(bubu.next()) ;//{value: "1", done: false}
    console.log(bubu.next()) ; // {value: "2", done: false}
    console.log(bubu.return("Hello word！！"));// {value: "Hello word！！", done: true}
    console.log(bubu.next()) ;// {value: undefined, done: true}
```
如果return方法调用时，不提供参数，则返回值的value属性为undefined。
### 11.7 yield*表达式

```javascript
 function* foo(){
        yield 'a'
        yield 'b'
    }
    function* bar(){
        yield 'x'
        foo();
        yield 'y'
    }
    for(let v of bar()){
        console.log(v);// x y
    }
```
上述代码存在Generator函数中调用Generator函数，总输出结果我们可以看出是不会有效果的，这个时候就需要使用yield*表达式，如下

```javascript
    function* foo(){
        yield 'a'
        yield 'b'
    }
    function* bar(){
        yield 'x'
        yield* foo();
        yield 'y'
    }
    //等同于
    function* bar(){
        yield 'x'
        yield 'a'
        yield 'b'
        yield 'y'
    }
    //等同于
    function* bar(){
        yield 'x'
        for(let v of foo()){
            yield v;
        }
        yield y
    }
    for(let v of bar()){
        console.log(v)
    }
```
## 12. class的基本语法

```javascript
class Point {
        constructor(x,y){
            this.x = x;
            this.y = y;
        }
        toString(){
            return '===>('+this.x+','+this.y+')';
        }
        show(){
            console.log("我是show方法，")
        }
    }
    let point = new Point(5,8);
    console.log(point);
    point.show();//与下面的调用方法一样
    Point.prototype.show();//调用原型上的方法，使用类名
```
* 上述代码中，定义了一个类，可以看到里面有一个构造方法，其中this关键字就表示实例对象，除了一个构造方法外，还有一个toString方法，注意，定义‘类’的方法的时候，不需要加上function关键字，直接将函数定义放进去就可以，另外，方法之间也不需要逗号隔开。使用的时候，采用new命令。和构造函数的用法一样。
* 由于类的方法都是定义在原型prototype对象上，所以类的新方法可以添加在prototype对象上面，Object.assign方法可以很方便的一次向类中添加多个方法。

```javascript
class Point1{
        constructor(x,y,z){
            this.x = x;
            this.y = y;
            this.z = z;
        }
    }
    Object.assign(Point1.prototype,{
        toString(){
            console.log('我是x坐标==>',this.x);
        },
        toValue(){
            console.log('我是y坐标==>',this.y);
        },
        toShow(){
            console.log('我是z坐标==>',this.z);
        }
    })
    let po = new Point1(5,8,7);
    po.toString();
    po.toValue();
    po.toShow();
```
在ES6的严格模式下：constructor方法是类的默认方法，通过new命令生成对象实例，自动调用该方法，一个类就必须有constructor方法，如果没有显示定义，一个空的constructor方法就会被默认添加。类必须通过new调用。否则就会报错。
### 12.1 class表达式
与函数一样，类可以使用表达式的形式定义

```javascript
 const MyClass = class Me{
        constructor(){

        }
        getClassName(){
            console.log(Me.name)
        }
    }
    new MyClass().getClassName();
```
上述代码就是使用表达式定义了一个类，需要注意的是，这个类的名字是MyClass,而不是Me,Me只在class内部代码可用，指向当前类。
### 12.2 this指向
类的方法内部如果含有this，它默认指向类的实例，但是，一旦单独使用，就很可能会报错。

```javascript
class Logger {
    printName(name = 'three'){
        this.print(`hello ${name}`);
    }
    print (text){
        console.log(text);
    }
}
const  logger = new Logger();
const {printName} = logger;
printName();
```
上述代码中，printName方法中的this,默认指向的是Logger类的实例，但是，如果将这个方法提取出来再外部使用的话，this就会指向该方法运行时所在的环境，因为找不到print方法而导致报错。

`解决办法之一：构造方法中绑定this`
	
```javascript
class Logger {
    constructor(){
        this.printName = this.printName.bind(this)
    }
    printName(name = 'three'){
        this.print(`hello ${name}`);
    }
    print (text){
        console.log(text);
    }
}
const  logger = new Logger();
const {printName} = logger;
printName();
```
`解决办法之二:使用箭头函数`

```javascript
 class Logger {
    constructor(){
        this.printName = (name='three')=>{
            this.print(`Hello ${name}`);
        }
    }
    print (text){
        console.log(text);
    }
}
const  logger = new Logger();
const {printName} = logger;
printName();
```
### 12.3 name属性
每一个类都具有name属性，name属性总是返回紧跟在class关键字后面的类名

```javascript
class Beauty{}
console.log(Beauty.name);
```
### 12.4 Class的静态方法
在一个类中定义的方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来直接调用，这个方法就叫静态方法。

```javascript
 class Foo{
    static getName(){
        return 'Hello world'
    }
}
let dtr = Foo.getName();
console.log(dtr)
```
> 注意：如果静态方法包含this关键字，这个this指向的是类，而不是类的实例。

```javascript
 class Foo{
    static getName(){
        let age = this.getAge();
        return `Hello World,你的年龄是：${age}`;
    }
    static getAge(){
        return 15;
    }
     getAge(){
        return 155;
    }
}
let dtr = Foo.getName();
console.log(dtr)// Hello World,你的年龄是：15
```
* 从上述代码可以看出。静态方法getName中调用了this.getAge(),这里的this指的是Foo类，而不是Foo的实例。等同于调用了Foo.getAge(),
* 还可以看出静态方法和非静态方法可以重名。
* 父类的静态方法，可以被子类继承。

```javascript
class Sun extends Foo{}
let sunstr = Sun.getName();
console.log(sunstr);// Hello World,你的年龄是：15
```
> 静态方法也是可以从supper对象上调用的。

```javascript
 class Fa{
    static printStr(){
        return 'Hello'
    }
}
class Son extends  Fa{
    static print(){
        return `${super.printStr()},world`
    }
}
console.log(Son.print());// Hello,world
```
## 13. class的继承
### 13.1 简介
`class可以通过extends关键字实现继承。`

```javascript
class Point{}
class ColorPoint extends Point{
    constructor(x,y,color){
        super(x,y);
        this.color = color;
    }
    toString(){
        return `${this.color}:${super.toString()}`
    }
}
let cp = new ColorPoint(4,5,85)
```
> 注意的是：子类必须再constructor方法中调用super方法，否则砸新建实例的时候就会报错，这是因为子类没有自己的this对象，而是继承父类的this对象，如果不调用super方法，子类就得不到this对象。
### 13.2 Object.getPrototypeOf()
Object.getPrototypeOf方法可以用来从子类上来获取父类，由此判定一个类是否继承了另一个类。

```javascript
 let isExtends = Object.getPrototypeOf(ColorPoint);
 console.log(isExtends);
```
## 14. Set和Map数组
### 14.1 Set
#### 基本用法
ES6几桶了一种新的数据结构Set，他类似数组，但是成员的值都是唯一的，没有重复的值，Set本身是一个构造函数，用来生成Set数据结构。

```javascript
const s = new Set();
[2,3,4,5,5,2,1].forEach(x => s.add(x));
for(let i of s){
    console.log(i);// 2 3 4 5 1
}
```
上述代码就可以看出通过add方法向Set结构中加入成员，结果表明Set结构不会添加重复的值。
Set函数可以接受一个数组（或者具有iterable接口的其他数据 结构）作为参数 ，用来初始化

```javascript
const  set = new Set([1,2,3,4,4]);
console.log([...set]);// (4) [1, 2, 3, 4]

const items = new Set([1,2,1,1,5,5,5,5,]);
console.log(items.size);// 3
```
上述代码也说明了一种去除数组重复成员的方法
```javascript
[...new Set(array)]
```
向Set加入值的时候，不会发生类型转换，也就是说，a和'a'是两个不同的值。另外，对象总是不相等的，

```javascript
let obj = {};
let objSet = new Set();
objSet.add(obj);
console.log(objSet.size);// 1
objSet.add(obj);
console.log(objSet.size);// 1

let objSet1 = new Set();
objSet1.add({});
console.log(objSet1.size)// 1
objSet1.add({});
console.log(objSet1.size)// 2
```
> 上述代码说明两个空对象是不相等的，如果传入同一个new出来的对象实例，那么就会被视为相同。
#### Set实例的属性和方法
Set结构的实例有以下属性
- Set.prototype.constructor：构造函数，默认就是Set函数。
- Set.prototype.size：返回Set的实例的成员总数。


Set实例的方法分为操作方法（用于操作数据）和遍历方法（用于遍历成员）
- add(value)：添加某一个值，返回Set结构本身。
- delete(value)：删除某一个值，返回一个布尔值，表示是否删除成功。
- has(value)：返回一个布尔值，表示该值是否为Set的成员。
- clear()：清除所有成员，没有返回值。


```javascript
let setMethod = new Set();
setMethod.add(1).add(2).add(2);// 2被添加了两次，
console.log(setMethod.size);// 2
console.log(setMethod.has(2));// true
console.log(setMethod.delete(2));// true
```
#### Array.from方法可以将Set结构转为数组

```javascript
const itemsSet = new Set([1,2,3,4,5]);
const  arry = Array.from(itemsSet);
console.log(arry);// (5) [1, 2, 3, 4, 5]
```
#### 数组去重的一种方法

```javascript
 function dedupe (array){
    return Array.from(new Set(array));
}
dedupe([1,2,1,2,1,3,1,5,1]);
```
#### 遍历操作
Set结构的实例有四个遍历方法，用于遍历成员
- key()：返回键名的遍历器
- values()：返回键值的遍历器
- entries()：返回键值对的遍历器
- forEach()：使用回调函数遍历每一个成员

> 说明：Set的遍历顺序就是插入顺序。

```javascript
let setTest = new Set(['张三','李思思','刘亦菲']);
    
for(let iten of setTest.keys()){
    console.log(iten);// 张三 李思思 刘亦菲
}
for(let iten of setTest.values()){
    console.log(iten);// 张三 李思思 刘亦菲
}
for(let iten of setTest.entries()){
    console.log(iten)// ["张三", "张三"] ["李思思", "李思思"] ["刘亦菲", "刘亦菲"]
}
```
由于Set结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys方法和values方法一样。
Set结构的实例默认可以遍历，他的默认遍历器函数就是他的values方法，意味着遍历时可以省略values()方法。

```javascript
let setSim = new Set(['red','green','blue']);
for(let x of setSim){
    console.log(x);
}
```
#### forEach()
Set结构和数组一样，也拥有forEach方法，用于对每个成员执行某种操作，没有返回值。

```javascript
setSim.forEach((value,key)=>console.log(key,value))
```
#### 遍历的应用
扩展运算符（...）内部使用for of 循环，所以也可以用于Set结构

```javascript
let arr = [...setSim];
console.log(arr);
```
扩展运算符和Set结合就可以去除数组中的重复成员

```javascript
let setArr = [3,54,1,5,4,1,5,4,8];
let unique = [...new Set(setArr)];
console.log(unique);// [3, 54, 1, 5, 4, 8]
```
而且数组的map和filter方法也可以间接用于Set。

```javascript
let setMap = new Set([1,2,3]);
setMap = new Set([...setMap].map(x => x*2));
console.log(setMap); // Set(3) {2, 4, 6}

let setFilter = new Set([1,2,3,4,5,6]);
setFilter = new Set([...setFilter].filter(x =>(x % 2)==0));
console.log(setFilter);// Set(3) {2, 4, 6}
```
因此，可以使用Set可以很容易的实现并集，交集和差集。

```javascript
let a = new Set([1,2,3]);
let b = new Set([4,3,2]);
//并集
let union = new Set([...a,...b]);
console.log(union);// Set(4) {1, 2, 3, 4}
//交集
let intersect = new Set([...a].filter(x =>b.has(x)));
console.log(intersect);// Set(2) {2, 3}
//差集,a对b的差集
let difference = new Set([...a].filter(x => !b.has(x)));
console.log(difference);// Set(1) {1}
```
#### WeakSet
WeakSet结构与Set类似，也是不重复的值的集合，但是，它与Set的不同之处在于WeakSet的成员只能是对象，而不能是其他类型的值。
### 14.2 Map
#### 含义和基本用法
JavaScript的对象（Object），本质上市键值对的集合（Hash结构），但是传统上只能用于字符串当键，这给他的使用带来限制。因此在ES6中，提出了Map的数据结构，他类似对象。也是键值对的集合。但是，键的范围不限制与字符串，各种类型的值（包括对象）都可以当做键，也就是说，Object结构提供了‘字符串-值’的对应，Map提供了‘值-值’的对应。

```javascript
const m = new Map();
const obj = {word:'Hello word'};
m.set(obj,'content');
console.log(m.get(obj));// content
console.log(m.has(obj));// true
console.log(m.delete(obj));// true
```
上述代码使用了Map结构的set方法，将对象obj作为m的一个键，然后使用get方法读取obj键的值，使用has方法判断是否灿在obj这个键，使用delete删除obj这个键。展示了如何向一个Map中添加成员，作为构造函数，Map也接受一个数组作为参数，该数组的成员是一个表示键值对的数组。

```javascript
const map = new Map([
        ['name','zhangsan'],
        ['title','Author']
]);
console.log(map.size);// 2
console.log(map.has('name'));// true
console.log(map.get('name'));// zhangsan
console.log(map.has('title'));// true
console.log(map.get('title'));// Author
```
上面代码在新建Map实例时，就指定了两个键name和title。
如果对同一个键进行多次赋值，后面的值将会覆盖前面的值。

```javascript
const mapTest = new Map();
mapTest.set(1,'A');
mapTest.set(1,'B');
console.log(mapTest.get(1));// B
```
注意：只有对同一个对象的引用，Map结构才将其视为同一个键。

```javascript
const unmap = new Map();
unmap.set(['a'],111);
console.log(unmap.get(['a']));// undefined
```
上述代码的set和get方法，表面是针对同一个键，但实际上是这两个值的内存地址不一样，因此get方法无法取到该键。
同理，同样的值的两个实例，在Map结构中被视为两个键。

```javascript
const map1 = new Map();
const k1 = ['a'];
const k2 = ['a'];
map1.set(k1,111);
map1.set(k2,222);
console.log(map1.get(k1));// 111
console.log(map1.get(k2));// 222
```
上述代码中变量k1和k2的值是一样的，但是在Map中他们是两个不同的键。由此可知，
* Map的键实际和内存地址绑定的，只要内存地址不一样，就视为两个键。
* 如果Map的键是一个简单类型的值（数字，字符串，布尔值），则只要这两个值严格相等，Map将其视为一个键，比如0和-0就是一个键，布尔值true和字符串true就是两个不同的键，
* 另外，undefined和null也是两个不同的键，

```javascript
let map2 = new Map();
map2.set(-0,123);
console.log(map2.get(+0));// 123
map2.set(true,1);
map2.set('true',2);
console.log(map2.get(true));// 1
console.log(map2.get('true'));// 2
map2.set(undefined,3);
map2.set(null,4);
console.log(map2.get(undefined));// 3
console.log(map2.get(null));// 4
```
#### 实例的属性和操作方法
- size属性：返回map结构的成员总数。
- set(key,value)：设置键名和对应的键值，然后返回整个Map结构，如果key已经存在值，则键值将会更新，否则生成新的键。由于set方法返回的是当前map对象，所以可以采用链式写法。
- get(key)：读取对应key的值，如果找不到key，就返回undefined。
- has(key)：返回一个布尔值，表示某一个键是否存在当前的Map对象中。
- delete(key)：删除一个键，返回删除结果，成功返回true，否则，返回false。
- clear()：清除所有成员，没有返回值


#### 遍历方法
Map结构原生提供了三个遍历器生成函数和一个遍历方法
- keys()：返回所有的键名的遍历器。
- values()：返回键值的遍历器。
- entries()：返回所有成员的遍历器。
- forEach()：遍历Map的所有成员

需要注意的是：map的遍历顺序就是插入顺序。

```javascript
 const map3 = new Map([
        ['F','no'],
        ['T','yes']
]);
for(let x of map3.keys()){
    console.log(x);// F T
}
for(let x of map3.values()){
    console.log(x);// no yes
}
for(let x of map3.entries()){
    console.log(x[0],x[1]);// F no T yes
}
for(let[key,value] of  map3.entries()){
    console.log(key,value);// F no T yes
}
```
map结构转为数组结构，比较快速的办法就是使用扩展运算符（...）

```javascript
const map4 = new Map([
        [1,'one'],
        [2,'two'],
        [3,'three']
]);
console.log([...map4.keys()]);// [1, 2, 3]
console.log([...map4.values()]);// ["one", "two", "three"]
console.log([...map4.entries()]);//  [Array(2), Array(2), Array(2)]
console.log([...map4]);//  [Array(2), Array(2), Array(2)]
```
结合数组的map方法，filter方法，可以实现Map的遍历和过滤，（Map本身并没有map和filter方法）

```javascript
const  map5 = new Map();
map5.set(1,'a');
map5.set(2,'b');
map5.set(3,'c');
const mapfilter = new Map([...map5].filter(([k,v]) => k <3));
console.log(mapfilter);// Map(2) {1 => "a", 2 => "b"}
const mapMethod = new Map(
        [...map5].map(([k,v]) => [k*2,`Hello ${v}`])
)
console.log(mapMethod);// Map(3) {2 => "Hello a", 4 => "Hello b", 6 => "Hello c"}
```
此外，map还有一个forEach方法，与数组类似，实现遍历。

```javascript
map5.forEach((v,k,m)=>console.log(`key:${k},value:${v}`));
// key:1,value:a key:2,value:b key:3,value:c
```
与其他数据结构互相转换


- Map转数组：使用扩展运算符
- 数组转Map：将数组传入Map构造函数中，就可以转为Map
- Map转对象：如果所有的Map的键都是字符串，他就可以转为对象


```javascript
function maptoObject(map) {
    let obj = Object.create(null);
    for(let [k,v] of map){
        obj[k] = v;
    }
    return obj;
}
const myMap = new Map();
myMap.set('yes',true);
myMap.set('no',false);
console.log(maptoObject(myMap));
```
- 对象转Map
```javascript
  function objtoMap(obj){
        let map = new Map();
        for(let k of Object.keys(obj)){
            map.set(k,obj[k]);
        }
        return map;
    }
    let obj1 = {yes:true,no:false};
    console.log(objtoMap(obj1));// Map(2) {"yes" => true, "no" => false}
```
- Map转为JSON

Map转为JSON要区分两种情况，一种情况是：Map的键名都是字符串，这时可以选择转为对象JSON。先将Map转为对象，在将对象转为JSON

```javascript
function maptoObject(map) {
    let obj = Object.create(null);
    for(let [k,v] of map){
        obj[k] = v;
    }
    return obj;
}
function maptoJson(map){
    return JSON.stringify(maptoObject(map))
}
let mapJson = new Map().set('yes',true).set('no',false);
console.log(maptoJson(mapJson));// {"yes":true,"no":false}
```
还有一种情况就是：Map的键有非字符串，这时候可以选择转为数组JSON

```javascript
 function maptoArrJson(map){
    return JSON.stringify([...map]);
}
let arrmap = new Map().set(true,7).set({foo:2},'abc');
console.log(maptoArrJson(arrmap));// [[true,7],[{"foo":2},"abc"]]
```
## 15 数组的扩展
### 15.1 扩展运算符
扩展运算符在前面也已经介绍过了，也叫三点运算符，将一个数组转为用逗号分隔的参数序列。
	
```javascript
console.log(...[1,2,3]);
```
#### 替代函数的apply方法
由于扩展运算符可以展开数组，所以就不需要apply方法，

```javascript
//ES5的写法
function f(x,y,z){
    console.log(x,y,z);// 0 1 2
};
var args = [0,1,2];
f.apply(null,args);
// ES6的写法
function fes6(x,y,z) {
    console.log(x,y,z);
}
fes6(...args);// 0 1 2 
```
运用扩展运算符实现的一个小例子，求一个数组中最大的元素

```javascript
// ES5的写法
var max1 =  Math.max.apply(null,[14,85,95]);
console.log(max1);
//ES6的写法
```
   let max2 = Math.max(...[14,85,95]);
    console.log(max2);
使用push方法，将一个数组添加到另一个数组的尾部

```javascript
//ES5的写法
var arr1 = [0,1,2];
var arr2 = [3,4,5];
Array.prototype.push.apply(arr1,arr2);
console.log(arr1);// [0, 1, 2, 3, 4, 5]
//ES6的写法
let arr3 = [0,1,2];
let arr4 = [3,4,5];
arr3.push(...arr4);
console.log(arr3);// [0, 1, 2, 3, 4, 5]
```
#### 扩展运算符的应用
##### 复制数组
数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆了一个全新的数组。
```javascript
	const a1 = [1,2];
    const a2 = a1;
    a2[0] = 3;
    console.log(a1[0]);// 3
```
上述代码中，a2并不是a1的克隆，而是指向同一份数据的另一个指针，修改a2，回直接导致a1的变化，
	
```javascript
const a3 = [1,2];
const a4 = [...a3];
const [...a5] = a3;
```
使用扩展运算符，就可以很方便的实现数组的克隆，这个时候你修改a4或者a5的值，不会影响a3的值。
##### 合并数组
使用扩展运算符可以很方便的实现数组的合并

```javascript
//ES5的写法
var array1 = [1,2];
var array2 = [3,4];
var array3 = [5,6];
console.log(array1.concat(array2));//  [1, 2, 3, 4]
console.log(array1.concat(array2,array3));// [1, 2, 3, 4, 5, 6]
//ES6的写法
console.log([...array1,...array2,...array3]);// [1, 2, 3, 4, 5, 6]
```
##### 与解构赋值结合
扩展运算符与解构赋值相结合，可以生成数组
	
```javascript
const [first,...end] = [1,2,3,4,5];
console.log(first);// 1
console.log(end);// [2, 3, 4, 5]
```
如果将扩展运算符用于数组赋值，只能放在参数的末尾，否则就会报错。

```javascript
const [...test,last] = [1,2,3,4,5];// 报错
```
##### 扩展运算符将字符串转为真正的数组

	[...'hello'] 
总结：扩展运算符内部调用的是数据结构的Iterator接口，因此只有具有Iterator接口的对象，才能使用扩展运算符，否则就不能使用。
#### Array.from()
Array.from()方法可以将两类对象转为真正的数组

```javascript
let arrayLike = {
    '0':'A',
    '1':'B',
    '2':'C',
    length:3
}
console.log(Array.from(arrayLike));// ["A", "B", "C"]
```
#### Array.of()
Array.of方法用于将一组值，转换为数组，是为了弥补Array构造函数的不足
	
```javascript
console.log(Array());// []
console.log(Array(3));// [empty × 3],length:3
console.log(Array(4,8,5));// [4, 8, 5]
```
上述代码中Array的参数个数有差异，说明只有参数个数大于1才能生成新的数组，of方法就是解决这个问题的。

```javascript
console.log(Array.of(3));// [3]
console.log(Array.of(3,5,8));//  [3, 5, 8]
console.log(Array.of(8).length);// 1
```
Array.of()方法基本可以代替Array()或者new Array(),该方法总是返回一个新的数组，如果没有参数就会返回一个空的数组。
#### 数组实例copyWithin()
数组实例的copyWithin()方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组，也就是说，使用这个方法，会修改当前数组。

```javascript
Array.prototype.copyWithin(target,start = 0,end = this.length)
```
该方法的参数说明
- target(必须)：从该位置开始替换数据，如果为负值，表示倒数。
- start(可选)：从该位置开始读取数据，默认为0，如果为负值，表示倒数。
- end(可选)：到该位置前停止读取数据，默认是数组的长度，如果为负值，表示倒数。

这三个参数都应该是数值，如果不是，会自动转为数值。

```javascript
 let carr = [4,8,6,7,9,2].copyWithin(0,3);
 console.log(carr);// [7, 9, 2, 7, 9, 2]
```
上述代码表示：从3号为位开始复制，替换0位以后的数据。
#### 数组实例的find()方法和findIndex()方法
数组实例的find方法用于找到第一个符合条件的数组的成员，它的参数是一个回调函数，所有的数组成员都将依次执行这个函数

```javascript
[8,4,5,7,101].find((n) => n >100);

let arrnew = [4,8,6,85,101].find(function(v,i,a){
        return v > 100;
    });
```
#### 数组实例的fill()
fill方法使用给定值，填充一个数组。
```javascript
let farr = ['a','b','c'].fill(7);
console.log(farr);// [7, 7, 7]
```
fill方法用于空数组的初始化就很方便，该方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。
#### 数组实例的include()方法
该方法返回一个布尔值，表示每一个数组是否包含给定的值，与字符串的include方法类似，该方法的第二个参数表示搜索的起始位置。如果为负数，表示倒数。

`ES6的相关知识就学习结束了，感谢阮一峰出版的《ECMAScript 6 入门》书籍。值得购买，作为前端人们书籍还是不错的。`





















