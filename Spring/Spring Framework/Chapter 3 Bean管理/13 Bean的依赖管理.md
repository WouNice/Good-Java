# Bean的依赖管理

有的时候创建一个bean的时候需要保证另外一个bean也被创建，这时我们称前面的bean对后面的bean有依赖。例如：要求创建Student对象的时候必须创建Computer。这里需要注意的是依赖关系不等于引用关系，Student即使依赖Computer也可以不引用它

举例一： student 对象依赖 computer 对象，但我们不创建 computer 对象

在配置文件中，我们只实例化 `student` 对象，并且执行其 `depends-on` 属性等于 `computer`，表示`student` 对象的创建依赖于 `computer` 对象的创建

```
<!-- 演示 bean 之间的依赖 -->
<bean id="student" class="org.example.Student" depends-on="computer">
    <property name="stuId" value="001"/>
    <property name="stuName" value="Tom"/>
</bean>
```

测试代码

```
public class SpringTest
{
    @Test
    public void test()
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

程序运行结果：

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'student' defined in class path resource [spring-config.xml]: 'student' depends on missing bean 'computer'; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'computer' available
```

报错信息已经很明显了：`'student' depends on missing bean 'computer'`，说是缺少一个 `computer` 对象

既然 `student` 对象依赖 `computer` 对象，那么我们在配置文件中创建 `computer` 对象

```
<bean id="computer" class="org.example.Computer"/>
```

运行结果：

```
Student{stuId=1, stuName='Tom', computer=null}
```
