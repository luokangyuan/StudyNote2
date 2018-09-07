# MyBatis简介

## MyBatis概述

* MyBatis 是支持定制化SQL、存储过程以及高级映射的优秀的持久层框架。
* MyBatis 避免了几乎所有的JDBC 代码和手动设置参数以及获取结果集。
* MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录.

## Mybatis与其他持久化方式对比

* MyBatis是一个半自动化的持久化框架
* JDBC是SQL夹在Java代码中，耦合度高导致硬编码，维护不易且实际开发中SQL会经常变化
* Hibernate和JPA是内部自动产生的SQL语句，不容易做特殊优化，长而复杂的SQL，hibernate处理也不容易，是基于全映射的全自动化框架，大量子弹的pojo进行部分映射比较困难，导致数据库性能下降

**对于开发人员，核心SQL需要自己优化，所以需要SQL和java编码分开，功能界面明显，一个专注业务，一个专注数据**

## 文档资料

**下载地址：**https://github.com/mybatis/mybatis-3

**中文文档：**http://www.mybatis.org/mybatis-3/zh/index.html

# MyBatis的HelloWord

## 概述

随着Maven的流行，现在几乎很少有使用jar的方式来搭建开发环境，这里也就不在单个使用Mybatis去操作数据库，不会的可以自行百度，MyBatis只是一个持久化框架，只有和其他框架整合才能更好的使用，例如SpringMVC，SpringBoot等，与Spring整合后，Mybatis的一些配置文件都会交于Spring管理。

# MyBatis的全局配置文件

## 概述

MyBatis的全局配置文件可以配置的属性如下

- properties 属性
- settings 设置
- typeAliases 类型别名
- typeHandlers 类型处理器
- objectFactory 对象工厂
- plugins 插件
- environments 环境
  - environment 环境变量
    - transactionManager 事务管理器
    - dataSource 数据源
- databaseIdProvider 数据库厂商标识
- mappers 映射器

## properties属性

MyBatis使用properties来引入外部properties配置文件的内容，resource：引入类路径下的资源，url引入网络路径或者磁盘路径下的资源。可以用于将数据源连接信息放在properties文件中，与Spring整合后就写在Spring的配置文件中。

**引入外部properties文件**

```xml
<properties resource="org/mybatis/example/config.properties"></properties>
```

**使用引入的properties文件**

```xml
<dataSource type="POOLED">
  <property name="driver" value="${jdbc.driver}"/>
  <property name="url" value="${jdbc.url}"/>
  <property name="username" value="${jdbc.username}"/>
  <property name="password" value="${jdbc.password}"/>
</dataSource>
```

## settings运行时设置

这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。下表描述了设置中各项的意图、默认值等。

| 设置参数                                     | 描述                                                         | 有效值                      | 默认值  |
| :------------------------------------------- | ------------------------------------------------------------ | --------------------------- | ------- |
| cacheEnabled                                 | 全局开启或关闭配置文件中的所有映射器任何缓存                 | true \| false               | true    |
| lazyLoadingEnabled                           | 延迟加载的全局开关                                           | true \| false               | false   |
| aggressive<br />LazyLoading                  | 开启，任何方法的调用都会加载该对象的所有属性。<br />否则，每个属性会按需加载 | true \| false               | false   |
| multipleResult<br />SetsEnabled              | 是否允许单一语句返回多结果集                                 | true \| false               | true    |
| useColumnLabel                               | 使用列标签代替列名。                                         | true \| false               | true    |
| useGeneratedKeys                             | 允许 JDBC 支持自动生成主键<br /> 如果设置为 true 则这个设置强制使用自动生成主键 | true \| false               | False   |
| autoMappingBehavior                          | 指定 MyBatis 应如何自动映射列到字段或属性。<br /> NONE 表示取消自动映射；<br />PARTIAL 只会自动映射没有定义嵌套结果集映射的结果集。<br /> FULL 会自动映射任意复杂的结果集 | NONE, <br>PARTIAL, <br>FULL | PARTIAL |
| autoMapping<br />Unknown<br />ColumnBehavior | 指定发现自动映射目标未知列（或者未知属性类型）的行为。<br />`NONE`: 不做任何反应<br />`WARNING`: 输出提醒 | NONE, WARNING, FAILING      | NONE    |
| defaultExecutorType                          | 配置默认的执行器。SIMPLE 就是普通的执行器；REUSE 执行器会重用预处理语句（prepared statements）； BATCH 执行器将重用语句并执行批量更新。 | SIMPLE REUSE BATCH          | SIMPLE  |
| default<br />StatementTimeout                | 设置超时时间，它决定驱动等待数据库响应的秒数。               | 任意正整数                  |         |
| defaultFetchSize                             | 为驱动的结果集获取数量（fetchSize）设置一个提示值。此参数只可以在查询设置中被覆盖。 | 任意正整数                  |         |
| safeRow<br />BoundsEnabled                   | 允许在嵌套语句中使用分页（RowBounds）。如果允许使用则设置为false。 | true \| false               | False   |
| safeResult<br />HandlerEnabled               | 允许在嵌套语句中使用分页（ResultHandler）。如果允许使用则设置为false。 | true \| false               | True    |
| mapUnderscore<br />ToCamelCase               | 是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。 | true \| false               | False   |
| localCacheScope                              | MyBatis 利用本地缓存机制（Local Cache）防止循环引用（circular references）和加速重复嵌套查询。 默认值为 SESSION，这种情况下会缓存一个会话中执行的所有查询。 若设置值为 STATEMENT，本地会话仅用在语句执行上，对相同 SqlSession 的不同调用将不会共享数据。 | SESSION \|<br> STATEMENT    | SESSION |
| jdbcTypeForNull                              | 当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型。 某些驱动需要指定列的 JDBC 类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR 或 OTHER。 |                             | OTHER   |
| lazyLoadTrigger<br />Methods                 | 指定哪个对象的方法触发一次延迟加载。                         |                             |         |
| defaultScripting<br />Language               | 指定动态 SQL 生成的默认语言。                                |                             |         |
| defaultEnum<br />TypeHandler                 | 指定 Enum 使用的默认 `TypeHandler` 。 (从3.4.5开始)          |                             |         |
| callSettersOnNulls                           | 指定当结果集中值为 null 的时候是否调用映射对象的 setter（map 对象时为 put）方法，这对于有 Map.keySet() 依赖或 null 值初始化的时候是有用的。注意基本类型（int、boolean等）是不能设置成 null 的。 | true \| false               | false   |
| returnInstance<br />ForEmptyRow              | 当返回行的所有列都是空时，MyBatis默认返回`null`。 当开启这个设置时，MyBatis会返回一个空实例。 请注意，它也适用于嵌套的结果集 (i.e. collectioin and association)。（从3.4.2开始） | true \| false               | false   |
| logPrefix                                    | 指定 MyBatis 增加到日志名称的前缀。                          | 任何字符串                  |         |
| logImpl                                      | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。        |                             |         |
| proxyFactory                                 | 指定 Mybatis 创建具有延迟加载能力的对象用到的代理工具。      | CGLIB \| JAVASSIST          |         |

## 常用的Setting设置

| 设置参数                 | 描述                                         | 默认值 |
| ------------------------ | -------------------------------------------- | ------ |
| mapUnderscoreToCamelCase | 是否开启驼峰命名规则映射A_COLUNM到aColumn    | false  |
| defaultStatementTimeout  | 设置超时时间，它决定驱动等待数据库响应的秒数 |        |

## Settings设置示例

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```

## typeAliases别名

类型别名是为 Java 类型设置一个短的名字。它只和 XML 配置有关，存在的意义仅在于用来减少类完全限定名的冗余,但是往往我们不会使用别名，是为了方便查看代码。

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```

指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，给包和子包下的所有类起一个默认的别名（类名小写） 

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

每一个在包 `domain.blog` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。 比如 `domain.blog.Author` 的别名为 `author`；若有注解，则别名为其注解值。 

```java
@Alias("author")
public class Author {}
```

## typeHandlers 类型处理器

无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。下表描述了一些默认的类型处理器。 

| 类型处理器                   | Java类型                        | JDBC类型                                                     |
| ---------------------------- | ------------------------------- | ------------------------------------------------------------ |
| BooleanTypeHandler           | `java.lang.Boolean`, `boolean`  | 数据库兼容的 `BOOLEAN`                                       |
| `ByteTypeHandler`            | `java.lang.Byte`, `byte`        | 数据库兼容的 `NUMERIC` 或 `BYTE`                             |
| `ShortTypeHandler`           | `java.lang.Short`, `short`      | 数据库兼容的 `NUMERIC` 或 `SHORT INTEGER`                    |
| `IntegerTypeHandler`         | `java.lang.Integer`, `int`      | 数据库兼容的 `NUMERIC` 或 `INTEGER`                          |
| `LongTypeHandler`            | `java.lang.Long`, `long`        | 数据库兼容的 `NUMERIC` 或 `LONG INTEGER`                     |
| `FloatTypeHandler`           | `java.lang.Float`, `float`      | 数据库兼容的 `NUMERIC` 或 `FLOAT`                            |
| `DoubleTypeHandler`          | `java.lang.Double`, `double`    | 数据库兼容的 `NUMERIC` 或 `DOUBLE`                           |
| `BigDecimalTypeHandler`      | `java.math.BigDecimal`          | 数据库兼容的 `NUMERIC` 或 `DECIMAL`                          |
| `StringTypeHandler`          | `java.lang.String`              | `CHAR`, `VARCHAR`                                            |
| `ClobReaderTypeHandler`      | `java.io.Reader`                | -                                                            |
| `ClobTypeHandler`            | `java.lang.String`              | `CLOB`, `LONGVARCHAR`                                        |
| `NStringTypeHandler`         | `java.lang.String`              | `NVARCHAR`, `NCHAR`                                          |
| `NClobTypeHandler`           | `java.lang.String`              | `NCLOB`                                                      |
| `BlobInputStreamTypeHandler` | `java.io.InputStream`           | -                                                            |
| `ByteArrayTypeHandler`       | `byte[]`                        | 数据库兼容的字节流类型                                       |
| `BlobTypeHandler`            | `byte[]`                        | `BLOB`, `LONGVARBINARY`                                      |
| `DateTypeHandler`            | `java.util.Date`                | `TIMESTAMP`                                                  |
| `DateOnlyTypeHandler`        | `java.util.Date`                | `DATE`                                                       |
| `TimeOnlyTypeHandler`        | `java.util.Date`                | `TIME`                                                       |
| `SqlTimestampTypeHandler`    | `java.sql.Timestamp`            | `TIMESTAMP`                                                  |
| `SqlDateTypeHandler`         | `java.sql.Date`                 | `DATE`                                                       |
| `SqlTimeTypeHandler`         | `java.sql.Time`                 | `TIME`                                                       |
| `ObjectTypeHandler`          | Any                             | `OTHER` 或未指定类型                                         |
| `EnumTypeHandler`            | Enumeration Type                | VARCHAR-任何兼容的字符串类型，存储枚举的名称（而不是索引）   |
| `EnumOrdinalTypeHandler`     | Enumeration Type                | 任何兼容的 `NUMERIC` 或 `DOUBLE` 类型，存储枚举的索引（而不是名称）。 |
| `InstantTypeHandler`         | `java.time.Instant`             | `TIMESTAMP`                                                  |
| `LocalDateTimeTypeHandler`   | `java.time.LocalDateTime`       | `TIMESTAMP`                                                  |
| `LocalDateTypeHandler`       | `java.time.LocalDate`           | `DATE`                                                       |
| `LocalTimeTypeHandler`       | `java.time.LocalTime`           | `TIME`                                                       |
| `OffsetDateTimeTypeHandler`  | `java.time.OffsetDateTime`      | `TIMESTAMP`                                                  |
| `OffsetTimeTypeHandler`      | `java.time.OffsetTime`          | `TIME`                                                       |
| `ZonedDateTimeTypeHandler`   | `java.time.ZonedDateTime`       | `TIMESTAMP`                                                  |
| `YearTypeHandler`            | `java.time.Year`                | `INTEGER`                                                    |
| `MonthTypeHandler`           | `java.time.Month`               | `INTEGER`                                                    |
| `YearMonthTypeHandler`       | `java.time.YearMonth`           | `VARCHAR` or `LONGVARCHAR`                                   |
| `JapaneseDateTypeHandler`    | `java.time.chrono.JapaneseDate` | `DATE`                                                       |

## plugins插件

MyBatis 允许你在已映射语句执行过程中的某一点进行拦截调用。默认情况下，MyBatis 允许使用插件来拦截的方法调用

- Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
- ParameterHandler (getParameterObject, setParameters)
- ResultSetHandler (handleResultSets, handleOutputParameters)
- StatementHandler (prepare, parameterize, batch, update, query)

## environments环境配置

MyBatis可以配置多种环境，default代表指定使用某种环境，这样就可以快速切换环境，**尽管可以配置多个环境，每个 SqlSessionFactory 实例只能选择其一** ，所以，如果你想连接两个数据库，就需要创建两个 SqlSessionFactory 实例，每个数据库对应一个。而如果是三个数据库，就需要三个实例 。**每个数据库对应一个 SqlSessionFactory 实例**

**环境配置实例**

```xml
<environments default="development">
  <environment id="development">
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
</environments>
```

**环境配置说明**

- 默认的环境 ID（比如:default="development"）。
- 每个 environment 元素定义的环境 ID（比如:id="development"）。
- 事务管理器的配置（比如:type="JDBC"）。
- 数据源的配置（比如:type="POOLED"）。

# MyBatis映射文件

MyBatis 的真正强大在于它的映射语句，也是它的魔力所在 。SQL 映射文件有很少的几个顶级元素 ，如下

- `cache` – 给定命名空间的缓存配置。
- `cache-ref` – 其他命名空间缓存配置的引用。
- `resultMap` – 是最复杂也是最强大的元素，用来描述如何从数据库结果集中来加载对象。
- `sql` – 可被其他语句引用的可重用语句块。
- `insert` – 映射插入语句
- `update` – 映射更新语句
- `delete` – 映射删除语句
- `select` – 映射查询语句

## Select查询

查询语句是 MyBatis 中最常用的元素之一 ，简单查询的 select 元素是非常简单的。比如 

```xml
<select id="selectPerson" parameterType="int" resultType="hashmap">
  SELECT * FROM PERSON WHERE ID = #{id}
</select>
```

该查询接受一个 int（或 Integer）类型的参数，返回一个 HashMap 类型的对象，键是列名，值是结果行中的对应值。 

select 元素有很多属性允许你配置，来决定每条语句的作用细节 ，如下

```xml
<select
  id="selectPerson"
  parameterType="int"
  parameterMap="deprecated"
  resultType="hashmap"
  resultMap="personResultMap"
  flushCache="false"
  useCache="true"
  timeout="10000"
  fetchSize="256"
  statementType="PREPARED"
  resultSetType="FORWARD_ONLY">
```

| 属性            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| `id`            | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| parameterMap    | 这是引用外部 parameterMap 的已经被废弃的方法。使用内联参数映射和 parameterType 属性。 |
| `resultType`    | 从这条语句中返回的期望类型的类的完全限定名或别名。注意如果是集合情形，那应该是集合可以包含的类型，而不能是集合本身。使用 resultType 或 resultMap，但不能同时使用。 |
| `resultMap`     | 外部 resultMap 的命名引用。结果集的映射是 MyBatis 最强大的特性，对其有一个很好的理解的话，许多复杂映射的情形都能迎刃而解。使用 resultMap 或 resultType，但不能同时使用。 |
| `flushCache`    | 将其设置为 true，任何时候只要语句被调用，都会导致本地缓存和二级缓存都会被清空，默认值：false。 |
| `useCache`      | 将其设置为 true，将会导致本条语句的结果被二级缓存，默认值：对 select 元素为 true。 |
| `timeout`       | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为 unset（依赖驱动）。 |
| `fetchSize`     | 这是尝试影响驱动程序每次批量返回的结果行数和这个设置值相等。默认值为 unset（依赖驱动）。 |
| `statementType` | STATEMENT，PREPARED 或 CALLABLE 的一个。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `resultSetType` | FORWARD_ONLY，SCROLL_SENSITIVE 或 SCROLL_INSENSITIVE 中的一个，默认值为 unset （依赖驱动）。 |
| `databaseId`    | 如果配置了 databaseIdProvider，MyBatis 会加载所有的不带 databaseId 或匹配当前 databaseId 的语句；如果带或者不带的语句都有，则不带的会被忽略。 |
| `resultOrdered` | 这个设置仅针对嵌套结果 select 语句适用：如果为 true，就是假设包含了嵌套结果集或是分组了，这样的话当返回一个主结果行的时候，就不会发生有对前面结果集的引用的情况。这就使得在获取嵌套的结果集的时候不至于导致内存不够用。默认值：`false`。 |
| `resultSets`    | 这个设置仅对多结果集的情况适用，它将列出语句执行后返回的结果集并每个结果集给一个名称，名称是逗号分隔的。 |

## insert update delete

数据变更语句 insert，update 和 delete 的实现非常接近 ,如下

```xml
<insert
  id="insertAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  keyProperty=""
  keyColumn=""
  useGeneratedKeys=""
  timeout="20">

<update
  id="updateAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">

<delete
  id="deleteAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">
```

**属性说明**

| `id`               | 命名空间中的唯一标识符，可被用来代表这条语句。               |
| ------------------ | ------------------------------------------------------------ |
| `parameterType`    | 将要传入语句的参数的完全限定类名或别名。这个属性是可选的，因为 MyBatis 可以通过 TypeHandler 推断出具体传入语句的参数，默认值为 unset。 |
| `flushCache`       | 将其设置为 true，任何时候只要语句被调用，都会导致本地缓存和二级缓存都会被清空，默认值：true（对应插入、更新和删除语句）。 |
| `timeout`          | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为 unset（依赖驱动）。 |
| `statementType`    | STATEMENT，PREPARED 或 CALLABLE 的一个。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `useGeneratedKeys` | （仅对 insert 和 update 有用）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系数据库管理系统的自动递增字段），默认值：false。 |
| `keyProperty`      | （仅对 insert 和 update 有用）唯一标记一个属性，MyBatis 会通过 getGeneratedKeys 的返回值或者通过 insert 语句的 selectKey 子元素设置它的键值，默认：`unset`。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表。 |
| `keyColumn`        | （仅对 insert 和 update 有用）通过生成的键值设置表中的列名，这个设置仅在某些数据库（像 PostgreSQL）是必须的，当主键列不是表中的第一列的时候需要设置。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表。 |
| `databaseId`       | 如果配置了 databaseIdProvider，MyBatis 会加载所有的不带 databaseId 或匹配当前 databaseId 的语句；如果带或者不带的语句都有，则不带的会被忽略。 |

**示例**

```xml
<insert id="insertAuthor">
  insert into Author (id,username,password,email,bio)
  values (#{id},#{username},#{password},#{email},#{bio})
</insert>

<update id="updateAuthor">
  update Author set
    username = #{username},
    password = #{password},
    email = #{email},
    bio = #{bio}
  where id = #{id}
</update>

<delete id="deleteAuthor">
  delete from Author where id = #{id}
</delete>
```

**多行插入**

```xml
<insert id="insertAuthor" useGeneratedKeys="true"
    keyProperty="id">
  insert into Author (username, password, email, bio) values
  <foreach item="item" collection="list" separator=",">
    (#{item.username}, #{item.password}, #{item.email}, #{item.bio})
  </foreach>
</insert>
```

**自动生成主键**

```xml
<insert id="insertAuthor" useGeneratedKeys="true"
    keyProperty="id">
  insert into Author (username,password,email,bio)
  values (#{username},#{password},#{email},#{bio})
</insert>
```



## 映射文件小结

* Mybatis允许增删改直接定义的返回值：Integer，Long，Boolean,返回的是改变数据库表的记录数和true和false
* 支持自动生成主键的字段，设置 useGeneratedKeys=”true”，然后再把 keyProperty 设置到目标属性上就OK了。
* 数据库还支持多行插入, 你也可以传入一个数组或集合，并返回自动生成的主键 

## 映射文件参数处理

* 单个参数：MyBatis不会做特殊处理，#{参数名/任意名}：取出参数值
* 多个参数：MyBatis会做特殊处理，多个参数被封装为一个map,key：param1...param10或者参数的索引
* 命名参数，多个参数使用#{param1}来取值导致错乱，故使用命名参数，明确指定封装map的key,如下

```java
public Person getPerson(@Param("id") Integer id,@Param("name") String laseName);
```

这个时候在xml文件中可以使用#{id}和#{name}来取值

* 如果传入多个参数正好是POJO：可以使用#{属性名}直接获取。
* 如果传入多个参数不是POJO,为了方便，我们可以传入map,如下

```java
public Person getPerson(Map<String,Object> map);
```

这个时候也就可以在xml文件中使用#{key}直接取出传入map的key对应的值

* 如果传入多个参数不是POJO,但是查询频率高，使用map不方便，可以编写一个TO(数据传输对象)
* 如果传入了一个Collection（list，set）类型或者数组，也会特殊处理，也是吧list或者数组封装到map中，传入的类型不一样，对应的key如下：Collection（collection）,List（list），数组（array）,示例如下

```java
public Person get(List<Integer> ids);
```

传入的是一个list集合，在xml文件中，我们如果需要取出list中的第一个元素为：#{list[0]}

## #和$取值区别

* 取值的方式#{}是以预编译的形式，将参数设置到sql语句中；PreparedStatement；防止sql注入
* ${}:取出的值直接拼装在sql语句中；会有安全问题；大多情况下，我们去参数的值都应该去使用#{}；
* 原生jdbc不支持占位符的地方我们就可以使用${}进行取值比如分表、排序。；按照年份分表拆分

```sql
select * from ${year}_salary where xxx;
select * from tbl_employee order by ${f_name} ${order}
```

## select查询返回类型

* 返回一个集合：resultType写集合中元素的类型
* 返回一条记录的map，key为列名，value为列对应的值，例如

```java
public Map<String ,Object> getPersonByMap(Integer id);
```

```xml
<select id = "getPersonByMap" resultType="map">
	select * from person where id = #{id}
</select>
```

* 返回多条记录封装的一个map，Map<Integer,Person>:key是这条记录的主键，值是记录封装后的pojo,如下

```java
@MapKey("id")
public Map<Integer,Person> getPersonByName(String name);
```

```xml
<select id = "getPersonByName" resultType="com.test.Person">
	select * from person where last_name like #{name}
</select>
```

@MapKey("id")注解表示使用那个属性作为返回结果map的key。

## resultMap自定义结果集

**示例**

```xml
<restMap id="baseMap" type="com.test.Person">
	<id column="id" property="id"/>
    <result column="last_name" property="lastName" ></result>
</restMap>
<select id="getPerson" restMap="baseMap">
	select * from person
</select>
```

**resultMap关联属性_级联属性封装结果集**

例如：员工有部门属性，联合查询返回封装结果

```xml
<restMap id="baseMap" type="com.test.Person">
	<id column="id" property="id"/>
    <result column="last_name" property="lastName" ></result>
     <result column="dept_id" property="dept.id" ></result>
     <result column="dept_name" property="dept.name" ></result>
</restMap>
```

**使用association指定联合的java对象**

```xml
<restMap id="baseMap" type="com.test.Person">
	<id column="id" property="id"/>
    <result column="last_name" property="lastName" ></result>
    <association property="dept" javaType="com.test.DepartMent">
         <id column="dept_id" property="id"/>
    	 <result column="dept_name" property="name" ></result>
    </association>
</restMap>
```

**使用collection定义关联集合的封装规则**

例如，查询部门的时候，查询出当前部门下的所有员工

```xml
<restMap id="baseMap" type="com.test.DepartMent">
	<id column="dept_id" property="id"/>
    <result column="dept_name" property="name" ></result>
    <collection property="persons" javaType="com.test.Person">
         <id column="id" property="id"/>
    	 <result column="last_name" property="lastName" ></result>
    </collection>
</restMap>
```

## sql标签

这个元素可以被用来定义可重用的 SQL 代码段，可以包含在其他语句中。它可以被静态地(在加载参数) 参数化. 不同的属性值通过包含的实例变化. 比如： 

```xml
<sql id="userColumns"> ${alias}.id,${alias}.username,${alias}.password </sql>
```

这个 SQL 片段可以被包含在其他语句中，例如： 

```xml
<select id="selectUsers" resultType="map">
  select
    <include refid="userColumns"><property name="alias" value="t1"/></include>,
    <include refid="userColumns"><property name="alias" value="t2"/></include>
  from some_table t1
    cross join some_table t2
</select>
```

# MyBatis的动态SQL

MyBatis 的强大特性之一便是它的动态 SQL。如果你有使用 JDBC 或其它类似框架的经验，你就能体会到根据不同条件拼接 SQL 语句的痛苦。例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL 这一特性可以彻底摆脱这种痛苦。 MyBatis 采用功能强大的基于 OGNL 的表达式来淘汰其它大部分元素。 如下

- if
- choose (when, otherwise)
- trim (where, set)
- foreach

## if的使用

动态 SQL 通常要做的事情是根据条件包含 where 子句的一部分。比如 

**注意：在xml文件中特殊符号，像<，>要使用转义字符**

```xml
<select id="findActiveBlogWithTitleLike"
     resultType="Blog">
  SELECT * FROM BLOG 
  WHERE state = ‘ACTIVE’ 
  <if test="title != null">
    AND title like #{title}
  </if>
</select>
```

## choose，when，otherwise

有时我们不想应用到所有的条件语句，而只想从中择其一项 ，如下

```xml
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <choose>
    <when test="title != null">
      AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}
    </when>
    <otherwise>
      AND featured = 1
    </otherwise>
  </choose>
</select>
```

## trim, where, set

在前面，如果所有的条件都是动态sql,那么可能会出现一下情况的SQL语句

```sql
SELECT * FROM BLOG WHERE
SELECT * FROM BLOG WHERE AND title like ‘someTitle’
```

出现以上错误的sql语句，MyBatis提供了一种解决方式

```xml
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG 
  <where> 
    <if test="state != null">
         state = #{state}
    </if> 
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
```

*where* 元素只会在至少有一个子元素的条件返回 SQL 子句的情况下才去插入“WHERE”子句。而且，若语句的开头为“AND”或“OR”，*where* 元素也会将它们去除 。**注意：WHERE只会去掉开头第一个AND或OR**

**使用where会出错的情况，And放在后面**

```xml
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG 
  <where> 
    <if test="state != null">
         state = #{state}  AND
    </if> 
    <if test="title != null">
        title like #{title} AND
    </if>
    <if test="author != null and author.name != null">
        author_name like #{author.name}
    </if>
  </where>
</select>
```

另外一种解决办法就是使用<trim>标签，使用where，也可能造成最后一个and，使用trim方法如下

```xml
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ... 
</trim>
```

*prefixOverrides* 属性会忽略通过管道分隔的文本序列（注意此例中的空格也是必要的）。它的作用是移除所有指定在 *prefixOverrides* 属性中的内容（移除前面多余的AND 或者OR），并且插入 *prefix* 属性中指定的内容。 使用suffixOverrides会移除后面多余的AND或者OR。

**set标签与if结合实现动态更新**

```xml
<update id="updateAuthorIfNecessary">
  update Author
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
  where id=#{id}
</update>
```

这里，*set* 元素会动态前置 SET 关键字，同时也会删掉无关的逗号，因为用了条件语句之后很可能就会在生成的 SQL 语句的后面留下这些逗号,也可以使用trim，注意这里我们删去的是后缀值，同时添加了前缀值。 

```xml 
<trim prefix="SET" suffixOverrides=",">
  ...
</trim>
```

## foreach

动态 SQL 的另外一个常用的操作需求是对一个集合进行遍历，通常是在构建 IN 条件语句的时候。比如： 

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")" index="i">
        #{item}
  </foreach>
</select>
```

**说明:**

* collection：指定要遍历的集合
* item：将当前遍历的每一个元素赋给指定的变量
* separator：每一个元素之间的分隔符
* open：遍历出所有的结果拼接一个开始的字符
* close：遍历出所有的结果拼接一个结束的字符
* index：遍历list的就是索引，遍历map的时候就是map的key,item是map的值

## Mysql下的批量插入

```java
public void addEmp(@Param("emps") List<Employee> emps);
```

```xml
<insert id="addEmp">
	INSERT into employee(name,age)values
  <foreach item="emp" index="index" collection="emps"
      open="(" separator="," close=")" index="i">
        #{emp.name}, #{emp.age}
  </foreach>
</insert>
```

## bind

`bind` 元素可以从 OGNL 表达式中创建一个变量并将其绑定到上下文。比如 

```xml
<select id="selectBlogsLike" resultType="Blog">
  <bind name="pattern" value="'%' + _parameter.getTitle() + '%'" />
  SELECT * FROM BLOG
  WHERE title LIKE #{pattern}
</select>
```

如果是模糊查询，使用下面的方式是行不通的，如下

```xml
<select>
    select * from person
    <if test="lastName != null">
        where lastName like '%#{lastName}%'
    </if>
</select>
```

解决方式之一，可以使用$符号(不安全)

```xml
<select>
    select * from person
    <if test="lastName != null">
        where lastName like '%${lastName}%'
    </if>
</select>
```

解决方式之二，使用bind标签

```xml
<select>
    <bind name="_lastName" value="'%'+lastName+'%'"></bind>
    select * from person
    <if test="lastName != null">
        where lastName like #{_lastName}
    </if>
</select>
```

```xml
<bind name="_lastName" value="'_'+lastName+'%'"></bind><!--表示以什么开始，后面是参数的模糊查询-->
```



# MyBatis的缓存机制

## 概述

MyBatis 包含一个非常强大的查询缓存特性,它可以非常方便地配置和定制。缓存可以极大的提升查询效率。MyBatis系统中默认定义了两级缓存，一级缓存和二级缓存。

* 默认情况下，只有一级缓存（SqlSession级别的缓存，也称为本地缓存）开启。
* 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
* 为了提高扩展性。MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

## 一级缓存（本地缓存）

与数据库同一次会话期间查询的数据会放在本地缓存中，以后如果需要获取相同数据，直接从缓存中拿，不再查询数据库

**一级缓存失效的四种情况**

一级缓存是sqlSession级别的缓存，也就说一个sqlSession拥有自己的一级缓存，一级缓存是一直开启的，没有使用一级缓存的情况

* 不同的sqlSession对应不同的一级缓存
* 同一个sqlSession,但是查询条件不一样
* 同一个sqlSession两次查询期间执行了任何一次增删改操作
* 同一个sqlSession两次查询期间手动清空了缓存

## 二级缓存（全局缓存）

* 二级缓存基于namespace默认不开启，需要手动配置
* MyBatis提供二级缓存的接口以及实现，缓存实现要求POJO实现Serializable接口
* **二级缓存在SqlSession 关闭或提交之后才会生效**

**工作机制**

* 一个会话，查询一条数据，这个数据就会放在当前会话的一级缓存中
* 如果会话关闭，一级缓存中的数据就会保存到二级缓存中，新的查询信息，就参照二级缓存

**使用步骤**

* 全局配置文件中开启二级缓存

```xml
<setting name="cacheEnabled" value="true"/>
```

* 需要使用二级缓存的映射文件mapper.xml处使用cache配置缓存

```xml
<cach></cach>
```

* 注意：POJO需要实现Serializable接口

Mybatis提供了整合ehcache缓存，具体整合方法参考官网文档，

# MyBatis的逆向工程

## 概述

MyBatis Generator：简称MBG，是一个专门为MyBatis框架使用者定制的代码生成器，可以快速的根据表生成对应的映射文件，接口，以及bean类。支持基本的增删改查，以及QBC风格的条件查询。但是表连接、存储过程等这些复杂sql的定义需要我们手工编写

**官方文档地址：**http://www.mybatis.org/generator/

**官方工程地址：**https://github.com/mybatis/generator/releases

## MBG使用步骤

* 编写MBG的配置文件（重要几处配置）
  * jdbcConnection配置数据库连接信息
  * javaModelGenerator配置javaBean的生成策略
  * sqlMapGenerator配置sql映射文件生成策略
  * javaClientGenerator配置Mapper接口的生成策略
  * table配置要逆向解析的数据表
    * tableName：表名
    * domainObjectName：对应的javaBean名
* 运行代码生成器生成代码

**注意：**

Context标签
targetRuntime=“MyBatis3“可以生成带条件的增删改查
targetRuntime=“MyBatis3Simple“可以生成基本的增删改查
如果再次生成，建议将之前生成的数据删除，避免xml向后追加内容出现的问题。

## MBG配置文件

```xml
<generatorConfiguration>
    <context id="DB2Tables" targetRuntime="MyBatis3">
    //数据库连接信息配置
    <jdbcConnection driverClass="com.mysql.jdbc.Driver"
    connectionURL="jdbc:mysql://localhost:3306/bookstore0629"
    userId="root" password="123456">
    </jdbcConnection>
    //javaBean的生成策略
    <javaModelGenerator targetPackage="com.atguigu.bean" targetProject=".\src">
    <property name="enableSubPackages" value="true" />
    <property name="trimStrings" value="true" />
    </javaModelGenerator>
    //映射文件的生成策略
    <sqlMapGenerator targetPackage="mybatis.mapper" targetProject=".\conf">
    <property name="enableSubPackages" value="true" />
    </sqlMapGenerator>
    //dao接口java文件的生成策略
    <javaClientGenerator type="XMLMAPPER" targetPackage="com.atguigu.dao"
    targetProject=".\src">
    <property name="enableSubPackages" value="true" />
    </javaClientGenerator>
    //数据表与javaBean的映射
    <table tableName="books" domainObjectName="Book"></table>
    </context>
</generatorConfiguration>
```

## 生成器代码

```java
public static void main(String[] args) throws Exception {
    List<String> warnings = new ArrayList<String>();
    boolean overwrite = true;
    File configFile = new File("mbg.xml");
    ConfigurationParser cp = new ConfigurationParser(warnings);
    Configuration config = cp.parseConfiguration(configFile);
    DefaultShellCallback callback = new efaultShellCallback(overwrite);
    MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
    callback, warnings);
    myBatisGenerator.generate(null);
}
```

## QBC风格的带条件查询

```java
@Test
public void test01(){
    SqlSession openSession = build.openSession();
    DeptMapper mapper = openSession.getMapper(DeptMapper.class);
    DeptExample example = new DeptExample();
    //所有的条件都在example中封装
    Criteria criteria = example.createCriteria();
    //select id, deptName, locAdd from tbl_dept WHERE
    //( deptName like ? and id > ? )
    criteria.andDeptnameLike("%部%");
    criteria.andIdGreaterThan(2);
    List<Dept> list = mapper.selectByExample(example);
    for(Dept dept : list) {
    System.out.println(dept);
}
```

# Mybatis的插件开发

## PageHelper分页插件

**项目地址：**https://github.com/pagehelper/Mybatis-PageHelper

**文档地址：**https://github.com/pagehelper/Mybatis-PageHelper/blob/master/README_zh.md

## 使用步骤

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>最新版本</version>
</dependency>
```

**在 MyBatis 配置 xml 中配置拦截器插件**

```xml
<plugins>
    <!-- com.github.pagehelper为PageHelper类所在包名 -->
    <plugin interceptor="com.github.pagehelper.PageInterceptor">
        <!-- 使用下面的方式配置参数，后面会有所有的参数介绍 -->
        <property name="param1" value="value1"/>
	</plugin>
</plugins>
```

## 代码中使用方法

```java
//第二种，Mapper接口方式的调用，推荐这种使用方式。
PageHelper.startPage(1, 10);
List<Country> list = countryMapper.selectIf(1);

//第三种，Mapper接口方式的调用，推荐这种使用方式。
PageHelper.offsetPage(1, 10);
List<Country> list = countryMapper.selectIf(1);
//第六种，ISelect 接口方式
//jdk6,7用法，创建接口
Page<Country> page = PageHelper.startPage(1, 10).doSelectPage(new ISelect() {
    @Override
    public void doSelect() {
        countryMapper.selectGroupBy();
    }
});
//jdk8 lambda用法
Page<Country> page = PageHelper.startPage(1, 10).doSelectPage(()-> countryMapper.selectGroupBy());

//也可以直接返回PageInfo，注意doSelectPageInfo方法和doSelectPage
pageInfo = PageHelper.startPage(1, 10).doSelectPageInfo(new ISelect() {
    @Override
    public void doSelect() {
        countryMapper.selectGroupBy();
    }
});
//对应的lambda用法
pageInfo = PageHelper.startPage(1, 10).doSelectPageInfo(() -> countryMapper.selectGroupBy());

//count查询，返回一个查询语句的count数
long total = PageHelper.count(new ISelect() {
    @Override
    public void doSelect() {
        countryMapper.selectLike(country);
    }
});
//lambda
total = PageHelper.count(()->countryMapper.selectLike(country));
```

## 常用方法介绍

**RowBounds方式的调用**

```java
List<Country> list = sqlSession.selectList("x.y.selectIf", null, new RowBounds(1, 10));
```

使用这种调用方式时，你可以使用RowBounds参数进行分页，这种方式侵入性最小，我们可以看到，通过RowBounds方式调用只是使用了这个参数，并没有增加其他任何内容。分页插件检测到使用了RowBounds参数时，就会对该查询进行**物理分页**

**PageHelper.startPage静态方法调用**

在你需要进行分页的 MyBatis 查询方法前调用 `PageHelper.startPage` 静态方法即可，紧跟在这个方法后的**第一个MyBatis 查询方法**会被进行分页。 

**PageInfo用法**

```java
//获取第1页，10条内容，默认查询总数count
PageHelper.startPage(1, 10);
List<Country> list = countryMapper.selectAll();
//用PageInfo对结果进行包装
PageInfo page = new PageInfo(list);
//测试PageInfo全部属性
//PageInfo包含了非常全面的分页属性
assertEquals(1, page.getPageNum());
assertEquals(10, page.getPageSize());
assertEquals(1, page.getStartRow());
assertEquals(10, page.getEndRow());
assertEquals(183, page.getTotal());
assertEquals(19, page.getPages());
assertEquals(1, page.getFirstPage());
assertEquals(8, page.getLastPage());
assertEquals(true, page.isFirstPage());
assertEquals(false, page.isLastPage());
assertEquals(false, page.isHasPreviousPage());
assertEquals(true, page.isHasNextPage());
```

## Mybatis批量保存

在MyBatis的全局设置中有设置，defaultExecutorType  配置默认的执行器

- SIMPLE 就是普通的执行器；
- REUSE 执行器会重用预处理语句（prepared statements）； 
- BATCH 执行器将重用语句并执行批量更新

但是，如果在全局设置中设置批量执行器，那么每一个mapper中的方法都会执行批量操作，所以我们一般都是在与Spring整合后在Application.xml中配置一个可以执行批量操作的sqlSession,如下

```xml
<!--创建出SqlSessionFactory对象  -->
<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
    <!-- configLocation指定全局配置文件的位置 -->
    <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    <!--mapperLocations: 指定mapper文件的位置-->
    <property name="mapperLocations" value="classpath:mybatis/mapper/*.xml"></property>
</bean>
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactoryBean"></constructor-arg>
    <constructor-arg name="executorType" value="BATCH"></constructor-arg>
</bean>
```

在Service中自动注入SQLSession

```java
@Autowired
private SqlSession sqlSession;
public List<Employee> getEmps(){
    EmployeeMapper mapper = sqlSession.getMapper(EmployeeMapper.class);
    return mapper.getEmps();
}
```

