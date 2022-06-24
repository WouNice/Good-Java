# Pom模型

pom.xml是Maven的核心

```
<?xml version="1.0" encoding="utf-8"?>
 
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/maven-v4_0_0.xsd">  
  <!--父项目的坐标。如果项目中没有规定某个元素的值，
那么父项目中的对应值即为项目的默认值。 
坐标包括group ID，artifact ID和 version。-->  
  <parent> 
    <!--被继承的父项目的构件标识符-->  
    <artifactId/>  
    <!--被继承的父项目的全球唯一标识符-->  
    <groupId/>  
    <!--被继承的父项目的版本-->  
    <version/> 
  </parent>  
  <!--声明项目描述符遵循哪一个POM模型版本。模型本身的版本很少改变，虽然如此，
但它仍然是必不可少的，这是为了当Maven引入了新的特性或者其他模型变更的时候，
确保稳定性。-->  
  <modelVersion>4.0.0</modelVersion>  
  <!--项目的全球唯一标识符，通常使用全限定的包名区分该项目和其他项目。
并且构建时生成的路径也是由此生成， 如com.mycompany.app生成的相对路径为：
/com/mycompany/app-->  
  <groupId>cn.missbe.web</groupId>  
  <!-- 构件的标识符，它和group ID一起唯一标识一个构件。换句话说，
你不能有两个不同的项目拥有同样的artifact ID和groupID；在某个 
特定的group ID下，artifact ID也必须是唯一的。构件是项目产生的或使用的一个东西，
Maven为项目产生的构件包括：JARs，源码，二进制发布和WARs等。-->  
  <artifactId>search-resources</artifactId>  
  <!--项目产生的构件类型，例如jar、war、ear、pom。插件可以创建
他们自己的构件类型，所以前面列的不是全部构件类型-->  
  <packaging>war</packaging>  
  <!--项目当前版本，格式为:主版本.次版本.增量版本-限定版本号-->  
  <version>1.0-SNAPSHOT</version>  
  <!--项目的名称, Maven产生的文档用-->  
  <name>search-resources</name>  
  <!--项目主页的URL, Maven产生的文档用-->  
  <url>http://www.missbe.cn</url>  
  <!-- 项目的详细描述, Maven 产生的文档用。  当这个元素能够用HTML格式描述时
（例如，CDATA中的文本会被解析器忽略，就可以包含HTML标 签）， 
不鼓励使用纯文本描述。如果你需要修改产生的web站点的索引页面，
你应该修改你自己的索引页文件，而不是调整这里的文档。-->  
  <description>A maven project to study maven.</description>  
  <!--描述了这个项目构建环境中的前提条件。-->  
  <prerequisites> 
    <!--构建该项目或使用该插件所需要的Maven的最低版本-->  
    <maven/> 
  </prerequisites>  
  <!--构建项目需要的信息-->  
  <build> 
    <!--该元素设置了项目源码目录，当构建项目的时候，
构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。-->  
    <sourceDirectory/>  
    <!--该元素设置了项目脚本源码目录，该目录和源码目录不同：
绝大多数情况下，该目录下的内容 会被拷贝到输出目录(因为脚本是被解释的，而不是被编译的)。-->  
    <scriptSourceDirectory/>  
    <!--该元素设置了项目单元测试使用的源码目录，当测试项目的时候，
构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。-->  
    <testSourceDirectory/>  
    <!--被编译过的应用程序class文件存放的目录。-->  
    <outputDirectory/>  
    <!--被编译过的测试class文件存放的目录。-->  
    <testOutputDirectory/>  
    <!--使用来自该项目的一系列构建扩展-->  
    <extensions> 
      <!--描述使用到的构建扩展。-->  
      <extension> 
        <!--构建扩展的groupId-->  
        <groupId/>  
        <!--构建扩展的artifactId-->  
        <artifactId/>  
        <!--构建扩展的版本-->  
        <version/> 
      </extension> 
    </extensions>  
    <!--这个元素描述了项目相关的所有资源路径列表，例如和项目相关的属性文件，
这些资源被包含在最终的打包文件里。-->  
    <resources> 
      <!--这个元素描述了项目相关或测试相关的所有资源路径-->  
      <resource> 
        <!-- 描述了资源的目标路径。该路径相对target/classes目录（例如${project.build.outputDirectory}）。举个例 子，如果你想资源在特定的包里(org.apache.maven.messages)，你就必须该元素设置为org/apache/maven /messages。
然而，如果你只是想把资源放到源码目录结构里，就不需要该配置。-->  
        <targetPath/>  
        <!--是否使用参数值代替参数名。参数值取自properties元素或者文件里配置的属性，
文件在filters元素里列出。-->  
        <filtering/>  
        <!--描述存放资源的目录，该路径相对POM路径-->  
        <directory/>  
        <!--包含的模式列表，例如**/*.xml.-->  
        <includes/>  
        <!--排除的模式列表，例如**/*.xml-->  
        <excludes/> 
      </resource> 
    </resources>  
    <!--这个元素描述了单元测试相关的所有资源路径，例如和单元测试相关的属性文件。-->  
    <testResources> 
      <!--这个元素描述了测试相关的所有资源路径，参见build/resources/resource元素的说明-->  
      <testResource> 
        <targetPath/>
        <filtering/>
        <directory/>
        <includes/>
        <excludes/> 
      </testResource> 
    </testResources>  
    <!--构建产生的所有文件存放的目录-->  
    <directory/>  
    <!--产生的构件的文件名，默认值是${artifactId}-${version}。-->  
    <finalName/>  
    <!--当filtering开关打开时，使用到的过滤器属性文件列表-->  
    <filters/>  
    <!--子项目可以引用的默认插件信息。该插件配置项直到被引用时才会被解析或绑定到生命周期。
给定插件的任何本地配置都会覆盖这里的配置-->  
    <pluginManagement> 
      <!--使用的插件列表 。-->  
      <plugins> 
        <!--plugin元素包含描述插件所需要的信息。-->  
        <plugin> 
          <!--插件在仓库里的group ID-->  
          <groupId/>  
          <!--插件在仓库里的artifact ID-->  
          <artifactId/>  
          <!--被使用的插件的版本（或版本范围）-->  
          <version/>  
          <!--是否从该插件下载Maven扩展（例如打包和类型处理器），由于性能原因，
只有在真需要下载时，该元素才被设置成enabled。-->  
          <extensions/>  
          <!--在构建生命周期中执行一组目标的配置。每个目标可能有不同的配置。-->  
          <executions> 
            <!--execution元素包含了插件执行需要的信息-->  
            <execution> 
              <!--执行目标的标识符，用于标识构建过程中的目标，或者匹配继承过程中需要合并的执行目标-->  
              <id/>  
              <!--绑定了目标的构建生命周期阶段，如果省略，目标会被绑定到源数据里配置的默认阶段-->  
              <phase/>  
              <!--配置的执行目标-->  
              <goals/>  
              <!--配置是否被传播到子POM-->  
              <inherited/>  
              <!--作为DOM对象的配置-->  
              <configuration/> 
            </execution> 
          </executions>  
          <!--项目引入插件所需要的额外依赖-->  
          <dependencies> 
            <!--参见dependencies/dependency元素-->  
            <dependency>......</dependency> 
          </dependencies>  
          <!--任何配置是否被传播到子项目-->  
          <inherited/>  
          <!--作为DOM对象的配置-->  
          <configuration/> 
        </plugin> 
      </plugins> 
    </pluginManagement>  
    <!--使用的插件列表-->  
    <plugins> 
      <!--参见build/pluginManagement/plugins/plugin元素-->  
      <plugin> 
        <groupId/>
        <artifactId/>
        <version/>
        <extensions/>  
        <executions> 
          <execution> 
            <id/>
            <phase/>
            <goals/>
            <inherited/>
            <configuration/> 
          </execution> 
        </executions>  
        <dependencies> 
          <!--参见dependencies/dependency元素-->  
          <dependency>......</dependency> 
        </dependencies>  
        <goals/>
        <inherited/>
        <configuration/> 
      </plugin> 
    </plugins> 
  </build>  
  <!--模块（有时称作子项目） 被构建成项目的一部分。
列出的每个模块元素是指向该模块的目录的相对路径-->  
  <modules/>  
  <!--发现依赖和扩展的远程仓库列表。-->  
  <repositories> 
    <!--包含需要连接到远程仓库的信息-->  
    <repository> 
      <!--如何处理远程仓库里发布版本的下载-->  
      <releases> 
        <!--true或者false表示该仓库是否为下载某种类型构件（发布版，快照版）开启。 -->  
        <enabled/>  
        <!--该元素指定更新发生的频率。Maven会比较本地POM和远程POM的时间戳。这里的选项是：always（一直），daily（默认，每日），interval：X（这里X是以分钟为单位的时间间隔），或者never（从不）。-->  
        <updatePolicy/>  
        <!--当Maven验证构件校验文件失败时该怎么做：ignore（忽略），fail（失败），或者warn（警告）。-->  
        <checksumPolicy/> 
      </releases>  
      <!-- 如何处理远程仓库里快照版本的下载。有了releases和snapshots这两组配置，
POM就可以在每个单独的仓库中，为每种类型的构件采取不同的 策略。
例如，可能有人会决定只为开发目的开启对快照版本下载的支持。
参见repositories/repository/releases元素 -->  
      <snapshots> 
        <enabled/>
        <updatePolicy/>
        <checksumPolicy/> 
      </snapshots>  
      <!--远程仓库唯一标识符。可以用来匹配在settings.xml文件里配置的远程仓库-->  
      <id>banseon-repository-proxy</id>  
      <!--远程仓库名称-->  
      <name>banseon-repository-proxy</name>  
      <!--远程仓库URL，按protocol://hostname/path形式-->  
      <url>http://192.168.1.169:9999/repository/</url>  
      <!-- 用于定位和排序构件的仓库布局类型-可以是default（默认）或者legacy（遗留）。Maven 2为其仓库提供了一个默认的布局；然 而，Maven 1.x有一种不同的布局。我们可以使用该元素指定布局是default（默认）还是legacy（遗留）。-->  
      <layout>default</layout> 
    </repository> 
  </repositories>  
  <!--发现插件的远程仓库列表，这些插件用于构建和报表-->  
  <pluginRepositories> 
    <!--包含需要连接到远程插件仓库的信息.参见repositories/repository元素-->  
    <pluginRepository>......</pluginRepository> 
  </pluginRepositories>  
  <!--该元素描述了项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。
它们自动从项目定义的仓库中下载。要获取更多信息，请看项目依赖机制。-->  
  <dependencies> 
    <dependency> 
      <!--依赖的group ID-->  
      <groupId>org.apache.maven</groupId>  
      <!--依赖的artifact ID-->  
      <artifactId>maven-artifact</artifactId>  
      <!--依赖的版本号。 在Maven 2里, 也可以配置成版本号的范围。-->  
      <version>3.8.1</version>  
      <!-- 依赖类型，默认类型是jar。它通常表示依赖的文件的扩展名，但也有例外
。一个类型可以被映射成另外一个扩展名或分类器。类型经常和使用的打包方式对应，
 尽管这也有例外。一些类型的例子：jar，war，ejb-client和test-jar。
如果设置extensions为 true，就可以在 plugin里定义新的类型。所以前面的类型的例子不完整。-->  
      <type>jar</type>  
      <!-- 依赖的分类器。分类器可以区分属于同一个POM，但不同构建方式的构件。
分类器名被附加到文件名的版本号后面。例如，如果你想要构建两个单独的构件成 JAR，
一个使用Java 1.4编译器，另一个使用Java 6编译器，你就可以使用分类器来生成两个单独的JAR构件。-->  
      <classifier/>  
      <!--依赖范围。在项目发布过程中，帮助决定哪些构件被包括进来。欲知详情请参考依赖机制。    
                - compile ：默认范围，用于编译      
                - provided：类似于编译，但支持你期待jdk或者容器提供，类似于classpath      
                - runtime: 在执行时需要使用      
                - test:    用于test任务时使用      
                - system: 需要外在提供相应的元素。通过systemPath来取得      
                - systemPath: 仅用于范围为system。提供相应的路径      
                - optional:   当项目自身被依赖时，标注依赖是否传递。用于连续依赖时使用-->  
      <scope>test</scope>  
      <!--仅供system范围使用。注意，不鼓励使用这个元素，
并且在新的版本中该元素可能被覆盖掉。该元素为依赖规定了文件系统上的路径。
需要绝对路径而不是相对路径。推荐使用属性匹配绝对路径，例如${java.home}。-->  
      <systemPath/>  
      <!--当计算传递依赖时， 从依赖构件列表里，列出被排除的依赖构件集。
即告诉maven你只依赖指定的项目，不依赖项目的依赖。此元素主要用于解决版本冲突问题-->  
      <exclusions> 
        <exclusion> 
          <artifactId>spring-core</artifactId>  
          <groupId>org.springframework</groupId> 
        </exclusion> 
      </exclusions>  
      <!--可选依赖，如果你在项目B中把C依赖声明为可选，你就需要在依赖于B的项目（例如项目A）中显式的引用对C的依赖。可选依赖阻断依赖的传递性。-->  
      <optional>true</optional> 
    </dependency> 
  </dependencies>  
  <!-- 继承自该项目的所有子项目的默认依赖信息。这部分的依赖信息不会被立即解析,
而是当子项目声明一个依赖（必须描述group ID和 artifact ID信息），
如果group ID和artifact ID以外的一些信息没有描述，
则通过group ID和artifact ID 匹配到这里的依赖，并使用这里的依赖信息。-->  
  <dependencyManagement> 
    <dependencies> 
      <!--参见dependencies/dependency元素-->  
      <dependency>......</dependency> 
    </dependencies> 
  </dependencyManagement>  
  <!--项目分发信息，在执行mvn deploy后表示要发布的位置。
有了这些信息就可以把网站部署到远程服务器或者把构件部署到远程仓库。-->  
  <distributionManagement> 
    <!--部署项目产生的构件到远程仓库需要的信息-->  
    <repository> 
      <!--是分配给快照一个唯一的版本号（由时间戳和构建流水号）？
还是每次都使用相同的版本号？参见repositories/repository元素-->  
      <uniqueVersion/>  
      <id>banseon-maven2</id>  
      <name>banseon maven2</name>  
      <url>file://${basedir}/target/deploy</url>  
      <layout/> 
    </repository>  
    <!--构件的快照部署到哪里？如果没有配置该元素，默认部署到repository元素配置的仓库，
参见distributionManagement/repository元素-->  
    <snapshotRepository> 
      <uniqueVersion/>  
      <id>banseon-maven2</id>  
      <name>Banseon-maven2 Snapshot Repository</name>  
      <url>scp://svn.baidu.com/banseon:/usr/local/maven-snapshot</url>  
      <layout/> 
    </snapshotRepository>  
    <!--部署项目的网站需要的信息-->  
    <site> 
      <!--部署位置的唯一标识符，用来匹配站点和settings.xml文件里的配置-->  
      <id>banseon-site</id>  
      <!--部署位置的名称-->  
      <name>business api website</name>  
      <!--部署位置的URL，按protocol://hostname/path形式-->  
      <url>scp://svn.baidu.com/banseon:/var/www/localhost/banseon-web</url> 
    </site>  
    <!--项目下载页面的URL。如果没有该元素，用户应该参考主页。
使用该元素的原因是：帮助定位那些不在仓库里的构件（由于license限制）。-->  
    <downloadUrl/>  
    <!-- 给出该构件在远程仓库的状态。不得在本地项目中设置该元素，
因为这是工具自动更新的。有效的值有：none（默认），
converted（仓库管理员从 Maven 1 POM转换过来），partner（直接从伙伴Maven 2仓库同步过来），deployed（从Maven 2实例部 署），verified（被核实时正确的和最终的）。-->  
    <status/> 
  </distributionManagement>  
  <!--以值替代名称，Properties可以在整个POM中使用，也可以作为触发条件（见settings.xml配置文件里activation元素的说明）。格式是<name>value</name>。-->  
  <properties/> 
</project>
```

## 打包方式

在package时会将项目打包成对应类型

```
<packaging>jar</packaging>
```

-   Jar工程：创建一个 Java Project，在打包时会将项目打成 jar 包
-   War工程：创建一个 Web Project，在打包时会将项目打成 war 包。
-   Pom工程：POM 工程是逻辑工程。用在聚合工程中，或者父级工程用来做 jar 包的版本控制。

## 配置jdk版本

```
<profiles>
  <profile>
    <id>development</id>
    <activation>
      <jdk>17</jdk>
      <activeByDefault>true</activeByDefault>
    </activation>
    <properties>
      <maven.compiler.source>17</maven.compiler.source>
      <maven.compiler.target>17</maven.compiler.target>
      <maven.compiler.compilerVersion>17</maven.compiler.compilerVersion>
    </properties>
  </profile>
</profiles>
```

## 包的引入

### 坐标

在Maven中，坐标是包的唯一标识，Maven通过坐标在仓库中找到项目所需的Jar包。

如下代码中，groupId和artifactId构成了一个Jar包的坐标。

```
<dependency>
   <groupId>cn.missbe.web.search</groupId>
   <artifactId>resource-search</artifactId>
   <packaging>jar</packaging>
   <version>1.0-SNAPSHOT</version>
</dependency>
```

-   groupId:所需Jar包的项目名
-   artifactId:所需Jar包的模块名
-   version:所需Jar包的版本号
-   packaging:打包方式

### 传递依赖

传递依赖：如果我们的项目引用了一个Jar包，而该Jar包又引用了其他Jar包，那么在默认情况下项目编译时，Maven会把直接引用和间接引用的Jar包都下载到本地。

### 依赖冲突

若项目中多个Jar同时引用了相同的Jar时，会产生依赖冲突，但Maven采用了两种避免冲突的策略，因此在Maven中是不存在依赖冲突的。

1.短路优先

```
本项目——>A.jar——>B.jar——>X.jar
本项目——>C.jar——>X.jar
```

若本项目引用了A.jar，A.jar又引用了B.jar，B.jar又引用了X.jar，并且C.jar也引用了X.jar。

在此时，Maven只会引用引用路径最短的Jar。

2.声明优先

若引用路径长度相同时，在pom.xml中谁先被声明，就使用谁。

### 排除依赖

在特定的情况下，你可能想排除一部分依赖，这时你可以使用exclusions。比如说 A 依赖于 B 、C、D。但是你项目中只希望导入 B、C。可能是别的依赖已经导入了 D，或者其他原因，你不需要导入D，这时可以使用exclusions。需要注意的是，exclusion只能排除依赖，不能排除包含。若A 是依赖的 B ，那么可以用exclusion排除依赖；若A 里面中包含 B ，那么即使使用了exclusion，也不能排除。

```
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
	<version>4.0.0</version>
	<exclusions> 
		<exclusion> 
			<groupId>org.springframework</groupId> 
			<artifactId>spring-aop</artifactId> 
		</exclusion>
	</exclusions>
</dependency>
```

### 依赖范围scope

在项目发布过程中，帮助决定哪些构件被包括进来。

-   compile ：这是默认范围。如果没有指定，就会使用该依赖范围。表示该依赖在编译和运行时生效。在项目打包时会将该依赖包含进去。      
-   provided：可以参与编译，测试，运行等周期，但是不会被打包到最终的 artifact 中。典型的例子是 servlet-api ，编译和测试项目的时候需要该依赖，但在项目打包的时候，由于容器已经提供，就不需要 Maven 重复的引入一遍
-   runtime：runtime 范围表明编译时不需要生效，而只在运行时生效。典型的例子是 JDBC 驱动实现，项目主代码的编译只需要 JDK 提供 JDBC 接口，只有在执行测试或者运行项目的时候需要实现上述接口的具体 JDBC 驱动。
-   test:   test 范围表示使用此范围的依赖，只在编译测试代码和运行测试的时候需要，应用的正常运行不需要此类依赖。典型的例子就是 JUnit ，它只有在编译测试代码及运行测试的时候才需要。   
-   system: 如果你有些依赖的 jar 包没有 Maven 坐标，它完全不在 Maven 体系中，这时候你可以把它下载到本地磁盘，然后通过 system 来引用。不推荐使用 system，因为一个项目的 pom.xml 如果使用了 scope 为 system 的 depend 后，会导致传递依赖中断，即所有其他依赖本项目的项目都无法传递依赖了。
-   systemPath: 仅用于范围为system。提供相应的路径      
-   optional:   当项目自身被依赖时，标注依赖是否传递。用于连续依赖时使用

### 依赖管理dependencyManagement

Maven 提供了一个机制来集中管理依赖信息，叫做依赖管理元素dependencyManagement。一般是父工程中使用 dependencyManagement 配合properties 来使用。properties 内可以写任意的 key ，然后 value 就写对应的版本。这样以后直接在父工程中写版本信息，子工程中直接写 groupid、artifictid 就行，不用写 version，方便管理。dependencyManagement 和 dependencies 的区别是： dependencyManagement 不会直接将依赖注入到项目中，只是定义；而 dependencies 会直接将依赖注入到项目中。dependencyManagement 下包含 dependencies 。

## 继承关系

Maven 中的继承跟 Java 中的继承概念一样，需要有父项目以及子项目。如果A工程继承B工程，则代表A工程默认依赖B工程依赖的所有资源，且可以应用B工程中定义的所有资源信息。被继承的工程(B工程)只能是POM工程。

注意：在父项目中放在<dependencyManagement>中的内容时不被子项目继承，不可以直接使用放在<dependencyManagement>中的内容主要目的是进行版本管理，里面的内容在子项目中依赖时坐标只需要填写<group id>和<artifact id>即可。(注意：如果子项目不希望使用父项目的版本，可以明确配置version)

继承的优点:

-   依赖或插件的统一管理（在 parent 中定义，需要变更 dependency 版本时，只需要修改一处）。
-   代码简洁（子model只需要指定 groupId，artifactId 即可）。
-   dependencyManagement 是 “ 按需引入 ” ， 即 子 model 不会继承 parent 中 dependencyManagement 所有预定义的dependency。

如何实现继承？

父pom配置：将需要继承的Jar包的坐标放入标签即可。

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

子pom配置：

```
<parent>
      <groupId>父pom所在项目的groupId</groupId>
      <artifactId>父pom所在项目的artifactId</artifactId>
      <version>父pom所在项目的版本号</version>
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```

### Maven中的多继承

Maven的继承采用的也是单继承，就是一个子工程，只能有一个父工程。但是某些特定情况，我们需要从多个项目中继承依赖时，有两种方案：

①在子工程内使用parent标签配合dependencyManagement标签

②在子工程内直接使用dependencyManagement标签。

>   方式1解释：子工程内的第一个父工程的依赖，仍用parent标签；当你想从第二个项目中引用时，在子工程内添加dependencyManagement，dependencyManagement标签内的dependencies内的dependency可以有多个，需要继承几个父工程，就添加几个dependency标签。dependency内需要添加 type 标记，值为 pom。添加 scope 标记，值为 import。
>
>   方式2解释：直接在子工程内使用dependencyManagement

```
	<parent>
        <groupId>com</groupId>
        <artifactId>parent</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <groupId>com</groupId>
    <artifactId>child</artifactId>
    <version>1.0-SNAPSHOT</version>


<!--    maven要想实现多继承的话，有两种方式：-->
<!--    第一种：(子工程中，同时又parent、dependencyManagement)
            parent标签配合dependencyManagement标签
            parent标签内的依赖，是第一个父工程的依赖；
            dependencyManagement里面的dependency，是第二个父工程的依赖
            dependencyManagement只能有一个，但是他里面的dependency可以是多个，
            需要依赖多个父工程，就创建多个dependency标签即可
        第二种：(子工程中，直接用dependencyManagement，若依赖多个父工程，则创建多个dependency)
            直接用dependencyManagement标签

-->
    <dependencyManagement>
        <dependencies>

<!--     要依赖的第一个父工程       -->
            <dependency>
                <groupId>com</groupId>
                <artifactId>parent_b</artifactId>
                <version>1.0-SNAPSHOT</version>
<!--         下面这俩也是必须的，不能少的。       -->
                <type>pom</type>
                <scope>import</scope>
            </dependency>

<!--            要依赖的，第二个父工程-->


        </dependencies>
    </dependencyManagement>
```

## 聚合

当我们开发的工程拥有2个以上模块的时候，每个模块都是一个独立的功能集合。比如某大学系统中拥有搜索平台，学习平台，考试平台等。开发的时候每个平台都可以独立编译，测试，运行。这个时候我们就需要一个聚合工程。

在创建聚合工程的过程中，总的工程必须是一个POM工程(Maven Project) (聚合项目必须是一个pom类型的项目，jar项目war项目是没有办法做聚合工程的)，各子模块可以是任意类型模块(Maven Module)。

前提：继承。

聚合包含了继承的特性。

聚合时多个项目的本质还是一个项目。这些项目被一个大的父项目包含。且这时父项目类型为pom类型。同时在父项目的pomxml中出现<modules>表示包含的所有子模块。

父工程pom.xml：

```
<groupId>org.example</groupId>
<artifactId>parent</artifactId>
<version>1.0-SNAPSHOT</version>

<modules>
    <module>child</module>
</modules>

<packaging>pom</packaging>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

子模块pom.xml：

```
<parent>
    <groupId>org.example</groupId>
    <artifactId>parent</artifactId>
    <version>1.0-SNAPSHOT</version>
</parent>

<artifactId>child</artifactId>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
    </dependency>
</dependencies>
```

