# Bean对象的获取

在容器的顶级接口 BeanFactory接口中，定义了如下几个方法，用于获取 bean 实例

-   `Object getbean(String name) throws beansException;`：通过 bean name 获取 bean 实例
-   `<T> T getBean(Class<T> requiredType) throws BeansException;`：通过 bean class 获取 bean 实例
-   `<T> T getBean(String name, Class<T> requiredType) throws BeansException;`：通过 bean name 和 bean class 获取 bean 实例
