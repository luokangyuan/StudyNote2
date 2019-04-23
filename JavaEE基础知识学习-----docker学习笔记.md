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

```shell
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -p 9200:9200 -p 9300:9300 --name ES01 5acf0e8da90b
```

* docker ps：列出所有正在运行的容器
  * -a：列出所有的容器，包括运行的或者之前运行的。
* docker start 817cf3ed90e8：启动某个容器。
* docker restart 817cf3ed90e8：重启某个容器。
* docker stop 817cf3ed90e8：停止某一个容器。
* docker kill  817cf3ed90e8：强行停止某一个容器。
* docker rm 容器ID ：删除已经停止的容器。
  * docker rm -f $(docker ps -a -q) ：一次删除多个容器。
  * docker ps -a -q | xargs docker rm ：一次删除多个容器，采用命令组合的批处理模式。





