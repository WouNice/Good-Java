# Bean注解

## 管理对象

### @Configuration

与在xml中配置bean意义是一样的

### @Value

`@Value`注解用于注入普通属性的值，比如`@Value(value = "Oneby")`表示将`"Oneby"`字符串注入到属性中

```
@Value(value = "Oneby") 
private String name;
```

### @PostConstruct：初始化bean

### @PreDestory：销毁bean 

### @Scope

spring默认是单例的 那么如果想要创建多例 spring提供了一个@Scope注解，该注解决定了是创建多例还是单例

## 创建对象的注解

当出现这些注解说明是交给IOC容器管理的

### @Controller

### @Service

### @Reponsitory

### @Component

### @Component、@Controller、@Repository、@Service的区别

-   @Component ： 这将 java 类标记为 bean。它是任何 Spring 管理组件的通用构造型。spring 的组件扫描机制现在可以将其拾取并将其拉入应用程序环境中。
-   @Controller ： 这将一个类标记为 Spring Web MVC 控制器。标有它的Bean 会自动导入到 IoC 容器中。
-   @Service ： 此注解是组件注解的特化。它不会对 @Component 注解提供任何其他行为。您可以在服务层类中使用@Service 而不是@Component，因为它以更好的方式指定了意图。
-   @Repository ： 这个注解是具有类似用途和功能的 @Component 注解的特化。它为 DAO 提供了额外的好处。它将 DAO 导入 IoC 容器， 并使未经检查的异常有资格转换为 Spring DataAccessException。

## 实现注入的注解

### @AutoWrite

### @Resource

@Resource注解要求提供一个bean名称的属性，若该属性为空，则自动采用标注处的变量或方法名作为bean的名称。@Resource是JDK提供的注解，咱还是尽量使用Spring提供的注解吧~

解释上面那句话：如果使用@Resource则表示按照类型进行注入，我觉得等同于@Autowire的效果吧；如果使用@Resource(name="Xxx")则表示根据bean的名称进行注入

```
//@Resource //根据类型进行注入 
@Resource(name = "orderDao1") //根据bean名称进行注入 
private OrderDao orderDao;
```

## `@Autowired`注解

-   根据类型实现自动装配。
-   构造器、普通字段(即使是非public)、一切具有参数的方法都可以应用@Autowired注解
-   默认情况下，所有使用@Autowired注解的属性都需要被设置。当Spring找不到匹配的bean装配属性时，会抛出异常。
-   若某一属性允许不被设置，可以设置@Autowired注解的required属性为 false
-   默认情况下，当IOC容器里存在多个类型兼容的bean时，Spring会尝试匹配bean的id值是否与变量名相同，如果相同则进行装配。如果bean的id值不相同，通过类型的自动装配将无法工作。此时可以在@Qualifier注解里提供bean的名称。Spring甚至允许在方法的形参上标注@Qualifiter注解以指定注入bean的名称。
-   @Autowired注解也可以应用在数组类型的属性上，此时Spring将会把所有匹配的bean进行自动装配。
-   @Autowired注解也可以应用在集合属性上，此时Spring读取该集合的类型信息，然后自动装配所有与之兼容的bean。
-   @Autowired注解用在java.util.Map上时，若该Map的键值为String，那么 Spring将自动装配与值类型兼容的bean作为值，并以bean的id值作为键。

## `@Qualifier`注解

通过类型的自动装配将无法工作。此时可以在`@Qualifier`注解里提供bean的名称，`@Qualifier`注解需要和上面`@Autowired`注解一起使用

```
@Autowired //根据类型进行注入 
@Qualifier(value = "orderDao1") //根据bean名称进行注入 
private OrderDao orderDao;
```

## `@Inject`注解

`@Inject`和`@Autowired`注解一样也是按类型注入匹配的bean，但没有reqired属性。

## @Required注解

@Required 应用于 bean 属性 setter 方法。此注解仅指示必须在配置时使用bean 定义中的显式属性值或使用自动装配填充受影响的 bean属性。如果尚未填充受影响的 bean 属性，则容器将抛出 eanInitializationException。

示例：

```
public class Employee {
	 private String name;
	@Required
	public void setName(String name){ 
		this.name=name;
	}
	public string getName(){ 
		return name;
	}
}
```

## @Autowired 注解有什么用？

当您创建多个相同类型的 bean 并希望仅使用属性装配其中一个 bean 时，您可以使用@Qualiﬁer 注解和 @Autowired 通过指定应该装配哪个确切的 bean来消除歧义。

例如，这里我们分别有两个类，Employee 和 EmpAccount。在 EmpAccount中，使用@Qualiﬁer 指定了必须装配 id 为 emp1 的 bean。

Employee.java

```
public class Employee { private String name;
	@Autowired
	public void setName(String name) { 
		this.name=name;
	}
	public string getName() { 
		return name;
	}
}
```

EmpAccount.java

```
public class EmpAccount { 
	private Employee emp;
	@Autowired
	@Qualifier(emp1)
	public void showName() {
		System.out.println(“Employee name : ”+emp.getName);
	}
}
```

