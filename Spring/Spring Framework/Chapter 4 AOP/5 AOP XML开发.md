# AOP XML开发

在 Spring 中使用 AspectJ，需要在maven中引入 aop 和 aspects 相关的依赖

```
<dependencies>
    <!-- spring-aop -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>5.3.20</version>
    </dependency>

    <!-- spring-aspects -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>5.3.20</version>
    </dependency>
    
    <!-- aspectjweaver -->
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.9.1</version>
    </dependency>

    <!-- aopalliance -->
    <dependency>
        <groupId>aopalliance</groupId>
        <artifactId>aopalliance</artifactId>
        <version>1.0</version>
    </dependency>

    <!-- cglib -->
    <dependency>
        <groupId>cglib</groupId>
        <artifactId>cglib</artifactId>
        <version>3.3.0</version>
    </dependency>
</dependencies>
```

编写 Spring 配置文件aop.xml：引入 `context` 和 `aop` 名称空间；开启组件扫描，并指明包路径；开启自动代理功能

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:context="http://www.springframework.org/schema/c"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context
                http://www.springframework.org/schema/aop
                http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 组件扫描 -->
    <context:component-scan base-package="org.example"/>
    <!-- 开启 Aspect生成代理对象 -->
    <aop:aspectj-autoproxy/>

</beans>
```

`ArithmeticCalculator` 接口：定义各种数学运算方法

```
public interface ArithmeticCalculator {
    void add(int i, int j);
    void sub(int i, int j);
    void mul(int i, int j);
    void div(int i, int j);
}
```

`ArithmeticCalculatorImpl` 类：实现了 `ArithmeticCalculator` 接口中各种抽象的数学运算方法

```
@Component
public class ArithmeticCalculatorImpl implements ArithmeticCalculator {
    @Override
    public void add(int i, int j) {
        int result = i + j;
        System.out.println("计算器计算得到的结果为： " + result);
    }

    @Override
    public void sub(int i, int j) {
        int result = i - j;
        System.out.println("计算器计算得到的结果为： " + result);
    }

    @Override
    public void mul(int i, int j) {
        int result = i * j;
        System.out.println("计算器计算得到的结果为： " + result);
    }

    @Override
    public void div(int i, int j) {
        int result = i / j;
        System.out.println("计算器计算得到的结果为： " + result);
    }
}
```

