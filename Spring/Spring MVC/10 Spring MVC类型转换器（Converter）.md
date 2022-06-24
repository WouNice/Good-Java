# Spring MVC类型转换器（Converter）

Spring MVC 框架的 Converter<S，T> 是一个可以将一种数据类型转换成另一种数据类型的接口，这里 S 表示源类型，T 表示目标类型。

类型转换是在视图与控制器相互传递数据时发生的。

参考：

- https://www.cnblogs.com/YIueil/p/15780708.html

## 内置的类型转换器

在 Spring MVC 框架中，对于常用的数据类型，开发者无须创建自己的类型转换器，因为 Spring MVC 框架有许多内置的类型转换器用于完成常用的类型转换。Spring MVC 框架提供的内置类型转换包括以下几种类型。

### 标量转换器

| 名称                           | 作用                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| StringToBooleanConverter       | String 到 boolean 类型转换                                   |
| ObjectToStringConverter        | Object 到 String 转换，调用 toString 方法转换                |
| StringToNumberConverterFactory | String 到数字转换（例如 Integer、Long 等）                   |
| NumberToNumberConverterFactory | 数字子类型（基本类型）到数字类型（包装类型）转换             |
| StringToCharacterConverter     | String 到 Character 转换，取字符串中的第一个字符             |
| NumberToCharacterConverter     | 数字子类型到 Character 转换                                  |
| CharacterToNumberFactory       | Character 到数字子类型转换                                   |
| StringToEnumConverterFactory   | String 到枚举类型转换，通过 Enum.valueOf 将字符串转换为需要的枚举类型 |
| EnumToStringConverter          | 枚举类型到 String 转换，返回枚举对象的 name 值               |
| StringToLocaleConverter        | String 到 java.util.Locale 转换                              |
| PropertiesToStringConverter    | java.util.Properties 到 String 转换，默认通过 ISO-8859-1 解码 |
| StringToPropertiesConverter    | String 到 java.util.Properties 转换，默认使用 ISO-8859-1 编码 |

### 集合、数组相关转换器

| 名称                            | 作用                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| ArrayToCollectionConverter      | 任意数组到任意集合（List、Set）转换                          |
| CollectionToArrayConverter      | 任意集合到任意数组转换                                       |
| ArrayToArrayConverter           | 任意数组到任意数组转换                                       |
| CollectionToCollectionConverter | 集合之间的类型转换                                           |
| MapToMapConverter               | Map之间的类型转换                                            |
| ArrayToStringConverter          | 任意数组到 String 转换                                       |
| StringToArrayConverter          | 字符串到数组的转换，默认通过“，”分割，且去除字符串两边的空格（trim） |
| ArrayToObjectConverter          | 任意数组到 Object 的转换，如果目标类型和源类型兼容，直接返回源对象；否则返回数组的第一个元素并进行类型转换 |
| ObjectToArrayConverter          | Object 到单元素数组转换                                      |
| CollectionToStringConverter     | 任意集合（List、Set）到 String 转换                          |
| StringToCollectionConverter     | String 到集合（List、Set）转换，默认通过“，”分割，且去除字符串两边的空格（trim） |
| CollectionToObjectConverter     | 任意集合到任意 Object 的转换，如果目标类型和源类型兼容，直接返回源对象；否则返回集合的第一个元素并进行类型转换 |
| ObjectToCollectionConverter     | Object 到单元素集合的类型转换                                |
