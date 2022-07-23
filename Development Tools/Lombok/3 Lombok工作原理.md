# Lombok工作原理

Lombok自动生成代码的原理是什么呢？通过` JSR 269: Pluggable Annotation Processing API`(Java 规范提案)


![](assets/Lombok.svg)

Javac 解析成抽象语法树之后(AST)，Lombok 根据自己的注解处理器，动态的修改 AST，增加新的节点(所谓代码)，最终通过分析和生成字节码。
