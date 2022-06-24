# SpEL表达式语言

SpEL的全称是 Spring Expression Language，即Spring表达式语言，支持运行时查询并可以操作对象图，和JSP页面上的EL表达式、Struts2中用到的OGNL表达式一样，SpEL根据JavaBean风格的getXxx()、setXxx()方法定义的属性访问对象图，完全符合我们熟悉的操作习惯。

## 基本语法

SpEL使用`#{…}`作为定界符，所有在大框号中的字符都将被认为是SpEL表达式

## 字面量

-   整数：<property name="count" value="#{1}"/>
-   小数：<property name="frequency" value="#{1.0}"/>
-   科学计数法：<property name="capacity" value="#{1e4}"/>
-   String类型的字面量可以使用单引号或者双引号作为字符串的定界符号
    -   <property name="name" value="#{'Oneby'}"/>
    -   <property name='name' value='#{"Tom"}'/>
-   Boolean：<property name="enabled" value="#{false}"/>

## 引用其他 bean

在 `<bean>` 标签的 `value` 属性中通过 `#{对象名}` 引用其他 bean，注意：不能使用 `ref` 属性

```
<!-- 引用其他 bean -->
<bean id="student" class="org.example.Student">
    <property name="stuId" value="001"/>
    <property name="stuName" value="Tom"/>
    <property name="computer" value="#{computer}"/>
</bean>

<bean id="computer" class="org.example.Computer">
    <property name="computerId" value="001"/>
    <property name="computerName" value="local"/>
</bean>
```

## 引用其他 bean 的属性

在 `<property>` 标签中通过 `#{对象名.属性名}` 引用其他 bean 的属性

```
<!-- 引用其他 bean 的属性 -->
<bean id="student" class="org.example.Student">
    <property name="stuId" value="001"/>
    <property name="stuName" value="Tom"/>
    <property name="computer">
        <bean class="org.example.Computer">
            <property name="computerId" value="#{computer.computerId}"/>
            <property name="computerName" value="#{computer.computerName}"/>
        </bean>
    </property>
</bean>

<bean id="computer" class="org.example.Computer">
    <property name="computerId" value="001"/>
    <property name="computerName" value="local"/>
</bean>
```

## 调用非静态方法

通过 `#{对象名.方法名}` 调用对象的非静态方法

```
<!-- 调用非静态方法 -->
<bean id="student" class="org.example.Student">
    <property name="stuId" value="001"/>
    <property name="stuName" value="Tom"/>
    <property name="computer">
        <bean class="org.example.Computer">
            <property name="computerId" value="#{computer.getComputerId()}"/>
            <property name="computerName" value="#{computer.getComputerName()}"/>
        </bean>
    </property>
</bean>

<bean id="computer" class="org.example.Computer">
    <property name="computerId" value="001"/>
    <property name="computerName" value="local"/>
</bean>
```

## 调用静态方法

定义获取随机整数的方法，随机整数的范围为 `[start,end]`

```
public class MathUtil
{

    public static int getRandomInt(int start, int end)
    {
        return (int) (Math.random() * (end - start + 1) + start);
    }

}
```

创建Num类：

```
import lombok.*;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class Num
{
    private Integer index;
}
```

通过 `T(静态类路径).方法名` 调用静态方法：

```
<bean id="number" class="org.example.Num">
    <property name="index" value="#{mathUtil.getRandomInt(1,6)}"/>
</bean>

<bean id="mathUtil" class="org.example.MathUtil"/>
```

测试代码：

```
public class SpringTest
{
    @Test
    public void test()
    {
        //1.创建IOC容器对象
        ApplicationContext iocContainer = new ClassPathXmlApplicationContext("spel-test.xml");

        //2.根据id值获取bean实例对象
        Num index = (Num) iocContainer.getBean("number");

        //3.打印bean
        System.out.println(index);
    }
}
```

运行结果：

```
Num{index=5}
```

## 运算符

-   算术运算符：`+`、`-`、`*`、`/`、`%`、`^`

-   字符串连接：`+`

-   比较运算符：`<`、`>`、`==`、`<=`、`>=`、`lt`、`gt`、`eq`、`le`、`ge`

-   逻辑运算符：`and`, `or`, `not`,`|`

-   三目运算符：判断条件`?`判断结果为true时的取值`:`判断结果为false时的取值

-   正则表达式：matches
