# Thymeleaf的高级用法

## 内联

虽然通过Thymeleaf中的标签属性已经几乎满足了开发中的所有需求，但是有些情况下需要在CSS或JS中访问后台返回的数据。所以Thymeleaf提供了th:inline="text/javascript/none"标签，使用[[…]]内联表达式的方式在HTML、JavaScript、CSS代码块中轻松访问model对象数据。

### 文本内联

Thymeleaf内联表达式使用[[...]]或[(...)]语法表达。先在父级标签定义使用内联方式th:inline="text"，然后在标签内使用[[…]]或[(...)]表达式操作数据对象。文本内联比th:text的代码更简洁。下面通过示例演示内联的使用方式。

首先，创建页面inline.html。示例代码如下：

```
<h1>Thymeleaf模板引擎</h1>
<div>
    <h1>内联</h1>
    <div th:inline="text" >
        <p>Hello, [[${userName}]] !</p>
        <br/>
    </div>
</div>
```

以上代码等价于：

```
<div>
    <h1>不使用内联</h1>
    <p th:text="'Hello, ' + ${userName} + ' !'"></p>
    <br/>
</div>
```

通过以上两个示例可以看出使用内联语法会更简洁一些。

-   th:inline="text"表示使用文本内联方式。
-   任何父标签都可以加上th:inline。
-   [[...]] 等价于th:text结果将被HTML转义，[(...)]等价于th:utext结果不会被HTML转义。

然后，创建后台路由/inline，示例代码如下：

```
@RequestMapping("/inline")
public String inline(ModelMap map) {
    map.addAttribute("userName", "admin");
    return "inline";
}
```

在上面的示例中，后台返回inline.html页面，同时返回userName=admin。

最后，运行测试。

启动项目后，在浏览器中输入地址http://localhost:8080/inline，查看结果。

页面显示后台返回的userName为admin，比之前介绍的th:text=${userName}的方式更加简单、清晰

### 脚本内联

脚本内联，顾名思义就是在JavaScript脚本中使用内联表达式。使用时只需要在<script>标签上加入th:inline="javascript"属性，然后在JavaScript代码块中就能使用[[]]表达式。实现在JavaScript脚本中获取后台传过来的参数。

首先，修改inline.html页面，增加如下脚本：

```
<script th:inline="javascript">
    var name ='hello,'+ [[${userName}]] ;
    alert(name);
</script>
```

在上面的示例中，在<script>标签内加入th:inline="javascript"，表示能在JavaScript中使用[ [] ]取值。在访问页面时，根据后端传值拼接name值，并以alert的方式弹框展示。

然后启动项目，在浏览器中输入地址http://localhost:8080/inline。

显示页面会先弹出一个alert提示框，显示“hello admin”，说明使用脚本内联绑定了后台传过来的数据。

###  样式内联

Thymeleaf还允许在<style>标签中使用内联表达式动态生成CSS属性样式。下面通过示例演示内联CSS样式的用法。

首先，修改inline.html页面，加入如下样式：

```
<style type="text/css" th:inline="css" th:with="color='yellow', fontSize='25px'">
    p {
        color: /*[[${color}]]*/ red;
        font-size: [(${fontSize}) ];
    }
</style>
```

在上面的示例中，与内联JavaScript一样，CSS内联也允许<style>标签静态和动态区分处理，当服务器动态打开时，字体颜色为黄色；当以原型静态打开时，显示的是红色，因为Thymeleaf会自动忽略掉CSS注释之后和分号之前的代码。需要注意的是，在获取变量赋值时，fontSize需要使用[(...)]表示不进行转义，如果使用[[...]]进行了转义，则会导致样式无效。

然后，修改/inline路由，返回fontSize和color，示例代码如下：

```
@RequestMapping("/inline")
public String inline(ModelMap map) {
    map.addAttribute("fontSize", "20px");
    map.addAttribute("color", "yellow");
    map.addAttribute("userName", "admin");

    return "inline";
}
```

在上面的示例中，增加了fontSize和color两个CSS的属性样式，设置fontSize为20px，color为yellow。

然后启动项目，在浏览器中输入地址http://localhost:8080/inline，可以看到结果。

页面显示的字体大小和颜色根据后台返回的数据显示，说明CSS内联生效。

###  禁用内联

Thymeleaf支持使用th:inline =“none”来禁止使用内联。示例代码如下：

```
<body>
<!--/*禁用内联表达式*/-->
<p th:inline="none">[[${info}]]</p>
<!--/*禁用内联表达式*/-->
<p th:inline="none">[[Info]]</p>
</body
```

## 内置对象

Thymeleaf包含一些内置的基本对象，可以用于视图中获取上下文对象、请求参数、Session等信息。这些基本对象使用#开头，如下所示。

-   #ctx：模板引擎的全局上下文对象；
-   #locale：在全局上下文中维护的java.util.Locale对象；
-   #request：表示HttpServletRequest对象，只在Web环境下使用；
-   #response：表示HttpServletResponse对象，只在Web环境下使用；
-   #session：表示HttpSession对象，只在Web环境下使用；
-   #servletContext：表示ServletContext对象，只在Web环境下使用。

如上所示，Thymeleaf提供了一系列的对象和属性用于访问请求参数、会话属性等应用属性。

下面以其中两个常用的对象作为示例来演示。

步骤01 定义后台方法传值。

创建一个后台方法，后台传回request请求参数和session属性，示例代码如下：

```
@RequestMapping("/object")
public String test1(HttpServletRequest request){
    request.setAttribute("request", "spring boot");
    request.getSession().setAttribute("session", "admin session");
    request.getServletContext().setAttribute("servletContext","Thymeleaf servletContext");
    return "baseobject";
}
```

在上面的示例中，我们分别在request和session对象中写入了相关的测试，验证前台是否能获取到这些自定义的Web请求信息。

步骤02 前端页面接收参数。

接下来看看前端页面如何通过Thymeleaf内置的基本对象获取后端传递的值，在/resources目录下新建一个前端页面baseobject.html，示例代码如下：

```
<h1>Thymeleaf模板引擎</h1>
<h3>基本对象</h3>
<p th:text="${#request.getAttribute('request')}"></p>
<p th:text="${#session.getAttribute('session')}"></p>
<p th:text="${#servletContext.getAttribute('servletContext')}"></p>
```

在上面的示例中，我们在HTML页面中通过#request、#session这些对象就能获取Web请求中的相关信息。

步骤03 启动验证。

启动项目后，在浏览器中输入地址http://localhost:8080/object，查看结果。

在HTML页面中，通过#request、#session这些对象成功获取了后台返回的Web请求信息。

## 内嵌变量

为了模板更加易用，Thymeleaf还提供了一系列公共的Utility对象（内置于Context中），可以使用"${#对象.方法(参数列表)}"直接访问。具体的对象如下所示。

-   dates : java.util.Date 的功能方法类
-   numbers : 格式化数字的功能方法
-   strings : 字符串对象的功能类
-   calendars : 类似#dates，面向java.util.Calendar
-   objects: 对objects的功能类操作    