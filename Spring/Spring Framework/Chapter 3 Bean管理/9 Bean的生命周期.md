# Bean的生命周期

参考：[大厂高频面试题Spring Bean生命周期最详解](https://copyfuture.com/blogs-details/20211209093606692T)

在传统的 Java 应用中，Bean 的生命周期很简单，使用 Java 关键字 new 进行 Bean 的实例化后，这个 Bean 就可以使用了。一旦这个 Bean 长期不被使用，Java 自动进行垃圾回收。

相比之下，Spring 中 Bean 的生命周期较复杂，大致可以分为以下 5 个阶段：

1. Bean 的实例化
2. Bean 属性赋值
3. Bean 的初始化
4. Bean 的使用
5. Bean 的销毁

Spring 根据 Bean 的作用域来选择 Bean 的管理方式，

- 对于 singleton 作用域的 Bean 来说，Spring IoC 容器能够精确地控制 Bean 何时被创建、何时初始化完成以及何时被销毁；
- 对于 prototype 作用域的 Bean 来说，Spring IoC 容器只负责创建，然后就将 Bean 的实例交给用户管理，Spring IoC 容器将不再跟踪其生命周期。

## Spring 的生命流程

Spring Bean 的完整生命周期从创建 Spring IoC 容器开始，直到最终 Spring IoC 容器销毁 Bean 为止。

ApplicationContext容器中，Bean的生命周期流程如下图所示：

![](assets/Bean 生命周期.svg)

Bean 生命周期的整个执行过程描述如下。

1. Spring 启动，查找并加载需要被 Spring 管理的 Bean，对 Bean 进行实例化。
2. 对 Bean 进行属性注入。
3. 如果 Bean 实现了 BeanNameAware 接口，则 Spring 调用 Bean 的 setBeanName() 方法传入当前 Bean 的 id 值。
4. 如果 Bean 实现了 BeanFactoryAware 接口，则 Spring 调用 setBeanFactory() 方法传入当前工厂实例的引用。
5. 如果 Bean 实现了 ApplicationContextAware 接口，则 Spring 调用 setApplicationContext() 方法传入当前 ApplicationContext 实例的引用。
6. 如果 Bean 实现了 BeanPostProcessor 接口，则 Spring 调用该接口的预初始化方法 postProcessBeforeInitialzation() 对 Bean 进行加工操作，此处非常重要，Spring 的 AOP 就是利用它实现的。
7. 如果 Bean 实现了 InitializingBean 接口，则 Spring 将调用 afterPropertiesSet() 方法。
8. 如果在配置文件中通过 init-method 属性指定了初始化方法，则调用该初始化方法。
9. 如果 BeanPostProcessor 和 Bean 关联，则 Spring 将调用该接口的初始化方法 postProcessAfterInitialization()。此时，Bean 已经可以被应用系统使用了。
10. 如果在 <bean> 中指定了该 Bean 的作用域为 singleton，则将该 Bean 放入 Spring IoC 的缓存池中，触发 Spring 对该 Bean 的生命周期管理；如果在 <bean> 中指定了该 Bean 的作用域为 prototype，则将该 Bean 交给调用者，调用者管理该 Bean 的生命周期，Spring 不再管理该 Bean。
11. 如果 Bean 实现了 DisposableBean 接口，则 Spring 会调用 destory() 方法销毁 Bean；如果在配置文件中通过 destory-method 属性指定了 Bean 的销毁方法，则 Spring 将调用该方法对 Bean 进行销毁。

## 管理Bean的生命周期

我们可以在 Spring Bean 生命周期的某个特定时刻，指定一些生命周期回调方法完成一些自定义的操作，对 Bean 的生命周期进行管理。

Bean 的生命周期回调方法主要有两种：

- 初始化回调方法：在 Spring Bean 被初始化后调用，执行一些自定义的回调操作。
- 销毁回调方法：在 Spring Bean 被销毁前调用，执行一些自定义的回调操作。

我们可以通过以下 3 种方式自定义 Bean 的生命周期回调方法：

- 通过XML 配置实现
- 通过接口实现
- 使用注解实现

如果一个 Bean 中有多种生命周期回调方法时，优先级顺序为：注解 > 接口 > XML 配置。

### 通过 XML 配置实现

我们可以在 Spring 的 XML 配置中，通过 <bean> 元素中的 init-method 和 destory-method 属性，指定 Bean 的生命周期回调方法。

| XML 配置属性   | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| init-method    | 指定初始化回调方法，这个方法会在 Spring Bean 被初始化后被调用，执行一些自定义的回调操作。 |
| destory-method | 指定销毁回调方法，这个方法会在 Spring Bean 被销毁前被调用，执行一些自定义的回调操作。 |

### 通过接口实现

我们可以在 Spring Bean 的 Java 类中，通过实现 InitializingBean 和 DisposableBean 接口，指定 Bean 的生命周期回调方法。

| 回调方式   | 接口             | 方法                 | 说明                                                         |
| ---------- | ---------------- | -------------------- | ------------------------------------------------------------ |
| 初始化回调 | InitializingBean | afterPropertiesSet() | 指定初始化回调方法，这个方法会在 Spring Bean 被初始化后被调用，执行一些自定义的回调操作。 |
| 销毁回调   | DisposableBean   | destroy()            | 指定销毁回调方法，这个方法会在 Spring Bean 被销毁前被调用，执行一些自定义的回调操作。 |

> 注意：通常情况下，我们不建议通过这种方式指定生命周期回调方法，这会导致代码的耦合性过高。

### 使用注解实现

我们还可以通过 JSR-250 的 @PostConstruct 和 @PreDestroy 注解，指定 Bean 的生命周期回调方法。

| 注解           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| @PostConstruct | 指定初始化回调方法，这个方法会在 Spring Bean 被初始化后被调用，执行一些自定义的回调操作。 |
| @PreDestroy    | 指定销毁回调方法，这个方法会在 Spring Bean 被销毁前被调用，执行一些自定义的回调操作。 |

### 示例

创建 `Order` 类，用于演示 bean 的生命周期

```
public class Order
{

    private String name;

    public Order()
    {
        System.out.println("执行无参数构造创建 bean 实例");
    }

    public void setName(String name)
    {
        this.name = name;
        System.out.println("调用 setter 方法为属性赋值");
    }

    //  init-method 初始化方法
    public void initMethod()
    {
        System.out.println("执行 init-method 初始化方法");
    }

    //  destroy-method 销毁方法
    public void destroyMethod()
    {
        System.out.println("执行 destroy-method 初销毁方法");
    }
}
```

在 `<bean>` 标签中指定 `order` 对象的 `init-method` 方法（初始化方法）和 `destroy-method` 方法（销毁方法）

```
<!-- 演示 bean 的生命周期 -->
<bean id="order" class="org.example.Order"
      init-method="initMethod" destroy-method="destroyMethod">
    <property name="name" value="guess"/>
</bean>
```

测试代码：记得要关闭 IOC 容器才会执行 `destroy-method` 方法，并且接口类型需要上升到 `ConfigurableApplicationContext` 才会提供 `close()` 方法

```
public class SpringTest
{
    @Test
    public void test()
    {
        //1.创建IOC容器对象
        ConfigurableApplicationContext iocContainer = new ClassPathXmlApplicationContext("spring-config.xml");

        //2.根据id值获取bean实例对象
        Order order = (Order) iocContainer.getBean("order");

        //3.打印bean
        System.out.println("使用创建好的 order 对象" + order);

        //4.关闭IOC容器
        iocContainer.close();
    }
}
```

运行结果：

```
执行无参数构造创建 bean 实例
调用 setter 方法为属性赋值
执行 init-method 初始化方法
使用创建好的 order 对象org.example.Order@47eaca72
执行 destroy-method 初销毁方法
```
