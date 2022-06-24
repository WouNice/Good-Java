# Bean的自动装配

## 什么是装配

当 bean 在 Spring 容器中组合在一起时，它被称为装配或 bean 装配。Spring容器需要知道需要什么 bean 以及容器应该如何使用依赖注入来将 bean 绑定在一起，同时装配 bean。

## 什么是自动装配

自动装配的概念：

-   手动装配：以value或ref的方式明确指定属性值都是手动装配。
-   自动装配：根据指定的装配规则，不需要明确指定，Spring自动将匹配的属性值注入bean中。

## 实现自动装配的模式

Spring 容器能够自动装配 bean。也就是说，可以通过检查 BeanFactory 的内容让 Spring 自动解析 bean 的协作者。

自动装配提供五种不同的模式供Spring容器用来自动装配beans之间的依赖注入:

-   no：默认设置，表示没有自动装配。应使用显式 bean 引用进行装配。通过手工设置ref 属性来进行装配bean。
-   byName：通过参数名自动装配，Spring容器查找beans的属性，这些beans在XML配置文件中被设置为byName。之后容器试图匹配、装配和该bean的属性具有相同名字的bean。
-   byType：byType：通过参数的数据类型自动装配，Spring容器查找beans的属性，这些beans在XML配置文件中被设置为byType。之后容器试图匹配和装配和该bean的属性类型一样的bean。如果有多个bean符合条件，则抛出错误。
-   构造函数(constructor)：这个同byType类似，不过是应用于构造函数的参数。如果在BeanFactory中不是恰好有一个bean与构造函数参数相同类型，则抛出一个严重的错误。当bean中存在多个构造器时，此种自动装配方式将会很复杂，不推荐使用。
-   autodetect：如果有默认的构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。

### 示例

相对于使用注解的方式实现的自动装配，在XML文档中进行的自动装配略显笨拙，在项目中更多的使用注解的方式实现。

通过 `<bean>` 标签的 `autowire="byType"`，指定 `student` 对象中的 `bean` 按照类型进行装配

```
<!-- 自动装配 -->
<bean id="student" class="org.example.Student" autowire="byType">
    <property name="stuId" value="001"/>
    <property name="stuName" value="Tom"/>
</bean>

<bean id="computer" class="org.example.Computer">
    <property name="computerId" value="001"/>
    <property name="computerName" value="local"/>
</bean>
```

测试代码：

```
public class SpringTest
{
    @Test
    public void gettingStart()
    {
        //1.创建IOC容器对象
        ApplicationContext iocContainer = new ClassPathXmlApplicationContext("spring-config.xml");

        //2.根据id值获取bean实例对象
        Student student = (Student) iocContainer.getBean("student");
        //3.打印bean
        System.out.println(student);
    }
}
```

运行结果：

```
Student{stuId=1, stuName='Tom', computer=Computer{computerId='001', computerName='local'}}
```

## 自动装配有什么局限？

-   覆盖的可能性：您始终可以使用 和 设置指定依赖项，这将覆盖自动装配。
-   基本元数据类型：简单属性（如原数据类型，字符串和类）无法自动装配。
-   令人困惑的性质：总是喜欢使用明确的装配，因为自动装配不太精确。

## 启动注解装配

默认情况下，Spring 容器中未打开注解装配。因此，要使用基于注解装配，我们必须通过配置`<context：annotation-conﬁg/>`元素在Spring 配置文件中启用它。



