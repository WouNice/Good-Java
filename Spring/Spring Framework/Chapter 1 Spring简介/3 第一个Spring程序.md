# 第一个Spring程序

## 创建 maven 工程

创建一个名为`springtest`的maven 工程。

在pom.xml中引入spring-context、spring-core的依赖，为了测试方便，我们引入`jupiter` 依赖，为了简化代码，我们引入`lombok` 依赖。

```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.20</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>5.3.20</version>
    </dependency>

    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.8.2</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.24</version>
    </dependency>
</dependencies>
```

## 创建实体类

创建 `Student` 实体类：

```
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class Student {

    private Integer stuId;

    private String stuName;

}
```

## 编写XML配置文件

在 resources 包下点击鼠标右键，选择【New】–>【XML Configuration File】–>【Spring Config】，创建`getting-start.xml`

添加如下配置：创建 Student 对象的实例，并注入属性的值

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 使用bean元素定义一个由IOC容器创建的对象 -->
    <!-- class属性指定用于创建bean的全类名 -->
    <!-- id属性指定用于引用bean实例的标识 -->
    <bean id="student" class="org.example.Student">
        <!-- 使用property子元素为bean的属性赋值 -->
        <property name="stuId" value="001"/>
        <property name="stuName" value="Tom"/>
    </bean>
</beans>
```

## 测试用例

首先创建 `ClassPathXmlApplicationContext` 对象，XML 配置文件的路径为类路径下的 `getting-start.xml`；然后获取容器中的 Student 对象，并打印此 Student 对象

```
import org.example.Student;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SpringTest
{

    @Test
    public void gettingStart()
    {
        //1.创建IOC容器对象
        ApplicationContext iocContainer = new ClassPathXmlApplicationContext("getting-start.xml");

        //2.根据id值获取bean实例对象
        Student student = (Student) iocContainer.getBean("student");

        //3.打印bean
        System.out.println(student);
    }

}
```

测试结果：从 Spring 容器中获取到的 Student 对象，其属性值已经被注入

```
Student(stuId=1, stuName=Tom)
```

> 对象的属性是由setter/getter方法决定的，而不是定义的成员属性

## 使用注解

在模块中引入`spring-test`模块：

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.3.20</version>
    <scope>test</scope>
</dependency>
```

使用 `@SpringJUnitConfig(locations = "classpath:getting-start.xml")` 注解指明 Spring 单元测试的配置文件路径，再使用 `@Autowired` 注解自动装配容器中的 Student 对象

```
@SpringJUnitConfig(locations = "classpath:getting-start.xml")
public class SpringTest {

    @Autowired
    private Student student;

    @Test
    public void gettingStart() {
        System.out.println(student);
    }

}
```
