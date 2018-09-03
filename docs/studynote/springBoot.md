#  一、SpringBoot入门

## 1.1.什么是SpringBoot

采用官方的话说，SpringBoot是简化Spring应用开发的一个框架，将整个Spring技术全家桶进行整合，被称为JavaEE开发的一站式解决方案；

## 1.2.SpringBoot和SpringCloud的关系

* SpringBoot是专注于快速方便的开发单个体微服务；
* SpringCloud是关注全局的微服务协调治理框架，他将多个SpringBoot开发的单个微服务进行整合管理，为微服务之间提供配置管理，服务发现等一系列服务；
* SpringBoot可以离开SpringCloud单独开发项目，但是SpringCloud离不来SpringBoot；

## 1.3.SpringBoot的入门demo

使用IDEA开发工具，新建一个Spring Boot项目，只选中web模块，服务端响应请求，如下：

```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello(){
        return "hello this is my first demo";
    }
}
```

# 二、SpringBoot配置文件

## 2.1.基本使用

在项目的resource目录下，有一个SpringBoot的全局配置文件`application.properties`，当我们需要修改配置文件的时候就可以修改，如下，修改项目的端口号：

```properties
server.port=8081
```

当然，properties配置文件也会存在很多重复的配置代码，这个时候就出现了一种新的配置文件`application.yaml`

```yaml
server:
  port: 8082
```

## 2.2.yaml语法说明

在前面的使用中，大家也可以看到yaml语法是采用`一对键值对（中间存在空格）`,同时使用`空格`来控制层级关系，只要是左对齐的一列数据，就说明是一个层级的；需要注意的是；属性和值对大小写是敏感的；

```yaml
server:
  port: 8082
# 在yaml语法中，值为数字，字符串或者布尔就直接写，字符串默认不用添加单引号或者双引号
name: 你好
# 字符串添加了双引号，代表特殊字符不会转义，会直接输出特殊字符所代表的含义，这里会换行
lastName: "你好 \n 世界"
# 字符串添加了单引号，特殊字符会被转义，这个的换行就会被转义为一个普通的字符串
firstname: '你好 \ 世界'
# 当值为对象，Map时，也是采用属性和值的写法，使用空格缩进来区分，如下所示
person:
  name: luokangyuan
  age: 23
# 另外一个写法
user: {name: luokangyuan, age: 34}
# 当值为数组，list或者set时候，使用-加空格的方式，如下：
age:
  - man
  - woman
# 数组的另外一种写法
sport: [football,pingpang]
```

## 2.3.配置文件注入

所谓的配置文件注入就是在java类中获取配置文件中配置的属性值，在SpringBoot中可以使用`@Value注解和@ConfigurationProperties注解完成`，两者的区别是：`@value`需要一个一个的绑定需要获取的属性值，不支持松散语法，不支持JSR303校验，也不支持复杂类型封装，仅支持SpEL，相对而言，`@ConfigurationProperties`就支持批量注入配置文件属性，支持JSR303数据校验，也支持复杂类型封装,当你只需要从配置文件中获取一个属性值的时候就可以使用`@Value注解`，当你需要将配置文件封装成一个JavaBean的时候，就可以使用`@ConfigurationProperties注解`，使用方法如下：

配置文件yaml中配置需要注入的属性值：

```yaml
# 配置文件注入
systemuser:
  userName: luokangyuan
  age: 24
  boss: false
  birthday: 1995/03/16
  habbit:
    - football
    - basketball
    - pingpangball
  friend: {cd: hanger,sc: biner}
  dog:
    name: mengmeng
    age: 3
```

映射的实体类如下（省略了getter和setter）：

```java
@Component
@ConfigurationProperties(prefix = "systemuser")
@Validated
public class SystemUser {

    private String userName;

    private Integer age;

    private Boolean boss;

    private Date birthday;

    private List<String> habbit;

    private Map<String,String> friend;

    private Dog dog;
}
```

在SpringBoot的测试类中，使用`@Autowired注解`注入SystemUser类，然后在执行方法中打印，就可以看见我们已经从配置文件中获取到了属性值；

> 注意：如果，配置文件中的别名和实体类名第一个字母小写一样，就会报错，例如：systemUser

## 2.4.配置文件占位符

在配置文件中我们可以使用占位符来给一些属性住设置默认值或者随机数等，例如`${random.int(10)}，${random.uuid}`或者使用冒号指定默认值

```yaml
cat:
  name: zhangsan${random.uuid}
  fullName: ${cat.name:xiaohong}_cat
```

## 2.5.多个配置文件切换

在SpringBoot的应用中可以书写多个配置文件，使用`applocation-dev.properties`来标识这是一个开发配置文件，在`application.properties`文件中使用配置`spring.profiles.active = dev`来激活我们需要的配置文件；如果使用yaml为配置文件格式，那么可以使用yaml特有的多文档模式来切换配置文件，如下：

```yaml
spring:
  profiles:
    active: prod

---
server:
  port: 8082
spring:
  profiles: dev

---
server:
  port: 8084
spring:
  profiles: prod
```

## 2.6.配置文件加载顺序

SpringBoot启动后会顺序扫描:`类路径下的config文件夹、项目根下的配置文件、classpath:config、classpath:/`，优先级从高到低，高优先级的配置会覆盖低优先级的配置；

# 三、SLF4J日志框架

日志框架就是记录系统运行时候产生了一些痕迹，方便问题的追踪和排查，这里我们说的是SLF4J+Logback

## 3.1.使用方法

```java
Logger logger = LoggerFactory.getLogger(getClass());

    @Test
    public void contextLoads() {
        logger.trace("这是trace日志信息");
        logger.debug("这是debug日志信息");
        logger.info("这是info日志信息");
        logger.warn("这是warn日志信息");
        logger.error("这是error日志信息");
    }
```

当然你也可以在项目的配置文件中配置日志的输出格式，输出路径等信息，例如：`logging.path配置在当前项目下生成log文件，使用logging.file指定完整的文件保存路径`等等一些列都可以设置；

# 四、web开发相关

在SpringBoot开发中，我们需要使用什么技术，只需要在创建项目的时候选中我们需要的模块，然后在配置文件中指定少数的配置就可以运行了；

## 4.1.静态资源文件映射规则

在动态web项目中，我们可以将静态资源文件放在app目录下，然后访问，在SpringBoot下，我们可以使用webjars的方式来引入我们需要的静态文件，首先我们可以在`WebMvcAutoConfiguration`类中找到我们的静态资源映射规则的配置方法，如下；

![carbon (http://image.luokangyuan.com/2018-09-02-094031.png)](/Users/luokangyuan/Downloads/carbon (1).png)

从上面的代码我们看出，任何以`/webjars/**`的访问请求都会去`classpath:/META-INF/resources/webjars/`下面找资源文件，所谓的webjars就是以一种maven的方式导入前端需要的框架资源，[下载地址](https://www.webjars.org/)，使用方式很简单，示例如下：

```xml
 <dependency>
     <groupId>org.webjars</groupId>
     <artifactId>jquery</artifactId>
     <version>3.3.1-1</version>
</dependency>
```

上面说的是我们的存在webjars的静态资源文件，如果是我们自己的资源文件，存放路径为：

```properties
"classpath:/META-INF/resources/",
"classpath:/resources/",
"classpath:/static/", 
"classpath:/public/"
"/":当前项目的根路径
```

系统欢迎页面的映射配置，是在静态资源文件下的所有的index.html文件，被`/**`映射；

```java
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext) {
    return new WelcomePageHandlerMapping(new TemplateAvailabilityProviders(applicationContext), applicationContext, this.getWelcomePage(), this.mvcProperties.getStaticPathPattern());
}
```

## 4.2.模版引擎Thymeleaf

**引入thymeleaf**

```xml
 <dependency>
     <groupId>com.springframework.boot</groupId>
     <artifactId>spring-boot-starter-thymeleaf</artifactId>
 </dependency>
```

**升级thymeleaf版本**

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>1.8</java.version>
    <thymeleaf.version>3.0.2.RELEASE</thymeleaf.version>
    <thymeleaf-layout-dialect.version>2.1.1</thymeleaf-layout-dialect.version>
</properties>
```

