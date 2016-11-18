---
layout:     post
title:      "<>DS - equals() and hashCode()"
subtitle:   "DS Note"
date:       2016-11-24
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Algorithms
    - Data Structure
---



## 探索equals()和hashCode()方法

### equals()和hashCode()区别？

equals()：反映的是对象或变量具体的值，即两个对象里面包含的值--可能是对象的引用，也可能是值类型的值。

hashCode()：计算出对象实例的哈希码，并返回哈希码，又称为散列函数。根类Object的hashCode()方法的计算依赖于对象实例的D（内存地址），故每个Object对象的hashCode都是唯一的；当然，当对象所对应的类重写了hashCode()方法时，结果就截然不同了。

之所以有hashCode方法，是因为在批量的对象比较中，hashCode要比equals来得快，很多集合都用到了hashCode，比如HashTable。
 
两个obj，如果equals()相等，hashCode()一定相等。

两个obj，如果hashCode()相等，equals()不一定相等（Hash散列值有冲突的情况，虽然概率很低）。

所以：

可以考虑在集合中，判断两个对象是否相等的规则是：

第一步，如果hashCode()相等，则查看第二步，否则不相等;

第二步，查看equals()是否相等，如果相等，则两obj相等，否则还是不相等。
 
1、首先equals()和hashcode()这两个方法都是从object类中继承过来的。

equals()是对两个对象的地址值进行的比较（即比较引用是否相同）。

hashCode()是一个本地方法，它的实现是根据本地机器相关的。

2、Java语言对equals()的要求如下，这些要求是必须遵循的：

**A 对称性**：如果x.equals(y)返回是“true”，那么y.equals(x)也应该返回是“true”。

**B 反射性**：x.equals(x)必须返回是“true”。

**C 类推性**：如果x.equals(y)返回是“true”，而且y.equals(z)返回是“true”，那么z.equals(x)也应该返回是“true”。

**D 一致性**：如果x.equals(y)返回是“true”，只要x和y内容一直不变，不管你重复x.equals(y)多少次，返回都是“true”。

任何情况下，x.equals(null)，永远返回是“false”；x.equals(和x不同类型的对象)永远返回是“false”。

3、equals()相等的两个对象，hashcode()一定相等；

反过来：hashcode()不等，一定能推出equals()也不等；

hashcode()相等，equals()可能相等，也可能不等。 

### 为什么选择hashcode方法？

以java.lang.Object来理解,JVM每new一个Object,它都会将这个Object丢到一个Hash哈希表中去,这样的话,下次做Object的比较或者取这个对象的时候,它会根据对象的hashcode再从Hash表中取这个对象。这样做的目的是提高取对象的效率。具体过程是这样:

1. new Object(),JVM根据这个对象的Hashcode值,放入到对应的Hash表对应的Key上,如果不同的对象确产生了相同的hash值,也就是发生了Hash key相同导致冲突的情况,那么就在这个Hash key的地方产生一个链表,将所有产生相同hashcode的对象放到这个单链表上去,串在一起。
2. 比较两个对象的时候,首先根据他们的hashcode去hash表中找他的对象,当两个对象的hashcode相同,那么就是说他们这两个对象放在Hash表中的同一个key上,那么他们一定在这个key上的链表上。那么此时就只能根据Object的equal方法来比较这个对象是否equal。当两个对象的hashcode不同的话，肯定他们不能equal.

可能经过上面理论的讲一下大家都迷糊了，我也看了之后也是似懂非懂的。下面我举个例子详细说明下。

就像我之前那篇博客《java中Map,List与Set的区别》中讲到的list和set的区别。

list是可以重复的，set是不可以重复的。那么set存储数据的时候是怎样判断存进的数据是否已经存在。使用equals()方法呢，还是hashcode()方法。

假如用equals()，那么存储一个元素就要跟已存在的所有元素比较一遍，比如已存入100个元素，那么存101个元素的时候，就要调用equals方法100次。

但如果用hashcode()方法的话，他就利用了hash算法来存储数据的，如果你不了解hash算法的话，可以看我之前的一篇博客05.Hash Tables（哈希表）。

这样的话每存一个数据就调用一次hashcode()方法，得到一个hashcode值及存入的位置。如果该位置不存在数据那么就直接存入，否则调用一次equals()方法，不相同则存，相同不存。这样下来整个存储下来不需要调用几次equals方法，虽然多了几次hashcode方法，但相对于前面来讲效率高了不少。

### 为什么要重写equals方法？

因为Object的equal方法默认是两个对象的引用的比较，意思就是指向同一内存,地址则相等，否则不相等；如果你现在需要利用对象里面的值来判断是否相等，则重载equal方法。

说道这个地方我相信很多人会有疑问，相信大家都被String对象的equals()方法和"=="纠结过一段时间，当时我们知道String对象中equals方法是判断值的，而==是地址判断。

那照这么说equals怎么会是地址的比较呢？

那是因为实际上JDK中，String、Math等封装类都对Object中的equals()方法进行了重写。

我们先看看Object中equals方法的源码：

```java
public boolean equals(Object obj) {  
         return (this == obj);  
}  
```

我们都知道所有的对象都拥有标识(内存地址)和状态(数据)，同时“==”比较两个对象的的内存地址，所以说使用Object的equals()方法是比较两个对象的内存地址是否相等，即若object1.equals(object2)为true，则表示equals1和equals2实际上是引用同一个对象。虽然有时候Object的equals()方法可以满足我们一些基本的要求，但是我们必须要清楚我们很大部分时间都是进行两个对象的比较，这个时候Object的equals()方法就不可以了，所以才会有String这些类对equals方法的改写，依次类推Double、Integer、Math。。。。等等这些类都是重写了equals()方法的，从而进行的是内容的比较。希望大家不要搞混了。

### 改写equals时总是要改写hashcode

这一点上《Effective java》中点7条和第8条讲的很详细，如果看完该博客后你还不懂得话，可以看看。里面也涉及到hashcode的设计，由于内容较深，我觉得我还接受不了，所以就不讨论这个了。

java.lnag.Object中对hashCode的约定：

1. 在一个应用程序执行期间，如果一个对象的equals方法做比较所用到的信息没有被修改的话，则对该对象调用hashCode方法多次，它必须始终如一地返回同一个整数。
2. 如果两个对象根据equals(Object o)方法是相等的，则调用这两个对象中任一对象的hashCode方法必须产生相同的整数结果。
3. 如果两个对象根据equals(Object o)方法是不相等的，则调用这两个对象中任一个对象的hashCode方法，不要求产生不同的整数结果。但如果能不同，则可能提高散列表的性能。

根据上一个问题，实际上我们已经能很简单的解释这一点了，比如改写String中的equals为基于内容上的比较而不是内存地址的话，那么虽然equals相等，但并不代表内存地址相等，由hashcode方法的定义可知内存地址不同，没改写的hashcode值也可能不同。所以违背了第二条约定。

又如new一个对象，再new一个内容相等的对象，调用equals方法返回的true，但他们的hashcode值不同，将两个对象存入HashSet中，会使得其中包含两个相等的对象，因为是先检索hashcode值，不等的情况下才会去比较equals方法的。

### hashCode方法使用介绍

Hash表数据结构常识： 

一、哈希表基于数组。 

二、缺点：基于数组的，数组创建后难以扩展。某些哈希表被基本填满时，性能下降得非常严重。 

三、没有一种简便得方法可以以任何一种顺序遍历表中数据项。 

四、如果不需要有序遍历数据，并且可以提前预测数据量的大小，那么哈希表在速度和易用性方面是无与伦比的。 

一、为什么HashCode对于对象是如此的重要： 

一个对象的HashCode就是一个简单的Hash算法的实现，虽然它和那些真正的复杂的Hash算法相比还不能叫真正的算法，它如何实现它，不仅仅是程序员的编程水平问题， 

而是关系到你的对象在存取是性能的非常重要的关系.有可能，不同的HashCode可能会使你的对象存取产生，成百上千倍的性能差别. 

先来看一下，在JAVA中两个重要的数据结构:HashMap和Hashtable，虽然它们有很大的区别，如继承关系不同，对value的约束条件(是否允许null)不同，以及线程安全性等有着特定的区别，但从实现原理上来说，它们是一致的.所以，我们只以Hashtable来说明： 

在java中，存取数据的性能，一般来说当然是首推数组，但是在数据量稍大的容器选择中，Hashtable将有比数组性能更高的查询速度.具体原因看下面的内容. 

Hashtable在存储数据时，一般先将该对象的HashCode和0x7FFFFFFF做与操作，因为一个对象的HashCode可以为负数，这样操作后可以保证它为一个正整数.然后以Hashtable的长度取模，得到该对象在Hashtable中的索引.

index = (o.hashCode() & 0x7FFFFFFF)%hs.length; 

这个对象就会直接放在Hashtable的每index位置，对于写入，这和数组一样，把一个对象放在其中的第index位置，但如果是查询，经过同样的算法，Hashtable可以直接从第index取得这个对象，而数组却要做循环比较.所以对于数据量稍大时，Hashtable的查询比数组具有更高的性能. 

既然一个对象可以根据HashCode直接定位它在Hashtable中的位置，那么为什么Hashtable还要用key来做映射呢?这就是关系Hashtable性能问题的最重要的问题：Hash冲突. 

常见的Hash冲突是不同对象最终产生了相同的索引，而一种非常甚至绝对少见的Hash冲突是，如果一组对象的个数大过了int范围，而HashCode的长度只能在int范围中，所以肯定要有同一组的元素有相同的HashCode，这样无论如何他们都会有相同的索引.当然这种极端的情况是极少见的，可以暂不考虑，但是对于同的HashCode经过取模，则会产中相同的索引，或者不同的对象却具有相同的HashCode，当然具有相同的索引. 

所以对于索引相同的对象，在该index位置存放了多个值，这些值要想能正确区分，就要依靠key来识别. 

事实上一个设计各好的HashTable，一般来说会比较平均地分布每个元素，因为Hashtable的长度总是比实际元素的个数按一定比例进行自增(装填因子一般为0.75)左右，这样大多数的索引位置只有一个对象，而很少的位置会有几个元素.所以Hashtable中的每个位置存放的是一个链表，对于只有一个对象是位置，链表只有一个首节点(Entry)，Entry的next为null.然后有hashCode，key，value属性保存了该位置的对象的HashCode，key和value(对象本身)，如果有相同索引的对象进来则会进入链表的下一个节点.如果同一个索引中有多个对象，根据HashCode和key可以在该链表中找到一个和查询的key相匹配的对象. 

从上面我看可以看到，对于HashMap和Hashtable的存取性能有重大影响的首先是应该使该数据结构中的元素尽量大可能具有不同的HashCode，虽然这并不能保证不同的HashCode产生不同的index，但相同的HashCode一定产生相同的index，从而影响产生Hash冲突. 

对于一个象，如果具有很多属性，把所有属性都参与散列，显然是一种笨拙的设计.因为对象的HashCode()方法几乎无所不在地被自动调用，如equals比较，如果太多的对象参与了散列. 

那么需要的操作常数时间将会增加很大.所以，挑选哪些属性参与散列绝对是一个编程水平的问题. 

从实现来说，一般的HashCode方法会这样: 

return Attribute1.HashCode() Attribute1.HashCode()..[ super.HashCode()]，我们知道，每次调用这个方法，都要重新对方法内的参与散列的对象重新计算一次它们的HashCode的运算，如果一个对象的属性没有改变，仍然要每次都进行计算，所以如果设置一个标记来缓存当前的散列码，只要当参与散列的对象改变时才重新计算，否则调用缓存的hashCode，这可以从很大程度上提高性能. 

默认的实现是将对象内部地址转化为整数作为HashCode，这当然能保证每个对象具有不同的HasCode，因为不同的对象内部地址肯定不同(废话)，但java语言并不能让程序员获取对象内部地址，所以，让每个对象产生不同的HashCode有着很多可研究的技术. 

如果从多个属性中采样出能具有平均分布的hashCode的属性，这是一个性能和多样性相矛盾的地方，如果所有属性都参与散列，当然hashCode的多样性将大大提高，但牺牲了性能，而如果只能少量的属性采样散列，极端情况会产生大量的散列冲突，如对"人"的属性中，如果用性别而不是姓名或出生日期，那将只有两个或几个可选的hashcode值，将产生一半以上的散列冲突.所以如果可能的条件下，专门产生一个序列用来生成HashCode将是一个好的选择(当然产生序列的性能要比所有属性参与散列的性能高的情况下才行，否则还不如直接用所有属性散列). 

如何对HashCode的性能和多样性求得一个平衡，可以参考相关算法设计的书，其实并不一定要求非常的优秀，只要能尽最大可能减少散列值的聚集.重要的是我们应该记得HashCode对于我们的程序性能有着重要的影响，在程序设计时应该时时加以注意. 

请记住：如果你想有效的使用HashMap，你就必须重写在其的HashCode()。 

还有两条重写HashCode()的原则： 

不必对每个不同的对象都产生一个唯一的hashcode，只要你的HashCode方法使get()能够得到put()放进去的内容就可以了。即“不为一原则”。生成hashcode的算法尽量使hashcode的值分散一些， 不要很多hashcode都集中在一个范围内，这样有利于提高HashMap的性能。即“分散原则”。至于第二条原则的具体原因，有兴趣者可以参考Bruce Eckel的《Thinking in Java》，

在那里有对HashMap内部实现原理的介绍，这里就不赘述了。 

掌握了这两条原则，你就能够用好HashMap编写自己的程序了。不知道大家注意没有， java.lang.Object中提供的三个方法：clone()，equals()和hashCode()虽然很典型，但在很多情况下都不能够适用，它们只是简单的由对象的地址得出结果。这就需要我们在自己的程序中重写它们，其实java类库中也重写了千千万万个这样的方法。利用面向对象的多态性——覆盖，Java的设计者很优雅的构建了Java的结构，也更加体现了Java是一门纯OOP语言的特性。 

Java提供的Collection和Map的功能是十分强大的，它们能够使你的程序实现方式更为灵活，执行效率更高。希望本文能够对大家更好的使用HashMap有所帮助。



弄了一天了，终于算是把hashcode弄懂了不少，先就这样吧，写这篇博客的目的是为了明天的HashMap和HashSet。理解了hashcode，就比较容易理解这两个类了。明天的明天的话，我应该会写TreeMap和TreeSet。