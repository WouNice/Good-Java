# SpringMVC配置核心文件

Spring MVC 使用扫描机制找到应用中所有基于注解的控制器类，所以，为了让控制器类被 Spring MVC 框架扫描到，需要在配置文件中声明 spring-context，并使用 `<context:component-scan/>` 元素指定控制器类的基本包（请确保所有控制器类都在基本包及其子包下）。

spring-mvc.xml：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--配置注解扫描-->
    <!-- 使用扫描机制扫描控制器类，控制器类都在org.example.controller包及其子包下 -->
    <context:component-scan base-package="org.example.controller" />
</beans>
```
