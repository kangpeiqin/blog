# JVM
## 简介
Java能够做到一次编写，到处运行，实现跨平台，主要是因为Java虚拟机对
不同平台的不同实现。源文件(.java)文件被编译成字节码文件(.class)后，交友Java虚拟机
来执行
## 运行时内存区域
- 线程私有
程序计数器、虚拟机栈、本地方法栈
- 线程共享
Java堆、方法区
## 垃圾收集算法
标记清除算法
## 类的加载