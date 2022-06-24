# Bean配置文件

Spring 配置文件支持两种格式，即 XML 文件格式和 Properties 文件格式。

- Properties 配置文件主要以 key-value 键值对的形式存在，只能赋值，不能进行其他操作，适用于简单的属性配置。
- XML 配置文件采用树形结构，结构清晰，相较于 Properties 文件更加灵活。但是 XML 配置比较繁琐，适用于大型的复杂的项目。

## XML文件格式

通常情况下，Spring 的配置文件都是使用 XML 格式的。XML 配置文件的根元素是 <beans>，该元素包含了多个子元素 <bean>。每一个 <bean> 元素都定义了一个 Bean，并描述了该 Bean 是如何被装配到 Spring 容器中的。

在 XML 配置的<beans> 元素中可以包含多个属性或子元素，常用的属性或子元素如下表所示。

| 属性名称        | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| id              | Bean 的唯一标识符，Spring IoC 容器对 Bean 的配置和管理都通过该属性完成。id 的值必须以字母开始，可以使用字母、数字、下划线等符号。 |
| name            | 该属性表示 Bean 的名称，我们可以通过 name 属性为同一个 Bean 同时指定多个名称，每个名称之间用逗号或分号隔开。Spring 容器可以通过 name 属性配置和管理容器中的 Bean。 |
| class           | 该属性指定了 Bean 的具体实现类，它必须是一个完整的类名，即类的全限定名。 |
| scope           | 表示 Bean 的作用域，属性值可以为 singleton（单例）、prototype（原型）、request、session 和 global Session。默认值是 singleton。 |
| constructor-arg | <bean> 元素的子元素，我们可以通过该元素，将构造参数传入，以实现 Bean 的实例化。该元素的 index 属性指定构造参数的序号（从 0 开始），type 属性指定构造参数的类型。 |
| property        | <bean>元素的子元素，用于调用 Bean 实例中的 setter 方法对属性进行赋值，从而完成属性的注入。该元素的 name 属性用于指定 Bean 实例中相应的属性名。 |
| ref             | <property> 和 <constructor-arg> 等元素的子元索，用于指定对某个 Bean 实例的引用，即 <bean> 元素中的 id 或 name 属性。 |
| value           | <property> 和 <constractor-arg> 等元素的子元素，用于直接指定一个常量值。 |
| list            | 用于封装 List 或数组类型的属性注入。                         |
| set             | 用于封装 Set 类型的属性注入。                                |
| map             | 用于封装 Map 类型的属性注入。                                |
| entry           | <map> 元素的子元素，用于设置一个键值对。其 key 属性指定字符串类型的键值，ref 或 value 子元素指定其值。 |
| init-method     | 容器加载 Bean 时调用该方法，类似于 Servlet 中的 init() 方法  |
| destroy-method  | 容器删除 Bean 时调用该方法，类似于 Servlet 中的 destroy() 方法。该方法只在 scope=singleton 时有效 |
| lazy-init       | 懒加载，值为 true，容器在首次请求时才会创建 Bean 实例；值为 false，容器在启动时创建 Bean 实例。该方法只在 scope=singleton 时有效 |

## 读取 properties 文件

为什么要使用外部的 properties 文件？

当bean的配置信息逐渐增多时，查找和修改一些bean的配置信息就变得愈加困难。这时可以将一部分信息提取到bean配置文件的外部，以properties格式的属性文件保存起来，同时在bean的配置文件中引用properties属性文件中的内容，从而实现一部分属性值在发生变化时仅修改properties属性文件即可。

### 引入数据库依赖

引入 `druid` 和 `mysql` 的驱动：

```
<!-- druid -->
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.2.10</version>
</dependency>

<!-- mysql -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.29</version>
</dependency>
```

### 创建外部properties配置文件

在类路径下创建 jdbc.properties 数据库配置文件

```
prop.driverClass=com.mysql.cj.jdbc.Driver
prop.url=jdbc:mysql:///test
prop.userName=root
prop.password=root
```

### 引用外部properties配置文件

创建spring-config.xml配置文件，引入 `context` 名称空间：将 xmlns="http://www.springframework.org/schema/beans" 和 http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd 复制，并将出现 beans 的位置全部替换为 context

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context
                http://www.springframework.org/schema/context/spring-context.xsd">
```

通过 `<context:property-placeholder>` 标签中的 `location` 来制定配置文件的路径，`classpath:` 表示该配置文件位于类路径下，并通过 `${prop.userName}` 的方式来取出配置文件中的属性值

```
<!-- 引用外部属性文件来配置数据库连接池 -->
<!-- 指定 properties 属性文件的位置，classpath:xxx 表示属性文件位于类路径下 -->
<context:property-placeholder location="classpath:jdbc.properties"/>
<!-- 从properties属性文件中引入属性值 -->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="username" value="${prop.userName}"/>
    <property name="password" value="${prop.password}"/>
    <property name="url" value="${prop.url}"/>
    <property name="driverClassName" value="${prop.driverClass}"/>
</bean>
```

## 配置文件整合

Spring可以很方便地整合其他配置文件。

方法一：

```
ApplicationContext ioc = new ClassPathXmlApplicationContext("Beans-*");
```

它表示配置全部以“`Beans-`”开头的xml配置文件。

方法二：

```
ApplicationContext ioc = new ClassPathXmlApplicationContext("文件一.xml","文件二.xml");
```

经过此方法可配置多个文件。

方法三：

Spring允许通过`<import>`将多个配置文件引入到一个文件中，进行配置文件的集成。这样在启动Spring容器时，仅需要指定这个合并好的配置文件就可以。

直接配置一个主配置文件，而后用import 把其余文件引入进来web

```
<import resource="文件1.xml"/>
<import resource="文件2.xml"/>
```

import 元素的 resource 属性支持 Spring 的标准的路径资源服务器

| 地址前缀     | 示例                          | 说明                   |
| ------------ | ----------------------------- | ---------------------- |
| `Classpath:` | `Classpath:beans.xml`         | 加载类路径下的资源资源 |
| `File:`      | `File:com/config/xxx.xml`     | 加载文件系统中的资源   |
| `http://`    | `http://www.xxxx.com/xxx.xml` | 加载web服务器中的文件  |
| `ftp://`     | `ftp://www.xxxx.com/xxx.xml`  | 加载ftp服务器中的资源  |
