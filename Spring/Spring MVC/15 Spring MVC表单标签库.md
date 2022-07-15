# Spring MVC表单标签库

Spring MVC表单标签是网页的可配置和可重用的构建基块。这些标记为JSP提供了一种开发，读取和维护的简便方法。

Spring MVC表单标记可以看作是具有数据绑定意识的标记，可以将数据自动设置为Java对象/bean并从中检索它。在这里，每个标签都支持与其对应的HTML标签对应物的属性集，从而使标签变得熟悉且易于使用。

参考：

- [Spring MVC 表单标签库](https://blog.csdn.net/qq_45297578/article/details/117284303)

## Spring MVC表单标签的配置

表单标签库位于spring-webmvc.jar下。要启用对表单标签库的支持，需要参考一些配置。因此，在JSP页面的开头添加以下指令:

```
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
```

## Spring MVC表单标签列表

我们来看看一些经常使用的Spring MVC表单标签。

| 表单标签       | 说明                                     |
| -------------- | ---------------------------------------- |
| form: form     | 这是一个包含所有其他表单标签的容器标签。 |
| form: input    | 此标签用于生成文本字段。                 |
| form: radio    | 此标签用于生成单选按钮。                 |
| form:checkbox  | 此标签用于生成复选框。                   |
| form:password  | 此标签用于生成密码输入字段。             |
| form: select   | 此标签用于生成下拉列表。                 |
| form: textarea | 此标签用于生成多行文本字段。             |
| form: hidden   | 此标签用于生成隐藏的输入字段。           |

## 表单标签

Spring MVC表单标签是容器标签。它是一个父标记，其中包含标记库的所有其他标记。该标签生成HTML表单标签，并向内部标签公开绑定路径以进行绑定。

示例：

```
<form:form action="nextFormPath" modelAttribute=?abc?>
```