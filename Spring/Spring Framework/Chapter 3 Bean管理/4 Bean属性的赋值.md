# Bean属性的赋值

## 属性注入的方式

### 通过 bean 的 setter 方法注入属性

通过 `<property>` 标签指定属性名，Spring 会查找该属性对应的 setter 方法，注入其属性值

```
<bean id="student" class="org.example.Student">
    <property name="stuId" value="001"/>
    <property name="stuName" value="Tom"/>
</bean>
```

### 通过bean的类型获取对象

```
@test
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("ioc.xml");
        Person person = context.getBean("person", Person.class);
        System.out.println(person);
    }
}
```

### 通过构造器注入属性值

通过 `constructor-arg` 标签为对象的属性赋值，通过 `name` 指定属性名，`value` 指定属性值

```
<bean id="student" class="org.example.Student">
    <constructor-arg name="stuId" value="001" />
    <constructor-arg name="stuName" value="Tom" />
</bean>
```

### 通过级联属性赋值

为了演示级联属性的赋值，我们添加 `Computer` 类

```
import lombok.*;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class Computer
{
    String computerId;
    String computerName;
}
```

在 `Student` 类中添加 `Computer` 类型的字段

```
import lombok.*;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class Student
{
    private Integer stuId;
    private String stuName;
    private Computer computer;
}
```

在 student 对象中有一个名为 computer 的字段，该字段指向了一个 Computer对象，级联属性赋值的含义为：在 student bean 中的 <property> 标签中可以通过 computer.computerId 和 computer.computerName 的方式为 computer 对象中的 computerId 和 computerName 字段赋值

```
<!-- 演示级联属性赋值 -->
<bean id="student" class="org.example.Student">
    <property name="computer" ref="computer"/>
    <!-- 设置级联属性(了解) -->
    <property name="computer.computerId" value="001"/>
    <property name="computer.computerName" value="local"/>
</bean>

<bean id="computer" class="org.example.Computer"/>
```

运行结果：

```
Student{stuId=null, stuName='null', computer=Computer{computerId='001', computerName='local'}}
```

### 通过p名称空间注入属性值

为了简化XML文件的配置，越来越多的XML文件采用属性而非子元素配置信息。Spring从2.5版本开始引入了一个新的p命名空间，可以通过`<bean>`元素属性的方式配置Bean的属性。使用p命名空间后，基于XML的配置方式将进一步简化。

准备工作：在配置文件中引入 p 名称空间

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
```

通过 p 名称空间注入属性值

```
<bean id="student" class="org.example.Student" p:stuId="001" p:stuName="Tom"/>
```

>   其实 p 名称空间赋值就是 `<property>` 标签赋值的简化版

## 注入属性的类型

### 字面量

1.  基本数据类型及其封装类、`String`等类型，可以通过`value`属性或`value`子节点的方式指定
2.  若字面值中包含特殊字符，可以使用`<![CDATA[]]>`把字面值包裹起来

示例：

```
<bean id="student" class="org.example.Student">
    <property name="stuId" value="001"/>
    <property name="stuName" value="Tom"/>
</bean>
```

### null

通过 `<null/>` 标签将引用类型字段的值设置为 `null`

```
<bean id="student" class="org.example.Student">
    <property name="stuId" value="001"/>
    <!-- 将 stuName 字段的值设置为 null -->
    <property name="stuName">
        <null/>
    </property>
    <!-- 将 computer 字段的值设置为 null -->
    <property name="computer">
        <null/>
    </property>
</bean>
```

### 引用外部 bean

通过 `<property>` 标签中的 `ref` 属性引用外部 bean

```
<!-- 引用外部声明的 bean -->
<bean id="student" class="org.example.Student">
    <property name="stuId" value="001"/>
    <property name="stuName" value="Tom"/>
    <!-- 通过 ref 属性引用外部的 bean -->
    <property name="computer" ref="computer"/>
</bean>
<!-- 外部 bean -->
<bean id="computer" class="org.example.Computer">
    <property name="computerId" value="local"/>
    <property name="computerName" value="local"/>
</bean>
```

### 引用内部 bean

当bean实例仅仅给一个特定的属性使用时，可以将其声明为内部bean。内部bean声明直接包含在`<property>`或`<constructor-arg>`元素里，不需要设置任何`id`或`name`属性，内部bean不能使用在其他地方

>   在 `<property>` 标签中不使用 `ref` 属性引用外部 bean，而是直接定义一个 内部 bean

```
<!-- 引用内部声明的 bean -->
<bean id="student" class="org.example.Student">
    <property name="stuId" value="001"/>
    <property name="stuName" value="Tom"/>
    <property name="computer">
        <!-- 通过 <bean> 标签定义内部 bean -->
        <bean class="org.example.Computer">
            <property name="computerId" value="local"/>
            <property name="computerName" value="local"/>
        </bean>
    </property>
</bean>
```

## 对集合属性赋值

在Spring中可以通过一组内置的XML标签来配置集合属性，比如：`<array>`、`<list>`、`<set>`、`<map>`、`<props>`，并且可以用过引入 `util` 名称空间来提取集合类型的 bean

### 准备工作

新建 `CollectionExample` 类，演示对集合属性的操作

```
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Set;
import lombok.*;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class CollectionExample {
    private String[] array;
    private List<String> list;
    private Set<String> set;
    private Map<String,String> map;
    private Properties properties;
}
```

单元测试代码：

```
public class SpringTest
{

    @Test
    public void gettingStart()
    {
        //1.创建IOC容器对象
        ApplicationContext iocContainer = new ClassPathXmlApplicationContext("collection-property-injection.xml");

        //2.根据id值获取bean实例对象
        CollectionExample collectionExample = (CollectionExample) iocContainer.getBean("collectionExample");

        //3.打印bean
        System.out.println(collectionExample);
    }

}
```

创建collection-property-injection.xml，在其中放置bean语句：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
```

### 数组

通过 `<array>` 标签定义数组集合，并且可以通过`<value>`指定简单的常量值，通过`<ref>`指定对其他bean的引用。通过`<bean>`指定内置bean定义。通过`<null/>`指定空元素，甚至可以内嵌其他集合。

```
<bean id="collectionExample" class="org.example.CollectionExample">
    <property name="array">
        <array>
            <value>One</value>
            <value>two</value>
        </array>
    </property>
</bean>
```

### List

通过 `<list>` 标签定义数组集合，并且可以通过`<value>`指定简单的常量值，通过`<ref>`指定对其他bean的引用。通过`<bean>`指定内置bean定义。通过`<null/>`指定空元素，甚至可以内嵌其他集合。

```
<bean id="collectionExample" class="org.example.CollectionExample">
    <property name="list">
        <list>
            <value>One</value>
            <value>two</value>
        </list>
    </property>
</bean>
```

### Set

通过 `<set>` 标签定义数组集合，并且可以通过`<value>`指定简单的常量值，通过`<ref>`指定对其他bean的引用。通过`<bean>`指定内置bean定义。通过`<null/>`指定空元素，甚至可以内嵌其他集合。

```
<bean id="collectionExample" class="org.example.CollectionExample">
    <property name="set">
        <set>
            <value>One</value>
            <value>two</value>
        </set>
    </property>
</bean>
```

### Map

Java.util.Map通过<map>标签定义，<map>标签里可以使用多个<entry>作为子标签，每个<entry>中包含一个键和一个值。因为键和值的类型没有限制，所以可以自由地为它们指定<value>、<ref>、<bean>或<null/>元素。因此对于常量型的key-value键值对可以使用key和value来定义；bean引用通过key-ref和value-ref属性定义。

```
<bean id="collectionExample" class="org.example.CollectionExample">
    <property name="map">
        <map>
            <entry key="name" value="Tom"/>
            <entry key="age" value="18"/>
        </map>
    </property>
</bean>
```

### Properties

使用`<props>`定义`java.util.Properties`，该标签使用多个`<prop>`作为子标签，每个`<prop>`标签中定义`key`和`value`

```
<bean id="collectionExample" class="org.example.CollectionExample">
    <property name="properties">
        <props>
            <prop key="name">Tom</prop>
            <prop key="age">18</prop>
        </props>
    </property>
</bean>
```

### 集合类型的 bean

如果只能将集合对象配置在某个bean内部，则这个集合的配置将不能重用。我们需要将集合bean的配置拿到外面，供其他bean引用。

**引入名称空间：配置集合类型的bean需要引入util名称空间**

将 beans 名称空间对应的这两项`xmlns="http://www.springframework.org/schema/beans"`和`"http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">`，将 beans 全部替换为 util 就行啦~

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/util
                http://www.springframework.org/schema/util/spring-util.xsd">
```

使用 `<util>` 标签完成对集合类型 bean 的抽取，并为其设置 `id` 属性，方便其他地方进行引用

```
<!-- 集合类型的 bean-->
<util:list id="list">
    <value>one</value>
    <value>two</value>
</util:list>
<bean id="collectionExample" class="org.example.CollectionExample">
    <property name="list" ref="list"/>
</bean>
```
