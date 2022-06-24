# Bean的定义

在Spring中，构成应用程序主干并由 Spring IoC 容器管理的对象称为 Bean，bean是一个由Spring IoC容器实例化、组装和管理的对象，其根据 Spring 配置文件中的信息创建。

我们可以把 Spring IoC 容器看作是一个大工厂，Bean 相当于工厂的产品。如果希望这个大工厂生产和管理 Bean，就需要告诉容器需要哪些 Bean，以哪种方式装配。

JavaBean和SpringBean和对象的区别？

对于JavaBean，简单理解就是Java类。Javabean是由自己实例化出来的，而springbean是由spring Ioc容器创建出来的。