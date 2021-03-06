<p> <a href="../README.md">返回首页</a></p>

# JVM
## 简介
Java能够做到一次编写，到处运行，实现跨平台，主要是因为Java虚拟机对不同平台的不同实现。源文件(.java)文件被编译成字节码文件(.class)后，交由JVM来执行。
JVM会自动管理内存，不需要程序员手动管理内存。
## 运行时内存区域
Java虚拟机在执行Java程序的过程中会把它所管理的内存划分为不同的数据区域
### 线程私有
- 程序计数器：当前线程所执行的字节码的行号指示器。字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等基础功能
- 虚拟机栈：描述Java方法执行的内存模型：每个方法在执行的同时都会创建一个栈帧用于存储局部变量表、操作数栈、动态链接、方法出口等信息。每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。
- 本地方法栈：为虚拟机执行Native方法服务
### 线程共享
- Java堆：存放对象实例，几乎所有的对象实例都在这里分配内存。Java堆是垃圾收集器管理的主要区域，因此很多时候也被称做“GC堆”
- 方法区(“永久代”)：它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据
## 垃圾收集算法
- 标记-清除算法：标记出所有需要回收的对象，在标记完成后统一回收所有被标记的对象
- 复制算法：将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后再把已使用过的内存空间一次清理掉
- 标记-整理算法：与“标记-清除”算法一样，但后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存，
- 分代收集算法：把Java堆分为新生代和老年代，根据各个年代的特点采用最适当的收集算法