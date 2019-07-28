<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">IDEA插件使用推荐</h1>

> 工欲善其事，必先利其器

## 1.1.Visualvm Launcher

[使用 VisualVM 进行性能分析及调优](https://www.ibm.com/developerworks/cn/java/j-lo-visualvm/)

这个插件执行的是`jdk`安装目录中的`bin`目录下的`jvisualvm`工具。安装后配置命令路径即可

![image-20190728004239941](http://image.luokangyuan.com/2019-07-27-164244.png)

在方法上运行就可

![image-20190728004437999](http://image.luokangyuan.com/2019-07-27-164442.png)

监控结果如下

![image-20190728004545405](http://image.luokangyuan.com/2019-07-27-164548.png)

## 1.2.查看字节码

这个不是插件，只是在`IDEA`中配置`JDK`的工具，可以更好更方便的查看一个类对应的字节码，在`IDEA`中新增两个扩展命令。

![image-20190728004843740](http://image.luokangyuan.com/2019-07-27-164847.png)

`javap-v`命令的配置信息：

```bash
# Program 指定javap命令路径
/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home/bin/javap
# Arguments
-v $FileClass$
# Working directory
$OutputPath$
```

`javap-c`命令的配置信息：

```bash
# Program 指定javap命令路径
/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home/bin/javap
# Arguments
-c $FileClass$
# Working directory
$OutputPath$
```

贴一张设置图吧

![image-20190728005504851](http://image.luokangyuan.com/2019-07-27-165508.png)

使用方法如下图

![image-20190728005636456](http://image.luokangyuan.com/2019-07-27-165640.png)

## 1.3.Lombok Plugin

这个不多说，必备插件，干净了实体类和赋值方法

![image-20190728010757990](http://image.luokangyuan.com/2019-07-27-170803.png)

## 1.4.GsonFormat

根据`JSONObject`格式的字符串,自动生成实体类参数

![image-20190728011815626](http://image.luokangyuan.com/2019-07-27-171821.png)

可以修改字段的类型

![image-20190728011906692](http://image.luokangyuan.com/2019-07-27-171910.png)

![image-20190728012016755](http://image.luokangyuan.com/2019-07-27-172021.png)

个人觉得，字段少就完全不用了，这个会生成`getter和setter`方法，我们使用`Lombok`只需要一个注解就好，另外，我们还要对字段添加注释和`swagger`注释，如果是接收消息队列，字段又多就可以一键复制，生成。

## 1.5.SonarLint

必备的代码检查工具，当然还有阿里巴巴的`P3C`和`FindBug`

## 1.6.Rainbow Brackets

就是彩虹括号，这个看个人喜好，有人觉得太花哨刺眼，萝卜白菜各有所爱

![image-20190728010410051](http://image.luokangyuan.com/2019-07-27-170415.png)

## 1.7.Material Theme UI

代码高亮插件，我本地的代码高亮就是其中的一种风格。

> 欢迎补充