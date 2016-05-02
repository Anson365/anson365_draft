---
layout: post
title: java虚拟是如何判读实例已废弃
category: 笔记
tags: jvm
description: 对象实例死亡判断方法——《深入理解java虚拟机》
---
java堆内存中存放着java对象的实例，虚拟机在对对象实例进行回收之前的第一步就是判断实例是否已经“死亡”，那么虚拟机是如何判断对象实例的状态的？
   
## 引用算法      
   
### 引用计数法   
  给对象添加一个引用计数器，每当一个地方引用该对象时，计数器加一；引用失效，计数器减一。那么计数器为0时就代表该对象已失去引用，应该被回收。    
  该算法虽然效率很高，但是存在比较明显的问题：如，A、B两对象相互引用，然后A、B实例在程序中并没有任何引用，此时使用该技术方式的GC就无法对此
  处的垃圾实例进行回收。   
   
### 可达性分析算法   
  通过一系列被称为“GC Roots”的对象作为起始点，从这些节点开始往下搜索，搜索所走过的路径称为引用链（Reference Chain），
  一个对象没有到GC Rootsd的任何引用链，则证明此对象是是不可用的。   
     
## 引用分类   
* 强引用 程序在代码之中普遍存在的，类似“Object obj = new Object()” ，只要强引用存在，垃圾收集器永远不会回收被引用的对象    
* 软引用 还有用但是并非必需的对象。在系统即将发生内存溢出前，将会把这些对象列进回收范围中进行二次回收。JDK1.2之后，提供了SoftReference类实现软引用    
* 弱引用 描述非必需的对象，被弱引用关联的对象只能生存到下一次垃圾收集发生之前，此时不论当前内存是否够用。JDK1.2之后，提供了WeakReference类实现引用    
* 虚引用 称为幽灵引用或者幻影引用，虚引用的存在，完全不会对生存时间构成影响，也无法通过虚引用取得一个对象的实例。虚引用的设置使该对象被回收的时候能够
收到一个系统通知。JDK1.2之后，提供了PhantomReference类来实现虚引用。

## 无用类判断条件
* 该类的所有实例都已经被回收
* 加载过该类的ClassLoader已经被回收
* 该类对应的java.lang.Class对象没有在任何地方被引用，无法在任何地方通过 **反射方法对该类进行访问**  
    