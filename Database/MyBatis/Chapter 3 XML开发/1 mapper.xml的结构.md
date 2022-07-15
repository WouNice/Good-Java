# mapper.xml的结构

在mapper.xml中，`<configuration>`元素是配置文件的根元素，其他元素都要在`<contiguration>`元素内配置。

参考：https://mybatis.net.cn/configuration.html

## `<properties>`元素

<properties>是一个配置属性的元素，通过外部配置来动态替换内部定义的属性。

在`<properties>`中配置数据库的连接等属性，具体方式如下：

在项目的resources目录下创建一个名称为mybatis.properties的配置文件，代码如下所示。

```
mysql.driver=com.mysql.cj.jdbc.Driver
mysql.url=jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8
mysql.username=root
mysql.password=root
```

在MyBatis配置文件mybatis-config.xml中配置<properties/>属性，具体如下。

```
<properties resource="mybatis.properties"/>
```

修改配置文件中数据库连接的信息，具体如下。

```
<dataSource type="POOLED">
    <property name="driver" value="${mysql.driver}"/>
    <property name="url" value="${mysql.url}"/>
    <property name="username" value="${mysql.username}"/>
    <property name="password" value="${mysql.password}"/>
</dataSource>
```

完成上述配置，dataSource中连接数据库的4个属性（driver、url、username和password）值将会由db.properties文件中对应的值来动态替换。这样就为配置提供了灵活性。

另外，还可以通过配置<properties>元素的子元素<property>以及通过方法参数传递的方式来获取属性值。

由于使用properties配置文件来配置属性值可以方便地在多个配置文件中使用这些属性值，并且方便维护和修改，因此在实际开发中常用。

## `<settings>`元素

<settings>元素主要用于改变MyBatis运行时的行为，例如开启二级缓存、开启延迟加载等。

<settings>元素中的常见配置及其描述参考https://mybatis.net.cn/configuration.html#settings。这些配置在配置文件中的使用方式如下。

```
<settings>
    <setting name="cacheEnabled" value="true"/>
    <setting name="lazyLoadingEnabled" value="true"/>
    .......
</settings>
```

## `<typeAliases>`元素

<typeAliases>元素用于为配置文件中的Java类型设置一个简短的名字，即设置别名。别名的设置与XML配置相关，其使用的意义在于减少全限定类名的冗余。

使用<typeAliases>元素配置别名的方法如下：

```
<typeAliases>
	<typeAlias alias="User" type="org.example.User"/>
</typeAliases>
```

在上述示例中，<typeAliases>元素的子元素<typeAlias>中的type属性用于指定需要被定义别名的类的全限定名；alias属性的属性值“User”就是自定义的别名，可以代替"org.example.User"使用在MyBatis文件的任何位置。如果省略alias属性，MyBatis会默认将类名首字母小写后的名称作为别名。

当POJO类过多时，还可以通过自动扫描包的形式自定义别名，具体示例如下。

```
<typeAliases>
	<typeAlias type="org.example"/>
</typeAliases>
```

在上述示例中，<typeAliases>元素的子元素<package>中的name属性用于指定要被定义别名的包，MyBatis会将所有org.example包中的POJO类以首字母小写的非限定类名作为它的别名。

注意上述方式的别名只适用于没有使用注解的情况。如果在程序中使用了注解，那么别名为其注解的值，具体如下。

```
@Alias(value = "user")
public class User{

}
```

除了可以使用<typeAliases>元素自定义别名外，MyBatis框架还默认为许多常见的Java类型（如数值、字符串、日期和集合等）提供相应的类型别名，如表所示。

| 别名       | 映射的类型 |
| :--------- | :--------- |
| _byte      | byte       |
| _long      | long       |
| _short     | short      |
| _int       | int        |
| _integer   | int        |
| _double    | double     |
| _float     | float      |
| _boolean   | boolean    |
| string     | String     |
| byte       | Byte       |
| long       | Long       |
| short      | Short      |
| int        | Integer    |
| integer    | Integer    |
| double     | Double     |
| float      | Float      |
| boolean    | Boolean    |
| date       | Date       |
| decimal    | BigDecimal |
| bigdecimal | BigDecimal |
| object     | Object     |
| map        | Map        |
| hashmap    | HashMap    |
| list       | List       |
| arraylist  | ArrayList  |
| collection | Collection |
| iterator   | Iterator   |

>   提示
>
>   上表所列举的别名可以在MyBatis中直接使用，但由于别名不区分大小写，因此在使用时要注意重复定义的覆盖问题。

## `<typeHandler>`元素

MyBatis在预处理语句（Prepared Statement）中设置一个参数或者从结果集（Resultset）中取出一个值时，都会用其框架内部注册了的typeHandler（类型处理器）进行相关处理。typeHandler的作用就是将预处理语句中传入的参数从javaType（Java类型）转换为jdbcType（JDBC类型），或者从数据库取出结果时将jdbcType转换为javaType。

为了方便转换，MyBatis框架提供了一些默认的类型处理器，如表所示。

| 类型处理器                   | Java 类型                       | JDBC 类型                                                    |
| :--------------------------- | :------------------------------ | :----------------------------------------------------------- |
| `BooleanTypeHandler`         | `java.lang.Boolean`, `boolean`  | 数据库兼容的 `BOOLEAN`                                       |
| `ByteTypeHandler`            | `java.lang.Byte`, `byte`        | 数据库兼容的 `NUMERIC` 或 `BYTE`                             |
| `ShortTypeHandler`           | `java.lang.Short`, `short`      | 数据库兼容的 `NUMERIC` 或 `SMALLINT`                         |
| `IntegerTypeHandler`         | `java.lang.Integer`, `int`      | 数据库兼容的 `NUMERIC` 或 `INTEGER`                          |
| `LongTypeHandler`            | `java.lang.Long`, `long`        | 数据库兼容的 `NUMERIC` 或 `BIGINT`                           |
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
| `EnumTypeHandler`            | Enumeration Type                | VARCHAR 或任何兼容的字符串类型，用来存储枚举的名称（而不是索引序数值） |
| `EnumOrdinalTypeHandler`     | Enumeration Type                | 任何兼容的 `NUMERIC` 或 `DOUBLE` 类型，用来存储枚举的序数值（而不是名称）。 |
| `SqlxmlTypeHandler`          | `java.lang.String`              | `SQLXML`                                                     |
| `InstantTypeHandler`         | `java.time.Instant`             | `TIMESTAMP`                                                  |
| `LocalDateTimeTypeHandler`   | `java.time.LocalDateTime`       | `TIMESTAMP`                                                  |
| `LocalDateTypeHandler`       | `java.time.LocalDate`           | `DATE`                                                       |
| `LocalTimeTypeHandler`       | `java.time.LocalTime`           | `TIME`                                                       |
| `OffsetDateTimeTypeHandler`  | `java.time.OffsetDateTime`      | `TIMESTAMP`                                                  |
| `OffsetTimeTypeHandler`      | `java.time.OffsetTime`          | `TIME`                                                       |
| `ZonedDateTimeTypeHandler`   | `java.time.ZonedDateTime`       | `TIMESTAMP`                                                  |
| `YearTypeHandler`            | `java.time.Year`                | `INTEGER`                                                    |
| `MonthTypeHandler`           | `java.time.Month`               | `INTEGER`                                                    |
| `YearMonthTypeHandler`       | `java.time.YearMonth`           | `VARCHAR` 或 `LONGVARCHAR`                                   |
| `JapaneseDateTypeHandler`    | `java.time.chrono.JapaneseDate` | `DATE`                                                       |

当MyBatis框架所提供的这些类型处理器不能够满足需求时，还可以通过自定义的方式对类型处理器进行扩展。自定义类型处理器可以通过实现TypeHandler接口或者继承BaseTypeHandle类来定义。<typeHandler>元素就是用于在配置文件中注册自定义的类型处理器的。

它的使用方式有两种，具体如下。

（1）注册一个类的类型处理器

```
<typeHandlers>
<!--  以单个类的形式配置  -->
    <typeHandler handler="org.example.Handler"/>
</typeHandlers>
```

上述代码中，子元素<typeHandler>的handler属性用于指定在程序中自定义的类型处理器类。

（2）注册一个包中所有的类型处理器

```
<typeHandlers>
<!--  以单个类的形式配置  -->
    <typeHandler handler="org.example"/>
</typeHandlers>
```

在上述代码中，子元素<package>的name属性用于指定类型处理器所在的包名，使用这种方式后，系统会在启动时自动扫描org.example包下所有的文件，并把它们作为类型处理器。

## `<objectFactory>`元素

MyBatis框架每次创建结果对象的新实例时，都会使用一个对象工厂（ObjectFactory）的实例来完成。MyBatis中默认的ObjectFactory的作用就是实例化目标类，既可以通过默认构造方法实例化，也可以在参数映射存在的时候通过参数构造方法来实例化。

在通常情况下，我们使用默认的ObjectFactory即可。MyBatis中默认的ObjectFactory是由org.apache.ibatis.reflection.factory.DefaultObjectFactory来提供服务的，大部分场景下都不用配置和修改。如果想覆盖ObjectFactory的默认行为，那么可以通过自定义ObjectFactory实现。

## `<plugins>`元素

MyBatis允许在已映射语句执行过程中的某一点进行拦截调用（通过插件来实现）。<plugins>元素的作用就是配置用户所开发的插件。

## `<environments>`元素

<environments>元素用于在配置文件中对环境进行配置。MyBatis的环境配置实际上就是数据源的配置，可以通过<environments>元素配置多种数据源，即配置多种数据库。

使用<environments>元素进行环境配置的示例如下。

```
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!--    数据库连接相关配置，db,properties文件中的内容-->
            <dataSource type="POOLED">
                <property name="driver" value="${mysql.driver}"/>
                <property name="url" value="${mysql.url}"/>
                <property name="username" value="${mysql.username}"/>
                <property name="password" value="${mysql.password}"/>
            </dataSource>
        </environment>
    </environments>
```

在上述示例代码中，<environments>元素是环境配置的根元素，它包含一个default属性，该属性用于指定默认的环境ID。<environment>是<environments>元素的子元素，它可以被定义多个，其id属性用于表示所定义环境的ID值。在<environment>元素内，包含事务管理和数据源的配置信息，其中<transactionManager>元素用于配置事务管理，它的type属性用于指定事务管理的方式，即使用哪种事务管理器；<dataSource>元素用于配置数据源，它的type属性用于指定使用哪种数据源。

在MyBatis中，可以配置两种类型的事务管理器，分别是JDBC和MANAGED。

-   JDBC：此配置直接使用JDBC的提交和回滚设置，依赖从数据源得到的连接来管理事务的作用域。
-   MANAGED：此配置从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期。在默认情况下，它会关闭连接，但一些容器并不希望这样，为此可以将closeConnection属性设置为false来阻止它默认的关闭行为。

>   注意
>
>   如果项目中使用Spring+MyBatis，就没有必要在MyBatis中配置事务管理器，因为实际开发中会使用Spring自带的管理器来实现事务管理。

对于数据源的配置，MyBatis框架提供了UNPOOLED、POOLED和JNDI三种数据源类型。

-   UNPOOLED：配置此数据源类型后，在每次被请求时会打开和关闭连接。它对没有性能要求的简单应用程序是一个很好的选择。
-   POOLED：此数据源利用“池”的概念将JDBC连接对象组织起来，避免在创建新的连接实例时需要初始化和认证的时间。这种方式使得并发Web应用可以快速地响应请求，是当前流行的处理方式。
-   JNDI：此数据源可以在EJB或应用服务器等容器中使用。容器可以集中或在外部配置数据源，然后放置一个JNDI上下文的引用。

## `<mappers>`元素

在配置文件中，<mappers>元素用于指定MyBatis映射文件的位置，一般可以使用以下4种方法引入映射器文件，具体如下所示。

（1）使用类路径引入

```
<mappers>
    <mapper resource="UserMapper.xml"/>
</mappers>
```

（2）使用本地文件路径引入

```
<mappers>
	<mapper url=file: ///<UserMapper.xml路径>"/>
</mappers>
```

（3）使用接口类引入

```
<mappers>
	<mapper class="org.example.UserMapper"/>
</mappers>
```

（4）使用包名引入

```
<mappers>
	<mapper class="org.example"/>
</mappers>
```
