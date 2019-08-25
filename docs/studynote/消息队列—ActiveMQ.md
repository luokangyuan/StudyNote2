<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">消息队列—ActiveMQ（使用篇）</h1>



> 撸一门技术，必先登其官网，扒其皮，喝其血

**官网地址**：[https://activemq.apache.org/](<https://activemq.apache.org/>)

**文档地址**：[https://activemq.apache.org/components/classic/documentation](https://activemq.apache.org/components/classic/documentation)

## 1.1.面试题

* 为什么需要引入**消息队列**？**消息队列**的优点和缺点？`Kafka`、`ActiveMQ`、`RabbitMQ`、`RocketMQ` 都有什么优点和缺点？
* 如何保证**消息队列**的高可用？
* 如何解决**消息队列**的延时以及过期失效问题？**消息队列**满了以后该怎么处理？有几百万消息持续积压几小时，说说怎么解决？
* 如何保证消息不被重复消费？（如何保证消息消费的幂等性）

## 1.2.MQ简介

`MQ`就是我们开发常说的消息队列，具体的落地产品有`Kafka`、`ActiveMQ`、`RabbitMQ`、`RocketMQ` 。`MQ`可以简单的拆分理解就是`消息`和`队列`。是系统之间的一种通信方式，`消息`就是和我们平时的微信消息一样，需要生成消息的生产者和看消息的消费者，`队列`可以理解为消息的一种数据存储结构。类比消息我们大致推理出`MQ`的具体落地产品应该具备如下的一些功能：

* 完善的发送消息和接收消息的`API`。
* 保证消息的高可用性。
* 消息的容错性和集群配置。
* 消息的持久化特性。
* 消息的延迟发送和定时消息投递。
* 消息的签收机制，确实消息是否被消费成功。

发送者将消息发送到消息服务器中，消息服务器将消息存放在不同的`队列或者主题`中，在合适的时候（`定时或者延时等`）发送给消费者，在这个过程中，**发送消息和消费消息是异步发生的**，同时还能满足**一对多**的通信模式。

## 1.3.为什么引入MQ

随着微服务的流行，一个庞大的系统就会被拆分为多个子系统，相互间进行依赖，系统之间的耦合就会变得越来越重，如果多个系统都依赖统一底层支撑服务，那么这个底层服务的压力就会变得很大，引入`MQ`的优点就在于可以解除上述问题，达到**解耦**、**异步**和**削峰**。

## 1.4.安装ActiveMQ

```bash
# 解压ActiveMQ
tar -zxvf apache-activemq-5.15.9-bin.tar.gz
```

### 普通的后台服务启动

```bash
# bin目录下启动ActiveMQ
[root@localhost bin]# ./activemq start
```

```bash
# 查看ActiveMQ服务是否启动
[root@localhost bin]# ps -ef|grep activemq|grep -v grep
```

> PS：检查是否启动的另外一种就是查看ActiveMQ的默认端口61616是否被占用

```bash
# 查看61616端口时候被占用
netstat -anp|grep 61616
```

```bash
tcp6       0      0 :::61616                :::*                    LISTEN      4273/java
```

> PS：如果出现找不到`netstat`命令，请使用`yum -y install net-tools`命令安装。

当然，查看的方法很多，例如还可以使用`lsof`命令

```bash
[root@localhost bin]# lsof -i:61616
```

> PS：如果出现找不到`lsof`命令，请使用`yum install lsof`命令安装。

### 指定日志文件的启动方式

首先，可以新建一个日志文件件，然后新建一个日志文件

```bash
[root@localhost log]# touch mq.log
```

```bash
# 指定日志文件夹的启动
[root@localhost bin]# ./activemq start > /opt/soft/apache-activemq-5.15.9/log/mq.log
```

```bash
# 查看日志
[root@localhost bin]# vim /opt/soft/apache-activemq-5.15.9/log/mq.log
```

### 控制台查看

首先，关闭`Linux`的防火墙

```bash
service iptables stop
```

> PS：如果存在关闭失败，可以具体原因具体Google，列举一个我关闭失败的一个解决方法

`Failed to start iptables.service: Unit iptables.service failed to load: No such file or directory.`

```bash
# 添加端口范围
firewall-cmd --permanent --add-port=1000-9000/tcp
```

```bash
# 安装iptables-services 
yum install iptables-services 
```

```bash
# 保存设置
service iptables save 
```

```bash
# 关闭
systemctl stop iptables.service
```

**访问控制台**：`http://192.168.1.12:8161/admin/`账号和密码都是`admin`，如下

![image-20190727131258654](http://image.luokangyuan.com/2019-07-27-051303.png)

## 1.5.队列HelloWord

### 一对一的队列生产消息

```java
 public static final String URL = "tcp://192.168.1.12:61616";
    public static final String DEFAULT_QUEUE = "queue01";

private void doProducer() throws JMSException {
    ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);
    Connection connection = factory.createConnection();
    connection.start();
    Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
    // 创建队列
    Queue queue = session.createQueue(DEFAULT_QUEUE);
    // 创建生产者
    MessageProducer producer = session.createProducer(queue);
    // 创建并发送消息
    for (int i = 1; i <= 10; i++) {
        TextMessage textMessage = session.createTextMessage("第" + i + "测试消息");
        producer.send(textMessage);
    }
    // 关闭资源
    producer.close();
    session.close();
    connection.close();
}
```

### 消费队列消息

```java
private static void doListenerConsumer() throws JMSException, IOException {
    ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);
    Connection connection = factory.createConnection();
    connection.start();
    Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
    // 创建队列
    Queue queue = session.createQueue(DEFAULT_QUEUE);
    // 创建消费者
    MessageConsumer consumer = session.createConsumer(queue);
    consumer.setMessageListener((message) -> {
        if (message instanceof TextMessage) {
            TextMessage textMessage = (TextMessage) message;
            try {
                System.out.println("接收到消息：" + textMessage.getText());
            } catch (JMSException e) {
                e.printStackTrace();
            }
        }
    });
    System.in.read();
    consumer.close();
    session.close();
    connection.close();
}
```

> PS：发送的消息格式是`TextMessage`，那么消费消息的格式也应该也之一致。
>
> 消息的队列模式中，如果多个消费者消费同一个队列，第一个消费者将消息消费后，后面启动的消费者将不会再消费消息，如果是多个消费者先启动，那么当生产者生产多个消息时候，消息就会被**多个消费者平均消费**。


## 1.6.主题Topic

生产者将消息发送到主题`Topic`中，每条消息存在多个消费者，与队列模式不同的是这个属于**一对多的模式**。类比现在的微信公众号，当然生产者和消费者存在**时间上的关系**，即消费者订阅生产者之前的消息，消费者不能消费。同时，`Topic`不保存消息，也就是说，没有消息没有被消费，那么这个消息就也就丢失了。

### topic的消息生产者

```java
public static final String URL = "tcp://192.168.1.12:61616";
public static final String TOPIC_NAME = "topic01";
private static void topicProducer() throws JMSException {
    ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);
    Connection connection = factory.createConnection();
    connection.start();
    Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
    // 创建topic
    Topic topic = session.createTopic(TOPIC_NAME);
    // 创建生产者
    MessageProducer producer = session.createProducer(topic);
    // 创建并发送消息
    for (int i = 1; i <= 10; i++) {
        TextMessage textMessage = session.createTextMessage("第" + i + "主题测试消息");
        producer.send(textMessage);
    }
    // 关闭资源
    producer.close();
    session.close();
    connection.close();
}
```

比较与`Queue`模式只是改变了两行代码：

```java
// 创建topic
Topic topic = session.createTopic(TOPIC_NAME);
// 创建生产者
MessageProducer producer = session.createProducer(topic);
```

### topic的消息消费者

```java
private static void doListenerConsumer() throws JMSException, IOException {
    ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory(URL);
    Connection connection = factory.createConnection();
    connection.start();
    Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
    // 创建主题
    Topic topic = session.createTopic(TOPIC_NAME);
    // 创建消费者
    MessageConsumer consumer = session.createConsumer(topic);
    consumer.setMessageListener((message) -> {
        if (message instanceof TextMessage) {
            TextMessage textMessage = (TextMessage) message;
            try {
                System.out.println("接收到消息：" + textMessage.getText());
            } catch (JMSException e) {
                e.printStackTrace();
            }
        }
    });
    System.in.read();
    consumer.close();
    session.close();
    connection.close();
}
```

比较与`Queue`模式只是改变了两行代码：

```java
// 创建主题
Topic topic = session.createTopic(TOPIC_NAME);
// 创建消费者
MessageConsumer consumer = session.createConsumer(topic);
```

## 1.7.Topic模式和Queue模式

| 比较纬度     | Topic模式                                                    | Queue模式                                                  |
| ------------ | ------------------------------------------------------------ | ---------------------------------------------------------- |
| 有无状态     | 无状态                                                       | 消息默认会以文件的形式保存在消息服务器上                   |
| 工作模式     | 发布-订阅模式，将消息推送给多个消费者                        | 类似负载均衡模式，多个消费者均分多个消息                   |
| 消息的完整性 | 没有消费者订阅就丢失该消息                                   | 不会丢失                                                   |
| 性能         | 消息服务器会将笑消息复制多份给订阅者，随着订阅者的增加，性能会有降低 | 单向一对一的消息发送，无论消费者的增加性能不会有太大的改变 |

## 1.8.JMS组成和属性

通过前面的调用`API`进行消息的发送和消费，使用的就是`JMS`，`JMS`即`Java`消息服务（`Java Message Service`）应用程序接口，是一个`Java`平台中关于面向消息中间件（`MOM`）的API，用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。`Java`消息服务是一个与具体平台无关的API，绝大多数`MOM`提供商都对`JMS`提供支持。简单讲就是使用消息中间件落地产品的一种套路规范，将上述使用的步骤总结一下就使用其它消息中间件产品，简单总结如下：

1. 创建一个`ConnectionFactory factory`。
2. 使用`ConnectionFactory factory` 创建一个`JMS`的`Connection connection`。
3. 启动`JMS`的`Connection connection`。
4. 使用`Connection connection`创建一个`JMS`的`Session session`。
5. 使用`Session session`创建生产者或者消费者。
6. 使用生产者生产消息或者使用消费者消费消息。
7. 最后，关闭所有的资源。

**JMS**规范中存在四个元素，**消息中间件落地产品（JMS Provider）**，**生产者（Producer）**，**消费者（Consumer）**，**消息（Message）**，重点说一下消息的组成部分，包括**消息头**，**消息体**，**消息属性**。

### 消息头的相关属性

```java
// 设置单条消息的传输模式-持久化和不持久化，持久化就是保证JMS产品出现故障后消息不会被丢失
textMessage.setJMSDeliveryMode(DeliveryMode.PERSISTENT);
// 设置消息的过期时间，默认不过期
textMessage.setJMSExpiration(1000L);
// 设置消息的优先级，值越大，优先级越高
textMessage.setJMSPriority(9);
// 设置消息的唯一标识，可用于避免消息的重复消费
textMessage.setJMSMessageID();
```

### 5种消息体数据格式

![image-20190727194559227](http://image.luokangyuan.com/2019-07-27-114606.png)

```java
// 键值对的消息格式
MapMessage mapMessage = session.createMapMessage();
// 字节消息格式
BytesMessage bytesMessage = session.createBytesMessage();
// 序列化的消息格式
ObjectMessage objectMessage = session.createObjectMessage();
```

### 消息属性

```java
// 发送消息的时候设置消息属性
textMessage.setStringProperty("","");
// 消费消息的时候获取到消息属性，可以对有消息属性的消息做特殊处理
textMessage.getStringProperty("");
```

> 消息属性就是为更好的标注某条消息的特殊性。

## 1.9.JMS的可靠性

### 持久化（`PERSISTENT`）

生产者生产的消息可以设置消息的**持久化**和**非持久化**，**持久化**：就是当生产者生产的消息还没被消费者消费前，`MQ`服务器宕机啦，然后重启`MQ`服务器后，消费者仍然能够消费未消费的消息，**非持久化**则不能消费，也就是造成啦消息的丢失。所以也可以说非持久化的消息是无用的。设置方法如下：

```java
// 消息的持久化
producer.setDeliveryMode(DeliveryMode.PERSISTENT);
// 消息的非持久化
producer.setDeliveryMode(DeliveryMode.NON_PERSISTENT);
```

#### `Queue`持久化和非持久化





#### `Topic`的持久化和非持久化







### 事务性



### 签收机制





## 参考资料

* [解决CentOS7关闭/开启防火墙出现Unit iptables.service failed to load: No such file or directory.](https://www.jianshu.com/p/739d6ab203c8)

