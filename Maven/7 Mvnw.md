# Mvnw

## 什么是mvnw

mvnw是Maven Wrapper的缩写。因为我们安装Maven时，默认情况下，系统所有项目都会使用全局安装的这个Maven版本。但是，对于某些项目来说，它可能必须使用某个特定的Maven版本，这个时候，就可以使用Maven Wrapper，它可以负责给这个特定的项目安装指定版本的Maven，而其他项目不受影响。

简单地说，Maven Wrapper就是给一个项目提供一个独立的，指定版本的Maven给它使用。

官网：https://maven.apache.org/wrapper/

## mvnw的作用

- 自动寻找系统中的maven环境变量。如果没有找到它就会自动下载maven到一个默认的路径下，之后你就可以运行maven命令了。

- 有时你会碰到一些项目的peoject和你本地的maven不兼容，它会帮你下载合适的maven版本，然后运行。
- 使用Maven Wrapper，可以为一个项目指定特定的Maven版本：把项目的mvnw、mvnw.cmd和.mvn提交到版本库中，可以使所有开发人员使用统一的Maven版本。

总结：mvnw是一个maven wrapper script，它可以让你在没有安装maven或者maven版本不兼容的条件下运行maven的命令。
## mvnw的安装

安装Maven Wrapper最简单的方式是在项目的根目录（即`pom.xml`所在的目录）下运行安装命令：

```bash
mvn wrapper:wrapper
```

它会自动使用最新版本的Maven。

安装后，查看项目结构：

```
my-project
├── .mvn
│   └── wrapper
│       ├── maven-wrapper.jar
│       └── maven-wrapper.properties
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   └── resources
    └── test
        ├── java
        └── resources
```

发现多了`mvnw`、`mvnw.cmd`和`.mvn`目录，`mvnw`文件适用于Linux（bash），`mvnw.cmd`适用于Windows环境。

我们只需要把`mvn`命令改成`mvnw`就可以使用跟项目关联的Maven。例如：

```kotlin
mvnw clean package
```

在Linux或macOS下运行时需要加上`./`：

```kotlin
./mvnw clean package
```

Maven Wrapper的另一个作用是把项目的`mvnw`、`mvnw.cmd`和`.mvn`提交到版本库中，可以使所有开发人员使用统一的Maven版本。

## mvnw的使用

 这使您可以运行Maven项目，而无需安装Maven并将其显示在路径上。如果找不到正确的Maven版本（就我所知，默认情况下在你的用户主目录中），它会下载它。

如果要指定使用的Maven版本，使用下面的安装命令指定版本，例如`3.8.6`：

```ruby
mvn wrapper:wrapper -Dmaven=3.8.6
```

Maven的URL在每个项目中指定`.mvn/wrapper/maven-wrapper.properties`：

```
distributionUrl=https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.zip
```

要更新或更改Maven版本，也可以使用：

```
mvn wrapper:wrapper -Dmaven=3.8.6
```

或者手动修改`.mvn/wrapper/maven-wrapper.properties`
