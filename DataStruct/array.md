# 数组

数组是一种线型表数据结构，它用一组连续的内存空间来存储一组具有相同类型的数据

线型表：数据排成一条线一样的结构，每个线型表上的数据最多只有前和后两个方向

数组、链表、队列、栈等都是线型表结构

![](../Picture/DataStruct/array/01.jpg)

非线性表：二叉树、堆、图等，数据之间并不是简单的前后关系
  
![](../Picture/DataStruct/array/02.jpg)

数组的连续内存空间和相同的数据类型，使得它可以随机访问；但为了保证连续性，就需要做大量的数据搬移工作
  
数组动态扩容，当数组空间不够用时，我们就重新申请一块更大的内存，将原来数组中数据全部拷贝过去，从而实现动态扩容的