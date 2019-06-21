# 一、ActiveMQ消息中间件

<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">ActiveMQ消息中间件学习笔记</h1>

> 撸一门技术，必先登其官网，扒其皮，喝其血

消息中间件目前主流的有炙手可热的大数据的消息中间件`kafka`，老牌的`RabbitMQ`，阿里巴巴的`RocketMQ`和`Apache`的`ActiveMQ`，既然是消息中间件，可以类比我们平时使用的微信消息，我们可以知道消息应该是具备以下的一些特性：

* 发送消息和接受消息的一些操作的API
* 不可或缺的高可用性和容错性
* 必然会存在的消息数据的持久化特性
* 业务中常用的晚上同步数据的延时发送和定时发送
* 接收消息成功的签收机制
* 必然可以很轻松的与`SpringBoot/Spring`进行整合



**官网地址**：