# 一、MybatisPlus简介

## 1.1.简介

Mybatis-Plus（简称MP）是一个 `Mybatis 的增强工具`，在 Mybatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

## 1.2.文档地址

[官网文档地址](http://mp.baomidou.com/#/?id=%E7%AE%80%E4%BB%8B)

## 1.3.MybatisPlus的特性

-   **无侵入**：Mybatis-Plus 在 Mybatis 的基础上进行扩展，只做增强不做改变，引入 Mybatis-Plus 不会对您现有的 Mybatis 构架产生任何影响，而且 MP 支持所有 Mybatis 原生的特性
-   **依赖少**：仅仅依赖 Mybatis 以及 Mybatis-Spring
-   **损耗小**：`启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作`
-   **预防Sql注入**：内置 Sql 注入剥离器，有效预防Sql注入攻击
-   **通用CRUD操作**：`内置通用 Mapper`、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
-   **多种主键策略**：支持多达4种主键策略（内含分布式唯一ID生成器），可自由配置，完美解决主键问题
-   **支持热加载**：Mapper 对应的 XML 支持热加载，对于简单的 CRUD 操作，甚至可以无 XML 启动
-   **支持ActiveRecord**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可实现基本 CRUD 操作
-   **支持代码生成**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用（P.S. 比 Mybatis 官方的 Generator 更加强大！）
-   **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
-   **支持关键词自动转义**：支持数据库关键词（order、key......）自动转义，还可自定义关键词
-   **内置分页插件**：基于 Mybatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通List查询
-   **内置性能分析插件**：`可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能有效解决慢查询`
-   **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，预防误操作

# 二、集成MybatisPlus

## 2.1.Maven导入MybatisPlus依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus</artifactId>
    <version>3.0-gamma</version>
</dependency>
```

## 2.2.修改sqpsessionFactoryBean

```xml
<!--  配置SqlSessionFactoryBean 
  Mybatis提供的: org.mybatis.spring.SqlSessionFactoryBean
  MP提供的:com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean
  -->
<!-- 配置sqlsessionFactoryBean -->
<bean id="sqlSessionFactoryBean" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
    <!-- 数据源 -->
    <property name="dataSource" ref="dataSource"></property>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property> 
    <!-- 别名处理 -->
    <property name="typeAliasesPackage" value="com.luo.beans"></property>
</bean>
```



# 三、入门的Hello World

## 3.1.准备数据表

```sql
DROP TABLE IF EXISTS `tbl_user`;
CREATE TABLE `tbl_user` (
  `email` varchar(50) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `gender` char(255) DEFAULT NULL,
  `user_name` varchar(255) DEFAULT NULL,
  `id` int(11) NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

INSERT INTO `tbl_user` VALUES ('243518985@qq.com', 24, '1', '罗康元 ', 1);
INSERT INTO `tbl_user` VALUES ('1835856@qq.com', 34, '2', '张三', 2);
INSERT INTO `tbl_user` VALUES ('323134435@163.com', 53, '1', '李四', 3);
INSERT INTO `tbl_user` VALUES ('345464566@qq.com', 43, '1', '王五', 4);
INSERT INTO `tbl_user` VALUES ('luokangyuansb@gmail.com', 45, '1', '赵六', 5);
```

## 3.2.准备Java实体类

```java
public class User {
	
	private Integer id;
	
	private String userName;
	
	private String email;
	
	private Integer gender;
	
	private Integer age;
}
```

## 3.3.加入依赖的jar包

```xml
<dependencies>
  <!-- 加入mybatisPlus依赖 -->
	<dependency>
	  <groupId>com.baomidou</groupId>
	  <artifactId>mybatis-plus</artifactId>
	  <version>3.0-gamma</version>
	</dependency>
	<!-- https://mvnrepository.com/artifact/junit/junit -->
	<dependency>
	    <groupId>junit</groupId>
	    <artifactId>junit</artifactId>
	    <version>4.11</version>
	    <scope>test</scope>
	</dependency>
	<!-- https://mvnrepository.com/artifact/log4j/log4j -->
	<dependency>
	    <groupId>log4j</groupId>
	    <artifactId>log4j</artifactId>
	    <version>1.2.17</version>
	</dependency>
	<!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
	<dependency>
	    <groupId>com.mchange</groupId>
	    <artifactId>c3p0</artifactId>
	    <version>0.9.5.2</version>
	</dependency>
	<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
	<dependency>
	    <groupId>mysql</groupId>
	    <artifactId>mysql-connector-java</artifactId>
	    <version>6.0.6</version>
	</dependency>
	<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>4.3.18.RELEASE</version>
	</dependency>
	<!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-orm</artifactId>
	    <version>4.3.18.RELEASE</version>
	</dependency>
  </dependencies>
```

## 3.5.mybatis-config.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

## 3.6.log4j.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
 
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
 
 <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
   <param name="Encoding" value="UTF-8" />
   <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m  (%F:%L) \n" />
   </layout>
 </appender>
 <logger name="java.sql">
   <level value="debug" />
 </logger>
 <logger name="org.apache.ibatis">
   <level value="info" />
 </logger>
 <root>
   <level value="debug" />
   <appender-ref ref="STDOUT" />
 </root>
</log4j:configuration>
```

## 3.7.db.properties文件

```properties
jdbc.driver=com.mysql.jdbc.Driver 
jdbc.url=jdbc:mysql://localhost:3306/mybatis
jdbc.username=root
jdbc.password=jiamei@20141107.
```

## 3.8.applicationContext.xml文件

```xml
<!-- 数据源 -->
<context:property-placeholder location="classpath:db.properties" />
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="${jdbc.driver}"></property>
    <property name="jdbcUrl" value="${jdbc.url}"></property>
    <property name="user" value="${jdbc.username}"></property>
    <property name="password" value="${jdbc.password}"></property>
</bean>
<!--事务管理器  -->
<bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>
<!--基于注解的事务管理  -->
<tx:annotation-driven transaction-manager="dataSourceTransactionManager" />
<!--  配置SqlSessionFactoryBean 
  Mybatis提供的: org.mybatis.spring.SqlSessionFactoryBean
  MP提供的:com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean
  -->
<!-- 配置sqlsessionFactoryBean -->
<bean id="sqlSessionFactoryBean" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
    <!-- 数据源 -->
    <property name="dataSource" ref="dataSource"></property>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property> 
    <!-- 别名处理 -->
    <property name="typeAliasesPackage" value="com.luo.beans"></property>
</bean>
<!--  配置mybatis扫描的mapper接口路径-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.luo.mapper"></property>
</bean>
```

## 3.9.通用的CRUD操作

假设我们有一张user表，我们需要对这个表进行增删改查操作，如果说没有代码生成的话，在mybatis我们`需要编写UserMapper接口，手写CRUD方法，在UserMapper.xml文件中写对应的sql语句`，但是，在MybatisPlus中，我们只需要创建UserMapper接口，继承BaseMapper接口，就可以完成CRUD操作，甚至不需要创建SQl映射文件。

```java
public interface UserMapper extends BaseMapper<User>{

}
```





# 四、条件查询



# 五、活动记录



# 六、代码生成器



# 七、插件扩展



# 八、自定义全局操作



# 九、公共字段填充





