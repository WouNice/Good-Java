# Bean安全

## Bean是否是线程安全？

spring中的bean是ioc中获取的，ioc中的bean是通过扫描配置文件，读取className，通过反射创建而来的。所以说线程是否安全取决于自己写出的bean是否是线程安全的。