# Bean配置方式

## 基于XML

在 XML 格式的配置文件中，配置bean所需的依赖项和服务指定。这些配置文件通常包含许多 bean 定义和特定于应用程序的配置选项。它们通常以 bean 标签开头。例如：

```
<bean id="studentbean" class="org.example.StudentBean">
	<property name="name" value="example"></property>
</bean>
```

## 基于注解配置

您可以通过在相关的类，方法或字段声明上使用注解@Component（@Controller、@Service、@Repostory），将 bean 配置为组件类本身，而不是使用 XML 来描述 bean 装配，但需要配置扫描包<component-scan>。底层通过反射调用构造方法。默认情况下，Spring 容器中未打开注解装配。因此，您需要在使用它之前在 Spring 配置文件中启用它。例如：

```
<beans>
	<context:annotation-config/>
	<!-- bean definitions go here -->
</beans>
```

## 基于JavaConfig

Spring的Java配置是通过使用@Bean和@Conﬁguration来实现。

-   @Bean注解扮演与元素相同的角色。

-   @Conﬁguration类允许通过简单地调用同一个类中的其他@Bean方法来定义bean间依赖关系。

例如：

```
@Configuration
public class StudentConfig {
	@Bean
	public StudentBean myStudent() { 
		return new StudentBean();
	}
}
```

## 使用注解@Import

3种方式：ImportSelector，ImportBeanDefinitionRegistrar，直接导入一个其他的配置类@Import(SecondJavaConfig.class)
