# Thymeleaf表达式

## 变量表达式

变量表达式即获取后台变量的表达式。使用${}获取变量的值，例如：

```
<p th:text="${name}">hello</p>
```

在上面的示例中，通过${name}获取后台返回的model的属性。标签中的th:text属性用来填充该标签的内容。

如果后台返回的是对象，则使用变量名.属性名方式获取，这一点和EL表达式一样。

```
<p th:text="${user.memo}">备注</p>
```

在上面的示例中，使用${user. memo}可以获取model中的user对象的memo属性。

## 选择或星号表达式

选择表达式与变量表达式类似，不过它用一个预先选择的对象来代替上下文变量容器（map）执行*{name}。什么是预先选择的对象？就是父标签的值。示例代码如下：

```
<div th:object="${session.user}">
    <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
    <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
    <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
</div>

// 等价于
<div>
    <p>Name: <span th:text="${session.user.firstName}">Sebastian</span>.</p>
    <p>Surname: <span th:text="${session.user.lastName}">Pepper</span>.</p>
    <p>Nationality: <span th:text="${session.user.nationality}">Saturn</span>.</p>
</div>
```

在上面的示例中，我们用th:object预先定义了对象变量`${session.user}`，然后使用星号（`*`）获取了user变量中的各个属性。例如`*{firstName}`等价于`${session.user.firstName}`。这两种使用方式的区别如下：

-   在不考虑上下文的情况下，两者没有区别，只是星号语法评估在选定对象上表达，而不是整个上下文。
-   美元符号（`$`）和星号（`*`）语法可以混合使用。

## URL表达式

URL在Web应用中占据着十分重要的地位，如引用静态资源文件、处理URL链接等。Thymeleaf通过@{...}语法来处理URL表达式，主要使用`th:href`、`th:src`等属性引用CSS、JS等静态资源文件、下面通过示例演示URL表达式的使用。

### 1. 引入静态资源文件

Thymeleaf页面使用`th:href`属性引入CSS资源文件：

```
<link rel="stylesheet" th:href="@{/resources/css/bootstrap.min.css}" />
```

在上面的示例中，默认访问resources下的css文件夹。

Thymeleaf页面使用`th:src`属性引入JS资源文件：

```
<script th:src="@{/resource/js/bootstrap.min.js}"></script>
```

默认访问resources下的js文件夹。

### 2. 使用 @{...} 设置背景图片

```
<div th:style="'background:url(' + @{${imgurl}} + ');'">
```

上面的路径使用`@{${imgurl}`指定图片的路径，Thymeleaf也通过`th:background`设置背景。

### 3. URL链接

Thymeleaf支持在`<a>`标签中使用`th:href`来处理URL链接：

```
<a th:href="@{http://www.a.com/user/u2376052 }">绝对路径</a>
<a th:href="@{/order}">相对路径</a>
```

在上面的示例中，th:href属性修饰符将计算并替换使用href链接的URL值，并放入href属性中。

同样，th:href也支持URL参数传递，我们可以使用带参数的URL表达式，示例如下：

```
<a th:href="@{/order/details(orderId=${orderId})}">view</a>
```

在上面的示例中，@{...}表达式中通过{orderId}访问上下文中的orderId变量。最后的(orderId=${o.id})表示将括号内的内容作为URL参数处理，该语法避免使用“&”拼接URL参数，大大提高了可读性。

如果需要多个参数，将用逗号分隔，比如：

```
<a th:href="@{/order/process(execId=${execId},execType='FAST')}">view</a>
```

在上面的示例中，使用@{}表达式创建URL，同时传递execId和execType两个参数，比使用“&”拼接更简单易读。

## 文字国际化表达式

文字国际化表达式允许我们从一个外部文件获取区域文字信息，使用类似于#{login.tip}的表达式。下面通过示例演示Thymeleaf实现国际化。

步骤01 创建国际化资源。

Spring Boot支持国际化，我们在resources资源文件目录下新建i18n文件夹，在该文件夹下创建test_zh_CN.properties和test_en_US.properties两个文件（也可以直接创建Resource Bundle文件夹），然后增加测试属性。

在test_en_US.properties添加：

```
test.tip="Hello i18n"
```

在test_zh_CN.properties添加：

```
test.tip="你好 i18n"
```

我们创建了test的国际化资源配置文件，增加了login.tip属性并配置了对应的中英文。

步骤02 修改系统国际化配置。

在application.properties中加入上面定义的国际化配置：

```
spring.messages.basename=i18n.test
```

步骤03 在页面中引用国际化资源。

在resource/templates目录下创建i18n.html页面，示例代码如下：

```
<!DOCTYPE html>
<html lang="zh" xmlns:th="http://www.thymeleaf.org/">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>Thymeleaf模板引擎</h1>
<h2>国际化</h2>
<p th:text="#{test.tip}">Hello i18n</p>
</body>
</html>
```

在上面的示例中，通过文字国际化表达式#{test.tip}获取属性配置。

步骤04 创建后台请求。

在之前的HelloController中加入如下代码：

```
@RequestMapping("/i18n")
public String i18n() {
    return "i18n";
}
```

步骤05 运行测试。

配置完成之后启动项目，在浏览器中访问http://localhost:8080/i18n来验证国际化配置是否生效。

可以看到，通过#{test.tip}表达式，获取到了国际化资源配置中的中文内容。