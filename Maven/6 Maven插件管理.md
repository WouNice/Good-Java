# Maven插件管理

## 内置插件

Maven有很多内置插件，每一个内置插件都代表了 Maven 的一种行为。Maven 在管理项目整个生命周期时，在不同的阶段处理的过程都是使用插件来具体完成。如：构建项目时使用构建插件、编译项目时使用编译插件、清除构建使用清除构建的插件、测试项目时使用测试插件、打包时使用资源拷贝插件以及打包插件。 我们可以在不同阶段使用 Maven 中的不同命令来触发不同的插件来执行不同的工作。换言之，Maven 的插件是需要依赖命令来执行的。

Maven 在管理插件时也是通过坐标的概念和管理依赖的方式相同，通过坐标来定位唯一的一个插件。

### 编译版本插件

#### 在settings.xml中配置

Maven的settings.xml，是统一的配置，位置在你的Maven安装位置，或者是idea中内置的Maven的settings.xml的位置。

```
<profile> 
	<!-- 定义的编译器插件 ID，全局唯一 --> 
	<id>jdk-1.8</id> 
	<!-- 插件标记，activeByDefault 默认编译器，jdk 提供编译器版本 --> 
	<activation> 
		<activeByDefault>true</activeByDefault>
		<jdk>1.8</jdk> 
	</activation>
	<!-- 配置信息 source-源信息，target-字节码信息，compilerVersion-编译过程版 本 --> 
	<properties> 
		<maven.compiler.source>1.8</maven.compiler.source> 
		<maven.compiler.target>1.8</maven.compiler.target> 
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion> 
	</properties> 
</profile>
```

#### 在pom.xml中配置

在 build 标签下配置 plugins 标签，piugins 标签内可以有多个 plugin 标签，一个 plugin 标签就代表着一个插件，plugin 标签内部的 groupId、artifactId、versioin 代表着这个插件的坐标，configuration 代表着是对这个插件的配置，只针对于当前plugin的配置。

```
<!-- 在pom。xml当中配置局部插件-->
<build>
    <plugins>
        <!-- 配置编译插件-->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <!-- 这个configuration代表着，是给这个plugin的配置-->
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins> 
</build>
```

### 资源拷贝插件

Maven 在打包时，默认只将 src/main/resources 下的文件拷贝到项目中并做打包处理，而非resources目录下的文件在打包时并不会添加到项目中。在某些情况下(比如说Mybatis的mapper文件)，当我们希望将 src/main/java 下的某些文件也一起打包时，可以配置资源拷贝插件，指定打包时一起打包的文件的位置。值得注意的是，若你不专门配置资源拷贝插件，则默认将src/main/resources 下的文件打包；若你配置了资源路径，则只打包你配置的路径，而不打包默认的 src/main/resources 包下的文件。

在 build 标签下配置 resources 标签，一个 resources 标签中可以有多个 resource 标签，resource 标签下的 directory 标签表示是哪个文件夹，includes 标签下可以有多个 include 标签， include 表示哪些文件需要被打包处理。

```
<!--    在pom.xml当中配置局部插件-->
    <build>
<!--        资源拷贝插件，当你不配置时，打包install时默认将resources下的文件跟着打包；
            但是你一旦配置了，就按你配置的资源路径打包，不在将默认的resources下的文件打包了
            就跟构造方法一样，你不写，他给你默认提供一个；你写了，他就用你的，不用默认的。
            一个resources下可以有多个resource标签
-->
        <resources>
<!--            这个是配置的java下的xml文件能被打包-->
            <resource>
                <directory>src/main/java</directory>
<!--                这个意思是，将src/main/java目录下的全部的子文件的。xml。properties打包-->
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>

<!--            这个是将默认的resource目录下的文件能被打包-->
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>

    </build>
```

## 插件管理

类似于依赖的父子关系 dependencyManagement、dependency 。插件也有对应的父子关系 pluginManagement、plugin。

父工程中：

```
<pluginManagement> 
	<plugins> 
		<plugin> 
			<groupId>org.apache.tomcat.maven</groupId> 
			<artifactId>tomcat7-maven-plugin</artifactId> 
			<version>2.2</version> 
		</plugin> 	
	</plugins> 
</pluginManagement>
```

子工程中：

```
<plugin> 
	<groupId>org.apache.tomcat.maven</groupId> 
	<artifactId>tomcat7-maven-plugin</artifactId> 
	<configuration> 
		<!-- 配置 Tomcat 监听端口 --> 
		<port>8080</port> 
		<!-- 配置项目的访问路径(Application Context) --> 
		<path>/</path> 
	</configuration> 
</plugin>
```
