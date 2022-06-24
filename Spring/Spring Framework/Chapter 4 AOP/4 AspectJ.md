# AspectJ

AspectJ是一个面向切面编程的框架，使用AspectJ不需要改动Spring配置文件，就可以实现Spring AOP功能。

在spring-aspects模块中引入了AspectJ工程。在Spring2.0以上版本中，可以使用基于AspectJ注解或基于XML配置的AOP。

@AspectJ可以通过注解声明切面。为了能够使用@AspectJ注解，必须要开启Spring 的配置支持。@AspectJ注解支持以XML的方式进行切面声明配置，如`<aop:aspectj-autoproxy/>`标签配置。也可以通过Java配置的方式进行切面声明配置，`@EnableAspectJAutoProxy`注解用于开启AOP代理自动配置。

AspectJ的重要注解如下所示。

-   @Aspect：把当前类标识为一个切面
-   @Pointcut：植入Advice(通知)的触发条件。
-   @Before：前置增强，相当于BeforeAdvice，目标方法执行前执行
-   @After：后置增强，不管是抛出异常或者正常退出都会执行
-   @AfterReturning：后置增强，相当于AfterReturningAdvice，方法正常退出时执行
-   @AfterThrowing：异常抛出增强，相当于ThrowsAdvice，目标方法抛出异常后执行
-   @Around：环绕增强，相当于MethodInterceptor

@Pointcut切点注解是匹配一个执行表达式。表达式类型如下所示。

-   `execution`：用于匹配方法执行的连接点；
-   `within`：用于匹配指定类型内的方法执行；
-   `this`：用于匹配当前AOP代理对象类型的执行方法；注意是AOP代理对象的类型匹配，这样就可能包括引入接口也类型匹配；
-   `target`：用于匹配当前目标对象类型的执行方法；注意是目标对象的类型匹配，这样就不包括引入接口也类型匹配；
-   `args`：用于匹配当前执行的方法传入的参数为指定类型的执行方法；
-   `@within`：用于匹配所以持有指定注解类型内的方法；
-   `@target`：用于匹配当前目标对象类型的执行方法，其中目标对象持有指定的注解；
-   `@args`：用于匹配当前执行的方法传入的参数持有指定注解的执行；
-   `@annotation`：用于匹配当前执行方法持有指定注解的方法；

@Pointcut切点表达式可以组合使用`&&`、`||`或`!`三种运算符。示例代码如下：

```
@Pointcut("com.xyz.myapp.SystemArchitecture.dataAccessOperation() && args(account,..)")
```

@Around通知注解的方法可以传入ProceedingJoinPoint参数，ProceedingJoinPoint类实例可以获取切点方法的相关参数及实例等。

## 在 Spring 中使用 AspectJ 进行 AOP 操作

实现 AOP 操作的步骤

1.  编写切面类（通过 @Aspect 注解标识这是一个切面类），并且不要忘记将切面类交给 Spring IOC 管理（Component 注解），并编写相应的通知方法与切入点表达式

2.  在 Spring 配置文件中开启 aop 功能：通过 <`aop:aspectj-autoproxy/`> 注解开启 aop 功能。当Spring IOC容器侦测到bean配置文件中的`<aop:aspectj-autoproxy>`元素时，会自动为与AspectJ切面匹配的bean创建代理

AspectJ支持5种类型的通知注解

-   @Before：前置通知，在方法执行之前执行

-   @After：后置通知，在方法执行之后执行

-   @AfterRunning：返回通知，在方法返回结果之后执行

-   @AfterThrowing：异常通知，在方法抛出异常之后执行

-   @Around：环绕通知，围绕着方法执行