# Maven基本命令

## clean

清除已编译信息，删除工程中的 target 目录

## validate

验证项目是否正确

## compile

将java源文件编译成class文件 ，javac 命令

## test

用于执行项目的测试。如果在 test 目录下含有测试代码，那么 Maven 在执行 install 命令会先去执行 test 命令将所有的 test 目录下的测试代码执行一遍，如果测试代码执行失败，那么 instatll 命令将会终止。

## package

打包。包含编译、打包两个功能。

## verify

运行任何检查，验证包是否有效且达到质量标准。

## install

本地安装，包含编译、打包、安装到本地仓库。

-   编译 - javac
-   打包 - jar ，将 java 代码打包为 jar 文件
-   安装到本地仓库 - 将打包的 jar 文件，保存到本地仓库目录中。

## site

项目站点文档创建的处理，该命令需要配置插件。

## deploy

远程部署命令。