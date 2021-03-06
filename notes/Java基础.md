<p> <a href="../README.md">返回首页</a></p>

# 基础

[Java I/O](#IO)  [泛型](#泛型)   [并发](#并发)  [反射](#反射)   [注解](#注解)   [类加载](#类加载) [正则表达式](#正则表达式) [函数式编程](#函数式编程)

## I/O
I/O本质是将什么样的数据写到什么地方。所以传输数据的格式和传输数据的方式会影响的I/O的效率。Java的I/O操作类在包java.io下
> 传输数据的数据格式
- 基于字节：InputStream和OutputStream
- 基于字符：Writer和Reader
> 传输数据的方式
- 基于磁盘：File
- 基于网络：Socket
### Q&A
#### 为什么有操作字符的I/O接口？
以InputStream/OutputStream为基类的流基本都是**以二进制形式处理数据的，不能够方便地处理文本文件**，没有编码的概念，能够方便地按字符处理文本数据的基类是Reader和Writer
### 序列化和反序列化
序列化就是将内存中的Java对象持久保存到一个流中，反序列化就是从流中恢复Java对象到内存。序列化和反序列化主要有两个用处：一是对象状态持久化，二是网络远程调用，用于传递和返回对象。
- Java主要通过接口Serializable和类ObjectInputStream/ObjectOutputStream提供对序列化的支持
- 缺点：列化后的形式比较大、浪费空间，序列化/反序列化的性能也比较低，是Java特有的技术，不能与其他语言交

1、将字段声明为transient，默认序列化机制将忽略该字段，不会进行保存和恢复

2、定义版本号：在序列化时，会将该版本号写入流，在反序列化时，会将流中的值与类定义中的版本号进行比较，如果不匹配，会抛出InvalidClassException

- 其他方式：文本格式：ⅩML和JSON、二进制：ProtoBuf、Thrift、MessagePack
> 更多可以参考<a href="https://www.cyc2018.xyz/Java/Java%20IO.html#%E4%B8%80%E3%80%81%E6%A6%82%E8%A7%88" target="view_window">Java I/O</a>或者<a href="https://snailclimb.gitee.io/javaguide/#/docs/java/basis/Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86?id=io-%e6%b5%81" target="view_window">I/O流</a>
## 静态域与静态方法
- 静态域：static修饰，每个类中只有一个这样的域。属于类(所有的对象共享)，而不属于任何独立的对象。
- 实例域：每一个对象对于所有的实例域都有自己的一份拷贝
- 静态方法：静态方法是一种不能向对象实施操作的方法，可以认为静态方法是没有this参数的方法。使用：(1)一个方法不需要访问对象状态，其所需参数都是通过显式参数提供（例如：Math.pow）
(2)一个方法只需要访问类的静态域（例如：Employee.getNextId）

## 泛型
**类型参数化**，处理的数据类型不是固定的，而是可以作为参数传入。代码与它们能够操作的数据类型不再绑定在一起，同一套代码可以用于多种数据类型
- 作用：复用代码，降低耦合，保证类型安全，泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。
- 原理：类型擦除：Java 在编译期间，所有的泛型信息都会被擦掉，替换成Object类型
- 使用：泛型类、泛型接口、泛型方法。
## 并发
线程表示一条单独的执行流，它有自己的程序执行计数器，有自己的栈(虚拟机栈、本地方法栈)。
### Q&A
#### 为什么调用的是start，执行的却是run方法
start表示启动该线程，使其成为一条单独的执行流，操作系统会分配线程相关的资源，每个线程会有单独的程序执行计数器和栈，操作系统会把这个线程作为一个独立的个体进行调度，分配时间片让它执行，执行的起点就是run方法
## 反射
在运行时，而非编译时，动态获取类型的信息，比如接口信息、成员信息、方法信息、构造方法信息等，根据这些动态获取到的信息创建对象、访问/修改成员、调用方法
### Class类
每个已加载的类在内存都有一份类信息，每个对象都有指向它所属类信息的引用。类信息对应的类：`java.lang.Class`，所有类的根父类Object有一个方法，可以获取对象的Class对象
- 根据类名直接加载Class，获取Class对象`Class<?> cls = Class.forName("java.util.HashMap") `
- 通过Class对象可以获取名称信息、字段信息(Field)、方法信息(Method)、创建对象和构造方法(Constructor)、类型信息、声明信息
### 缺点
- 反射更容易出现运行时错误，使用显式的类和接口，编译器能帮我们做类型检查，但使用反射，类型是运行时才知道的
- 反射的性能要低一些，在访问字段、调用方法前，反射先要查找对应的Field/Method，要慢一些。
## 注解
注解是给程序添加一些信息，这些信息用于修饰它后面紧挨着的其他代码元素，比如类、接口、字段、方法、方法中的参数、构造方法等。注解可以被编译器、程序运行时和其他工具使用，用于增强或修改程序行为
### 内置注解
@Override、@Deprecated、@SuppressWarnings(都是给编译器用的，所以@Retention都是Retention-Policy.SOURCE)
### 声明式风格
使用声明式风格使得程序员可以在更高的抽象层次上思考和解决问题，而不是陷于底层的细节实现
>声明关键字和语法本身。系统/框架/库，它们负责解释、执行声明式的语句。应用程序，使用声明式风格写程序。
### 注解的创建
- @Target表示注解的目标，如果没有声明@Target，默认为适用于所有类型
- @Retention表示注解信息保留到什么时候
- 注解内参数类型：合法的类型有基本类型、String、Class、枚举、注解，以及这些类型的数组。参数定义时可以使用default指定一个默认值
### 查看注解的信息
@Retention为RetentionPolicy.RUNTIME的注解，可以利用反射机制在运行时进行查看和利用这些信息。
- Class、Field、Method、Constructor类中都有获取注解(getAnnotations、getParameterAnnotations)、判断是否具有指定注解(isAnnotationPresent)的方法
## 类加载
类加载器ClassLoader就是加载其他类的类，它负责将字节码文件加载到内存，创建Class对象
### 应用场景
- 热部署。在不重启Java程序的情况下，动态替换类的实现。
- 应用的模块化和相互隔离。不同的ClassLoader可以加载相同的类但互相隔离、互不影响。
- 从不同地方灵活加载。系统默认的ClassLoader一般从本地的．class文件或jar文件中加载字节码文件，通过自定义的ClassLoader，我们可以从共享的Web服务器、数据库、缓存服务器等其他地方加载字节码文件
### 类加载器种类
- 引导类加载器（Bootstrap ClassLoader）：这个加载器是Java虚拟机实现的一部分，一般是C语言实现的，它负责加载Java的基础类，主要是<JAVA_HOME>/lib/rt.jar，日常用的Java类库比如String、ArrayList等都位于该包内
- 扩展类加载器（Extension ClassLoader）：这个加载器的实现类是sun.misc.Laun-cher$ExtClassLoader，它负责加载Java的一些扩展类，一般是<JAVA_HOME>/lib/ext目录中的jar包
- 系统类加载器（Application ClassLoader）：这个加载器的实现类是sun.misc. Launcher$AppClassLoader，它负责加载应用程序的类，包括自己写的和引入的第三方法类库，即所有在类路径中指定的类。
### 类加载的流程
负责加载类的类就是类加载器，它的输入是完全限定的类名，输出是Class对象
- 1、判断是否已经加载过了，加载过了，直接返回Class对象，一个类只会被一个Class-Loader加载一次。
- 2、如果没有被加载，先让父ClassLoader去加载，如果加载成功，返回得到的Class对象。
- 3、在父ClassLoader没有加载成功的前提下，自己尝试加载类。
### Q&A
“双亲委派”模型
- 优先让父ClassLoader去加载，可以避免Java类库被覆盖的问题。例如，当要求系统类加载器(Application ClassLoader)加载一个系统类（比如，java.util.ArrayList）时，它首先要求扩展类加载器(Extension ClassLoader)进行加载，该扩展类加载器则首先要求引导类加载器(Bootstrap ClassLoader)进行加载。
## 正则表达式
正则表达式是一串字符，它描述了一个文本模式，利用它可以方便地处理文本，包括文本的查找、替换、验证、切分等。
## 函数式编程
Lambda表达式：一种紧凑的传递代码的方式。针对常见的集合数据处理，Java 8引入了一套新的类库，位于包java.util.stream下，称为Stream API。