# Spring MVC传递参数

Controller 接收请求参数的方式主要有以下几种方式：

## 通过实体Bean接收请求参数

适用于 get 和 post 提交请求方式。需要注意，Bean 的属性名称必须与请求参数名称相同。示例代码如下。

```
@RequestMapping("/login")
public String login(User user, Model model) {
    if ("Tom".equals(user.getName()) && "123456".equals(user.getPwd())) {
        model.addAttribute("message", "登录成功");
        return "main"; // 登录成功，跳转到 main.jsp
    } else {
        model.addAttribute("message", "用户名或密码错误");
        return "login";
    }
}
```

## 通过处理方法的形参接收请求参数

通过处理方法的形参接收请求参数就是直接把表单参数写在控制器类相应方法的形参中，即形参名称与请求参数名称完全相同。该接收参数方式适用于 get 和 post 提交请求方式。示例代码如下：

```
@RequestMapping("/login")
public String login(String name, String pwd, Model model) {
    if ("Tom".equals(name) && "123456".equals(pwd)) {
        model.addAttribute("message", "登录成功");
        return "main"; // 登录成功，跳转到 main.jsp
    } else {
        model.addAttribute("message", "用户名或密码错误");
        return "login";
    }
}
```

## 通过HttpServletRequest接收请求参数

通过 HttpServletRequest 接收请求参数适用于 get 和 post 提交请求方式，示例代码如下：

```
@RequestMapping("/login")
public String login(HttpServletRequest request, Model model) {
    String name = request.getParameter("name");
    String pwd = request.getParameter("pwd");
    if ("Tom".equals(name) && "123456".equals(pwd)) {
        model.addAttribute("message", "登录成功");
        return "main"; // 登录成功，跳转到 main.jsp
    } else {
        model.addAttribute("message", "用户名或密码错误");
        return "login";
    }
}
```

## 通过@PathVariable接收URL中的请求参数

通过 @PathVariable 获取 URL 中的参数，示例代码如下。

```
@RequestMapping("/login/{name}/{pwd}")
public String login(@PathVariable String name, @PathVariable String pwd, Model model) {
    if ("Tom".equals(name) && "123456".equals(pwd)) {
        model.addAttribute("message", "登录成功");
        return "main"; // 登录成功，跳转到 main.jsp
    } else {
        model.addAttribute("message", "用户名或密码错误");
        return "login";
    }
}
```

在访问“http://localhost:8080/springMVCDemo02/user/register/Tom/123456”路径时，上述代码会自动将 URL 中的模板变量 {name} 和 {pwd} 绑定到通过 @PathVariable 注解的同名参数上，即 name=Tom、pwd=123456。

## 通过@RequestParam接收请求参数

在方法入参处使用 @RequestParam 注解指定其对应的请求参数。

@RequestParam 有以下三个参数：

-   value：参数名
-   required：是否必须，默认为 true，表示请求中必须包含对应的参数名，若不存在将抛出异常
-   defaultValue：参数默认值

通过 @RequestParam 接收请求参数适用于 get 和 post 提交请求方式，示例代码如下。

```
@RequestMapping("/login")
public String login(@RequestParam String name, @RequestParam String pwd, Model model) {
    if ("Tom".equals(name) && "123456".equals(pwd)) {
        model.addAttribute("message", "登录成功");
        return "main"; // 登录成功，跳转到 main.jsp
    } else {
        model.addAttribute("message", "用户名或密码错误");
        return "login";
    }
}
```

该方式与“通过处理方法的形参接收请求参数”部分的区别如下：当请求参数与接收参数名不一致时，“通过处理方法的形参接收请求参数”不会报 404 错误，而“通过 @RequestParam 接收请求参数”会报 404 错误。

## 通过@ModelAttribute接收请求参数

@ModelAttribute 注解用于将多个请求参数封装到一个实体对象中，从而简化数据绑定流程，而且自动暴露为模型数据，在视图页面展示时使用。

而“通过实体 Bean 接收请求参数”中只是将多个请求参数封装到一个实体对象，并不能暴露为模型数据（需要使用 model.addAttribute 语句才能暴露为模型数据，数据绑定与模型数据展示后面教程中会讲解）。

通过 @ModelAttribute 注解接收请求参数适用于 get 和 post 提交请求方式，示例代码如下。

```
@RequestMapping("/login")
public String login(@ModelAttribute("user") User user, Model model) {
    if ("Tom".equals(name) && "123456".equals(pwd)) {
        model.addAttribute("message", "登录成功");
        return "main"; // 登录成功，跳转到 main.jsp
    } else {
        model.addAttribute("message", "用户名或密码错误");
        return "login";
    }
}
```

## @RequestHeader

 使用@RequestHeader可以获得请求头信息，@RequestHeader注解的属性如下：

-   value：请求头的名称
-   required：是否必须携带此请求头

```
@RequestMapping("/quick17")
@ResponseBody
public void quickMethod17(@RequestHeader(value = "User-Agent",required = false) String headerValue){
    System.out.println(headerValue);
}
```

## @CookieValue

使用@CookieValue可以获得指定Cookie的值

@CookieValue注解的属性如下：

-   value：指定cookie的名称
-   required：是否必须携带此cookie

```
@RequestMapping("/quick18")
@ResponseBody
public void quickMethod18(@CookieValue(value = "JSESSIONID",required = false) String jsessionid){
    System.out.println(jsessionid);
}
```

## 获得Servlet相关API

SpringMVC支持使用原始ServletAPI对象作为控制器方法的参数进行注入，常用的对象如下：

-   HttpServletRequest
-   HttpServletResponse
-   HttpSession

```
@RequestMapping("/quick16")
@ResponseBody
public void quickMethod16(HttpServletRequest request,HttpServletResponse response,HttpSession session){
    System.out.println(request);
    System.out.println(response);
    System.out.println(session);
}
```
