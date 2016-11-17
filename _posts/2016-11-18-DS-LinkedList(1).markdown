---
layout:     post
title:      "<>DS - LinkedList (1)"
subtitle:   "DS Note"
date:       2016-11-18
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Algorithms
    - Data Structure
---



## LinkedList

### 问题

那么，既然插入/删除操作在ArrayList中是线性时间，假如我们每天需要增删许多数据，这种安排的速度就不够好了。我们有没有办法将其更加优化，变成O(1)的时间呢？这样，我们进行数据增删的效率就大大增加了。

让我们来想象一下，现实中我们怎样解决这个问题。还是想象出一排箱子，ArrayList中，当我们需要插入一个新物体到某个位置，我们就要把它右边的箱子，往后移动一位，所以就有了O(N)的最坏情况复杂度。那么，有没有办法让每次插入，不需要移动右边的箱子，仍然可以记录箱子的顺序呢？你可能已经想到了。有一种办法，即使我们的箱子放的乱七八糟，每次插入都不需要移动箱子的位置，仍然可以记录它们的顺序。就是把这些箱子，用绳子串联起来。当我们要在箱子A和B之间增加物体的时候，把两个箱子的链接绳打开，箱子A链接到新的箱子上，再把新箱子链接到箱子B上，就完成了。不需要移动任何一个箱子，所以复杂度是O(1)。【图】

这就是LinkedList的思想。LinkedList就是将一个个元素装入一个Node，再把Node用内部引用（绳子）链接起来，同样可以线性存储和访问数据。

根据绳子链接的方向，LinkedList可以分成Singly LinkedList（只链接下一个箱子），Doubly LinkedList（两边都有链接）。后者可以进行回溯的遍历，比较方便，但是需要使用双倍的空间来储存下一个箱子的地址。 类似以空间换时间的策略，表现在很多数据结构的设计思想中，我们以后将会看到更多例子。我们这里介绍的主要是Singly LinkedList。

### 功能、方法及复杂度

当我们插入元素时，只要知道实在哪两个Node之间插入，插入的复杂度就是O(1)，删除同理，这就解决了ArrayList插入、删除线性时间的问题。此外，LinkedList还不需要动态更新内部数组，也节省了这部分时间。

但是，这样的处理方式带来了另一个问题。在ArrayList中，我们可以迅速完成“打开第7个箱子”这个操作，只要走到第7个箱子面前就可以了。我们也可以把箱子从1到n编号，作用是一样的。但LinkedList中，完成这个操作就没那么容易了：箱子都是散乱堆放在一起的，搞清它们顺序的唯一方法，就是从第1个箱子往后找，找到第7个为止。所以，LinkedList中，定位功能是线性时间的。那么，当我们需要频繁地使用定位功能时，LinkedList就不适合了。

以下是LinkedList各主要功能的时间复杂度。 

| DS                   | Addition | Removal  | Searching | Locating | Sort     |
|:--------------------:|:--------:|:--------:|:---------:|:--------:|:--------:|
|LinkedList (Unordered)| O(1)     | O(1)     |O(N)       |O(N)      |O(Nlog(N))|

为什么sort也是Nlog(N)呢？Java的LinkedList的排序方法使用了一个workaround，就是将该list转换为ArrayList，然后使用了ArrayList的排序。因为转换过程是O(N)，在分析复杂度时可以忽略，所以复杂度还是Nlog(N)。

常见方法：

* O(1)

```java
addFirst(element: Object) : void
addLast(element: Object) : void
getFirst() : Object
getLast() : Object
removeFirst() : Object
removeLast() : Object
```

* O(N)

```java
insertAfter(AnyType key, AnyType toInsert) : void
insertBefore(AnyType key, AnyType toInsert) (triky) : void
iterator() : Iterator<AnyType>
```

insertAfter/insertBefore，就是搜索到一个元素，然后在它前面或后面添加一个元素。添加的部分复杂度为O(1)，搜索的部分复杂度为O(N)。

在java中LinkedList Class并不能在iterator方法以外直接访问next/prev/data等instance variable，它是被封装的。但是，LeetCode中经常会有直接使用LinkedList的ivar的题目，此时可以直接访问。所以，熟悉用prev,next,curr来遍历、插入、删除和访问是很有用的。

### 实现

实现LinkedList可以分成两部分去理解。第一部分，是实现它的基本结构，即内含的Node结构。第二部分，是使用Node内部类，实现List的各种方法及LinkedList独特的方法。

1. Node 内含的Node结构怎样实现？这就涉及了Java嵌套类的概念，即“类中之类”——声明在另一个类里面的类。我们需要的内部Node就是用嵌套类定义的。 嵌套类根据声明时是否使用static关键字，分为两类。就像static方法一样，static嵌套类是附着在大类上面的，所以不能访问外面的外部类（outer class）的内容; non-static嵌套类则是附着在大类的具体每个实例上面的，所以可以访问外部类内容，即使private也可以。我们的Node类不需要访问外部类内容，所以是static嵌套类。【举一个不抽象的例子】

```java
class OuterClass { ....
    class NestedClass { ....
    }
}
```

Node实现：

```java 
private static class Node { 
    private AnyType data; 
    private Node next;
    public Node(AnyType data, Node<AnyType> next) {
        this.data = data;
        this.next = next;
    }
}
```

i.	Methods

ii.	Constructor 有了内部类，我们就可以利用它来实现LinkedList了。我们只须对每一个实例保存该list的第一个node，也就是head node，就可以实现它的所有方法。

```java
public MySinglyLinkedList() {
head = null;
}
```
	addFirst 像addFirst这样的方法，实现方式是非常直观的，只要完成“重新链接”的过程就可以了。

```java
public void addFirst(AnyType data) {
    head = new Node<AnyType>\(data, head\);
}
```

iii.	insertBefore 一个比较难实现的方法是insertBefore，就是先找到一个元素，再往它前面添加一个元素。难度在于，当我们找到这个元素时，我们已经失去了它自己的链接，只有它自带的next链接。那么，我们在找它的时候，就需要能够一次“看到”两个元素，当第二个元素符合搜索条件时，在第一和第二个元素中间插入新元素。（画图讲思路）

```java
public void insertBefore(AnyType key, AnyType toInsert) {
    Node tmp = head; if (head == null) return;
// Edge case for head containing key, insert a new node before head
    if (head.data.equals(key)) {
        head = new Node<AnyType>(toInsert, head);
        return;
    }

    while (tmp.next != null && !tmp.next.data.equals(key))
        tmp = tmp.next;
    
    if (tmp.next != null) {
        tmp.next = new Node<AnyType>(toInsert, tmp.next);
    }
}
```

* remove一个特定元素，也就是搜索之后再删除

```java
public void remove(AnyType key) {
    if (head == null)
        return;
    if (head.data.equals(key)) {
        head = null;
    }
    else {
        Node<AnyType> tmp = head;
    while (tmp.next != null && !tmp.next.data.equals(key))
        tmp = tmp.next;
    if (tmp.next != null)
        tmp.next = tmp.next.next;
    }
}
```

iv.	reverseLinkedList 一般API里不会有，但是是经典考题。给一个head，把list的连接关系反过来，返回一个linkedList的新head。 有recursive和iterative两种经典的写法。 

```java
// Iterative 
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head; 
    ListNode prev = null, cur = head; 
    ListNode next;
    while (cur != null) {
        next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    return prev;
}

// Recursive
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode second = head.next;
    head.next = null;
    ListNode rest = reverseList(second);
    second.next = head;
    return rest;
}
```



