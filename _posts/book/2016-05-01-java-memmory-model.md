---
layout: post
title: java内存区域
category: 笔记
tags: jvm
description: java内存区域——《深入理解java虚拟机》
---

![](/assets/image/web/java_vm_runtime_memmory.png)
上图所示为java虚拟机运行时数据区：  

|名称       |English                |定义                       |是否线程共享|可能的异常及类型|
|:----------:|:-----------------------:|--------------------------|:---------:|--------------|
|程序计数器   |Program Counter Register  |当前线程所执行的字节码行号指示器      |否|不会发生异常|
|java虚拟机栈 |Java Virtual Machine Stacks|java方法执行的内存模型，用于存储局部变量表、操作数栈、动态链接、方法出口等信息|是|StackOverflowError,OutOfMemmoryError|
|本地方法栈   |Native Method Stack       |为虚拟机使用到的Native方法服务     |是|StackOverflowError,OutOfMemmoryError|
|java堆      |Java Heap                 |java对象实例、数据的内存分配区|是|OutOfMemmoryError|
|方法区       |Method Area               |存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据|是|OutOfMemmoryError|
|运行时常量池  |Runtime Constant Pool    |方法区的一部分，用于存放编译区生成的各种字面量和符号引用|是|OutOfMemmoryError|
