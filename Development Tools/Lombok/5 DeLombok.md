# DeLombok

`Delombok` 把 `Lombok` 注解实现的类文件转换为不使用 `Lombok` 的 `Java` 源文件。如果是 `src` 整个目录，可以递归的实现转换，`Delombok` 会自动过滤非 `Lombok` 注解的文件进行原样拷贝。

参考：https://www.jianshu.com/p/4a76c5d98966