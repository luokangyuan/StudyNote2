# 一、ActiveMQ消息中间件

<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">ActiveMQ消息中间件学习笔记</h1>

> 撸一门技术，必先登其官网，扒其皮，喝其血

**ActiveMQ官网地址**：[<http://activemq.apache.org>](<http://activemq.apache.org/>)

## 1.1.MQ简介

消息中间件目前主流的有炙手可热的大数据的消息中间件`kafka`，老牌的`RabbitMQ`，阿里巴巴的`RocketMQ`和`Apache`的`ActiveMQ`，既然是消息中间件，可以类比我们平时使用的微信消息，我们可以知道消息应该是具备以下的一些特性：

* 发送消息和接受消息的一些操作的API
* 不可或缺的高可用性和容错性
* 必然会存在的消息数据的持久化特性
* 业务中常用的晚上同步数据的延时发送和定时发送
* 接收消息成功的签收机制
* 必然可以很轻松的与`SpringBoot/Spring`进行整合

## 1.2.为什么出现MQ

为什么会有这么一个消息中间件技术的出现，技术的出现到成熟都是业务的需要，随着微服务的出现和流行，一个系统往往会依赖很多其它的系统，同时很多的业务系统也必须会依赖底层的基础服务，就会存在一个不多的系统发生数据改变的时候都会和底层基础支撑进行数据的同步更新，造成了**严重的耦合调用**问题，而且还是**同步模型**，一个系统交涉结束，下一个系统开始交涉，在高并发的情况下，就会出现一个**访问峰值**。消息中间件的出现就是解决上述的问题，将系统基础服务和业务服务进行**完全解耦**，使用**异步模型**进行数据的同步和**消峰**。降低业务系统的负担。

## 1.3.ActiveMQ安装

**官网下载安装包**：`apache-activemq-5.15.9-bin.tar.gz`

**解压安装包**：`tar -zxvf apache-activemq-5.15.9-bin.tar.gz`

**启动**：`[root@localhost bin]# ./activemq start`

> 提示：ActiveMQ启动的时候依赖jdk



## 1.4.Docker安装

