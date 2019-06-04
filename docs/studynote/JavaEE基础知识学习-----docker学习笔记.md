# 一、Docker学习笔记

> 撸一门技术，必先登其官网，扒其皮，喝其血

**官网地址：**[https://docs.docker.com](<https://docs.docker.com/>)

**中文网址：**[https://www.docker-cn.com](<https://www.docker-cn.com/>)

**Docker 中国官方镜像加速地址：**[https://www.docker-cn.com/registry-mirror](<https://www.docker-cn.com/registry-mirror>)

**Docker hub地址：**[https://hub.docker.com](<https://hub.docker.com/>)

## 1.1.简介

`Docker`是基于`Go`语言开发的一个项目，它的理念就是：“一次封装，到处运行”，基于我们常用的虚拟机而演变而来，也就是说`Docker`是一种虚拟化技术。诞生的原因是为了**解决了运行环境和配置问题软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术**。

## 1.2.安装

参看我写的另一篇[Elasticsearch学习笔记](<http://luokangyuan.com/elasticsearchchu-ji-pian/>)中的附录三。

## 1.3.名词说明

**Docker镜像**：镜像是一个只读的模版，我们使用镜像来创建我们的`Docker`容器，一个镜像可以创建多个实例，

**Docker容器**：容器就是使用镜像创建的一个个运行实例。类比`java`中的面向对象，容器就是对象，镜像就是类。

**Docker仓库**：就是存放`docker`镜像的地方。

## 1.4.常用命令

* **docker version**：查看`docker`版本。
* **docker info**：查看`docker`相信信息。
* **docker --help**：查看`docker`的命令说明。

### 1.4.1.docker镜像命令

* **docker images**：查看本机里已经存在的docker镜像。
  * -a ：列出所有的docker镜像，含中间镜像层。
  * -q：只显示docker镜像的ID。
  * --digests：显示docker镜像的摘要信息。
  * --no-trunc ：显示完整的镜像信息 .
* **docker search redis**：搜索某个镜像。
  * -s : 列出收藏数不小于指定值的镜像。 
  * --no-trunc : 显示完整的镜像描述 。
  * --automated : 只列出 automated build类型的镜像。
* **docker pull redis**：拉取redis的最新版本。
* **docker rmi c6ffcb0ee97e**：删除镜像。
  * docker rmi  -f 镜像ID ：删除单个。
  * docker rmi -f 镜像名1:TAG 镜像名2:TAG  ：删除多个。
  * docker rmi -f $(docker images -qa) ：删除全部。

### 1.4.2.docker容器命令

* docker run [OPTIONS] IMAGE [COMMAND][ARG...] ：新建并启动容器
  * --name：指定容器的名字。
  * -d：后台运行。
  * -p：端口映射。

* docker ps：列出所有正在运行的容器
  * -a：列出所有的容器，包括运行的或者之前运行的。
* docker start 817cf3ed90e8：启动某个容器。
* docker restart 817cf3ed90e8：重启某个容器。
* docker stop 817cf3ed90e8：停止某一个容器。
* docker kill  817cf3ed90e8：强行停止某一个容器。
* docker rm 容器ID ：删除已经停止的容器。
  * docker rm -f $(docker ps -a -q) ：一次删除多个容器。
  * docker ps -a -q | xargs docker rm ：一次删除多个容器，采用命令组合的批处理模式。
* docker logs -f -t --tail 容器ID ：查看容器日志
  * -t 是加入时间戳 
  * -f 跟随最新的日志打印 
  * --tail 数字 显示最后多少条 
* docker top 容器ID ：查看容器内运行的进程 
* docker inspect 容器ID ：查看容器内部细节 
* docker cp  容器ID:容器内路径 目的主机路径 ：从容器内拷贝文件到主机上 

**启动ES示例：**

```bash
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -p 9200:9200 -p 9300:9300 --name ES01 5acf0e8da90b
```

### 1.4.3.启动容器的两种方式

**启动交互式容器**

```bash
docker run -it 容器名
```

**启动守护式容器**

```bash
docker run -d 容器名
```

> Docker容器后台运行,就必须有一个前台进程.

### 1.4.4.进入容器进行命令操作

可以使用启动交互式容器的命令`docker run -it 容器名`进入容器，使用`exit`容器停止退出，或者`ctrl+P+Q`容器不停止退出，退出后使用下列命令再次进入容器进行交互。

* docker attach 容器ID ：这种命令是直接进入容器并启动命令终端，不会启动新的进程。
* docker exec -it 容器ID bashShell （脚本：例如：ls -l /tmp）,在宿主机上执行脚本得到容器内部的信息。这个命令不会打开容器的命令终端。

## 1.5.Docker镜像

在docker Hub上通过命令`docker pull 镜像名`拉取的就是镜像，存在的疑问是拉取的镜像都是非常的大，如下：

![image-20190426175110943](http://image.luokangyuan.com/2019-04-26-095124.png)

### 1.5.1.什么是镜像

首先当我们拉取镜像的时候，便面的看到是一种怎样的情景，如下图：

![image-20190426175820030](http://image.luokangyuan.com/2019-04-26-095824.png)

可以很明显的看出，拉取了很多像是一层一层的文件，所以根据官方翻译，`docker`镜像是`UnionFS`（联合文件系统） ,是一种分层的文件系统，但是我们能看见的就是最外层的那一个文件系统，例如，当我们拉取`tomcat`镜像的时候，镜像中包括了如下的文件层次结构：

![image-20190426181602862](http://image.luokangyuan.com/2019-04-26-101607.png)

通过前面我们可以知道`docker`镜像是`UnionFS`分层文件系统，其中`UnionFS`包括了`bootloader`和`kernel`，`docker`镜像的底层是`bootFS`文件系统，和`linux`系统基本一样，所以可以容docker镜像都是缩小版本的`linux`系统。`docker`采用分层的文件系统的特点就是**共享资源**。有多个镜像都从相同的 base 镜像构建而来，那么宿主机只需在磁盘上保存一份base镜像，同时内存中也只需加载一份 base 镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。

但是。并不是我们可以对`docker`镜像进行修改，如上示例，我们能进入或者修改的是`tomcat`文件层，其他的不能修改，因此最外层能被修改的称为**容器层**，里面不能被修改的称之为**镜像层**。

### 1.5.2.镜像提交

我们可以修改某一个镜像，并且提交为一个新的镜像，例如，我们从`docker hub`中拉取了一个`tomcat`镜像，我们可以根据镜像生成容器实例，也可以将我们修改后的容器实例，生成一个镜像，命令如下：

```bash
docker commit -m=“提交的描述信息” -a=“作者” 容器ID 要创建的目标镜像名:[标签名]
```

## 1.6.容器数据持久化

容器中的数据可以通过容器数据卷来持久化数据和实现容器之间的数据共享。在容器中添加数据卷可以使用命令的方式，也可以使用`dockerFile`添加。

### 1.6.1.使用命令实现容器和主机间的数据持久化和共享

```bash
 docker run -it -v /宿主机绝对路径目录:/容器内目录      镜像名
```

![image-20190426203011307](http://image.luokangyuan.com/2019-04-26-123019.png)

如下图，在宿主机或者容器的数据就可以实现数据持久化和数据共享了，

![image-20190426203638874](http://image.luokangyuan.com/2019-04-26-123644.png)

和`redis`的数据持久化基本一样，当容器停止的时候，如果在主机数据持久化目录中对文件进行的改变，那么，当我们再次启动容器的时候，容器里面的数据卷就会进行一次数据的全量同步，保存数据的一致性。

> 命令默认的容器数据卷会有读写权限，只读权限命令如下

```bash
docker run -it -v /宿主机绝对路径目录:/容器内目录:ro 镜像名
```

只读并不是说主机的文件不能同步到数据数据卷中，而是说，在容器中不能对数据卷进行新增文件或者修改从主机同步过来的文件。

## 1.7.Docker File

前面说到容器和主机实现数据共享的方式，其中之一就是使用`docker file`的形式，我们先看一下官网的样子。

![image-20190426212752433](http://image.luokangyuan.com/2019-04-26-132759.png)

### 1.7.1.使用DockerFile实现数据持久化

* 可以在主机的根目录下新建`mydocker`目录。docker

```bash
makdir mydocker
```

* 编辑`dockerfile`文件

```bash
vi dockerfile
```

* 可在`Dockerfile`中使用`VOLUME`指令来给镜像添加一个或多个数据卷 .

```bash
# volume test
FROM centos
VOLUME ["/dataVolumeContainer1", "/dataVolumeContainer2"]
CMD echo "finished,-----------success"
CMD /bin/bash
```

* Doucker build根据dockerfile构建一个新的镜像

```bash
[root@localhost mydocker]# docker build -f /mydocker/dockerfile -t luokangyuan/centos .
```

![image-20190426214145291](http://image.luokangyuan.com/2019-04-26-134150.png)

从上图我们看出，`bulid`成一个新的镜像时候，执行我们编写的`dockerfile`文件，返回的id是多个，正好证实了docker镜像是分层式文件系统。

![image-20190426214347422](http://image.luokangyuan.com/2019-04-26-134351.png)

这个时候进去容器可以看到我们增加的两个数据卷。如下：

![image-20190426214519206](http://image.luokangyuan.com/2019-04-26-134523.png)

当时，我们的最终目的是容器中的数据卷和主机的数据目录进行数据的共享和持久化，所以，这两个容器的数据卷对应的主机数据目录在哪儿，可以使用命令`docker inspect 容器id`进行查看。如下：

![image-20190426215914404](http://image.luokangyuan.com/2019-04-26-135920.png)

### 1.7.2.容器数据卷

命名的容器挂载数据卷，其它容器通过挂载这个(父容器)实现数据共享，挂载数据卷的容器，称之为数据卷容器。

如上示例，我们先启动一个父容器实例：

```bash
docker run -it --name centos1 luokangyuan/cenos
```

我们，知道执行上述命令，会自动给容器中增加两个数据卷，如果我们在启动一个子容器实例，如下

```bash
docker run -it --name centos1 --volumes-from centos1 luokangyuan/cenos
```

这个时候，父容器中的数据卷被修改都会同步到子容器中，同理，子容器中的数据也会被同步到父容器中，我们还可以再建一个子容器，实现三者的数据共享，同时，如果这个时候父容器停止了，不影响子容器的数据共享。

> 结论：容器之间配置信息的传递，数据卷的生命周期一直持续到没有容器使用它为止 

## 1.8.docker的常用安装

### 1.8.1.安装mysql

**拉取mysql镜像**

![image-20190426230118015](http://image.luokangyuan.com/2019-04-26-150122.png)

**启动mysql**

```bash
[root@localhost /]# docker run -p 3306:3306 --name mysql -v /luo/mysql/conf:/etc/mysql/conf.d -v /luo/mysql/logs:/logs -v /luo/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```

**进入docker中的mysql容器中**

```bash
[root@localhost /]# docker exec -it fbfd7ba51893 /bin/bash
```

**进入mysql**

![image-20190426231259610](http://image.luokangyuan.com/2019-04-26-151304.png)

**外部直接连接即可**

![image-20190426231637973](http://image.luokangyuan.com/2019-04-26-151642.png)

### 1.8.2.安装redis

使用命令`docker pull redis`拉取最新版本，然后启动

```bash
docker run -p 6379:6379 -v /luo/myredis/data:/data -v /luo/myredis/conf/redis.conf:/usr/local/etc/redis/redis.conf -v redis:latest redis-server /usr/local/etc/redis/redis.conf --appendonly yes
```

## 1.9.本地镜像推送到阿里云

自行百度