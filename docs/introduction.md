
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
    <!-- mp依赖:mybatisPlus 会自动的维护Mybatis 以及MyBatis-spring相关的依赖-->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus</artifactId>
        <version>2.3</version>
    </dependency>
    <!--junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.9</version>
    </dependency>
    <!-- log4j -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <!-- c3p0 -->
    <dependency>
        <groupId>com.mchange</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.5.2</version>
    </dependency>
    <!-- mysql -->
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.11</version>
    </dependency>
    <!-- spring -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>4.3.10.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-orm</artifactId>
        <version>4.3.10.RELEASE</version>
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
jdbc.url=jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Hongkong
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

然后，我们就写一些基本的单元测试方法，测试我们的CRUD，来到我们的测试类中，如下：

```java
public class TestMp {

    private ApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");

    private UserMapper userMapper = ioc.getBean("userMapper",UserMapper.class);
}
```

> 说明：在测试类中我们将使用从SpringIOC容器中获取的userMapper进行CRUD一系列操作

`保存一条数据userMapper.insert(user);`

```java
@Test
public void insert(){
    User user = new User();
    user.setUserName("luokangyuan");
    user.setAge(66);
    user.setEmail("2356775645@qq.com");
    user.setGender(2);
    Integer result = userMapper.insert(user);
    System.out.println(result);
    // 直接获取插入数据返回的自增主键值
    System.out.println(user.getId()+"======");
}
```

插入数据的方法有两个：`insert和insertAllColumn`，二者的执行结果是一样的，区别在于，前者会根据实体类的每一个属性值进行一个非空校验，在插入的sql语句中不会出现实体类属性为空的字段；

> 注意：在没有使用全局配置之前，我们需要指定实体类对应的数据库表和主键生成策略

* 主键生成策略：`@TableId(type = IdType.AUTO,value = "id")`,value属性值当实体类字段名和数据库一致时可以不写，这里的value指的是数据库字段名称，type的类型有以下几种：
  * IdType.AUTO：数据库ID自增
  * IdType.INPUT：用户输入ID
  * IdType.ID_WORKER：全局唯一ID，内容为空自动填充（默认配置）
  * IdType.UUID：全局唯一ID，内容为空自动填充 
* 实体对应表名注解：`@TableName(value = "tbl_user")`;指定当前实体类对应的数据库表
* 数据库字段映射名称：`@TableField(value = "user_name")`,当禁止驼峰映射规则后可以使用
* 忽略插入到表的字段：`@ableField(exist = false)`,如下，数据库没有money这个字段，如果不忽略，那么插入就会报错，找不到这个字段；

```java
@TableField(exist = false)
private Double money;
```

`更新一条数据`

```java
@Test
public void update(){
    User user = new User();
    user.setId(7);
    user.setUserName("王八");
    user.setAge(56);
    //user.setEmail("2356775645@qq.com");
    user.setGender(2);
    Integer result = userMapper.updateById(user);
}
```

同理：更新方法也有两个`updateById和updateAllColumnById`,前者会对实体类属性名进行非空校验，为空的就不会出现在sql语句中，也就是不会更新原有数据，后者是会更新所有列，如果实体类属性值为空，则数据库对应字段名更新为null；

`查询一条数据`

```java
@Test
public void select(){
    // 1.通过ID查询一条数据
    User user = userMapper.selectById(7);
    // 2.通过多个列进行查询,如果查处的数据有多条就会报错
    User u = new User();
    u.setId(2);
    u.setUserName("张三");
    User user1 = userMapper.selectOne(u);
    // 3.查询符合多个ID的数据,使用的是in关键字查询
    List<Integer> ids = new ArrayList<Integer>();
    ids.add(3);
    ids.add(4);
    ids.add(5);
    List<User> users = userMapper.selectBatchIds(ids);
    // 4.通过封装map条件,注意的是封装的是列字段名，不是实体里属性名，
    // map中的key充当sql中的条件名称
    Map<String,Object> maps = new HashMap<String, Object>();
    maps.put("user_name","张三");
    maps.put("age",347);
    List<User> users1 = userMapper.selectByMap(maps);
    // 5.分页查询方法,查看第二页，每页2条数据,在sql语句并没有limit关键字
    // 所以要实现物理分页，还需借助插件，例如mybatis的pageHepler或者MybatisPlus提供的分页插件
    List<User> users2 = userMapper.selectPage(new Page<User>(2, 2), null);
}
```

`删除一条数据`

```java
@Test
public void delete(){
    // 1.根据ID删除
    Integer integer = userMapper.deleteById(8);
    // 2.根据条件删除，map中的key为列名，千万注意
    Map<String ,Object> maps = new HashMap<String, Object>();
    maps.put("age",66);
    maps.put("gender",2);
    Integer integer1 = userMapper.deleteByMap(maps);
    // 3.根据ID批量删除,使用in关键字
    List<Integer> ids = new ArrayList<Integer>();
    ids.add(5);
    ids.add(7);
    Integer integer2 = userMapper.deleteBatchIds(ids);
}
```



## 3.10.MybatisPlus全局配置

在前面的CRUD操作中，我们会使用直接注解指定主键生成策略和表名到实体类的映射，但是配置的仅仅会对当前实体类起作用，所以，引入了全局配置，如下：

```xml
<!-- 定义MybatisPlus的全局策略配置-->
<bean id ="globalConfiguration" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
    <!--映射数据库下划线字段名到数据库实体类的驼峰命名的映射-->
    <property name="dbColumnUnderline" value="true"></property>
    <!-- 全局的主键策略 -->
    <property name="idType" value="0"></property>
    <!-- 全局的表前缀策略配置 -->
    <property name="tablePrefix" value="tbl_"></property>
</bean>
```

然后，将MybatisPlus全局配置注入到sqlSessionFactoryBean中

```xml
<!-- 注入全局MP策略配置 -->
<property name="globalConfig" ref="globalConfiguration"></property>
```

## 3.11.mybatisPlusCRUD总结

在前面，我们实现了基本的CRUD操作，操作简单，仅仅只需继承一个BaseMapper就可以完成，实现单一，批量，分页等等一系列操作，很大的减少了开发负担，但这仅仅是Mybatisplus的冰山一角，当我们需要多条件查询的时候，就会使用到MybatisPlus中强大的条件构造器EntityWrapper；

# 四、条件查询

条件构造器就是EntityWrapper，就是一个封装查询条件对象，让开发者自由的定义查询条件，主要用于sql的拼接，排序或者实体参数等；[条件构造器](http://mp.baomidou.com/#/wrapper)

> 注意：使用的参数是数据库字段名称，不是Java类属性名

## 4.1.selectPage中的条件查询

```java
@Test
public void entityWrapperTedst(){
    // 分页查询第一页，每页2条记录，年龄在41-53之间，genger为1，user_name为王五的用户
    List<User> users = userMapper.selectPage(new Page<User>(1, 2),
         new EntityWrapper<User>()
         .between("age", 41, 53)
         .eq("gender",1)
         .eq("user_name","王五")
	);

}
```

## 4.2.模糊查询和或查询

```java
@Test
public void selectListTest(){
    List<User> users = userMapper.selectList(new EntityWrapper<User>()
          .eq("gender", 1)
          .like("user_name", "三")
          .orNew()
          .like("email", "5")
    );
}
```

使用或条件查询可以使用`or()`也可以使用`orNew()`,二者的区别在于sql中的条件部分不一样，如下：

`使用or()的sql语句`

```sql
SELECT id AS id,user_name AS userName,email,gender,age FROM tbl_user WHERE (gender = ? AND user_name LIKE ? OR email LIKE ?)
```

`使用orNew()的sql语句`

```sql
SELECT id AS id,user_name AS userName,email,gender,age FROM tbl_user WHERE (gender = ? AND user_name LIKE ?) OR (email LIKE ?)
```

## 4.3.修改满足条件的数据

```java
@Test
public void updataByEntityWrapper(){
    User user = new User();
    user.setEmail("luokangyuan@sina,com");
    user.setAge(24);
    user.setUserName("四川麻酱");
    Integer update = userMapper.update(user, new EntityWrapper<User>()
       .eq("user_name","李四")
       .eq("age",53)
       );
}
```

## 4.4.删除满足条件的数据

```java
@Test
public void deleteByEntityWrapper(){
    userMapper.delete(new EntityWrapper<User>()
      .eq("user_name","王八")
      .eq("age",56)
    );
}
```

## 4.5.条件查询之Condition

Condition继承了Wrapper类，另外，我们不需要再new一个Condition对象，直接调用condition类的静态方法create就可以得到一个condition对象，然后使用wrapper的所有方法，简单使用如下：

```java
@Test
public void testCondition(){
    userMapper.selectPage(new Page<User>(1,2), Condition.create()
     .between("age",45,56)
   );
}
```

# 五、活动记录AR

Active Record(活动记录)，简称AR，是一种领域模型模式，特点就是一个模型类对应关系型数据库中的一个表，而模型类的一个实例对应表中的一条记录；

## 5.1.开启AR模式

开启AR模式的方法很简单，就是让我们的实体类继承Model类，并实现其抽象方法，指定主键即可，如下

```java
public class User extends Model<User> {

    @Override
    protected Serializable pkVal() {
        return id;
    }
}
```

## 5.2.插入一条数据

```java
@Test
public void insert(){
    User user = new User();
    user.setUserName("杉木");
    user.setAge(25);
    user.setEmail("shancnu@163.com");
    user.setGender(1);
    boolean rs = user.insert();
}
```

## 5.3.修改一条数据

```java
@Test
public void update(){
    User user = new User();
    user.setUserName("杉木博客");
    user.setAge(35);
    user.setId(10);
    boolean rs = user.updateById();
}
```

> 说明：和通用的CRUD中的更新方法一样，`updateAllColumnById()`会更新所有列

## 5.4.查询数据

```java
@Test
public void select(){
    User user = new User();
    user.setId(2);
    // 根据ID查询一条数据
    User user1 = user.selectById();
    // 查询所有的数据
    List<User> users = user.selectAll();
    // 根据条件查询
    List<User> usersList = user.selectList(new EntityWrapper<User>()
           .like("user_name", "三"));
    // 统计满足条件的数据数量
    int gender = user.selectCount(new EntityWrapper<User>().eq("gender", 1));
    // 统计全表数量
    int count = user.selectCount(null);
}
```

## 5.5.删除一条数据

```java
@Test
public void delete(){
    User user = new User();
    user.setId(7);
    boolean rs = user.deleteById();
    System.out.print(rs);
}
```

当然，也可以根据条件删除多条数据，这里需要注意的是：`当删除不存在的数据时候，返回的结果也是true；`

```java
// 删除不存在逻辑属于成功
public static boolean delBool(Integer result) {
    return null != result && result >= 0;
}
```

## 5.6.分页查询数据

在前面的CRUD中的分页查询返回的是list数据集合，但是在AR中返回的却是Page对象，如下

```java
@Test
public void selectPage(){
    User user = new User();
    Page<User> userPage = user.selectPage(new Page<User>(1, 2),
          new EntityWrapper<User>().like("user_name", "三"));
    List<User> records = userPage.getRecords();
}
```

## 5.7.AR总结

AR提供的是一种更为快速的实现CRUD操作，本质很是调用Mybatis对应的方法，说的简单一点就是语法糖；

糖虽然好吃，但是，不要管不住嘴；

# 六、代码生成器

我们知道mybatis有一个代码生成器MBG，可以生成Java实体类mapper接口和映射文件，但是MybatisPlus却更加强大，可以生成service和controller，可以配置实体类是否支持AR等，[代码生成器](http://mp.baomidou.com/#/generate-code)

> 说明：建议数据库表名和字段名采用驼峰命名方式，和实体来一致，可以避免在对应实体类产生的性能损耗

## 6.1.导入依赖

```xml
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.0</version>
</dependency>
```

> 说明：MybatisPlus默认使用的是velocity模版引

## 6.2.编写配置类

```java
@Test
public void testMbg(){

    // 1.全局配置
    GlobalConfig globalConfig = new GlobalConfig();
    globalConfig.setActiveRecord(true)// 是否开启AR模式
        .setAuthor("luokangyuan") // 指定作者
        .setOutputDir("/Users/luokangyuan/Documents/project/mybatisdemo/src/main/java")
        .setFileOverride(true) // 指定文件覆盖
        .setIdType(IdType.AUTO) // 设置主键自增策略
        .setServiceImplName("%sService") // 设置生成的services接口的名字的首字母是否为I
        .setBaseResultMap(true) // 基本的字段映射
        .setBaseColumnList(true); // 基本的sql片段
    // 2.配置数据源
    DataSourceConfig dataSourceConfig = new DataSourceConfig();
    dataSourceConfig.setDbType(DbType.MYSQL) // 设置数据库类型
        .setDriverName("com.mysql.jdbc.Driver")
        .setUrl("jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Hongkong")
        .setUsername("root")
        .setPassword("jiamei@20141107.");
    // 3.策略配置
    StrategyConfig strategyConfig = new StrategyConfig();
    strategyConfig.setCapitalMode(true) //全局大写命名
        .setDbColumnUnderline(true) // 指定表名和字段名是否使用了下划线
        .setNaming(NamingStrategy.underline_to_camel) // 数据库字段下划线转驼峰命令策略
        .setTablePrefix("tbl_") // 设置表前缀
        .setInclude("tbl_dept","tbl_file"); // 设置需要生成的表
    // 4.包名策略配置
    PackageConfig packageConfig = new PackageConfig();
    packageConfig.setParent("com.luo") // 设置父包
        .setMapper("mapper")
        .setService("service")
        .setController("controller")
        .setEntity("beans")
        .setXml("mapper");
    // 5. 开始生成代码
    AutoGenerator autoGenerator = new AutoGenerator();
    autoGenerator.setGlobalConfig(globalConfig)
        .setDataSource(dataSourceConfig)
        .setStrategy(strategyConfig)
        .setPackageInfo(packageConfig);
    autoGenerator.execute();
}
```

## 6.3.生成的service代码查看

```java
@Service
public class DeptService extends ServiceImpl<DeptMapper, Dept> implements IDeptService {

}
```

DeptService继承了ServiceImpl，在ServiceImpl中就已经注入了DeptMapper，所以，我们就不需要再次注入，在ServiceImpl中也帮我们提供了常用的CRUD方法，我们可以直接使用，

```java
@Controller
@RequestMapping("/dept")
public class DeptController {
    @Autowired
    private IDeptService service;

    public String select(){
        service.selectList(null);
        return null;
    }
}
```

# 七、插件扩展

## 7.1.注册分页插件

```xml
<bean id="sqlSessionFactoryBean" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
    <!-- 数据源 -->
    <property name="dataSource" ref="dataSource"></property>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    <!-- 别名处理 -->
    <property name="typeAliasesPackage" value="com.luo.beans"></property>
    <!-- 注入全局MP策略配置 -->
    <property name="globalConfig" ref="globalConfiguration"></property>
    <property name="plugins">
        <list>
            <bean class="com.baomidou.mybatisplus.plugins.PaginationInterceptor"></bean>
        </list>
    </property>
</bean>
```

`真正的分页查询`

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");

private UserMapper userMapper = ioc.getBean("userMapper",UserMapper.class);

@Test
public void testPage(){
    Page<User> page = new Page<User>(1,4);
    List<User> users = userMapper.selectPage(page,null);
    page.setRecords(users);
    System.out.print("总记录数"+page.getTotal());
    System.out.print("当前页码"+page.getCurrent());
    System.out.print("总页码"+page.getPages());
    System.out.print("每页记录数"+page.getSize());
    System.out.print("是否有前一页"+page.hasPrevious());
    System.out.print("是否有后一页"+page.hasNext());
}
```

> 说明：我们可以将分页查询的数据放在page对象中，返回前端一个page对象即可

## 7.2.执行分析插件

```xml
<bean class="com.baomidou.mybatisplus.plugins.SqlExplainInterceptor">
    <property name="stopProceed" value="true"></property>
</bean>
```

测试如下：

```java
@Test
public void testDeltetAll(){
    userMapper.delete(null);
}
```

sql分析插件只支持mysql5.6.3以上的版本，本质就是通过sql分析命令Explain分析当前的sql语句，根据结果集中的Extra列来断定当前是否全表操作；

## 7.3.性能分析插件

性能分析插件用于输出每秒sql语句和其执行时间,首先注册插件，如下：

```xml
<bean class="com.baomidou.mybatisplus.plugins.PerformanceInterceptor">
    <property name="format" value="true"></property>
</bean>
```

测试如下：

```java
@Test
public  void testPer(){
    Dept dept = new Dept();
    dept.setDeptName("开发部");
    dept.setDeptCount("34");
    dept.setDeptBegintime(new Date());
    dept.insert();
}
```

结果如下：

```sql
 Time：142 ms - ID：com.luo.mapper.DeptMapper.insert
 Execute SQL：
    INSERT 
    INTO
        tbl_dept
        ( dept_count, dept_name, dept_beginTime ) 
    VALUES
        ( '34', '开发部', '2018-08-26 23:09:17.293' )]
```

## 7.4.乐观锁插件

当我们在开发中，有时需要判断，当我们更新一条数据库记录时，希望这条记录没有被别人更新，这个时候就可以使用乐观锁插件，他的原理就是，取出记录时，获取当前的version，更新的时候带上这个version，执行更新的时候`set version = yourVersion+1 where version = yourVersion`,如果version不对，则更新失败，注意的是：`@version用于注解实体字段，必须要有`；

首先，注册插件

```xml
<bean class="com.baomidou.mybatisplus.plugins.OptimisticLockerInterceptor"></bean>
```

实体类添加对应属性,同时数据库表也要添加对应字段

```java
@Version
private Integer version;
```

测试如下：

```java
@Test
public void testVersion(){
    Dept dept = new Dept();
    dept.setDeptName("测试部");
    dept.setVersion(1);
    dept.setId(1);
    dept.updateById();
}
```

如果：这个时候将数据库version改为2，在执行更新就会显示更新记录数为0；

# 八、自定义全局操作

## 8.1.自定义全局实例

自定义全局操作，就是将我们需要的sql在项目启动的时候就注入到全局中，操作步骤如下：

* 在Mapper接口中定义我们需要注入的方法；
* 扩展AutoSqlInjector中的inject方法，实现Mapper中我们自定义方法要注入的sql；
* 最后，在全局配置中，配置我们自定义的注入器即可；

**第一步：mapper中定义方法**

```java
public interface UserMapper extends BaseMapper<User> {
    
    int deleteAll();
}
```

**第二步：重写inject方法**

```java
public class MySqlInjector  extends AutoSqlInjector {

    @Override
    public void inject(Configuration configuration, MapperBuilderAssistant builderAssistant, Class<?> mapperClass, Class<?> modelClass, TableInfo table) {
        // 构造sql语句
        String sql = "delete from " + table.getTableName();
        // 构造方法名
        String method = "deleteAll";
        // 构造SqlSource对象
        SqlSource sqlSource = languageDriver.createSqlSource(configuration, sql, modelClass);
        // 构造一个删除的MapperStatement
        this.addDeleteMappedStatement(mapperClass,method,sqlSource);
    }
}ß
```

**第三步：注入自定义配置**

```xml
<bean id ="globalConfiguration" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
     <!--映射数据库下划线字段名到数据库实体类的驼峰命名的映射-->
     <property name="dbColumnUnderline" value="true"></property>
     <!-- 全局的主键策略 -->
     <property name="idType" value="0"></property>
     <!-- 全局的表前缀策略配置 -->
     <property name="tablePrefix" value="tbl_"></property>
     <!--注入自定义全局操作-->
     <property name="sqlInjector" ref="mySqlInjector"></property>
</bean>
<bean class="com.luo.injector.MySqlInjector" id="mySqlInjector"></bean>
```

**测试**

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");

private UserMapper userMapper = ioc.getBean("userMapper",UserMapper.class);

@Test
public void testInject(){
    int rs = userMapper.deleteAll();
}
```

## 8.2.逻辑删除

所谓逻辑删除，就是不真正的删除数据的记录，而是变为无效状态，在MybatisPlus中，给我们提供logicSqlInjector

**第一步：数据库添加逻辑字段**

**第二步：实体类添加对应属性和注解**

```java
@TableLogic
private Integer logicFlag;
```

**第三步：MybatisPlus全局配置中加入logicSqlInjector**

```xml
<bean id ="globalConfiguration" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
    <!--映射数据库下划线字段名到数据库实体类的驼峰命名的映射-->
    <property name="dbColumnUnderline" value="true"></property>
    <!-- 全局的主键策略 -->
    <property name="idType" value="0"></property>
    <!-- 全局的表前缀策略配置 -->
    <property name="tablePrefix" value="tbl_"></property>
    <!--注入自定义全局操作-->
    <property ref="logicSqlInjector" name="sqlInjector"></property>
    <!--注入逻辑删除全局值-->
    <property name="logicDeleteValue" value="-1"></property>
    <property name="logicNotDeleteValue" value="1"></property>
</bean>
<!--逻辑删除-->
<bean class="com.baomidou.mybatisplus.mapper.LogicSqlInjector" id="logicSqlInjector"></bean>
```

**测试**

```java
@Test
public void testLogin(){
    Integer integer = userMapper.deleteById(1);
}
```

> 说明：我们做的是删除操作，但是，执行的却是update操作，同时，`查询的时候自动添加了有效判断`

# 九、公共字段填充

这里涉及到一个元数据处理接口`MetaObjectHandler`,元对象是Mybatis提供的一个用于更加方便，更加优雅的访问对象的属性，给对象的属性赋值的一个对象，本质上metaObject获取对象的值或者是给对象的属性赋值，都是通过反射获取到属性对应方法的Invoker；

## 9.1.使用实例

**第一步：注解需要填充的字段**

```java
@TableField(value = "user_name",fill = FieldFill.INSERT_UPDATE)
private String userName;
```

**第二步：自定义填充处理器**

```java
public class MetaHandler extends MetaObjectHandler {
    /**
     * 插入操作：自动填充
     * @param metaObject
     */
    public void insertFill(MetaObject metaObject) {
        // 获取到需要被填充的字段值
        Object userName = getFieldValByName("userName", metaObject);
        if(userName == null){
            setFieldValByName("userName","四川码酱",metaObject);
        }
    }

    /**
     * 更新操作：自动填充
     * @param metaObject
     */
    public void updateFill(MetaObject metaObject) {
        // 获取到需要被填充的字段值
        Object userName = getFieldValByName("userName", metaObject);
        if(userName == null){
            setFieldValByName("userName","康哥哥",metaObject);
        }
    }
}
```

**第三步：注入全局配置**

```xml
<bean id ="globalConfiguration" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
    <!--映射数据库下划线字段名到数据库实体类的驼峰命名的映射-->
    <property name="dbColumnUnderline" value="true"></property>
    <!-- 全局的主键策略 -->
    <property name="idType" value="0"></property>
    <!-- 全局的表前缀策略配置 -->
    <property name="tablePrefix" value="tbl_"></property>
    <!--注入自定义全局操作-->
    <!--<property name="sqlInjector" ref="mySqlInjector"></property>-->
    <property ref="logicSqlInjector" name="sqlInjector"></property>
    <!--注入逻辑删除全局值-->
    <property name="logicDeleteValue" value="-1"></property>
    <property name="logicNotDeleteValue" value="1"></property>
    <!--注入公共字段填充处理器-->
    <property name="metaObjectHandler" ref="metaHandler"></property>
</bean>
<!--自定义公共字段填充处理器-->
<bean class="com.luo.bandler.MetaHandler" id="metaHandler"></bean>
```

**第四步：测试**

```java
@Test
public void testCom(){
    User user = new User();
    user.setId(11);
    user.setLogicFlag(1);
    user.updateById();
}
```

# 十、IEDA开发插件

## 10.1.安装方法

打开IDEA设置--Plugins--Browse repositories --搜索mybatisx,安装即可

## 10.2.支持的功能

根据mapper接口方法自动生成xml文件，接口方法定位xml,xml自动定位mapper接口；





