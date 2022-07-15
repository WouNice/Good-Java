# SpringMVC注解

使用基于注解的控制器具有以下 2 个优点：

1.  在基于注解的控制器类中可以编写多个处理方法，进而可以处理多个请求（动作），这就允许将相关的操作编写在同一个控制器类中，从而减少控制器类的数量，方便以后维护。
2.  基于注解的控制器不需要在配置文件中部署映射，仅需要使用 @RequestMapping 注解一个方法进行请求处理即可。

## 控制器注解

### @Controller

@Controller标记在一个类上，使用它标记的类就是一个SpringMVC Controller对象。处理器适配器将会扫描使用了该注解的类的方法，并检测该方法是否使用了@RequestMapping注解。@Controller 只是定义了一个控制器类，而使用@RequestMapping 注解的方法才是真正处理请求的处理器。

单单使用@Controller 标记在一个类上还不能真正意义上的说它就是Spring MVC 的一个控制器类，因为这个时候Spring 还不认识它。那么要如何做Spring 才能认识它呢？这个时候就需要我们把这个控制器类交给Spring 来管理。有两种方式：

-   在Spring MVC 的配置文件中定义 Controller 的bean 对象。
-   在Spring MVC 的配置文件中告诉Spring 该到哪里去找标记为@Controller 的Controller 控制器。

### @RestController

@RestController注解相当于@ResponseBody ＋ @Controller，表示是表现层，除此之外，一般不用别的注解代替。

## @RequestMapping

@RequestMapping用于处理请求 url 映射的注解，可用于类或方法上。

-   类上，请求URL 的第一级访问目录。
-   方法上，请求 URL 的第二级访问目录，与类上的使用@ReqquestMapping标注的一级目录一起组成访问路径

返回值会通过视图解析器解析为实际的物理视图，对于 InternalResourceViewResolver 视图解析器，通过 prefix + returnValue + suffix 这样的方式得到实际的物理视图，然后做转发操作。

```
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

@RequestMapping有如下6个属性：

-   value：指定请求的实际地址。
-   method：指定请求的method类型， GET、POST、PUT、DELETE等。
-   consumes：指定处理请求的提交内容类型（Content-Type），例如application/json, text/html。
-   produces：指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回；
-   params：指定request中必须包含某些参数值是，才让该方法处理。
-   headers：指定request中必须包含某些指定的header值，才能让该方法处理请求。

### value 属性

value 属性是 @RequestMapping 注解的默认属性，因此如果只有 value 属性时，可以省略该属性名，如果有其它属性，则必须写上 value 属性名称。

```
@RequestMapping(value="toUser")
@RequestMapping("toUser")
```

value 属性支持通配符匹配，如 @RequestMapping(value=“toUser/*”) 表示 http://localhost:8080/toUser/1 或 http://localhost:8080/toUser/hahaha 都能够正常访问。

### path属性

path 属性和 value 属性都用来作为映射使用。即 @RequestMapping(value=“toUser”) 和 @RequestMapping(path=“toUser”) 都能访问 toUser() 方法。

path 属性支持通配符匹配，如 @RequestMapping(path=“toUser/*”) 表示 http://localhost:8080/toUser/1 或 http://localhost:8080/toUser/hahaha 都能够正常访问。

### name属性

name属性相当于方法的注释，使方法更易理解。如 @RequestMapping(value = “toUser”,name = “获取用户信息”)。

### method属性

method 属性用于表示该方法支持哪些 HTTP 请求。如果省略 method 属性，则说明该方法支持全部的 HTTP 请求。

@RequestMapping(value = “toUser”,method = RequestMethod.GET) 表示该方法只支持 GET 请求。也可指定多个 HTTP 请求，如 @RequestMapping(value = “toUser”,method = {RequestMethod.GET,RequestMethod.POST})，说明该方法同时支持 GET 和 POST 请求。

### params属性

params 属性用于指定请求中规定的参数，代码如下。

```
@RequestMapping(value = "toUser",params = "type")
public String toUser() {
    return "showUser";
}
```

以上代码表示请求中必须包含 type 参数时才能执行该请求。即 http://localhost:8080/toUser?type=xxx 能够正常访问 toUser() 方法，而 http://localhost:8080/toUser 则不能正常访问 toUser() 方法。

```
@RequestMapping(value = "toUser",params = "type=1")
public String toUser() {
    return "showUser";
}
```

以上代码表示请求中必须包含 type 参数，且 type 参数为 1 时才能够执行该请求。即 http://localhost:8080/toUser?type=1 能够正常访问 toUser() 方法，而 http://localhost:8080/toUser?type=2 则不能正常访问 toUser() 方法。

### header属性

header 属性表示请求中必须包含某些指定的 header 值。

@RequestMapping(value = “toUser”,headers = “Referer=http://www.xxx.com”) 表示请求的 header 中必须包含了指定的“Referer”请求头，以及值为“http://www.xxx.com”时，才能执行该请求。

### consumers属性

consumers 属性用于指定处理请求的提交内容类型（Content-Type），例如：application/json、text/html。如
@RequestMapping(value = “toUser”,consumes = “application/json”)。

### produces属性

produces 属性用于指定返回的内容类型，返回的内容类型必须是 request 请求头（Accept）中所包含的类型。如 @RequestMapping(value = “toUser”,produces = “application/json”)。

除此之外，produces 属性还可以指定返回值的编码。如 @RequestMapping(value = “toUser”,produces = “application/json,charset=utf-8”)，表示返回 utf-8 编码。

使用 @RequestMapping 来完成映射，具体包括 4 个方面的信息项：请求 URL、请求参数、请求方法和请求头。

## @Autowired和@Service

将依赖注入到 Spring MVC 控制器时需要用到 @Autowired 和 @Service 注解。

-   @Autowired 注解位于 org.springframework.beans.factory. annotation 包，可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作。

-   @Service 注解位于 org.springframework.stereotype 包，会将标注类自动注册到 Spring 容器中。


在配置文件中需要添加元素来扫描依赖基本包。

```
<context:component-scan base-package=“org.example.service”/>
```

示例：

User 实体类如下。

```
package org.example.po;
public class User {
    private String name;
    private String pwd;
    /*省略setter和getter方法*/
}
```

新建 org.example.service 包，创建 UserService 接口，代码如下。

```
package org.example.service;
import org.example.po.User;
public interface UserService {
    boolean login(User user);
    boolean register(User user);
}
```

创建 UserServiceImpl 类，实现 UserService 接口，代码如下。

```
package org.example.service;
import org.springframework.stereotype.Service;
import org.example.po.User;
@Service
public class UserServiceImpl implements UserService {
    @Override
    public boolean login(User user) {
        if ("Tom".equals(user.getName()) && "123456".equals(user.getPwd())) {
            return true;
        }
        return false;
    }
    @Override
    public boolean register(User user) {
        if ("Tom".equals(user.getName()) && "123456".equals(user.getPwd())) {
            return true;
        }
        return false;
    }
}
```

为了使类能被 Spring 扫描到，必须为其标注 @Service。

新建 org.example.controller 包，创建 UserController 类，代码如下。

```
package org.example.controller;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.example.po.User;
import org.example.service.UserService;

@Controller
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;
    
    @RequestMapping("/login")
    public String getLogin(Model model) {
        User us = new User();
        us.setName("Tom");
        userService.login(us);
        model.addAttribute("user", us);
        return "login";
    }
    
    @RequestMapping("/register")
    public String getRegister(Model model) {
        User us = new User();
        us.setName("Tom");
        userService.login(us);
        model.addAttribute("user", us);
        return "register";
    }
}
```

在 UserService 上添加 @Autowired 注解会使 UserService 的一个实例被注入到 UserController 实例中。

## @RequestBody

@responseBody注解的作用是将controller的方法返回的对象通过适当的HttpMessageConverter转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据。

>    注意：在使用此注解之后不会再走视图处理器，而是直接将数据写入到输入流中，他的效果等同于通过response对象输出指定格式的数据。

## @Valid

## @PathVariable

@PathVariable用于接收uri地址传过来的参数，Url中可以通过一个或多个{Xxx}占位符映射，通过@PathVariable可以绑定占位符参数到方法参数中，在RestFul接口风格中经常使用。

例如：请求URL：`http://localhost/user/21/张三/query`（Long类型可以根据需求改变为String或int，SpringMVC会自动做转换)

```
@RequestMapping("/user/{userId}/{userName}/query")
public User query(
    @PathVariable("userId") Long userId, 
    @PathVariable("userName") String userName
){

}
```

## @RequestParam

@RequestParam用于将请求参数映射到控制器方法的形参上，有如下三个属性

-   value：参数名。

-   required：是否必需，默认为true，表示请求参数中必须包含该参数，如果不包含抛出异常。
-   defaultValue：默认参数值，如果设置了该值自动将required设置为false，如果参数中没有包含该参数则使用默认值。

示例：@RequestParam(value = “pageNum”, required = false, defaultValue = “1”)

## @PathVariable和@RequestParam的区别

请求路径上有个id的变量值，可以通过@PathVariable来获取 @RequestMapping(value = “/page/{id}”, method = RequestMethod.GET)

@RequestParam用来获得静态的URL请求入参 spring注解时action里用到。

标志参数被Hibernate-Validator校验框架校验。

## @ControllerAdvice

@ControllerAdvice标识一个类是全局异常处理类。

```
@ControllerAdvice
public class ControllerTest {
    //全局异常处理类
}

@ExceptionHandler
@ExceptionHandler标识一个方法为全局异常处理的方法。

@ExceptionHandler
public void ExceptionHandler(){
    //全局异常处理逻辑...
}
```
