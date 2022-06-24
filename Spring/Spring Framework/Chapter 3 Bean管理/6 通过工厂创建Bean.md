# 通过工厂创建 bean

在利用工厂模式创建bean实例的时候有两种方式，分别是静态工厂和实例工厂。

-   静态工厂：工厂本身不需要创建对象，但是可以通过静态方法调用，对象=工厂类.静态工厂方法名（）；

-   实例工厂：工厂本身需要创建对象，工厂类。工厂对象=new 工厂类；工厂对象.get对象名（）；


## 静态工厂

将对象创建的过程封装到静态方法中。当客户端需要对象时，只需要简单地调用静态方法，而不用关心创建对象的细节。

声明通过静态方法创建的bean需要在<bean>的class属性里指定静态工厂类的全类名，同时在factory-method属性里指定工厂方法的名称。最后使用<constrctor-arg>元素为该方法传递方法参数。

PersonStaticFactory.java

```
public class PersonStaticFactory {
    public static Person getPerson(String name){
        Person person = new Person();
        person.setId(0);
        person.setName(name);
        return person;
    }
}
```

ioc.xml

```
<!--
静态工厂的使用：
class:指定静态工厂类
factory-method:指定哪个方法是工厂方法
-->
<bean id="person" class="org.example.PersonStaticFactory" factorymethod="getPerson">
    <!--constructor-arg：可以为方法指定参数-->
    <constructor-arg value="tom"></constructor-arg>
</bean>
```

## 实例工厂

将对象的创建过程封装到另外一个对象实例的方法里。当客户端需要请求对象时，只需要简单的调用该实例方法而不需要关心对象的创建细节。

实现方式

-   配置工厂类实例的bean
-   在`factory-method`属性里指定该工厂方法的名称
-   使用`construtor-arg`元素为工厂方法传递方法参数

PersonInstanceFactory.java

```
public class PersonInstanceFactory {
    public static Person getPerson(String name){
        Person person = new Person();
        person.setId(0);
        person.setName(name);
        return person;
    }
}
```

ioc.xml

```
<!--实例工厂使用-->
<!--创建实例工厂类-->
<bean id="personInstanceFactory" class="org.example.PersonInstanceFactory"></bean>
    <!--
    factory-bean:指定使用哪个工厂实例
    factory-method:指定使用哪个工厂实例的方法
    -->
    <bean id="person2" class="org.example.Person" factorybean="personInstanceFactory" factory-method="getPerson">
    <constructor-arg value="tom"></constructor-arg>
</bean>
```

## FactoryBean

以上两种方式通常使用的不多，我们一般通过实现 FactoryBean 接口，并重写其中的方法来获得工厂类

-   Spring中有两种类型的bean，一种是普通bean，另一种是工厂bean，即FactoryBean
-   工厂bean跟普通bean不同，其返回的对象不是指定类的一个实例，其返回的是该工厂bean的getObject()方法所返回的对象
-   工厂bean必须实现org.springframework.beans.factory.FactoryBean接口

FactoryBean是Spring规定的一个接口，当前接口的实现类，Spring都会将其作为一个工厂，但是在ioc容器启动的时候不会创建实例，只有在使用的时候才会创建对象

FactoryBean 接口中有如下三个方法：

-   getObject() 方法负责将创建好的 bean 实例返回给 IOC 容器；
-   getObjectType() 方法负责返回工厂生产的 bean 类型；
-   isSingleton() 方法用于指示该 bean 实例是否为单例，默认是单例 bean

```java
public interface FactoryBean<T> {
    
    // Return an instance (possibly shared or independent) of the object managed by this factory.
	@Nullable
	T getObject() throws Exception;
    
    // Return the type of object that this FactoryBean creates, or null if not known in advance.
    @Nullable
	Class<?> getObjectType();
    
    // Is the object managed by this factory a singleton?
	default boolean isSingleton() {
		return true;
	}
}
```

### 示例

创建 `StudentFactory` 类，该类实现了 `FactoryBean` 接口，并重写了其中的 `getObject()` 和 `getObjectType()` 方法

```
import org.springframework.beans.factory.FactoryBean;

public class StudentFactory implements FactoryBean<Student>
{
    @Override
    public Student getObject() throws Exception {
        return new Student(001,"Tom");
    }

    @Override
    public Class<?> getObjectType() {
        return Student.class;
    }
}
```

在 Spring 配置文件中使用 `StudentFactory` 工厂创建 `Student` 对象

```
<bean id="student" class="org.example.StudentFactory"/>
```

测试代码:

```
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class SpringTest
{
    @Test
    public void gettingStart()
    {
        ApplicationContext iocContainer = new ClassPathXmlApplicationContext("collection-property-injection.xml");
        System.out.println(iocContainer.getBean("student"));
    }
}
```

