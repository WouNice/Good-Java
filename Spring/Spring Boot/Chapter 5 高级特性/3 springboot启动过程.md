# springboot启动过程

## springboot启动过程

SpringBoot启动的时候，会构造一个SpringApplication的实例，然后调用这个实例的run方法，在run方法调用之前，也就是构造SpringApplication的时候会进行初始化的工作，初始化的时候会做以下几件事：

-   (1)把参数sources设置到SpringApplication属性中，这个sources可以是任何类型的参数.
-   (2)判断是否是web程序，并设置到webEnvironment的boolean属性中.
-   (3)创建并初始化ApplicationInitializer，设置到initializers属性中 。
-   (4)创建并初始化ApplicationListener，设置到listeners属性中 。
-   (5)初始化主类mainApplicatioClass。

 SpringApplication构造完成之后调用run方法，启动SpringApplication，run方法执行的时候会做以下几件事：

-   (1)构造一个StopWatch计时器，观察SpringApplication的执行 。
-   (2)获取SpringApplicationRunListeners并封装到SpringApplicationRunListeners中启动，用于监听run方法的执行。
-   (3)创建并初始化ApplicationArguments,获取run方法传递的args参数。
-   (4)创建并初始化ConfigurableEnvironment（环境配置）。
-   (5)打印banner（只用在Classpath下添加字符文件图标，就可以在启动时候打印）。
-   (6).构造Spring容器(ApplicationContext)上下文。
-   (7)SpringApplicationRunListeners发布finish事件。
-   (8)StopWatch计时器停止计时。

## Spring Boot初始化环境变量流程?

### **你如何理解SpringBoot配置加载顺序？**

1.  开发者工具 `Devtools` 全局配置参数;
2.  单元测试上的 `@TestPropertySource` 注解指定的参数;
3.  单元测试上的 `@SpringBootTest` 注解指定的参数;
4.  命令行指定的参数，如 `java -jar springboot.jar --name="码霸霸"`;
5.  命令行中的 `SPRING_APPLICATION_JSONJSON` 指定参数, 如 `java -Dspring.application.json='{"name":"码霸霸"}' -jar springboot.jar`;
6.  `ServletConfig` 初始化参数;
7.  `ServletContext` 初始化参数;
8.  JNDI参数（如 `java:comp/env/spring.application.json`）;
9.  Java系统参数（来源：`System.getProperties()`）;
10.  操作系统环境变量参数;
11.  `RandomValuePropertySource` 随机数，仅匹配：`ramdom.*`;
12.  JAR包外面的配置文件参数（`application-{profile}.properties（YAML）`）;
13.  JAR包里面的配置文件参数（`application-{profile}.properties（YAML）`）;
14.  JAR包外面的配置文件参数（`application.properties（YAML）`）;
15.  JAR包里面的配置文件参数（`application.properties（YAML）`）;
16.  `@Configuration`配置文件上 `@PropertySource` 注解加载的参数;
17.  默认参数（通过 `SpringApplication.setDefaultProperties` 指定）;

### **SpringBoot启动时都做了什么?**

1.  SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值
2.  将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作；
3.  整个J2EE的整体解决方案和自动配置都在springboot-autoconfigure的jar包中；
4.  它会给容器中导入非常多的自动配置类 （xxxAutoConfiguration）, 就是给容器中导入这个场景需要的所有组件 ， 并配置好这些组件 ；
5.  有了自动配置类 ， 免去了我们手动编写配置注入功能组件等的工作；

## Spring Boot扫描流程
