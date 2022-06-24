# 注解方式配置Bean

注解方式相对于XML方式而言，通过注解的方式配置bean更加简洁和优雅，而且和MVC组件化开发的理念十分契合，是开发中常用的使用方式。

## 什么是基于注解的容器配置

不使用 XML 来描述 bean 装配，开发人员通过在相关的类，方法或字段声明上使用注解将配置移动到组件类本身。它可以作为 XML 设置的替代方案。

例如：Spring 的 Java 配置是通过使用 @Bean 和 @Conﬁguration 来实现。

-    @Bean 注解扮演与元素相同的角色。
-   @Conﬁguration 类允许通过简单地调用同一个类中的其他 @Bean 方法来定义 bean 间依赖关系。

示例

```
@Configuration
public class StudentConfig {
	@Bean
	public StudentBean myStudent() { 
		return new StudentBean();
	}
}
```

## 标识组件

用于标识 bean 的四个注解

-   普通组件：@Component，用于标识一个受Spring IOC容器管理的组件
-   持久化层组件：@Respository，用于标识一个受Spring IOC容器管理的持久化层组件
-   业务逻辑层组件：@Service，用于标识一个受Spring IOC容器管理的业务逻辑层组件
-   表述层控制器组件：@Controller，用于标识一个受Spring IOC容器管理的表述层控制器组件

事实上Spring并没有能力识别一个组件到底是不是它所标记的类型，即使将@Respository注解用在一个表述层控制器组件上面也不会产生任何错误，所以@Respository、@Service、@Controller这几个注解仅仅是为了让开发人员自己明确当前的组件扮演的角色。

组件命名规则：

-   默认情况：使用组件的简单类名首字母小写后得到的字符串作为bean的`id`
-   使用组件注解的`value`属性指定bean的`id`

## 扫描组件

### 引入 AOP 依赖

引入 AOP 相关依赖，不然开启组件扫描时会报错：`org.springframework.beans.factory.BeanDefinitionStoreException: Unexpected exception parsing XML document from class path resource [annotation-config.xml]; nested exception is java.lang.NoClassDefFoundError: org/springframework/aop/TargetSource`

```
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aop</artifactId>
  <version>5.3.20</version>
</dependency>
```

### 引入 context 名称空间

将 xmlns="http://www.springframework.org/schema/beans" 和 http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd 复制，并将出现 beans 的位置全部替换为 context

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

### 开启组件扫描

开启组件扫描，并指明要扫描的包路径

```
<context:component-scan base-package="org.example"/>
```

-   base-package属性指定一个需要扫描的基类包，Spring容器将会扫描这个基类包及其子包中的所有类。

-   当需要扫描多个包时可以使用逗号分隔。

-   如果仅希望扫描特定的类而非基包下的所有类，可使用resource-pattern属性过滤特定的类，示例：

    ```
    <context:component-scan base-package="org.example" resource-pattern="autowire/*.class"/>
    ```

### 包含与排除

-   `<context:include-filter>`子节点表示要包含的目标类。注意：通常需要与use-default-filters属性配合使用才能够达到“仅包含某些组件”这样的效果。即：通过将use-default-filters属性设置为false，禁用默认过滤器，然后扫描的就只是include-filter中的规则指定的组件了。
-   `<context:exclude-filter>`子节点表示要排除在外的目标类
-   `component-scan`下可以拥有若干个`include-filter和exclude-filter`子节点

### 过滤表达式

| 类别       | 示例                  | 说明                                                         |
| ---------- | --------------------- | ------------------------------------------------------------ |
| annotation | com.xgj.XxxAnnotation | 所有标注了XxxAnnotation的类。该类型采用目标类是否标志了某个注解进行过滤 |
| assignable | com.xgj.XxService     | 所有继承或扩展XXXService的类。该类型采用目标类是否继承或者扩展了某个特定类进行过滤 |
| aspectj    | com.xgj..*Service+    | 所有类名以Service结束的类及继承或者扩展他们的类。该类型采用AspectJ表达式进行过滤 |
| regex      | com.xgj.auto..*       | 所有com.xgj.auto类包下的类。该类型采用正则表达式根据目标类的类名进行过滤 |
| custom     | com.xgj.XxxTypeFilter | 采用XxxTypeFilter代码方式实现过滤规则，该类必须实现org.springframework.core.type.TypeFilter接口 |

在所有的过滤类型中，除了custom类型外，aspectj的过滤表达能力是最强的，可以轻易实现其他类型所能表达的过滤规则。

### 包扫描举例

```
<!--示例1：use-default-filters="false" 表示现在不使用默认filter，自己配置filter 
context:include-filter用于设置扫描哪些内容（这里配置只扫描 Controller 注解） -->
<context:component-scan base-package="org.example" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>

<!--示例2：下面配置扫描包所有内容 context:exclude-filter： 设置哪些内容不进行扫描（这里排除 Controller 注解） -->
<context:component-scan base-package="org.example">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

## 组件装配

在项目中，组件装配的需求：Controller组件中往往需要用到Service组件的实例，Service组件中往往需要用到Repository组件的实例。Spring可以通过注解的方式帮我们实现属性的装配。

组件扫描的原理：在指定要扫描的包时，`<context:component-scan>` 元素会自动注册一个bean的后置处理器：AutowiredAnnotationBeanPostProcessor的实例。该后置处理器可以自动装配标记了@Autowired、@Resource或@Inject注解的属性

[示例]

DAO 层推荐使用 `@Repository` 注解标识 bean

```
@Repository
public class OrderDao
{

    public void sell()
    {
        System.out.println("DAO 层操作：商品库存减一");
    }

}
```

Service 层推荐使用 `@Service` 注解标识 bean，并通过 `@Autowired` 注解标识

```
@Service
public class OrderService
{

    @Autowired
    private OrderDao orderDao;

    public void sell()
    {
        orderDao.sell();
        System.out.println("Service 层操作：出售一件商品");
    }

}
```

测试代码

```
public class SpringTest
{
    @Test
    public void test()
    {
        //1.创建IOC容器对象
        ApplicationContext iocContainer = new ClassPathXmlApplicationContext("annotation-config.xml");

        //2.根据id值获取bean实例对象
        OrderService orderService = (OrderService) iocContainer.getBean("orderService");

        //3.调用bean中的方法
        orderService.sell();
    }
}
```

测试结果：

```
DAO 层操作：商品库存减一
Service 层操作：出售一件商品
```

## 完全注解开发

创建 `SpringConfig` 配置类，代替之前的 XML 配置文件

1.  `@Configuration` 标识这是一个配置类
2.  `@ComponentScan(basePackages = "org.example")` 配置组件扫描
3.  `@Bean` 用于向 IOC 容器中注入一个普通的 bean

```
@Configuration
@ComponentScan(basePackages = "org.example")
public class SpringConfig
{

    @Bean
    public OrderService getOrderService()
    {
        return new OrderService();
    }

}
```

测试代码：这次需要 `new` 一个 `AnnotationConfigApplicationContext` 对象，并传入配置类的类型

```
@Test
public void testCompleteAnnotation()
{
    //1.创建IOC容器对象
    ApplicationContext iocContainer = new AnnotationConfigApplicationContext(SpringConfig.class);

    //2.根据id值获取bean实例对象
    OrderService orderService = (OrderService) iocContainer.getBean("orderService");

    //3.调用bean中的方法
    orderService.sell();
}
```

