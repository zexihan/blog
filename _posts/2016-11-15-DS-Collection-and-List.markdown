---
layout:     post
title:      "<>DS - Collection and List"
subtitle:   "DS Note"
date:       2016-11-15
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Algorithms
    - Data Structure
---



## Collection and List

### Collection

“集合类”（Collections）这个概念，是指将其他实例归类的实例。比如，我有三个苹果，两个梨，将三个苹果放入一个袋子，两个梨放入另一个袋子，这两个装有水果的袋子，就形成了两个集合类实例。【集合类和容器的区别？能够用容器来形容集合类吗？】 在Java中，Collections这个概念，由Collection接口和Collections类来分别表述。Collection接口是一个高级接口，它包含了List，Set和Queue三个子接口。如图：
【图】

而Collections是一个类，它提供了一系列static方法，能够解决一些集合类的功能需求。比如，Collections.sort(List list)可以将传入的list参数进行排序（该方法的实现使用了优化后的Merge Sort算法）。
实现集合接口的类，需要具备以下几个功能，括号中是对应的方法名：

* 插入（add）
* 删除（remove）
* 搜索（contains）
* 排序（sort）
* 遍历和遍历器（iterator) 在上一节中，我们提到了遍历，Collection接口也需要实现遍历器功能。尽管我们可以将遍历简单地理解为访问集合类中的每一个元素各一次，但遍历的思想，其实比这一概念更加复杂。对于一个数据结构而言，假如要访问所有的数据，相当于我们在走一个迷宫，迷宫的每一个checkpoint，我们都要走一遍。而“遍历”思想意味着，我们不仅要把所有的checkpoint都走一遍，而且在遍历过程中的每一个时间横截面上，我们都应该能够知道，我们走到哪里了。所以，我们需要一边走，一边更新一个“记号”，这个记号记录了我们目前所在的位置（或下一个checkpoint的位置）。【讲的不够清楚，为什么要这么设计？这种设计思想的精髓在哪里？】 那么，如何保存这个“记号”呢？面向对象编程的思想，使用了一个对象来记录我们在遍历过程中的某个时间点上的信息，这个对象就是遍历器（iterator）。 一个Collection的遍历器，需要实现Iterable接口，亦即返回一个遍历器对象iterator,这个对象支持以下方法：
    * boolean hasNext() Returns true if the iteration has more elements.
    * E next() Returns the next element in the iteration.
    * void remove() Removes from the underlying collection the last element returned by this iterator (optional operation).

每当我们产生一个遍历器，就开始了一个遍历的过程。我们通过hasNext方法来检测是否还有未遍历的元素，然后通过next方法来返回下一个未遍历的元素，还可以选择性地通过remove方法删除元素。
* 对比（equals）
* 哈希（hashCode）
* 长度（size）
* 转换为数组（toArray）

### List

【图】 List是实现Collection接口的一个接口，是有序的Collection。

* 和数组类似的是，使用List能准确控制每个元素插入的位置，也可以实现上一节所提到的定位功能。与数组不同的是，使用List可以灵活地控制数据结构长度，自由地增删数据。
* List允许有相同的元素。
* 除了具有Collection接口必备的iterator()方法外，List还提供一个listIterator()方法，返回一个ListIterator，和标准的Iterator相比，ListIterator允许添加，删除，设定元素，还能向前或向后遍历。
* 实现List接口的常用类有LinkedList，ArrayList，Vector和Stack。其中前三者支持数据的定位（即用index来准确访问数据结构中的某个元素），在这里介绍；Stack则稍后为大家介绍。
