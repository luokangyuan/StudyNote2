# SpringCloud概述

## SpringCloud是什么

SpringCloud，基于SpringBoot提供的一套微服务解决方案，包括服务注册与发现，配置中心，全链路监控，服务网关，负载均衡，等组件。换句话说是分布式微服务架构下的一站式解决方案，是各个微服务架构落地技术的集合体，俗称微服务全家桶。

## SpringBoot和SpringCloud

* SpringBoot专注于快速方便的开发单个个体微服务
* SpringCloud是关注全局的微服务协调整理治理框架，它将SpringBoot开发的单体微服务整合并管理，为各个微服务之间提供配置管理，服务发现，路由，分布式会话等集成服务
* SpringBoot可以离开SpringCloud独立的开发项目，但是SpringCloud离不开SpringBoot，属于依赖关系
* SpringBoot专注于快速，方便的开发单个微服务个体，SpringCloud关注全局的服务治理框架

## Double和SpringCloud

首先可以在GitHub上看到二者的活跃度，其次是比较各功能组件的支持情况，最大的区别在于SpringCloud抛弃了Dubbo的RPC通信，采用的是HTTP的REST方式，如下：

|        | Dobbo         | SpringCloud                  |
| ------ | ------------- | ---------------------------- |
| 服务注册中心 | Zookeeper     | SpringCloud Netflix Eureka   |
| 服务调用方式 | RPC           | Rest API                     |
| 服务监控   | Dubbo-monitor | Spring Boot Admin            |
| 断路器    | 不完善           | Spring Cloud Netflix Hystrix |
| 服务网关   | 无             | Spring Cloud Netflix Zuul    |
| 分布式配置  | 无             | Spring Cloud Config          |
| 服务跟踪   | 无             | Spring Cloud Sleuth          |
| 消息总线   | 无             | Spring Cloud Bus             |
| 数据流    | 无             | Spring Cloud Stream          |
| 批量任务   | 无             | Spring Cloud Task            |

## SpringCloud资料

SpringCloud各个组件的文档：https://springcloud.cc/spring-cloud-netflix.html

SpringCloud中文API：https://springcloud.cc/spring-cloud-dalston.html

# SpringCloud实践准备

## 项目技术版本

SpringCloud版本：Dalston.SR1，SpringBoot版本：1.5.9

## 项目说明

项目是使用SpringCloud将四个工程进行整合，microservicecloud整体父工程Project，microservicecloud-api公共子模块Module，microservicecloud-provider-dept-8001部门微服务提供者Module，microservicecloud-consumer-dept-80部门微服务消费者Module。

## 1.父类项目创建

在逻辑视图中选择new-Maven Project-勾上创建简单项目-选择pom方式

### pom.xml文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.luo.springcloud</groupId>
	<artifactId>microservicecloud</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>pom</packaging>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<junit.version>4.12</junit.version>
		<log4j.version>1.2.17</log4j.version>
		<lombok.version>1.16.18</lombok.version>
	</properties>
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-dependencies</artifactId>
				<version>Dalston.SR1</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-dependencies</artifactId>
				<version>1.5.9.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>5.0.4</version>
			</dependency>
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>druid</artifactId>
				<version>1.0.31</version>
			</dependency>
			<dependency>
				<groupId>org.mybatis.spring.boot</groupId>
				<artifactId>mybatis-spring-boot-starter</artifactId>
				<version>1.3.0</version>
			</dependency>
			<dependency>
				<groupId>ch.qos.logback</groupId>
				<artifactId>logback-core</artifactId>
				<version>1.2.3</version>
			</dependency>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>${junit.version}</version>
				<scope>test</scope>
			</dependency>
			<dependency>
				<groupId>log4j</groupId>
				<artifactId>log4j</artifactId>
				<version>${log4j.version}</version>
			</dependency>
		</dependencies>
	</dependencyManagement>
</project>


```

## 2.公共组件项目创建

在父项目上创建microservicecloud-api项目，注意是在microservicecloud上new一个maven module，packaging选择jar

### POM.xml文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.luo.springcloud</groupId>
    <artifactId>microservicecloud</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
  <artifactId>microservicecloud-api</artifactId>
  <dependencies><!-- 当前Module需要用到的jar包，按自己需求添加，如果父类已经包含了，可以不用写版本号 -->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-feign</artifactId>
		</dependency>
	</dependencies>
</project>
```

### Dept实体类

```java
public class Dept implements Serializable{
	private Long deptno; // 主键
	private String dname; // 部门名称、
	private String db_source; // 来自那个数据库，因为微服务可以一个服务对应一个数据库，同一个信息被存储到不同的数据库
	
	public Dept() {
		super();
	}
	public Dept(Long deptno, String dname, String db_source) {
		super();
		this.deptno = deptno;
		this.dname = dname;
		this.db_source = db_source;
	}
	public Long getDeptno() {
		return deptno;
	}
	public void setDeptno(Long deptno) {
		this.deptno = deptno;
	}
	public String getDname() {
		return dname;
	}
	public void setDname(String dname) {
		this.dname = dname;
	}
	public String getDb_source() {
		return db_source;
	}
	public void setDb_source(String db_source) {
		this.db_source = db_source;
	}
	@Override
	public String toString() {
		return "Dept [deptno=" + deptno + ", dname=" + dname + ", db_source=" + db_source + "]";
	}
```

当我们每次都需要创建一个实体类的getter，setter，toString和构造器等方法时，如果增加一个字段就要重新生成方法，为了简化这种重复的操作，我们在前面的pom中引入了lombok，同样的实体类，使用方法如下

```java
@SuppressWarnings("serial")
@AllArgsConstructor
@NoArgsConstructor
@Data
@Accessors(chain=true)
public class Dept implements Serializable{
	private Long deptno; // 主键
	private String dname; // 部门名称
    // 来自那个数据库，因为微服务可以一个服务对应一个数据库，同一个信息被存储到不同的数据库
	private String db_source; 
}
```

#### lombok安装方法

拷贝lombok-1.16.18.jar到Eclipse目录下，执行java -jar D:\javasoft\eclipse-jee-neon-3-win32-x86_64\eclipse\ombok-1.16.18.jar，然后，弹框中选择Eclipse安装目录，选择install即可。

#### lombok注解使用

```properties
@Data ：注解在类上；提供类所有属性的 getting 和 setting 方法，此外还提供了equals、canEqual
@Setter：注解在属性上；为属性提供 setting 方法
@Getter：注解在属性上；为属性提供 getting 方法
@Log4j ：注解在类上；为类提供一个 属性名为log 的 log4j 日志对象
@NoArgsConstructor：注解在类上；为类提供一个无参的构造方法
@AllArgsConstructor：注解在类上；为类提供一个全参的构造方法
@Accessors(chain=true)：可以使用链式写法
```

#### lombok测试

```java
@SuppressWarnings("serial")
@AllArgsConstructor
@NoArgsConstructor
@Data
@Accessors(chain=true)
public class Dept implements Serializable{
	
	private Long deptno; // 主键
	private String dname; // 部门名称、
	private String db_source; // 来自那个数据库，因为微服务可以一个服务对应一个数据库，同一个信息被存储到不同的数据库
	public static void main(String[] args) {
		Dept dept = new Dept();
		dept.setDeptno(12L).setDname("开发部").setDb_source("DB01");
	}
}
```

#### 注意内容

实体类必须实现Serializable接口

### 打包使用

公共组件模块写好后可以点击run as 选择maven clean ，然后在选择maven install。其他模块引用的方法如下

```xml
<groupId>com.atguigu.springcloud</groupId>
<artifactId>microservicecloud-api</artifactId>
<version>0.0.1-SNAPSHOT</version>
```

## 3.部门微服务提供者

首先现在父类项目上new一个maven module,microservicecloud-provider-dept-8001修改pom.xml文件

### pom.xml文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<parent>
		<groupId>com.luo.springcloud</groupId>
		<artifactId>microservicecloud</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>

	<artifactId>microservicecloud-provider-dept-8001</artifactId>

	<dependencies>
		<!-- 引入自己定义的api通用包，可以使用Dept部门Entity -->
		<dependency>
			<groupId>com.luo.springcloud</groupId>
			<artifactId>microservicecloud-api</artifactId>
			<version>${project.version}</version>
		</dependency>
		<!-- actuator监控信息完善 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<!-- 将微服务provider侧注册进eureka -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
		</dependency>
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-core</artifactId>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jetty</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
		</dependency>
		<!-- 修改后立即生效，热部署 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
	</dependencies>

</project>
```

### application.yml文件

```yaml
server:
  port: 8001
  
mybatis:
  config-location: classpath:mybatis/mybatis.cfg.xml        # mybatis配置文件所在路径
  type-aliases-package: com.luo.springcloud.entities        # 所有Entity别名类所在包
  mapper-locations:
  - classpath:mybatis/mapper/**/*.xml                       # mapper映射文件
    
spring:
   application:
    name: microservicecloud-dept 
   datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包
    url: jdbc:mysql://localhost:3306/cloudDB01              # 数据库名称
    username: root
    password: 1234
    dbcp2:
      min-idle: 5                                           # 数据库连接池的最小维持连接数
      initial-size: 5                                       # 初始化连接数
      max-total: 5                                          # 最大连接数
      max-wait-millis: 200                                  # 等待连接获取的最大超时时间
      
eureka:
  client: #客户端注册进eureka服务列表内
    service-url: 
      defaultZone: http://localhost:7001/eureka
```

### mybatis下mybatis.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
	<settings>
		<setting name="cacheEnabled" value="true" /><!-- 二级缓存开启 -->
	</settings>
</configuration>
```

### SQL语句

```sql
DROP DATABASE IF EXISTS cloudDB01 ;

CREATE DATABASE cloudDB01 CHARACTER SET UTF8 ;

USE cloudDB01 ;

CREATE TABLE dept (
  deptno BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  dname VARCHAR (60),
  db_source VARCHAR (60)
) ;

INSERT INTO dept(dname,db_source) VALUES('开发部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('人事部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('财务部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('市场部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('运维部',DATABASE());
```

### dao接口

```java
@Mapper
public interface DeptDao {
	
	public boolean addDept(Dept dept);
	public Dept findById(Long id);
	public List<Dept> findAll();

}
```

### DeptMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.luo.springcloud.dao.DeptDao">
	<select id="findById" resultType="Dept" parameterType="Long">
		select deptno,dname,db_source from dept where deptno=#{deptno};
	</select>
	<select id="findAll" resultType="Dept">
		select deptno,dname,db_source from dept;
	</select>
	<insert id="addDept" parameterType="Dept">
		INSERT INTO dept(dname,db_source) VALUES(#{dname},DATABASE());
	</insert>
</mapper>
```

### DeptService

```java
public interface DeptService {
	public boolean add(Dept dept);
	public Dept get(Long id);
	public List<Dept> list();
}
```

### DeptServiceImpl

```java
@Service
public class DeptServiceImpl implements DeptService{
	@Autowired
	private DeptDao dao;

	@Override
	public boolean add(Dept dept) {
		return dao.addDept(dept);
	}

	@Override
	public Dept get(Long id) {
		return dao.findById(id);
	}

	@Override
	public List<Dept> list() {
		return dao.findAll();
	}
}
```

### DeptController

```java
@RestController
public class DeptController {
	@Autowired
	private DeptService service;
	
	@RequestMapping(value="/dept/add",method=RequestMethod.POST)
	public boolean add(@RequestBody Dept dept){
		return service.add(dept);
	}
	
	@RequestMapping(value="dept/get/{id}",method=RequestMethod.GET)
	public Dept get(@PathVariable("id") Long id){
		return service.get(id);
	}
	
	@RequestMapping(value="dept/list",method=RequestMethod.GET)
	public List<Dept> list(){
		return service.list();
	}
}
```

### 创建主启动类DeptProvider8001_App

```java
@SpringBootApplication
public class DeptProvider8001_App {
	public static void main(String[] args) {
		SpringApplication.run(DeptProvider8001_App.class, args);
	}
}
```

### 测试结果

输入http://localhost:8001/dept/list以JSON的方式返回数据

## 4.部门微服务消费者

首先现在父类项目上new一个maven module,microservicecloud-consumer-dept-80修改pom.xml文件

### POM.xml文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<parent>
		<groupId>com.luo.springcloud</groupId>
		<artifactId>microservicecloud</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>

	<artifactId>microservicecloud-consumer-dept-80</artifactId>
	<description>部门微服务消费者</description>

	<dependencies>
		<dependency><!-- 自己定义的api -->
			<groupId>com.luo.springcloud</groupId>
			<artifactId>microservicecloud-api</artifactId>
			<version>${project.version}</version>
		</dependency>
		<!-- Ribbon相关 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-ribbon</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<!-- 修改后立即生效，热部署 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
	</dependencies>
</project>
```

### application.yml文件

```yaml
server:
  port: 80
```

### ConfigBean注解类

```java
@Configuration
public class ConfigBean {
    @Bean
	public RestTemplate geRestTemplate(){
		return new RestTemplate();
	}
}
```

#### RestTemplate

RestTemplate提供了多种便捷访问远程Http服务的方法，是一种简单高效便捷的访问restful服务模板类，是Spring提供的用于访问Rest服务的客户端模板工具类集，使用方法如下

（url,requestMap,ResponseBean.class）三个参数分别代表Rest请求地址，请求参数，HTTP响应转换被转换的对象类型

### DeptController_Consumer

```java
@RestController
public class DeptController_Consumer {
	private static final String REST_URL_PREFIX = "http://localhost:8001";
	@Autowired
	private RestTemplate restTemplate;
	
	@RequestMapping(value="/consumer/dept/add")
	public boolean add(Dept dept){
		return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add", dept, Boolean.class);
	}
	
	@RequestMapping(value="/consumer/dept/get/{id}")
	public Dept get(@PathVariable("id") Long id){
		return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id, Dept.class);
	}
	
	@RequestMapping(value="/consumer/dept/list")
	public Dept list(){
		return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list/", Dept.class);
	}
}
```

### DeptConsumer80_App主类

```java
@SpringBootApplication
public class DeptConsumer80_App {
	public static void main(String[] args) {
		SpringApplication.run(DeptConsumer80_App.class, args);
	}
}
```

### 测试结果

http://localhost/consumer/dept/list

http://localhost/consumer/dept/get/2

http://localhost/consumer/dept/add?dname=AI

# Eureka服务注册与发现

## Eureka三大角色

* Eureka Server提供服务注册和发现
* Service Provider服务提供方将自身服务注册到Eureka， 从而使服务消费者能够找到
* Service Consumer服务消费方从Eureka获取注册服务列表，从而能够消费

## 1.Eureka Server注册

在上述项目的父工程中新建microservicecloud-eureka-7001，这个module是Eureka的服务中心

### POM.xml文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>com.luo.springcloud</groupId>
		<artifactId>microservicecloud</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>

	<artifactId>microservicecloud-eureka-7001</artifactId>
	<dependencies>
		<!--eureka-server服务端 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka-server</artifactId>
		</dependency>
		<!-- 修改后立即生效，热部署 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
	</dependencies>
</project>

```

### application.yml文件

```yaml
server: 
  port: 7001
eureka: 
  instance:
    hostname: localhost #eureka服务端的实例名称
  client: 
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: 
      #单机 defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/       #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

### EurekaServer主启动类

```java
@SpringBootApplication
@EnableEurekaServer// EurekaServer服务器端启动类，接收其它微服务注册进来
public class EurekaServer7001_App {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServer7001_App.class, args);
	}
}
```

### 测试EurekaServer

浏览器输入http://localhost:7001/，看到Spring Eureka界面表示成功，这个访问链接和程序中的application.yml配置吻合。

## 2.微服务注册

将microservicecloud-provider-dept-8001微服务注册到microservicecloud-eureka-7001中

### 修改microservicecloud-provider-dept-8001的POM.xml文件

```xml
<!-- 将微服务provider侧注册进eureka -->
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

### 修改microservicecloud-provider-dept-8001的application.yml文件

```yaml
eureka:
  client: #客户端注册进eureka服务列表内
    service-url: 
      defaultZone: http://localhost:7001/eureka
```

说明：defaultZone的地址对应Eureka Server服务注册中心的application.yml中的defaultZone路径

### microservicecloud-provider-dept-8001主程序类使用注解

```java
@SpringBootApplication
@EnableEurekaClient // 本服务启动后会注册到Eureka服务注册中心
public class DeptProvider8001_App {
	public static void main(String[] args) {
		SpringApplication.run(DeptProvider8001_App.class, args);
	}
}
```

### 测试是否注册成功

先启动Eureka服务注册中心microservicecloud-eureka-7001，启动微服务microservicecloud-provider-dept-8001，打开浏览器输入http://localhost:7001/，Application下出现**MICROSERVICECLOUD-DEPT**微服务名称，这个名称来源于microservicecloud-provider-dept-8001中application.ym文件中的配置属性，如下

```yaml
spring:
   application:
    name: microservicecloud-dept 
```

## 3.微服务常用设置

### 主机名称和服务名称修改

在Eureka中注册的微服务的Status的名称显示localhost或者显示电脑主机名，所以要修改服务的主机名称，修改方法如下，修改microservicecloud-provider-dept-8001中application.yml文件，修改后如下

```yaml
 instance:
    instance-id: microservicecloud-dept8001
```

### 访问信息有IP信息提示

修改microservicecloud-provider-dept-8001中application.yml文件，修改后如下

```yaml
  instance:
    instance-id: microservicecloud-dept8001
    prefer-ip-address: true     #访问路径可以显示IP地址 
```

### 微服务info内容详细信息

增加microservicecloud-provider-dept-8001中POM.xml文件

```xml
<!-- actuator监控信息完善 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

总的父工程microservicecloud修改pom.xml添加构建build信息

```xml
<build>
  <finalName>microservicecloud</finalName>
  <resources>
    <resource>
      <!-- 说明在src/main/resources目录下的配置文件 -->
      <directory>src/main/resources</directory>
      <filtering>true</filtering>
    </resource>
  </resources>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-resources-plugin</artifactId>
      <configuration>
        <delimiters>
          <!-- 表示以$开始和以$结束的表示方法 -->
          <delimit>$</delimit>
        </delimiters>
      </configuration>
    </plugin>
  </plugins>
</build>
```

修改microservicecloud-provider-dept-8001中application.yml文件，修改后如下

```yaml
info: 
  app.name: luokangyuan-microservicecloud
  company.name: www.luokangyuan.com
  build.artifactId: $project.artifactId$
  build.version: $project.version$
```

## 4.Eureka的自我保护机制

### 导致的原因

默认情况下，如果EurekaServer在一定的时间内没有接收到某一个微服务实例的心跳，EurekaServer将会注销该实例，页面就会看见一串红色提示，但是当网络分区发生故障时，微服务与EurekaServer无法进行正常的通信，此时本不应该注销这个微服务实例，这个时候，Eureka的自我保护机制就可以解决这个问题，当EurekaServer节点在短时间内丢失过多的客户端时（可能发生了网络故障），那么这个节点就会进入自我保护模式，一旦进入该模式，EurekaServer就会保护服务注册表中的信息，不再删除服务注册表中的数据（也就是不会注销任何微服务），当网络故障恢复后，该EurekaServer节点就会自动退出自我保护模式。

### 总结

在自我保护模式下，EurekaServer会保护服务注册表中的信息，不再注销任何服务实例，当它收到的心跳数重新到阈值以上，该EurekaServer就会自动退出自我保护模式，也就是宁可保留错误的服务注册信息，也不盲目的删除任何可能健康的服务实例。

## 5.服务发现

对于注册近Eureka里面的微服务，可以通过服务发现来获取该服务的信息

### 修改microservicecloud-provider-dept-8001的DeptController

```java
@Autowired
private DiscoveryClient client;

@RequestMapping(value = "/dept/discovery", method = RequestMethod.GET)
public Object discovery(){
  List<String> list = client.getServices();//得到Eureka中所有的微服务
  System.out.println("**********" + list);

  List<ServiceInstance> srvList = client.getInstances("MICROSERVICECLOUD-DEPT");
  for (ServiceInstance element : srvList) {
    System.out.println(element.getServiceId() + "\t" 
                       + element.getHost() + "\t" + element.getPort() + "\t"
                       + element.getUri());
  }
  return this.client;
}

```

### microservicecloud-provider-dept-8001主启动类添加注解

```java
@SpringBootApplication
@EnableEurekaClient // 本服务启动后会注册到Eureka服务注册中心
@EnableDiscoveryClient // 服务发现
public class DeptProvider8001_App {
	public static void main(String[] args) {
		SpringApplication.run(DeptProvider8001_App.class, args);
	}
}
```

### 自测试

启动服务注册中心microservicecloud-eureka-7001，再启动microservicecloud-provider-dept-8001，访问http://localhost:8001/dept/discovery可以得到这个服务的info信息，/dept/discovery接口就是microservicecloud-provider-dept-8001这个服务暴露给外部访问的接口。使用http://localhost:8001/dept/discovery测试，就是自己测试能不能使用

### 外部访服务暴露的接口

microservicecloud-consumer-dept-80调用microservicecloud-provider-dept-8001服务暴露在外的接口，修改microservicecloud-consumer-dept-80中的DeptController_Consumer，如下

```java
// 测试@EnableDiscoveryClient,消费端可以调用服务发现
@RequestMapping(value = "/consumer/dept/discovery")
public Object discovery(){
  return restTemplate.getForObject(REST_URL_PREFIX + "/dept/discovery", Object.class);
}
```

### 消费者访问接口测试

启动microservicecloud-consumer-dept-80访问http://localhost/consumer/dept/discovery得到8001微服务信息

### 总结

* microservicecloud-provider-dept-8001注册到EurekaServer服务中心
* microservicecloud-provider-dept-8001将Controller中的某一个方法暴露出去（提供服务发现）
* microservicecloud-consumer-dept-80中的Controller就可以调用微服务暴露出来的接口

# Eureka集群配置

microservicecloud-eureka-7001使EurekaServer服务注册中心，一旦这个出现问题，那么微服务就不能正常的工作，为防止这种情况，所以出现了集群，就是建立多个microservicecloud-eureka-7002，microservicecloud-eureka-7003等服务注册中心。

* 新建microservicecloud-eureka-7002，microservicecloud-eureka-7003服务注册中心
* 根据microservicecloud-eureka-7001的pom.xml修改7002和7003的pom.xml文件
* 复制7001的主程序启动类，并修改为7002,7003即可

## 修改映射配置

在7001注册中的application.yml文件中hostname，不能与7002,7003相同，所以要做映射配置

```yaml
eureka: 
  instance:
    hostname: localhost #eureka服务端的实例名称
```

**修改C:\Windows\System32\drivers\etc\host文件**,让127.0.0.1有三个别名

``` properties
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
127.0.0.1 eureka7003.com
```

## microservicecloud-eureka-7001中的yml修改

```yaml
server: 
  port: 7001
eureka: 
  instance:
    hostname: eureka7001.com #eureka服务端的实例名称
  client: 
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: 
      #单机 defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/       
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

## microservicecloud-eureka-7002中的yml修改

```yaml
server: 
  port: 7002
eureka: 
  instance:
    hostname: eureka7002.com #eureka服务端的实例名称
  client: 
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: 
      #单机 defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/       
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

## microservicecloud-eureka-7003中的yml修改

```yaml
server: 
  port: 7003
eureka: 
  instance:
    hostname: eureka7003.com #eureka服务端的实例名称
  client: 
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: 
      #单机 defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/       
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
```

## 修改dept微服务的yml文件

dept微服务会同时注册到7001,7002,7003服务注册中心

```yaml
eureka:
  client: #客户端注册进eureka服务列表内
    service-url: 
      #defaultZone: http://localhost:7001/eureka
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/ 
```

## 测试

* 访问eureka7001.com:7001
* 访问eureka7002.com:7002
* 访问eureka7003.com:7003


# Eureka和Zookeeper区别

## 遵循原则不同

**Eureka遵循AP原则，Zookeeper遵循CP原则**，C：强一致性，A：可用性，P：分区容错性

著名的CAP理论中提出，一个分布式系统不可能同时满足C(一致性)A(可用性)P(分区容错性)，由于分区容错性p是分布式系统中必须保证，因此只能在A和C之间权衡

## Zookeeper保证CP

在Zookeeper中存在一种情况下，当master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举，但是，选举leader的时间太长，且选举过程中这个Zookeeper集群是不可用的，这就导致在选举期间注册服务瘫痪，在云部署的环境中，因为网络问题使得Zookeeper集群失去master节点的可能性较大，虽然服务最终能够恢复，但是在漫长的选举时间导致的注册时间不可用是不能容忍的，当我们向注册中心查询注册列表时，可以忍受注册中心返回的是几分钟以前的注册信息，但是不能接收服务直接down不可用，也就是说，服务注册对可用性的要求高于一致性。

## Eureka保证AP

Eureka知道Zookeeper的不足，所以设计最初就保证可用性，Eureka各个节点都是平等的，几个节点的挂点不会影响其他正常节点的工作，剩余的节点仍然可以提供注册和查询服务，只不过不能保证查询的信息是最新的，除此之外，Eureka还有一种自我保护机制，当过多的节点没有正常的心跳时，那么Eureka就会认为客户端出现了网络故障，此时Eureka会

* Eureka不会从注册表中移除因为长时间没有收到心跳而应该过期的服务
* Eureka仍然能够接受新服务的注册和查询请求，但是不会同步到其他节点上（保证当前节点可用）
* 当网络稳定时，当前实例新的注册信息会被同步到其他节点上

# Ribbon负载均衡

## Ribbon概述

Spring Cloude Ribbon是基于Netfilx Ribbon实现的一套客户端 负载均衡的工具，简单说，Ribbon是Netfilix发布的开源项目，主要功能就是提供 **客户端的软件负载均衡算法**，将Netfilix的中间层服务连接在一起，Ribbon客户端组件提供了一系列完善的配置项如连接超时，重试等，简单说，就是在配置文件中列出Load Balance后面的所有机器，Ribbon会自动的帮助你基于某种算法规则（简单轮询，随机连接等）去连接这些机器，也可以使用Ribbon自定义负载均衡算法。LB，即负载均衡，在微服务或者分布式集群中常用的一种应用。负载均衡就是将用户的请求平摊的分配到多个服务上，从而达到HA，常见的负载均衡软件有Nginx，LVS，硬件F5等

## Ribbon配置初步

由于Ribbon是客户端的负载均衡工具，所以我们需要修改的是客户端项目microservicecloud-consumer-dept-80

### POM.xml文件

```xml
<!-- Ribbon相关 -->
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-ribbon</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

### 修改application.yml文件，添加Eureka的服务注册地址

```yaml
server:
  port: 80
eureka:
  client:
    register-with-eureka: false #自己不能注册
    service-url: 
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/  
```

### 修改客户端配置类

由于客户端使用restTemplate访问服务端中的数据接口，restTemplate配置在服务端的配置类中，所以修改如下

```java
@Configuration
public class ConfigBean {
	@Bean
	@LoadBalanced
	public RestTemplate geRestTemplate(){
		return new RestTemplate();
	}
}
```

### 修改客户端主程序启动类

```java
@SpringBootApplication
@EnableEurekaClient
public class DeptConsumer80_App {
	public static void main(String[] args) {
		SpringApplication.run(DeptConsumer80_App.class, args);
	}
}
```

### 修改客户端访问类DeptController_Consumer.java

```java
private static final String REST_URL_PREFIX = "http://MICROSERVICECLOUD-DEPT";
```

### 测试

启动7001,7002,7003三个服务注册中心，启动8001服务提供者，启动80客户端，使用http://localhost/consumer/dept/list可以渠道对应的数据，在DeptController_Consumer使用的是http://MICROSERVICECLOUD-DEPT服务名称来调用服务的接口，相比之前的http://localhost:8001，Ribbon和Eureka整合后，Consumer可以直接通过服务名称来调用服务，而不再关心地址和端口号。

## Ribbon负载均衡

 目前只有一个microservicecloud-provider-dept-8001服务提供者，为了实现Ribbon的负载均衡，所以我们需要多个服务提供者实例，新建microservicecloud-provider-dept-8002，microservicecloud-provider-dept-8003两个Module。参考8001的pom.xml文件修改8002,8003的pom.xml文件。拷贝8001中的所以类和配置文件mybatis和application.yml文件，将主启动类修改为对应的名字

### microservicecloud-provider-dept-8002服务提供者

#### 使用的数据库SQL语句

``` sql
DROP DATABASE IF EXISTS cloudDB02 ;

CREATE DATABASE cloudDB02 CHARACTER SET UTF8 ;

USE cloudDB02 ;

CREATE TABLE dept (
  deptno BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  dname VARCHAR (60),
  db_source VARCHAR (60)
) ;

INSERT INTO dept(dname,db_source) VALUES('开发部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('人事部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('财务部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('市场部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('运维部',DATABASE());
```

#### Application.yml文件

```yaml
server:
  port: 8002
  
mybatis:
  config-location: classpath:mybatis/mybatis.cfg.xml        # mybatis配置文件所在路径
  type-aliases-package: com.luo.springcloud.entities        # 所有Entity别名类所在包
  mapper-locations:
  - classpath:mybatis/mapper/**/*.xml                       # mapper映射文件
    
spring:
   application:
    name: microservicecloud-dept 
   datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包
    url: jdbc:mysql://localhost:3306/cloudDB02              # 数据库名称
    username: root
    password: 1234
    dbcp2:
      min-idle: 5                                           # 数据库连接池的最小维持连接数
      initial-size: 5                                       # 初始化连接数
      max-total: 5                                          # 最大连接数
      max-wait-millis: 200      
```



### microservicecloud-provider-dept-8003服务提供者

#### 使用的数据库SQL语句

``` sql
DROP DATABASE IF EXISTS cloudDB03 ;

CREATE DATABASE cloudDB03 CHARACTER SET UTF8 ;

USE cloudDB03 ;

CREATE TABLE dept (
  deptno BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  dname VARCHAR (60),
  db_source VARCHAR (60)
) ;

INSERT INTO dept(dname,db_source) VALUES('开发部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('人事部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('财务部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('市场部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('运维部',DATABASE());
```

#### Application.yml文件

```yaml
server:
  port: 8003
  
mybatis:
  config-location: classpath:mybatis/mybatis.cfg.xml        # mybatis配置文件所在路径
  type-aliases-package: com.luo.springcloud.entities        # 所有Entity别名类所在包
  mapper-locations:
  - classpath:mybatis/mapper/**/*.xml                       # mapper映射文件
    
spring:
   application:
    name: microservicecloud-dept 
   datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包
    url: jdbc:mysql://localhost:3306/cloudDB03              # 数据库名称
    username: root
    password: 1234
    dbcp2:
      min-idle: 5                                           # 数据库连接池的最小维持连接数
      initial-size: 5                                       # 初始化连接数
      max-total: 5                                          # 最大连接数
      max-wait-millis: 200      
```

### 微服务提供者说明

三个微服务提供者连接不同的数据库，因此在application.yml文件中，我们需要修改端口号和连接的数据库，注意的是三个微服务提供者的微服务名字保持一样，也就是如下的配置信息

```yaml
spring:
   application:
    name: microservicecloud-dept 
```

### 负载均衡自测

访问连接http://localhost:8001/dept/list，http://localhost:8002/dept/list，http://localhost:8003/dept/list得到不同数据库数据，当我们启动服务注册中心7001,7002,7003,再启动80客户端，这个时候访问localhost/consumer/dept/list，每次刷新就会得到不同数据库的数据。这就是Ribbon默认的轮询算法的负载均衡。

## Ribbon核心组件IRule

#### Ribbon负载均衡算法

Ribbon默认提供的是轮询的负载均衡算法，完整了还有如下

| RoundRobinRule            | 轮询                                       |
| ------------------------- | ---------------------------------------- |
| RandomRule                | 随机                                       |
| AvaliabilityFilteringRule | 会先过滤由于多次访问故障而处于断路器跳闸的状态的服务和并发的连接数量超过阈值的服务，然后对剩余的服务列表按照轮询策略 |
| WeightedResponseTimeRule  | 根据平均响应时间计算所有服务的权重，响应时间越快服务权重越大           |
| RetryRule                 | 先按照RoundRobinRule策略获取服务，如果获取服务失败会在指定时间内重试 |
| BestAvailableRule         | 会先过滤掉由于多次访问故障二处于断路器跳闸状态的服务，然后选择一个并发量最小的服务 |
| ZoneAvoidanceRule         | 默认规则，复合判断server所在的区域的性能和server的可用性选择服务器  |

#### Ribbon负载均衡算法使用方法

在客户端的配置类ConfigBean.java中添加IRule的实现

```java
@Configuration
public class ConfigBean {
	@Bean
	@LoadBalanced
	public RestTemplate geRestTemplate(){
		return new RestTemplate();
	}
	@Bean
	public IRule myRule(){
		return new RandomRule();
	}
}
```

## Ribbon自定义

如果不使用Ribbon默认的七种负载均衡算法，这个时候就需要使用自定义负载均衡算法

### 客户端主启动类使用注解@RibbonClient

```java
@SpringBootApplication
@EnableEurekaClient
@RibbonClient(name="MICROSERVICECLOUD-DEPT",configuration=MySelfRule.class)
public class DeptConsumer80_App {
	public static void main(String[] args) {
		SpringApplication.run(DeptConsumer80_App.class, args);
	}
}
```

**特此说明**

RibbonClient注解中的MySelfRule类使我们自定义负载均衡算法的类，但是，这个自定义配置类不能放在@ComponentScan所扫描的当前包下以及子包下，否则我们这个自定义的配置类会被所有的Ribbon客户端所共享，也就说，达不到我们特殊化定制的目的。举例说明，自定义配置类不能放在项目主启动类所有的包以及子包下，因为主启动类使用注解@SpringBootApplication，这个注解点进去使用@ComponentScan注解

### 自定义负载均衡算法

轮询算法中每一个服务轮询一次，现在需求是每一个服务调用五次后在轮询下一个服务

### 自定义配置类

```java
package com.luo.myrule;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import com.netflix.loadbalancer.IRule;

@Configuration
public class MySelfRule {
	@Bean
	public IRule myRule(){
		return new RandomRule_lky();
	}
}
```

### 自定义算法类

```java
package com.luo.myrule;

import java.util.List;

import com.netflix.client.config.IClientConfig;
import com.netflix.loadbalancer.AbstractLoadBalancerRule;
import com.netflix.loadbalancer.ILoadBalancer;
import com.netflix.loadbalancer.Server;

public class RandomRule_lky extends AbstractLoadBalancerRule{
	// total = 0 // 当total==5以后，我们指针才能往下走，
	// index = 0 // 当前对外提供服务的服务器地址，
	// total需要重新置为零，但是已经达到过一个5次，我们的index = 1
	// 分析：我们5次，但是微服务只有8001 8002 8003 三台，OK？
	private int total = 0; 			// 总共被调用的次数，目前要求每台被调用5次
	private int currentIndex = 0;	// 当前提供服务的机器号
	public Server choose(ILoadBalancer lb, Object key){
		if (lb == null) {
			return null;
		}
		Server server = null;
		while (server == null) {
			if (Thread.interrupted()) {
				return null;
			}
			List<Server> upList = lb.getReachableServers();
			List<Server> allList = lb.getAllServers();
			int serverCount = allList.size();
			if (serverCount == 0) {
				return null;
			}
//			private int total = 0; 			// 总共被调用的次数，目前要求每台被调用5次
//			private int currentIndex = 0;	// 当前提供服务的机器号
            if(total < 5)
            {
	            server = upList.get(currentIndex);
	            total++;
            }else {
	            total = 0;
	            currentIndex++;
	            if(currentIndex >= upList.size())
	            {
	              currentIndex = 0;
	            }
            }			
			if (server == null) {
				Thread.yield();
				continue;
			}
			if (server.isAlive()) {
				return (server);
			}
			server = null;
			Thread.yield();
		}
		return server;
	}
	@Override
	public Server choose(Object key){
		return choose(getLoadBalancer(), key);
	}
	@Override
	public void initWithNiwsConfig(IClientConfig clientConfig){}
}
```

# Feign负载均衡

Feign是一个声明式WebService客户端，使用Feign能够让编写Web Service客户端变得更简单，它的使用方法就是定义一个接口，然后在上面添加注解。SpringCloud对Feign进行了封装，支持SpringMVC注解和HTTPMessageConverters，Feign可以与Eureka和Ribbon组合使用以支持负载均衡。简单讲，只需要创建一个接口，然后在上面使用注解即可。

## Feign使用步骤

参考项目microservicecloud-consumer-dept-80新建microservicecloud-consumer-dept-feign,拷贝相应的包和配置文件，去掉IRule等信息，修改pom.xml文件，添加对Feign的支持

### pom.xml文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.luo.springcloud</groupId>
    <artifactId>microservicecloud</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
  <artifactId>microservicecloud-consumer-dept-feign</artifactId>
<dependencies>
		<dependency><!-- 自己定义的api -->
			<groupId>com.luo.springcloud</groupId>
			<artifactId>microservicecloud-api</artifactId>
			<version>${project.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-feign</artifactId>
		</dependency>
		<!-- Ribbon相关 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-ribbon</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<!-- 修改后立即生效，热部署 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
	</dependencies>
</project>
```

由于Feign是面向接口编程，为方便接口的互相调用，将接口和公共的方向在项目microservicecloud-api中，因此修改为：

### 修改microservicecloud-api工程的pom.xml文件

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-feign</artifactId>
</dependency>
```

### 新建DeptClientService接口

```java
@FeignClient(value = "MICROSERVICECLOUD-DEPT")
public interface DeptClientService {
	@RequestMapping(value = "/dept/get/{id}",method = RequestMethod.GET)
	public Dept get(@PathVariable("id") long id);
	
	@RequestMapping(value = "/dept/list",method = RequestMethod.GET)
	public List<Dept> list();
	
	@RequestMapping(value = "/dept/add", method = RequestMethod.POST)
	public boolean add(Dept dept);
}
```

### 修改microservicecloud-consumer-dept-feign中Controller添加新建的DeptClientService

```java
@RestController
public class DeptController_Consumer {
	@Autowired
	private DeptClientService service;
	
	@RequestMapping(value = "/consumer/dept/get/{id}")
	public Dept get(@PathVariable("id") Long id){
		return this.service.get(id);
	}
	
	@RequestMapping(value = "/consumer/dept/list")
	public List<Dept> list(){
		return this.service.list();
	}
	
	@RequestMapping(value = "/consumer/dept/add")
	public Object add(Dept dept){
		return this.add(dept);
	}
}
```

### 修改microservicecloud-consumer-dept-feign主启动类,添加注解

```java
@SpringBootApplication
@EnableEurekaClient
@EnableFeignClients(basePackages = {"com.luo.springcloud"})
@ComponentScan("com.luo.springcloud")
public class DeptConsumer80_Feign_App {
	public static void main(String[] args) {
		SpringApplication.run(DeptConsumer80_Feign_App.class, args);
	}
}
```

### 测试

启动3个Eureka集群，启动三个部门微服务提供者，启动Feign，访问http://localhost/consumer/dept/list即可

### 总结说明

Feign集成了Ribbon，利用Ribbon维护了MicroServiceCloud-Dept的服务列表信息，并通过轮询的方式实现了客户端的复杂均衡，与Ribbon不同的是，通过Feign只需要定义服务绑定接口且以声明式法人方法，优雅而简单的实现服务调用。

# Hystrix断路器

## 概述

Hystrix是一个用于处理分布式系统的**延迟**和**容错**的开源库，在分布式系统中，许多的依赖不可避免的会调用失败，比如超时，异常等，Hystrix能够保证在一个依赖出问题的情况下， **不会导致整体服务的失败，避免级联故障，以提高分布式系统的弹性。**断路器本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝）， **向调用方法返回一个预期的，可处理的备选响应（FallBack），而不是长时间的等待或者抛出调用方法异常无法处理的异常**，这样就保证服务调用方的线程不会被长时间，不必要的占用，从而避免了故障在分布式系统中的蔓延。

## 服务熔断

熔断机制是应对雪崩效应的一种微服务链路保护机制，当扇出链路的某一个微服务不可用或者响应时间太长，会进行服务的降级， **进而熔断该节点微服务的调用，快速返回“错误”的响应信息**，当检测到该节点微服务调用响应正常后恢复调用链路，在SpringCloud框架中熔断机制使用Hystrix实现，Hystrix会监控微服务调用情况，当失败达到一定阈值。就会启动熔断机制，熔断机制的注解是 **@HystrixCommand**

## Hystrix实操

### 参照microservicecloud-provider-dept-8001建立microservicecloud-provider-dept-hystrix-8001项目

### pom.xml文件

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-hystrix</artifactId>
</dependency>
```

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.luo.springcloud</groupId>
    <artifactId>microservicecloud</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
  <artifactId>microservicecloud-provider-dept-hystrix-8001</artifactId>
<dependencies>
		<!-- hystrix -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-hystrix</artifactId>
		</dependency>
		<!-- 引入自己定义的api通用包，可以使用Dept部门Entity -->
		<dependency>
			<groupId>com.luo.springcloud</groupId>
			<artifactId>microservicecloud-api</artifactId>
			<version>${project.version}</version>
		</dependency>
		<!-- actuator监控信息完善 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<!-- 将微服务provider侧注册进eureka -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
		</dependency>
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-core</artifactId>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jetty</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
		</dependency>
		<!-- 修改后立即生效，热部署 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
	</dependencies>

</project>
```

### application.yml文件

```yaml
eureka:
  client: #客户端注册进eureka服务列表内
    service-url: 
      #defaultZone: http://localhost:7001/eureka
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/      
  instance:
    instance-id: microservicecloud-dept8001-hystrix #自定义服务名称信息
    prefer-ip-address: true     #访问路径可以显示IP地址 
```

```yaml
server:
  port: 8001
  
mybatis:
  config-location: classpath:mybatis/mybatis.cfg.xml        # mybatis配置文件所在路径
  type-aliases-package: com.luo.springcloud.entities        # 所有Entity别名类所在包
  mapper-locations:
  - classpath:mybatis/mapper/**/*.xml                       # mapper映射文件
    
spring:
   application:
    name: microservicecloud-dept 
   datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包
    url: jdbc:mysql://localhost:3306/cloudDB01              # 数据库名称
    username: root
    password: 1234
    dbcp2:
      min-idle: 5                                           # 数据库连接池的最小维持连接数
      initial-size: 5                                       # 初始化连接数
      max-total: 5                                          # 最大连接数
      max-wait-millis: 200                                  # 等待连接获取的最大超时时间
      
eureka:
  client: #客户端注册进eureka服务列表内
    service-url: 
      #defaultZone: http://localhost:7001/eureka
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/      
  instance:
    instance-id: microservicecloud-dept8001-hystrix #自定义服务名称信息
    prefer-ip-address: true     #访问路径可以显示IP地址 
    
info: 
  app.name: luokangyuan-microservicecloud
  company.name: www.luokangyuan.com
  build.artifactId: $project.artifactId$
  build.version: $project.version$
```

### 修改DeptController

Hystrix的作用就是当调用服务出现异常时如何解决，模拟根据id查部门信息，查到null，人为抛出运行时异常，让Hystrix处理这种情况。

```java
@RequestMapping(value="dept/get/{id}",method=RequestMethod.GET)
@HystrixCommand(fallbackMethod = "processHystrix_GET")
public Dept get(@PathVariable("id") Long id){
  Dept dept = service.get(id);
  if(null == dept){
    throw new RuntimeException("该ID:"+id+"没有对应的部门信息");
  }
  return dept;
}

public Dept processHystrix_GET(@PathVariable("id") Long id){
  return new Dept().setDeptno(id)
    .setDname("该ID："+id+"没有对应的信息，null--@HystrixCommand")
    .setDb_source("no this database in Mysql");
}
```

### 修改主启动类添加Hystrix支持

```java
@SpringBootApplication
@EnableEurekaClient // 本服务启动后会注册到Eureka服务注册中心
@EnableDiscoveryClient // 服务发现
@EnableCircuitBreaker //对Hystrix熔断机制的支持
public class DeptProvider8001_Hystrix_App {
	public static void main(String[] args) {
		SpringApplication.run(DeptProvider8001_Hystrix_App.class, args);
	}
}
```

### 测试熔断机制

启动三个Eureka集群，启动服务主启动类DeptProvider8001_Hystrix_App，客户端启动microservicecloud-consumer-dept-80，页面访问http://localhost/consumer/dept/get/112

## 服务降级

服务降级处理是在客户端完成的，与服务端没有关系，在前面的服务熔断中，我们发现每一个业务方法都要写一个processHystrix_方法，这样就造成了很大耦合，根据Spring的学习，我们可将processHystrix_改写一个异常通知。

### 修改microservicecloud-api工程

根据已有的DeptClientService接口，新建一个实现了FallbackFactory接口的类DeptClientServiceFallbackFactory

```java
package com.luo.springcloud.service;

import java.util.List;

import org.springframework.stereotype.Component;

import com.luo.springcloud.entities.Dept;

import feign.hystrix.FallbackFactory;
@Component
public class DeptClientServiceFallbackFactory implements FallbackFactory<DeptClientService>{

	@Override
	public DeptClientService create(Throwable arg0) {
		return new DeptClientService() {
			
			@Override
			public List<Dept> list() {
				return null;
			}
			
			@Override
			public Dept get(long id) {
				return new Dept().setDeptno(id)
						.setDname("该ID："+id+"没有对应的信息，Consumer客户端提供的降级信息，此服务暂停使用")
						.setDb_source("no this database in Mysql");
			}
			
			@Override
			public boolean add(Dept dept) {
				return false;
			}
		};
	}

}
```

**注意：不要忘记新类上添加@Component注解**

### 修改microservicecloud-api

在DeptClientService接口在注解@FeignClient(value = "MICROSERVICECLOUD-DEPT")添加fallbackFactory属性值

```java
@FeignClient(value = "MICROSERVICECLOUD-DEPT",fallbackFactory = DeptClientServiceFallbackFactory.class)
```

```java
@FeignClient(value = "MICROSERVICECLOUD-DEPT",fallbackFactory = DeptClientServiceFallbackFactory.class)
public interface DeptClientService {
	@RequestMapping(value = "/dept/get/{id}",method = RequestMethod.GET)
	public Dept get(@PathVariable("id") long id);
	
	@RequestMapping(value = "/dept/list",method = RequestMethod.GET)
	public List<Dept> list();
	
	@RequestMapping(value = "/dept/add", method = RequestMethod.POST)
	public boolean add(Dept dept);
}
```

### 修改microservicecloud-consumer-dept-feign的Application.yml文件

``` yaml
server:
  port: 80
  
feign: 
  hystrix: 
    enabled: true
eureka:
  client:
    register-with-eureka: false
    service-url: 
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/  
```

### 测试服务降级

启动三个Eureka集群，microservicecloud-provider-dept-8001启动，microservicecloud-consumer-dept-feign启动，正常访问http://localhost/consumer/dept/get/1测试，故意关停microservicecloud-provider-dept-8001，客户端自己调用提示

## 服务监控Hystrix Dashboard

Hystrix还提供了准实时的调用监控Hystrix Dashboard，Hystx会持续的记录所有通过Hystrix发起的请求的执行信息，并以统计报表的图形的形式展示给用户，包括每秒执行多少次请求多少成功多少失败等，对监控内容转换为可视化界面。

**新建microservicecloud-consumer-hystrix-dashboard监控的一个微服务工程**

## POM.xml文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.luo.springcloud</groupId>
    <artifactId>microservicecloud</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
  <artifactId>microservicecloud-consumer-hystrix-dashboard</artifactId>
<dependencies>
		<!-- 自己定义的api -->
		<dependency>
			<groupId>com.luo.springcloud</groupId>
			<artifactId>microservicecloud-api</artifactId>
			<version>${project.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<!-- 修改后立即生效，热部署 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
		<!-- Ribbon相关 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-ribbon</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<!-- feign相关 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-feign</artifactId>
		</dependency>
		<!-- hystrix和 hystrix-dashboard相关 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-hystrix</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
		</dependency>
	</dependencies>
</project>
```

## application.yml文件

```yaml
server:
  port: 9001
```

## 主启动类DeptConsumer_DashBoard_App

``` java
@SpringBootApplication
@EnableHystrixDashboard
public class DeptConsumer_DashBoard_App {
	public static void main(String[] args) {
		SpringApplication.run(DeptConsumer_DashBoard_App.class, args);
	}
}
```

## 微服务提供者添加监控依赖配置

所有的Provider微服务提供类（8001,8002,8003）都需要监控依赖配置，也就是pom文件添加如下依赖

```xml
<!-- actuator监控信息完善 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

## 监控页面测试

**启动microservicecloud-consumer-hystrix-dashboard，访问http://localhost:9001/hystrix,出现豪猪页面**

## 全部测试

**启动3个Eureka集群，启动microservicecloud-provider-dept-hystrix-8001**，启动了microservicecloud-consumer-hystrix-dashboard用来监控8001服务提供者，访问http://localhost:8001/hystrix.stream

### 观察监控窗口

访问http://localhost:9001/hystrix，填写监控地址http://localhost:8001/hystrix.stream,时间2000，title:demo01,点击按钮

实心圆：两种含义，它通过颜色的变化代表了实例的健康程度，健康色是从绿色<黄色<橙色<红色递减，该实心圆除了颜色的变化之外，他的大小也会根据实例的请求流量发生变化，流量越大该实心圆就越大，所以通过实心圆的展示就可以在大量实例中快速的发现 **故障实例和高压力测试**。

# Zuul路由网关

## 概述

Zuul包含了对请求的路由和过滤两个主要的功能，其中路由的功能是负责将外部请求转发到具体的微服务实例上，是实现外部访问统一入口的基础而过滤功能是负责对请求的处理过程进行干预，是实现请求校验，服务聚合等功能的基础，Zuul和Eureka进行整合，将Zuul自身注册近Eureka服务治理的应用，同时从Eureka中获取其他微服务的消息，也及时以后的访问服务都是通过Zuul跳转后获得， **注意的是Zuul服务最终还是会注册近Eureka中**

## 路由基本配置

新建项目microservicecloud-zuul-gateway-9527，添加依赖如下

### pom.xml文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.luo.springcloud</groupId>
    <artifactId>microservicecloud</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
  <artifactId>microservicecloud-zuul-gateway-9527</artifactId>
<dependencies>
		<!-- zuul路由网关 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-zuul</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<!-- actuator监控 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<!-- hystrix容错 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-hystrix</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<!-- 日常标配 -->
		<dependency>
			<groupId>com.luo.springcloud</groupId>
			<artifactId>microservicecloud-api</artifactId>
			<version>${project.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jetty</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
		</dependency>
		<!-- 热部署插件 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
	</dependencies>

</project>
```

### application.yml文件

```yaml
server: 
  port: 9527
 
spring: 
  application:
    name: microservicecloud-zuul-gateway
 
eureka: 
  client: 
    service-url: 
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka,http://eureka7003.com:7003/eureka  
  instance:
    instance-id: gateway-9527.com
    prefer-ip-address: true 
 
 
zuul: 
  #ignored-services: microservicecloud-dept
  prefix: /luo
  ignored-services: "*"
info:
  app.name: luo-microcloud
  company.name: www.luo.com
  build.artifactId: $project.artifactId$
  build.version: $project.version$
```

### 修改host文件

```properties
127.0.0.1 myzuul.com
```

### 主启动类Zuul_9527_StartSpringCloudApp

```java
@SpringBootApplication
@EnableZuulProxy
public class Zuul_9527_StartSpringCloudApp {
	public static void main(String[] args) {
		SpringApplication.run(Zuul_9527_StartSpringCloudApp.class, args);
	}
}
```

### 启动

三个集群，一个服务提供类microservicecloud-provider-dept-8001，一个路由

### 测试

不使用路由：http://localhosat:8001/dept/get/2

使用路由：http://myzuul.com:9527/microservicecloud-dept/dept/get/2

## Zuul路由访问映射

在前面的测试中我们可以使用http://myzuul.com:9527/microservicecloud-dept/dept/get/2访问我们的接口，这样就暴露我们的微服务名称，需要做安全加固，就用到了路由访问映射，修改路由项目的yml文件,添加 mydept.path: /mydept/**

``` yaml
zuul: 
  #ignored-services: microservicecloud-dept #忽略真实地址，只让虚拟地址访问
  prefix: /luo #访问地址前缀
  ignored-services: "*"#忽略真实地址，只让虚拟地址访问
  routes: 
    mydept.serviceId: microservicecloud-dept ##真实地址
    mydept.path: /mydept/** # 虚拟地址
```

**访问连接：http://lyzuul.com:9527/luo/mydept/dept/get/1**

# SpringCloudConfig配置中心

## 概述

就前面项目而言，分布面临的问题是配置问题，每一个项目都有一个yml文件，不好运维管理，所有需要一套集中式，动态的配置管理设施，SpringCloud提供了ConfigServer来解决这个问题。

SpringCloud Config是为微服务架构中的微服务提供集中化的外部配置支持，配置服务器为 **各个不同的微服务应用**的环境提供了一个 **中心化的外部配置**。SpringCloud Config分为客户端和服务端，服务端也称 **分布式配置中心，它是一个独立的微服务应用**，用来连接配置服务器并为客户端提供获取配置信息，加密和解密信息等访问接口，客户端是通过指定的配置中心获取和加载配置信息配置服务器默认采用git来存储配置信息，这样就有助于对环境配置进行版本管理，并且可以通过git客户端工具管理和访问配置内容。

## 作用

* 集中管理配置文件
* 不同环境下不同配置，动态化的配置更新，分环境部署等
* 运行期间动态调整配置，不需要在每一个服务部署的机器编码上编写文件，服务会向配置中心拉取自己的配置信息
* 当配置发生变动时，服务不需要重启即可感知配置的变化并应用新的配置
* 将配置信息以REST接口的形式暴露

## config服务端与GitHub通信

**GitHUb上新建一个microservicecloud-config的Repository**

**本地硬盘目录新建git仓库并clone**

**在D:\workspace2018\micorservicecloude-config\microservicecloud-config新建application.yml文件**

```yaml
Spring:
    profiles:
    active:
    - dev
---
Spring:
    profiles: dev 
    application:
        name: micorservicecloud-config-luo-dev
---
Spring:
    profiles: test 
    application:
        name: micorservicecloud-config-luo-test
```

**注意保存为utf-8的文件格式**

**将yml文件推送到GitHub上**

```properties
git add .
git commit -m""
git push origin master
```

**新建项目microservicecloud-config-3344**

**POM.xml文件**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.luo.springcloud</groupId>
    <artifactId>microservicecloud</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
  <artifactId>microservicecloud-config-3344</artifactId>
<dependencies>
		<!-- springCloud Config -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-config-server</artifactId>
		</dependency>
		<!-- 避免Config的Git插件报错：org/eclipse/jgit/api/TransportConfigCallback -->
		<dependency>
			<groupId>org.eclipse.jgit</groupId>
			<artifactId>org.eclipse.jgit</artifactId>
			<version>4.10.0.201712302008-r</version>
		</dependency>
		<!-- 图形化监控 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<!-- 熔断 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-hystrix</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jetty</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
		</dependency>
		<!-- 热部署插件 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
	</dependencies>

</project>
```

**application.yml文件**

```yaml
server: 
  port: 3344 
  
spring:
  application:
    name:  microservicecloud-config
  cloud:
    config:
      server:
        git:
          uri: git@github.com:luokangyuan/microservicecloud-config.git #GitHub上面的git仓库名字
```

**主启动类**

```java
@SpringBootApplication
@EnableConfigServer
public class Config_3344_StartSpringCloudApp {
	public static void main(String[] args) {
		SpringApplication.run(Config_3344_StartSpringCloudApp.class, args);
	}
}
```

**修改host文件**

```properties
127.0.0.1 config-3344.com
```

**测试通过config微服务从GitHub上获取配置内容**

启动服务3344，访问http://config-3344.com:3344/application-dev.yml，http://config-3344.com:3344/application-test.yml

## config客户端获取github配置

**本地新建microservicecloud-config-client.yml文件,并推送到github**

```yaml
server:
    port: 8201
spring:
    profiles: dev
    application:
        name: microservicecloud-config-client
eureka:
    client:
        service-url:
            defaultZone: http://eureka-dev.com:7001/eureka/
---
server:
    port: 8202
spring:
    profiles: test
    application:
        name: microservicecloud-config-client
eureka:
    client:
        service-url:
            defaultZone: http://eureka-test.com:7001/eureka/
```

**新建项目microservicecloud-config-client-3355，pom.xml文件如下**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>com.luo.springcloud</groupId>
		<artifactId>microservicecloud</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
	<artifactId>microservicecloud-config-client-3355</artifactId>

	<dependencies>
		<!-- SpringCloud Config客户端 -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-hystrix</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jetty</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
	</dependencies>
</project>
```

**新建bootstrap.yml文件**

```yaml
spring:
  cloud:
    config:
      name: microservicecloud-config-client #需要从github上读取的资源名称，注意没有yml后缀名
      profile: test   #本次访问的配置项
      label: master   
      uri: http://config-3344.com:3344  #本微服务启动后先去找3344号服务，通过SpringCloudConfig获取GitHub的服务地址
```

application.yml是用户级的资源配置文件，bootstrap.yml是系统级，优先级更高，保证不会被本地配置文件所覆盖

**修改host文件，增加映射**

```properties
127.0.0.1 client-config.com
```

**新建测试controller，从github读取配置信息**

```java
package com.luo.springcloud.rest;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ConfigClientRest
{

	@Value("${spring.application.name}")
	private String applicationName;

	@Value("${eureka.client.service-url.defaultZone}")
	private String eurekaServers;

	@Value("${server.port}")
	private String port;

	@RequestMapping("/config")
	public String getConfig()
	{
		String str = "applicationName: " + applicationName + "\t eurekaServers:" + eurekaServers + "\t port: " + port;
		System.out.println("******str: " + str);
		return "applicationName: " + applicationName + "\t eurekaServers:" + eurekaServers + "\t port: " + port;
	}
}
```

**新建主启动类**

```java
@SpringBootApplication
public class ConfigClient_3355_StartSpringCloudApp {
	public static void main(String[] args) {
		SpringApplication.run(ConfigClient_3355_StartSpringCloudApp.class, args);
	}
}
```

**测试**

启动3344服务，启动3355服务，bootstrap.yml中的profile值是什么，决定从github上读取什么,ruguo

访问http://client-config.com:8201/config得到是github上的microservicecloud-config-client.yml文件中dev相关的配置信息

访问http://client-config.com:8202/config得到是github上的microservicecloud-config-client.yml文件中test相关的配置信息