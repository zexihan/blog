---
layout:     post
title:      "<>DS - LinkedList (2)"
subtitle:   "DS Note"
date:       2016-11-19
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Algorithms
    - Data Structure
---



## LinkedList

LinkedList类是双向列表,列表中的每个节点都包含了对前一个和后一个元素的引用.
LinkedList的构造函数如下

1. public LinkedList():  ——生成空的链表
2. public LinkedList(Collection col):  复制构造函数

>1、获取链表的第一个和最后一个元素

```java
import java.util.LinkedList;  
  
public class LinkedListTest{  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
  
  
    System.out.println("链表的第一个元素是 : " + lList.getFirst());  
    System.out.println("链表最后一个元素是 : " + lList.getLast());  
  }  
}  
```
>2、获取链表元素 

```java
for (String str: lList) {  
      System.out.println(str);  
    } 
```

>3、从链表生成子表

```java 
List subl = lList.subList(1, 4);  
    System.out.println(subl);  
    lst.remove(2);  
    System.out.println(lst);  
    System.out.println(lList);  
```

>4、添加元素：添加单个元素
 如果不指定索引的话，元素将被添加到链表的最后.
public boolean add(Object element)
public boolean add(int index, Object element)
也可以把链表当初栈或者队列来处理:
public boolean addFirst(Object element)
public boolean addLast(Object element)
addLast()方法和不带索引的add()方法实现的效果一样.

```java
import java.util.LinkedList;  
  
public class LinkedListTest{  
  public static void main(String[] a) {  
    LinkedList list = new LinkedList();  
    list.add("A");  
    list.add("B");  
    list.add("C");  
    list.add("D");  
    list.addFirst("X");  
    list.addLast("Z");  
    System.out.println(list);  
  }  
} 
```
>5、删除元素

```java
public Object removeFirst()  
public Object removeLast()  
import java.util.LinkedList;  
  
  
public class MainClass {  
  public static void main(String[] a) {  
  
  
    LinkedList list = new LinkedList();  
    list.add("A");  
    list.add("B");  
    list.add("C");  
    list.add("D");  
    list.removeFirst();  
    list.removeLast();  
    System.out.println(list);  
  }  
}  
```
>6、使用链表实现栈效果

```java
import java.util.LinkedList;  
public class MainClass {  
  public static void main(String[] args) {  
    StackL stack = new StackL();  
    for (int i = 0; i < 10; i++)  
      stack.push(i);  
    System.out.println(stack.top());  
    System.out.println(stack.top());  
    System.out.println(stack.pop());  
    System.out.println(stack.pop());  
    System.out.println(stack.pop());  
  }  
}  
class StackL {  
  private LinkedList list = new LinkedList();  
  public void push(Object v) {  
    list.addFirst(v);  
  }  
  public Object top() {  
    return list.getFirst();  
  }  
  public Object pop() {  
    return list.removeFirst();  
  }  
}  
```

>7、使用链表来实现队列效果

```java
import java.util.LinkedList;  
public class MainClass {  
  public static void main(String[] args) {  
    Queue queue = new Queue();  
    for (int i = 0; i < 10; i++)  
      queue.put(Integer.toString(i));  
    while (!queue.isEmpty())  
      System.out.println(queue.get());  
  }  
}  
class Queue {  
  private LinkedList list = new LinkedList();  
  public void put(Object v) {  
    list.addFirst(v);  
  }  
  public Object get() {  
    return list.removeLast();  
  }  
  public boolean isEmpty() {  
    return list.isEmpty();  
  }  
}  
```

>8、将LinkedList转换成ArrayList

```java
ArrayList<String> arrayList = new ArrayList<String>(linkedList);  
    for (String s : arrayList) {  
      System.out.println("s = " + s);  
    }
```  
>9、删掉所有元素：清空LinkedList

```java
    lList.clear();
```

>10、删除列表的首位元素

```java
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    System.out.println(lList);  
        //元素在删除的时候，仍然可以获取到元素  
    Object object = lList.removeFirst();  
    System.out.println(object + " has been removed");  
    System.out.println(lList);  
    object = lList.removeLast();  
    System.out.println(object + " has been removed");  
    System.out.println(lList);  
  }  
}  
```

>11、根据范围删除列表元素

```java
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    System.out.println(lList);  
    lList.subList(2, 5).clear();  
    System.out.println(lList);  
  }  
} 
```
>12、删除链表的特定元素

```java
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    System.out.println(lList);  
    System.out.println(lList.remove("2"));//删除元素值=2的元素  
    System.out.println(lList);  
    Object obj = lList.remove(2);  //删除第二个元素  
    System.out.println(obj + " 已经从链表删除");  
    System.out.println(lList);  
  }  
}  
```

>13、将LinkedList转换为数组，数组长度为0

```java
import java.util.LinkedList;  
import java.util.List;  
public class Main {  
  public static void main(String[] args) {  
    List<String> theList = new LinkedList<String>();  
    theList.add("A");  
    theList.add("B");  
    theList.add("C");  
    theList.add("D");  
    String[] my = theList.toArray(new String[0]);  
    for (int i = 0; i < my.length; i++) {  
      System.out.println(my[i]);  
    }  
  }  
}  
```

>14、将LinkedList转换为数组，数组长度为链表长度

```java
import java.util.LinkedList;  
import java.util.List;  
public class Main {  
  public static void main(String[] args) {  
    List<String> theList = new LinkedList<String>();  
    theList.add("A");  
    theList.add("B");  
    theList.add("C");  
    theList.add("D");  
    String[] my = theList.toArray(new String[theList.size()]);  
    for (int i = 0; i < my.length; i++) {  
      System.out.println(my[i]);  
    }  
  }  
} 
```

>15、将LinkedList转换成ArrayList

```java
import java.util.ArrayList;  
import java.util.LinkedList;  
import java.util.List;  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> myQueue = new LinkedList<String>();  
    myQueue.add("A");  
    myQueue.add("B");  
    myQueue.add("C");  
    myQueue.add("D");  
    List<String> myList = new ArrayList<String>(myQueue);  
    for (Object theFruit : myList)  
      System.out.println(theFruit);  
  }  
} 
```

>16、实现栈

```java
import java.util.Collections;  
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] argv) throws Exception {  
    LinkedList stack = new LinkedList();  
    Object object = "";  
    stack.addFirst(object);  
    Object o = stack.getFirst();  
    stack = (LinkedList) Collections.synchronizedList(stack);  
  }  
} 
```

>17、实现队列

```java
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] argv) throws Exception {  
    LinkedList queue = new LinkedList();  
    Object object = "";  
    // Add to end of queue  
    queue.add(object);  
    // Get head of queue  
    Object o = queue.removeFirst();  
  }  
}
```

>18 、同步方法

```java
import java.util.Collections;  
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] argv) throws Exception {  
    LinkedList queue = new LinkedList();  
    Object object = "";  
    queue.add(object);  
    Object o = queue.removeFirst();  
    queue = (LinkedList) Collections.synchronizedList(queue);  
  }  
} 
```

>19、查找元素位置

```java 
import java.util.LinkedList;  
  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    lList.add("2");  
    System.out.println(lList.indexOf("2"));  
    System.out.println(lList.lastIndexOf("2"));  
  }  
}
```

>20、替换元素

```java
import java.util.LinkedList;  
  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    System.out.println(lList);  
    lList.set(3, "Replaced");//使用set方法替换元素，方法的第一个参数是元素索引，后一个是替换值  
    System.out.println(lList);  
  }  
} 
```

>21、链表添加对象

```java
import java.util.LinkedList;  
class Address {  
  private String name;  
  private String street;  
  private String city;  
  private String state;  
  private String code;  
  Address(String n, String s, String c, String st, String cd) {  
    name = n;  
    street = s;  
    city = c;  
    state = st;  
    code = cd;  
  }  
  public String toString() {  
    return name + " " + street + " " + city + " " + state + " " + code;  
  }  
}  
  
  
class MailList {  
  public static void main(String args[]) {  
    LinkedList<Address> ml = new LinkedList<Address>();  
    ml.add(new Address("A", "11 Ave", "U", "IL", "11111"));  
    ml.add(new Address("R", "11 Lane", "M", "IL", "22222"));  
    ml.add(new Address("T", "8 St", "C", "IL", "33333"));  
    for (Address element : ml)  
      System.out.println(element + "\n");  
  }  
} 
```

>22、确认链表是否存在特定元素

```java
import java.util.LinkedList;  
  
  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    if (lList.contains("4")) {  
      System.out.println("LinkedList contains 4");  
    } else {  
      System.out.println("LinkedList does not contain 4");  
    }  
  }  
}  
```

>23、根据链表元素生成对象数组

```java
Object[] objArray = lList.toArray();  
for (Object obj: objArray) {  
   System.out.println(obj);  
}  
```

>24、链表多线程

```java

import java.util.Collections;  
import java.util.LinkedList;  
import java.util.List;  
class PrepareProduction implements Runnable {  
  private final List<String> queue;  
  PrepareProduction(List<String> q) {  
    queue = q;  
  }  
  public void run() {  
    queue.add("1");  
    queue.add("done");  
  }  
}  
class DoProduction implements Runnable {  
  private final List<String> queue;  
  DoProduction(List<String> q) {  
    queue = q;  
  }  
  public void run() {  
    String value = queue.remove(0);  
    while (!value.equals("*")) {  
      System.out.println(value);  
      value = queue.remove(0);  
    }  
  }  
}  
public class Main {  
  public static void main(String[] args) throws Exception {  
    List q = Collections.synchronizedList(new LinkedList<String>());  
    Thread p1 = new Thread(new PrepareProduction(q));  
    Thread c1 = new Thread(new DoProduction(q));  
    p1.start();  
    c1.start();  
    p1.join();  
    c1.join();  
  }  
}  
```

>25、优先级链表（来自JBOSS）

```java
import java.util.ArrayList;  
import java.util.LinkedList;  
import java.util.List;  
import java.util.ListIterator;  
import java.util.NoSuchElementException;  
  
  
public class BasicPriorityLinkedList {  
  
  
  protected LinkedList[] linkedLists;  
  protected int priorities;  
  protected int size;  
  
  
  public BasicPriorityLinkedList(int priorities) {  
    this.priorities = priorities;  
    initDeques();  
  }  
  public void addFirst(Object obj, int priority) {  
    linkedLists[priority].addFirst(obj);  
    size++;  
  }  
  public void addLast(Object obj, int priority) {  
    linkedLists[priority].addLast(obj);  
    size++;  
  }  
  public Object removeFirst() {  
    Object obj = null;  
    for (int i = priorities - 1; i >= 0; i--) {  
      LinkedList ll = linkedLists[i];  
      if (!ll.isEmpty()) {  
        obj = ll.removeFirst();  
        break;  
      }  
    }  
    if (obj != null) {  
      size--;  
    }  
    return obj;  
  }  
  public Object removeLast() {  
    Object obj = null;  
    for (int i = 0; i < priorities; i++) {  
      LinkedList ll = linkedLists[i];  
      if (!ll.isEmpty()) {  
        obj = ll.removeLast();  
      }  
      if (obj != null) {  
        break;  
      }  
    }  
    if (obj != null) {  
      size--;  
    }  
    return obj;  
  }  
  
  
  public Object peekFirst() {  
    Object obj = null;  
    for (int i = priorities - 1; i >= 0; i--) {  
      LinkedList ll = linkedLists[i];  
      if (!ll.isEmpty()) {  
        obj = ll.getFirst();  
      }  
      if (obj != null) {  
        break;  
      }  
    }  
    return obj;  
  }  
  
  
  public List getAll() {  
    List all = new ArrayList();  
    for (int i = priorities - 1; i >= 0; i--) {  
      LinkedList deque = linkedLists[i];  
      all.addAll(deque);  
    }  
    return all;  
  }  
  
  
  public void clear() {  
    initDeques();  
  }  
  
  
  public int size() {  
    return size;  
  }  
  
  
  public boolean isEmpty() {  
    return size == 0;  
  }  
  
  
  public ListIterator iterator() {  
    return new PriorityLinkedListIterator(linkedLists);  
  }  
  
  
  protected void initDeques() {  
    linkedLists = new LinkedList[priorities];  
    for (int i = 0; i < priorities; i++) {  
      linkedLists[i] = new LinkedList();  
    }  
    size = 0;  
  }  
  
  
  class PriorityLinkedListIterator implements ListIterator {  
    private LinkedList[] lists;  
    private int index;  
    private ListIterator currentIter;  
    PriorityLinkedListIterator(LinkedList[] lists) {  
      this.lists = lists;  
      index = lists.length - 1;  
      currentIter = lists[index].listIterator();  
    }  
  
  
    public void add(Object arg0) {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public boolean hasNext() {  
      if (currentIter.hasNext()) {  
        return true;  
      }  
      while (index >= 0) {  
        if (index == 0 || currentIter.hasNext()) {  
          break;  
        }  
        index--;  
        currentIter = lists[index].listIterator();  
      }  
      return currentIter.hasNext();  
    }  
  
  
    public boolean hasPrevious() {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public Object next() {  
      if (!hasNext()) {  
        throw new NoSuchElementException();  
      }  
      return currentIter.next();  
    }  
  
  
    public int nextIndex() {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public Object previous() {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public int previousIndex() {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public void remove() {  
      currentIter.remove();  
      size--;  
    }  
  
  
    public void set(Object obj) {  
      throw new UnsupportedOperationException();  
    }  
  }  
  
  
}
```

>26、生成list的帮助类（来自google）

```java
import java.util.ArrayList;  
import java.util.Collections;  
import java.util.LinkedList;  
import java.util.List;  
public class Lists {  
  private Lists() { }  
  public static <E> ArrayList<E> newArrayList() {  
    return new ArrayList<E>();  
  }  
  public static <E> ArrayList<E> newArrayListWithCapacity(int initialCapacity) {  
    return new ArrayList<E>(initialCapacity);  
  }  
  public static <E> ArrayList<E> newArrayList(E... elements) {  
    ArrayList<E> set = newArrayList();  
    Collections.addAll(set, elements);  
    return set;  
  }  
  public static <E> ArrayList<E> newArrayList(Iterable<? extends E> elements) {  
    ArrayList<E> list = newArrayList();  
    for(E e : elements) {  
      list.add(e);  
    }  
    return list;  
  }  
  public static <E> LinkedList<E> newLinkedList() {  
    return new LinkedList<E>();  
  }  
}  
```   