---
layout: post
title: java垃圾回收
category: 笔记
tags: jvm
description: 垃圾收集——《深入理解java虚拟机》
---
## 算法    
|名称                   |详情                   |优缺点                   |    
|:--------------------:|-----------------------|:-----------------------|    
|标记-清除（Mark-Sweep) |首先标记出所有需要回收的对象，标记完成后统一回收所有被标记的对象|1.效率不高；2.造成内存空间不连续|    
|复制算法（Copying）    |可用内存按容量大小化为相等的两块，当其中一块使用完后，将存活的对象复制到另外一块，然后把使用的内存空间清空|1.无内存碎片；2.内存利用率不高，只有一半的可用空间;3.对象存活率较高的是时候，复制内容多，效率低下|    
|标记-整理（Mark-Compact）|标记过程相同，然后让存活的对象向一端移动，再然后清除点边界以外的内存|1.提高了对象存活率较高时的效率问题|    
|分代收集算法（Generational Collection)|根据对象存活周期的不同分为新生代（复制算法），老年代（标记-整理算法），然后采用适当的收集算法|           |      
    
注: 新生代：刚创建的对象，对象存活率不高；老年代：使用一段时间的对象，对象存活率较高。    
    
## 收集器   
|名称                 |线程数            |优缺点               |其他           |可用调节指令         |    
|:-------------------:|:---------------:|--------------------|---------------|------------------|    
|Serial收集器          |单线程            |1.收集时需暂停所有线程；2.简单高效。|用在Client模式下的虚拟机|-XX:SurvivorRatio,-XX:PretenureSizeThreshold,-XX:HandlePromotionFailure|    
|ParNew收集器          |多线程            |Serial的多线程版本               |单CPU下效率不及Serial|同上，-XX:ParallelGCThreads|    
|Parallel Scavenge收集器|多线程           |利用可控吞吐量控制收集时的线程停顿时间|使用复制算法|-XX:MaxGCPauseMillis,-XX:GCTimeRatio,-XX:+UserAdaptiveSizePolicy,-XX:PretenureSizeThreshold|    
|Serial Old收集器      |单线程            |                         |使用标记-整理法，用在Client模式下的虚拟机，作为CMS的后备预案|  |
|Parallel Old收集器    |多线程            |ParNew收集器的老年代版本|使用标记整理法|       |    
|CMS(Concurrent Mark Sweep)收集器|多线程  |获取最短停顿时间为目标的收集器，对CPU资源非常敏感，无法处理浮动垃圾，收集后产生空间碎片|分为初始标记，并发标记，重新标记，并发清除四个步骤使用标记-清除算法|-XX:CMSInitiatingOccupancyFraction(提高触发百分百)，-XX:CMSCompactAtFullCollection(控制FullGC开关)，-XX:CMSFullGCsBeforeCompaction(控制带压缩的FullGC间隔）|    
|G1收集器              |多线程            |并发与并行，分代收集，空间整合，可预测的停顿|使用标记-整理法，分为初始标记，并发标记，最终标记，筛选回收几个步骤|   |    
   
## 内存分配与回收策略   
1.对象优先分配到新生代的Eden区 一般情况下，当Eden区没有足够的空间时，主动触发一次Minor GC;
2.大对象直接进入老年代 虚拟机提供-XX:PretenureSizeThreshold参数设置临界值；
3.长期存活的对象将进入老年代 对象如果在Eden出生并经历过一次Minor GC后任然存活，并被Survivor容纳，将移动到Survivor空间中，并且对象年龄设为1。对象每经历过一次Minor GC，年龄就增加1。超过阈值就晋升为老年代。通过参数-XX:MaxTenuringThreshold设置阈值。

## Sun JDK监控和故障处理工具  
|名称             |英文全称        |主要作用               |    
|:--------------:|---------------|----------------------|    
|jps             |JVM Process Status Tool|显示指定系统内所欲偶的HotSpot虚拟机进程|    
|jstat           |JVM Statistics Monitoring Tool|用于手机HotSpot虚拟机各方面的运行数据|    
|jinfo           |Configuration Info for Java|显示虚拟机配置信息|    
|jmap            |Memory Map for Java |生成虚拟机的内存转储快照（heapdump 文件）|    
|jhat            |JVM Heap Dump Browser|用户分析heapdump文件，建立一个HTTP/HTML服务器，让用户可以在浏览器上查看分析结果|    
|jstack          |Stack Trace for Java|显示虚拟机的线程快照|    
|JConsole        |Java Monitoring and Management Console|java监视与管理工具|    
|VisualVM        |All-in-one Java Troubleshooting Tool|多合一故障处理工具|    
