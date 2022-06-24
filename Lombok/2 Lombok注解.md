# Lombok注解

## 概览

Lombok使用比较方便简单，只需要添加相应的注解即可，lombok提供的注解有：

|                    注解                     | 描述                                                         |
| :-----------------------------------------: | ------------------------------------------------------------ |
|                    @Data                    | 注解在类上，将类提供的所有属性都添加，包括get、set、equals、hashCode、toString方法 |
|             @NoArgsConstructor              | 注解在类上，为类创建一个无参构造函数                         |
|             @AllArgsConstructor             | 注解在类上，为类创建一个全参构造函数                         |
|                   @Setter                   | 注解在类上，为类所有属性添加set方法；注解在属性上，为该属性提供set方法 |
|                   @Getter                   | 注解在类上，为所有的属性添加get方法；注解在属性上，为该属性提供get方法 |
|                  @ToString                  | 注解在类上，为类提供一个toString方法                         |
|                  @NotNull                   | 在参数中使用时，如果调用时传了null值，就会抛出空指针异常     |
|                @Synchronized                | 用于方法，可以锁定指定的对象，如果不指定，则默认创建一个对象锁定 |
|                    @Log                     | 作用于类，创建一个log属性                                    |
|                  @Builder                   | 使用builder模式创建对象                                      |
|          @Accessors(chain = true)           | 使用链式设置属性，set方法返回的是this对象                    |
|          @RequiredArgsConstructor           | 创建对象, 例: 在class上添加                                  |
| @RequiredArgsConstructor(staticName = "of") | 会创建生成一个静态方法                                       |
|                @UtilityClass                | 工具类                                                       |
|              @ExtensionMethod               | 设置父类                                                     |
|               @FieldDefaults                | 设置属性的使用范围，如private、public等，也可以设置属性是否被final修饰 |
|                  @Cleanup                   | 关闭流、连接点                                               |
|             @EqualsAndHashCode              | 重写equals和hashcode方法                                     |
|                  @toString                  | 创建toString方法                                             |
|                  @Cleanup                   | 用于流等可以不需要关闭使用流对象                             |

## @Getter/@Setter

@Getter 或 @Setter 注解作用在类或字段上，Lombok 会自动生成默认的 getter/setter 方法。

@Getter 也可用于枚举。@Setter 不能用于枚举。

>   作用于类上，只能对非静态成员变量执行。
>
>   如果静态变量需要，对变量单独进行修饰。

使用示例

```
@Getter
@Setter
public class GetterAndSetterDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```
public class GetterAndSetterDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;
    public GetterAndSetterDemo() {
    }
    // 省略其它setter和getter方法
    public String getFirstName() {
        return this.firstName;
    }
    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }
}
```

## 构造方法相关注解

### @NoArgsConstructor

@NoArgsConstructor 注解可以为指定类，生成默认的无参构造函数。如果类中存在 final 字段，则会报编译错误。

-   `access`参数用于指定构造器的访问权限，默认为`AccessLevel.PUBLIC`表示生成public访问权限的构造器。
-   `staticName`参数用于自动生成一个静态的“构造器”工厂，其内部包裹着一个私有的构造器，对外提供创建对象的功能，传入的参数是构造方法的名称。

使用示例

```
@NoArgsConstructor(staticName = "getInstance")
public class NoArgsConstructorDemo {
    private long id;
    private String name;
    private int age;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```
public class NoArgsConstructorDemo {
    private long id;
    private String name;
    private int age;
    private NoArgsConstructorDemo() {
    }
    public static NoArgsConstructorDemo getInstance() {
        return new NoArgsConstructorDemo();
    }
}
```

### @AllArgsConstructor

使用 @AllArgsConstructor 注解可以为指定类，生成包含所有成员的构造函数

使用示例

```
@AllArgsConstructor
public class AllArgsConstructorDemo {
    private long id;
    private String name;
    private int age;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```
public class AllArgsConstructorDemo {
    private long id;
    private String name;
    private int age;
    public AllArgsConstructorDemo(long id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }
}
```

### @RequiredArgsConstructor

@RequiredArgsConstructor 注解作用于类，可以为指定类必需初始化的成员变量，用于生成包含final和@NonNull注解的成员变量的构造方法。当类中没有 final 和 @NonNull 注解的成员变量时会生成一个无参构造方法（因为没有符合要求的参数）。

-   staticName：使生成的构造方法是私有的并且生成一个参数为final变量和@NonNull注解变量，返回类型为当前对象的静态方法，方法名为staticName值。
-   access：设置构造方法的访问修饰符，如果设置了staticName，那么将设置静态工厂方法的访问修饰符的静态工厂方法共有 PUBLIC、MODULE、PROTECTED、PACKAGE、PRIVATE、NONE，MODULE 是 Java 9 的新特性，NONE 表示不生成构造方法也不生成静态方法，即停用注解功能。
-   onConstructor：列出的所有注解都放在生成的构造方法上，JDK 8 之后的写法是 onConstructor_ = {@Deprecated}。

>   所有未初始化的 `final` 字段，以及未初始化的用 `@NonNull` 标记的字段，都会获得一个参数。对于标有 `@NonNull` 的字段，还会生成显式空检查

使用示例

```
@RequiredArgsConstructor
public class RequiredArgsConstructorDemo {
    private final long id;
    private String name;
    private int age;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```
public class RequiredArgsConstructorDemo {
    private final long id;
    private String name;
    private int age;
    public RequiredArgsConstructorDemo(long id) {
        this.id = id;
    }
}
```

## @EqualsAndHashCode

使用位置：类

使用 @EqualsAndHashCode 注解可以为指定类生成 equals 和 hashCode 方法，包括所有非静态变量和非 transient 的变量

-   exclude：字符串数组，表示在生成时排除掉字段名称。
-   of：字符串数组，明确在生成时输出的字段名称。
-   callSuper：默认为false，表示生成时不输出超类中的字段内容。
-   doNotUseGetters：默认为false，表示获取字段值时通过get方法获取，设置为true表示直接通过字段获取。

使用示例

```
@EqualsAndHashCode
public class EqualsAndHashCodeDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```
public class EqualsAndHashCodeDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;
    public EqualsAndHashCodeDemo() {
    }
    public boolean equals(Object o) {
        if (o == this) {
            return true;
        } else if (!(o instanceof EqualsAndHashCodeDemo)) {
            return false;
        } else {
            EqualsAndHashCodeDemo other = (EqualsAndHashCodeDemo)o;
            if (!other.canEqual(this)) {
                return false;
            } else {
              // 已省略大量代码
        }
    }
    public int hashCode() {
        int PRIME = true;
        int result = 1;
        Object $firstName = this.firstName;
        int result = result * 59 + ($firstName == null ? 43 : $firstName.hashCode());
        Object $lastName = this.lastName;
        result = result * 59 + ($lastName == null ? 43 : $lastName.hashCode());
        Object $dateOfBirth = this.dateOfBirth;
        result = result * 59 + ($dateOfBirth == null ? 43 : $dateOfBirth.hashCode());
        return result;
    }
}
```

## @ToString

使用位置：类

@ToString 标注于类之上，用于生成 toString() 方法。

使用示例

```
@ToString(exclude = {"dateOfBirth"})
public class ToStringDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```
public class ToStringDemo {
    String firstName;
    String lastName;
    LocalDate dateOfBirth;
    public ToStringDemo() {
    }
    public String toString() {
        return "ToStringDemo(firstName=" + this.firstName + ", lastName=" +
          this.lastName + ")";
    }
}
```

## @Data

@Data 注解与同时使用以下的注解的效果是一样的：

-   @Getter
-   @Setter
-   @RequiredArgsConstructor
-   @EqualsAndHashCode
-   @ToString

@Data注解在类上，会为类的所有属性自动生成setter/getter、equals、canEqual、hashCode、toString方法，如为final属性，则不会为该属性生成setter方法。

使用示例

```
@Data
public class DataDemo {
    private Long id;
    private String summary;
    private String description;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```
public class DataDemo {
    private Long id;
    private String summary;
    private String description;
    public DataDemo() {
    }
    // 省略summary和description成员属性的setter和getter方法
    public Long getId() {
        return this.id;
    }
 
    public void setId(Long id) {
        this.id = id;
    }
    public boolean equals(Object o) {
        if (o == this) {
            return true;
        } else if (!(o instanceof DataDemo)) {
            return false;
        } else {
            DataDemo other = (DataDemo)o;
            if (!other.canEqual(this)) {
                return false;
            } else {
               // 已省略大量代码
            }
        }
    }
    protected boolean canEqual(Object other) {
        return other instanceof DataDemo;
    }
    public int hashCode() {
        int PRIME = true;
        int result = 1;
        Object $id = this.getId();
        int result = result * 59 + ($id == null ? 43 : $id.hashCode());
        Object $summary = this.getSummary();
        result = result * 59 + ($summary == null ? 43 : $summary.hashCode());
        Object $description = this.getDescription();
        result = result * 59 + ($description == null ? 43 : $description.hashCode());
        return result;
    }
    public String toString() {
        return "DataDemo(id=" + this.getId() + ", summary=" + this.getSummary() + ", description=" + this.getDescription() + ")";
    }
}
```

## @Value

@Value注解作用于类上，和@Data类似，区别在于它会把所有成员变量默认定义为private final修饰，并且不会生成set方法。

@Value注解等于同时加了以下注解

-   @Getter (注意没有@setter)
-   @ToString
-   @EqualsAndHashCode
-   @RequiredArgsConstructor

## @Synchronized

@Synchronized 是同步方法修饰符的更安全的变体。与 synchronized 一样，该注解只能应用在静态和实例方法上。它的操作类似于 synchronized 关键字，但是它锁定在不同的对象上。synchronized 关键字应用在实例方法时，锁定的是 this 对象，而应用在静态方法上锁定的是类对象。

使用示例：

```
public class SynchronizedDemo {
    private final Object readLock = new Object();
    @Synchronized
    public static void hello() {
        System.out.println("world");
    }
    @Synchronized
    public int answerToLife() {
        return 42;
    }
    @Synchronized("readLock")
    public void foo() {
        System.out.println("bar");
    }
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```
public class SynchronizedDemo {
    private static final Object $LOCK = new Object[0];
    private final Object $lock = new Object[0];
    private final Object readLock = new Object();
    public SynchronizedDemo() {
    }
    public static void hello() {
        synchronized($LOCK) {
            System.out.println("world");
        }
    }
    public int answerToLife() {
        synchronized(this.$lock) {
            return 42;
        }
    }
    public void foo() {
        synchronized(this.readLock) {
            System.out.println("bar");
        }
    }
}
```

## @NonNull

你可以在方法或构造函数的参数上使用 @NonNull 注解，它将会为你自动生成非空校验语句。

使用位置：字段、方法、参数列表

使用示例

```
public class NonNullDemo {
    @Getter
    @Setter
    @NonNull
    private String name;
}
```

以上代码经过 Lombok 编译后，会生成如下代码：

```
public class NonNullDemo {
    @NonNull
    private String name;
    public NonNullDemo() {
    }
    @NonNull
    public String getName() {
        return this.name;
    }
    public void setName(@NonNull String name) {
        if (name == null) {
            throw new NullPointerException("name is marked non-null but is null");
        } else {
            this.name = name;
        }
    }
}
```
