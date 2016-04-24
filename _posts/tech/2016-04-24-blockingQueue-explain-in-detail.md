---
layout: post
title: java BlockingQueue分类及说明
category: 笔记
tags: BlockingQueue
description: jdk自带BlockingQueue详解——《java并发编程的艺术》学习笔记
---
## 简介，什么是阻塞队列
简单的说就是支持阻塞的进行插入和移除的队列。当队列满时，阻塞插入操作；当队列为空时阻塞其移除操作。

## 分类
### ArrayBlockingQueue:      
    由数组结构构成的有界阻塞队列，按照FIFO的原则对元素进行排序；
### LinkedBlockingQueue:     
    由链表结构组成的有界阻塞队列，默认和最大长度为Integer.MAX_VALUE。排序原则——FIFO；
### PriorityBlockingQueue:   
    支持优先级排序的无界阻塞队列，默认情况下采用升序进行排序，指定构造函数Comparator对元素进行排序；
### DelayQueue:              
    使用优先级队列实现无界的阻塞队列，队列使用PriorityQueue来实现。队列中的元素必须实现Delayed接口，可指定多久才能从队列中获取当前元素
### SynchronousQueue:        
    不存储元素的阻塞队列；每一个put操作必须等待一个take操作
### LinkedTransferQueue:     
    由链表结构组成的无界阻塞队列；tryTransfer试探生产者传入的元素是否能直接传递给消费者。transfer方法必须等待消费者消费量了才返回；
### LinkedBlockingDeque:     
    由链表结构组成的双向阻塞队列；

        

