# URL映射

## URL路径匹配

### 精确匹配

@RequestMapping的value属性用于匹配URL映射，value支持简单表达式：

```
@RequestMapping("/getDataById/{id}")
public String getDataById(@PathVariable("id") Long id) {
    return "getDataById:"+id ;
}
```

在上面的示例中，@PathVariable注解作用在方法参数中，用于表示参数的值来自URL路径。如果URL中的参数名称与方法中的参数名称一致，则可以简化为：

```
@RequestMapping("/getDataById/{id}")
public String getDataById(@PathVariable Long id) {
    return "getDataById:"+id ;
}
```

在上面的示例中，当在浏览器中访问/getDataById/1时，会自动映射到后台的getDataById方法，传入参数id的值为1。

### 通配符匹配

@RequestMapping支持使用通配符匹配URL，用于统一映射某些URL规则类似的请求，示例代码如下：

```
@RequestMapping("/getJson/*.json")
public String getJson() {
    return "get json data";
}
```

在上面的示例中，当在浏览器中请求/getJson/a.json或者/getJson/b.json时都会匹配到后台的Json方法。

@RequestMapping的通配符匹配非常简单实用，支持`*`、`?` 、`**`等通配符。通配符匹配规则如下：

-   符号“`*`”匹配任意字符，符号“`**`”匹配任意路径，符号“`?`”匹配单个字符。
-   有通配符的优先级低于没有通配符的，比如`/user/add.json`比`/user/*.json`优先匹配。
-   有“`**`”通配符的优先级低于有“`*`”通配符的。

## Method匹配

HTTP请求Method有GET、POST、PUT、DELETE等方式。

HTTP支持的全部Method和说明如表所示。

| HTTP Method | 说明                                                    |
| ----------- | ------------------------------------------------------- |
| GET         | 用于获取URL对应的数据                                   |
| POST        | 用于提交后台数据                                        |
| HEAD        | 类型为GET，不返回消息体，用于返回对应的URL的元数据      |
| PUT         | 类型为POST，对同一个数据进行多次PUT操作不会导致数据改变 |
| DELETE      | 删除操作                                                |
| PATCH       | 类似于PUT，表示消息的局部更新                           |

@RequestMapping注解提供了method参数指定请求的Method类型，包括RequestMethod.GET、RequestMethod.POST、RequestMethod.DELETE、RequestMethod.PUT等值，分别对应HTTP请求的Method。示例代码如下：

```
@RequestMapping(value="/getData",method = RequestMethod.GET)
public String getData() {
    return "RequestMethod GET";
}

@RequestMapping(value="/getData",method = RequestMethod.POST)
public String PostData() {
    return "RequestMethod POST";
}
```

上面的示例实现了GET和POST两种方式。当使用GET方式请求/data/getData接口时，会返回“RequestMethod GET”，使用POST方式请求/data/getData接口时，则返回“RequestMethod POST”，说明@RequestMapping通过HTTP请求Method映射不同的后台方法。

## consumes和produces匹配

@RequestMapping注解提供了consumes和produces参数用于验证HTTP请求的内容类型和返回类型。

-   consumes表示请求的HTTP头的Content-Type媒体类型与consumes的值匹配才可以调用方法。
-   produces表示HTTP请求中的Accept字段只有匹配成功才可以调用。

下面通过示例演示consumes和produces参数的用法。

```
//处理request Content-Type为“application/json”类型的请求
@RequestMapping(value = "/Content", method = RequestMethod.POST, consumes = "application/json")
public String Consumes(
    @RequestBody
    Map param)
{
    return "Consumes POST  Content-Type=application/json";
}
```

上面的示例只允许`Content-Type=application/json`的HTTP请求映射此方法，其他类型则返回“Unsupported MediaType”的错误。

## params和header匹配

@RequestMapping注解提供了header参数和params参数，通过header参数可以根据HTTP请求中的消息头内容映射URL请求，通过params参数可以匹配HTTP中的请求参数实现URL映射。

### params

Spring Boot除了通过匹配URL和Method的方式实现映射HTTP请求之外，还可以通过匹配params的方式来实现。Spring Boot从请求参数或HTTP头中提取参数，通过判断参数，如params="action=save"确定是否通过。同时还可以设置请求参数包含某个参数、不包含某个参数或者参数等于某个值时通过，具体如下：

-   params={"username"}，存在“username”参数时通过。
-   params={"!password"}，不存在“password”参数时通过。
-   params={"age=20"}，参数age等于20时通过。

通过@PostMapping设置的params参数来检查请求的params，实现HTTP的URL映射。示例代码如下：

```
@RequestMapping(value="/paramsTest",params="action=save")
public String paramsTest(@RequestBody Map param){
    return "params test";
}
```

### header

header的使用和params类似，它检查HTTP的header头中是否有Host=localhost:8080的参数，如果有则匹配此方法。示例代码如下：

```
@RequestMapping(value="/headerTest",headers={"Host=localhost:8080"})
public String headerTest()
{
    return "header test";
}
```
