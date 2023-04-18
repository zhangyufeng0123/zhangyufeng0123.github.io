---
title: Java 运行机制
categories:
  - Java基础
tags:
  - Java
abbrlink: 840dc5ee
date: 2023-03-20 16:16:26
---
# Java运行机制

## Java运行的一般步骤是

1. Java首先利用文本编辑器编写Java源程序，源文件的后缀名为.java;
2. 再利用编译器（javac）将源程序编译成字节码，字节码文件的后缀名为.class;
3. 最后利用虚拟机（解释器，Java）解释执行。如下图所示

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230320160450.png)

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230320160702.png)

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230320160716.png)

## JVM、JRE和JDK三者的不同

**JVM**(Java Virtual Machine):用于执行bytecode字节码的“虚拟计算机”；

不同的操作系统有不同版本的JVM，屏蔽了底层运行平台的差别，是实现跨平台的核心。

**JRE**(Java Runtime Environment),它包含了Java虚拟机(JVM)、库函数等。

**JDK**(Java Development Kit)包含了JRE，编译器和调试器等。

![](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/20230320161326.png)