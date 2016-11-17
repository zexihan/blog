---
layout:     post
title:      "<>DS - Thread Safe Lists"
subtitle:   "DS Note"
date:       2016-11-20
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Algorithms
    - Data Structure
---



## Thread Safe Lists

现在我们介绍了ArrayList和LinkedList两种实现List的方法，我们可以把它们都想象为箱子，只是用不同的方法存储顺序信息。那么，如果操作箱子的不是一个工人，而是两个工人，每个工人只知道自己上次操作时箱子的情况，这两种数据结构还能发挥它本来的作用吗？

比如说，如果一个工人想要拿走箱子A，但是另一个工人在他到来之前，已经把箱子A拿走了，他就找不到箱子A了。或者，如果箱子A是第7个箱子，他仍然能找到第7个箱子，但它已经不是箱子A，而是另一个箱子了。

很显然，我们介绍的两种List都无法解决这个问题。第二个工人可能会一脸茫然，也可能搬走错误的箱子。许多现代计算机语言使用多线程(multi-threading)的方式，来增加处理任务的效率，就像我们使用很多个工人搬箱子一样，很容易引起冲突问题。一种解决这个问题的方法，是对操作”上锁”。对于某一项操作，比如insertBefore，在完成之前，其他工人不可以动这些箱子。完成之后，数据结构更新，其他工人再访问这些箱子时，就知道了新的情况。这样做降低了工作的效率，但保证了工作的安全。经过这样处理的数据结构，就是线程安全的——多线程工作时可以照常使用这些数据结构而不发生冲突。【操作上锁还是整个结构都上锁？】

Java为我们提供了两个方便的机制，提供线程安全的List。 

1.	Collections.synchronizedList 
Returns a synchronized (thread-safe) list backed by the specified list. In order to guarantee serial access, it is critical that all access to the backing list is accomplished through the returned list.

```java
List list = Collections.synchronizedList(new LinkedList(...));
```

2.	Vector怎么实现线程安全的
* Vector implements a dynamic array. It is similar to ArrayList, but with two differences:
  *	Vector is synchronized 线程安全. This means if one thread is working on Vector, no other thread can get a hold of it. Unlike ArrayList, only one thread can perform an operation o vector at a time.
  *	Vector contains many legacy methods that are not part of the collections framework.
* Vector proves to be very useful if you don't know the size of the array in advance or you just need one that can change sizes over the lifetime of a program.
* //不讲了Fail fast Vector is fail fast. If the Vector is structurally modified at any time after the Iterator is created, in any way except through the Iterator’s own remove or add methods, the Iterator will throw a ConcurrentModificationException.
ArrayList iterator is also fail-fast.
*148LinkedList用merge来sort LinkedList 不讲代码了
