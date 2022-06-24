# Bean的继承

Spring允许继承bean的配置，被继承的bean称为父bean，继承的bean称为子bean，子bean也可以覆盖从父bean继承过来的配置

创建 `Person` 类：

```
import lombok.*;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class Person
{
    private Integer id;
    private String name;
    private String company;
    private String address;
    private String profession;
}
```

如果不使用继承配置 bean，那么`company`、`hobby`、`profession` 属性的值均相同，这样配置显得有些冗余

```
<!-- 不使用继承配置 bean -->
<bean id="corporateSlave1" class="org.example.CorporateSlave">
    <property name="id" value="1"/>
    <property name="name" value="father"/>
    <!-- 以下都是重复的属性 -->
    <property name="company" value="OneTech"/>
    <property name="hobby" value="Code"/>
    <property name="profession" value="Programer"/>
</bean>

<bean id="corporateSlave2" class="org.example.CorporateSlave">
    <property name="id" value="2"/>
    <property name="name" value="son"/>
    <!-- 以下都是重复的属性 -->
    <property name="company" value="OneTech"/>
    <property name="hobby" value="Code"/>
    <property name="profession" value="Programer"/>
</bean>
```

使用配置信息的继承配置 bean后，`son` 的配置信息继承于 `father`（指定 bean 的 `parent` 属性），自然就获得了 `father` 的所有配置信息，只需要重写自己不一样的配置信息即可

```
<!-- 演示配置信息的继承 -->
<bean id="person1" class="org.example.Person">
    <property name="id" value="1"/>
    <property name="name" value="father"/>
    <!-- 以下都是重复的属性 -->
    <property name="company" value="OneTech"/>
    <property name="address" value="bj"/>
    <property name="profession" value="Programer"/>
</bean>

<!-- 演示配置信息的继承 -->
<bean id="person2" parent="person1" class="org.example.Person">
    <property name="id" value="2"/>
    <property name="name" value="son"/>
</bean>
```

测试代码

```
public class SpringTest
{
    @Test
    public void test() {
        //1.创建IOC容器对象
        ApplicationContext iocContainer =
            new ClassPathXmlApplicationContext("spring-config.xml");

        //2.根据id值获取bean实例对象
        Person person1 = (Person) iocContainer.getBean("person1");
        Person person2 = (Person) iocContainer.getBean("person2");

        //3.打印bean
        System.out.println(person1);
        System.out.println(person2);
    }
}
```

运行结果：

```
Person{id=1, name='father', company='OneTech', address='bj', profession='Programer'}
Person{id=2, name='son', company='OneTech', address='bj', profession='Programer'}
```

父bean可以作为配置模板，也可以作为bean实例。若只想把父bean作为模板，可以设置`<bean>`的`abstract` 属性为`true`，这样Spring将不会实例化这个bean。