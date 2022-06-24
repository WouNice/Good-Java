# Bean的后置处理器

bean的后置处理器

-   bean后置处理器允许在调用初始化方法前后对bean进行额外的处理
-   bean后置处理器对IOC容器里的所有bean实例逐一处理，而非单一实例。其典型应用是：检查bean属性的正确性或根据特定的标准更改bean的属性。

bean后置处理器时需要实现接口：org.springframework.beans.factory.config.BeanPostProcessor。在初始化方法被调用前后，Spring将把每个bean实例分别传递给上述接口的以下两个方法：

-   postProcessBeforeInitialization(Object, String)
-   postProcessAfterInitialization(Object, String)

添加bean后置处理器后bean的生命周期

-   通过构造器或工厂方法创建bean实例
-   为bean的属性设置值和对其他bean的引用
-   将bean实例传递给bean后置处理器的postProcessBeforeInitialization()方法
-   调用bean的初始化方法
-   将bean实例传递给bean后置处理器的postProcessAfterInitialization()方法
-   bean的使用过程
-   当容器关闭时调用bean的销毁方法

[示例] 创建 `MyBeanPost` 类，继承自 `BeanPostProcessor` 类，并重写其中的 `postProcessBeforeInitialization` 和 `postProcessAfterInitialization` 方法：

```
public class MyBeanPost implements BeanPostProcessor
{
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException
    {
        System.out.println("第三步：执行 postProcessBeforeInitialization 方法");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException
    {
        System.out.println("第五步：执行 postProcessAfterInitialization 方法");
        return bean;
    }
}
```

配置文件：在配置文件中实例化我们自定义的 `MyBeanPost` 后置处理器

```
<!-- 演示 bean 的生命周期 -->
<bean id="order" class="org.example.Order"
      init-method="initMethod" destroy-method="destroyMethod">
    <property name="name" value="iPad"/>
</bean>

<!-- 加上 BeanPostProcessor 后的生命周期 -->
<bean id="myBeanPost" class="org.example.MyBeanPost"/>
```

测试代码：现在使用 `order` 对象变成了第六步

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
执行 postProcessBeforeInitialization 方法
执行 init-method 初始化方法
执行 postProcessAfterInitialization 方法
使用创建好的 order 对象org.example.Order@235834f2
执行 destroy-method 初销毁方法
```
