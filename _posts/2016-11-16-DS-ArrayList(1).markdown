---
layout:     post
title:      "<>DS - ArrayList (1)"
subtitle:   "DS Note"
date:       2016-11-16
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Algorithms
    - Data Structure
---



## ArrayList (1)

ArrayList，顾名思义，就是用数组来实现List接口的类。我们可以把ArrayList想象为一排内含物体的箱子，当我们需要插入一个新物体，我们就把一个新的箱子，推进这一排的某个位置。我们需要删除一个物体，就把它从箱子排列中拿走，再把右边的箱子推回来。因为是数组实现，ArrayList也同样具有无缝性（no holes），数据的存储是连续的。

### 功能、方法及复杂度

ArrayList支持的主要功能复杂度，如下表所示：

| DS                  | Addition | Removal  | Searching | Locating | Sort     |
|:-------------------:|:--------:|:--------:|:---------:|:--------:|:--------:|
|ArrayList (Unordered)| O(N)     | O(N)     |O(N)       |O(1)      |O(Nlog(N))|

为什么排序需要NLog(N)时间呢？我们后面会讲到排序的各种方法。

常见的方法：

*	add(object) : adds a new element to the end
*	add(index, object) : inserts a new element at the specified index
*	set(index, object) : replaces an existing element at the specified index with the new element.
*	get(index) : returns the element at the specified index.
*	remove(index) : deletes the element at the specified index.
*	size() : returns the number of elements.
*	contains
*	iterator

### 实现

Simplified Implementation ArrayList，顾名思义，即用数组array实现的list。我们定义一个数组作为instance variable，该数组储存了list中需要储存的元素，并且根据需求，添加、删除、搜索、遍历元素。 

1. 灵活控制：动态更新数组长度

那么，ArrayList如何做到灵活地控制数据数量呢？理解这一点，是理解ArrayList实现最关键的部分。

ArrayList的实现，是依靠动态更新内部数组的长度，来实现灵活控制长度的。

对于加长数组的情况，ArrayList类中有这样一个private方法：EnsureCapacity，每当添加操作检测到list的新长度已经超过了内部数组的长度，它就新建一个更大的数组，把旧数组的内容移动进去，新添加的元素也添加到新数组中，然后用新的数组来替代旧的数组作为新的内部数组。Java中，旧的数组会被垃圾回收。

每个ArrayList实例都有一个容量（Capacity），即用于存储元素的数组的大小。这个容量可随着不断添加新元素而自动增加，但是增长算法并没有定义。当需要插入大量元素时，在插入前可以调用ensureCapacity方法来增加ArrayList的容量以提高插入效率。目前java所用的增长函数是(oldCapacity*3)/2 + 1【后面是否改了？】;

```java
public boolean add(E e) {
    ensureCapacity(size+1); // Increments modCount!!
    elementData[size++] = e;
    return true;
}

public void ensureCapacity(int minCapacity) {
    modCount++; // 维护线程安全用
    int oldCapacity = elementData.length;
    if (minCapacity > oldCapacity) {
        Object oldData[] = elementData;
        int newCapacity = (oldCapacity*3)/2 + 1;
        if (newCapacity < minCapacity)
            newCapacity = minCapacity;
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
}
```

*modCount后面会提及

*那么，如果删除了元素，使得实际需求的数组长度缩短了呢？【【trimToSize，要讲吗】】

2. 插入和删除（Insert/Remove）

有了动态更新方法，添加和删除操作的实现就很直观了。

```java
public Object remove(int index){
    if(index < actSize){
        Object obj = myStore[index];
        myStore[index] = null;
        int tmp = index;
        while(tmp < actSize){
            myStore[tmp] = myStore[tmp+1];
            myStore[tmp+1] = null;
            tmp++;
        }
    actSize--;
    return obj;
    } else {
        throw new ArrayIndexOutOfBoundsException();
    }
}
```

3. Iterator/ListIterator 

上面介绍List接口时谈到了iterator，遍历器。我们实现一个数据结构，都需要override这个iterator遍历器的功能，使得该类能够实现数据遍历。实现的方式，就是实现Iterable接口，就像之前提到的，它提供了一个iterator实例，该实例有hasNext, next和remove三个方法，其中remove是可选的。

关键：listIterator为了能够修改，要引入modCount的概念。【需要详细讲吗？还是vector讲？】

### 注意事项

1. 线程安全 源码实现中有一个modCount，它的作用在于：iterator遍历ArrayList的过程中，如果其他线程改变了该ArrayList的内在结构，需要通知iterator，及时抛出异常，就是检查modCount的数量。
2. 线性插入/删除 正如我们在上面的功能列表中所见，插入和删除操作在arrayList中都是线性时间O(N)的。为什么呢？想象我们在内部数组中，插入了一个元素，那么它右边的元素，就要顺序后移一位。我们可能会问，如果我就是在尾部插入或删除一个元素，没有元素需要后移，为什么会O(N)呢？这便涉及到average time和worst case time的概念。我们计算数据结构算法复杂度时，一般使用最坏情况时间，在这个例子中，就是插入和删除第一个元素的时间。
3. 均摊分析 add（也就是append）是一种特殊的插入，也就是插入最后一个元素。我们可以确切地知道，插入最后一个元素，没有其他元素需要移动，所以时间复杂度是O(1)。但是，我们刚才介绍过动态更新内部数组的原理，如果我们插入过程中，需要动态更新数组了，就要申请一块更大的新内存，把旧数组拷贝到新数组中，这个过程是O(N)的时间，为什么没有计入呢？ 这就涉及了平摊分析（Amortized Analysis）这个概念，它是一种求算法复杂度的分析方式。 在平摊分析中，执行一系列数据结构操作所需要的时间是通过执行的所有操作求平均而得出的。平摊分析可以用来证明在一系列操作中，通过对所有操作求平均之后，即使其中单一的操作具有较大的代价，平均代价还是很小的。在这个例子中，ArrayList的append，一般是O(1)，个别时候需要expand array，就不是O(1)了，但均摊到每个操作，还是O(1)。我们需要注意，平摊分析和上面提到的average case/worst case是两个概念，它只是对操作求平均时间，而不是最坏的操作的时间。所以不涉及概率问题。平摊分析保证在最坏情况下，每个操作具有的平均性能。 平均分析的概念比较抽象，但只要我们结合这个例子，就会很好理解。它的意思就是说，即使个别情况下，数组需要动态更新，但绝大多数情况下，我们是不需要动态更新数组的，只要在旧数组里添加新的元素即可，所以当数据量足够大时，平均情况下，我们的add操作，就是O(1)时间完成的。
