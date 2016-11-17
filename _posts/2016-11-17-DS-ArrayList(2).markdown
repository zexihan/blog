---
layout:     post
title:      "<>DS - ArrayList in Java (2)"
subtitle:   "DS Note"
date:       2016-11-17
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Algorithms
    - Data Structure
---



## ArrayList in Java

1. 什么是ArrayList 
ArrayList就是传说中的动态数组，用MSDN中的说法，就是Array的复杂版本，它提供了如下一些好处： 
动态的增加和减少元素 
实现了ICollection和IList接口 
灵活的设置数组的大小

2. 如何使用ArrayList 
最简单的例子：

```java
ArrayList List = new ArrayList(); 
for( int i=0;i <10;i++ ) //给数组增加10个Int元素 
List.Add(i); 
//..程序做一些处理 
List.RemoveAt(5);//将第6个元素移除 
for( int i=0;i <3;i++ ) //再增加3个元素 
List.Add(i+20); 
Int32[] values = (Int32[])List.ToArray(typeof(Int32));//返回ArrayList包含的数组
```

这是一个简单的例子，虽然没有包含ArrayList所有的方法，但是可以反映出ArrayList最常用的用法

3. ArrayList重要的方法和属性 
1）构造器 
ArrayList提供了三个构造器： 
public ArrayList(); 
默认的构造器，将会以默认（16）的大小来初始化内部的数组 
public ArrayList(ICollection); 
用一个ICollection对象来构造，并将该集合的元素添加到ArrayList 
public ArrayList(int); 
用指定的大小来初始化内部的数组

2）IsSynchronized属性和ArrayList.Synchronized方法 
IsSynchronized属性指示当前的ArrayList实例是否支持线程同步，而ArrayList.Synchronized静态方法则会返回一个ArrayList的线程同步的封装。 
如果使用非线程同步的实例，那么在多线程访问的时候，需要自己手动调用lock来保持线程同步，例如： 

```java
ArrayList list = new ArrayList(); 
//... 
lock( list.SyncRoot ) //当ArrayList为非线程包装的时候，SyncRoot属性其实就是它自己，但是为了满足ICollection的SyncRoot定义，这里还是使用SyncRoot来保持源代码的规范性 
{ 
list.Add( “Add a Item” ); 
}
```

如果使用ArrayList.Synchronized方法返回的实例，那么就不用考虑线程同步的问题，这个实例本身就是线程安全的，实际上ArrayList内部实现了一个保证线程同步的内部类，ArrayList.Synchronized返回的就是这个类的实例，它里面的每个属性都是用了lock关键字来保证线程同步。

3）Count属性和Capacity属性 
Count属性是目前ArrayList包含的元素的数量，这个属性是只读的。 
Capacity属性是目前ArrayList能够包含的最大数量，可以手动的设置这个属性，但是当设置为小于Count值的时候会引发一个异常。

4）Add、AddRange、Remove、RemoveAt、RemoveRange、Insert、InsertRange 
这几个方法比较类似 
Add方法用于添加一个元素到当前列表的末尾 
AddRange方法用于添加一批元素到当前列表的末尾 
Remove方法用于删除一个元素，通过元素本身的引用来删除 
RemoveAt方法用于删除一个元素，通过索引值来删除 
RemoveRange用于删除一批元素，通过指定开始的索引和删除的数量来删除 
Insert用于添加一个元素到指定位置，列表后面的元素依次往后移动 
InsertRange用于从指定位置开始添加一批元素，列表后面的元素依次往后移动

另外，还有几个类似的方法： 
Clear方法用于清除现有所有的元素 
Contains方法用来查找某个对象在不在列表之中

其他的我就不一一累赘了，大家可以查看MSDN，上面讲的更仔细 
5）TrimSize方法 
这个方法用于将ArrayList固定到实际元素的大小，当动态数组元素确定不在添加的时候，可以调用这个方法来释放空余的内存。 
6）ToArray方法 
这个方法把ArrayList的元素Copy到一个新的数组中。 

4. ArrayList与数组转换 
例1： 

```java
ArrayList List = new ArrayList(); 
List.Add(1); 
List.Add(2); 
List.Add(3);

Int32[] values = (Int32[])List.ToArray(typeof(Int32));
```

例2：

```java 
ArrayList List = new ArrayList(); 
List.Add(1); 
List.Add(2); 
List.Add(3);

Int32[] values = new Int32[List.Count]; 
List.CopyTo(values);
```

上面介绍了两种从ArrayList转换到数组的方法

例3：

```java 
ArrayList List = new ArrayList(); 
List.Add( “string” ); 
List.Add( 1 ); 
//往数组中添加不同类型的元素

object[] values = List.ToArray(typeof(object)); //正确 
string[] values = (string[])List.ToArray(typeof(string)); //错误
```

和数组不一样，因为可以转换为Object数组，所以往ArrayList里面添加不同类型的元素是不会出错的，但是当调用ArrayList方法的时候，要么传递所有元素都可以正确转型的类型或者Object类型，否则将会抛出无法转型的异常。

5. ArrayList最佳使用建议 
这一节我们来讨论ArrayList与数组的差别，以及ArrayList的效率问题 
1）ArrayList是Array的复杂版本 
ArrayList内部封装了一个Object类型的数组，从一般的意义来说，它和数组没有本质的差别，甚至于ArrayList的许多方法，如Index、IndexOf、Contains、Sort等都是在内部数组的基础上直接调用Array的对应方法。 
2）内部的Object类型的影响 
对于一般的引用类型来说，这部分的影响不是很大，但是对于值类型来说，往ArrayList里面添加和修改元素，都会引起装箱和拆箱的操作，频繁的操作可能会影响一部分效率。 
但是恰恰对于大多数人，多数的应用都是使用值类型的数组。 
消除这个影响是没有办法的，除非你不用它，否则就要承担一部分的效率损失，不过这部分的损失不会很大。 
3）数组扩容 
这是对ArrayList效率影响比较大的一个因素。 
每当执行Add、AddRange、Insert、InsertRange等添加元素的方法，都会检查内部数组的容量是否不够了，如果是，它就会以当前容量的两倍来重新构建一个数组，将旧元素Copy到新数组中，然后丢弃旧数组，在这个临界点的扩容操作，应该来说是比较影响效率的。 
例1：比如，一个可能有200个元素的数据动态添加到一个以默认16个元素大小创建的ArrayList中，将会经过： 
16*2*2*2*2 = 256 
四次的扩容才会满足最终的要求，那么如果一开始就以： 
ArrayList List = new ArrayList( 210 ); 
的方式创建ArrayList，不仅会减少4次数组创建和Copy的操作，还会减少内存使用。

例2：预计有30个元素而创建了一个ArrayList： 
ArrayList List = new ArrayList(30); 
在执行过程中，加入了31个元素，那么数组会扩充到60个元素的大小，而这时候不会有新的元素再增加进来，而且有没有调用TrimSize方法，那么就有1次扩容的操作，并且浪费了29个元素大小的空间。如果这时候，用： 
ArrayList List = new ArrayList(40); 
那么一切都解决了。 
所以说，正确的预估可能的元素，并且在适当的时候调用TrimSize方法是提高ArrayList使用效率的重要途径。 
4）频繁的调用IndexOf、Contains等方法（Sort、BinarySearch等方

法经过优化，不在此列）引起的效率损失 
首先，我们要明确一点，ArrayList是动态数组，它不包括通过Key或者Value快速访问的算法，所以实际上调用IndexOf、Contains等方法是执行的简单的循环来查找元素，所以频繁的调用此类方法并不比你自己写循环并且稍作优化来的快，如果有这方面的要求，建议使用Hashtable或SortedList等键值对的集合。 

```java
ArrayList al=new ArrayList();

al.Add("How"); 
al.Add("are"); 
al.Add("you!");

al.Add(100); 
al.Add(200); 
al.Add(300);

al.Add(1.2); 
al.Add(22.8);
```

> 使用ArrayList类
ArrayList类实现了List接口，由ArrayList类实现的List集合采用数组结构保存对象。数组结构的优点是便于对集合进行快速的随机访问，如果经常需要根据索引位置访问集合中的对象，使用由ArrayList类实现的List集合的效率较好。数组结构的缺点是向指定索引位置插入对象和删除指定索引位置对象的速度较慢，如果经常需要向List集合的指定索引位置插入对象，或者是删除List集合的指定索引位置的对象，使用由ArrayList类实现的List集合的效率则较低，并且插入或删除对象的索引位置越小效率越低，原因是当向指定的索引位置插入对象时，会同时将指定索引位置及之后的所有对象相应的向后移动一位，如图1所示。当删除指定索引位置的对象时，会同时将指定索引位置之后的所有对象相应的向前移动一位，如图2所示。如果在指定的索引位置之后有大量的对象，将严重影响对集合的操作效率。

就是因为用ArrayList类实现的List集合在插入和删除对象时存在这样的缺点，在编写例程06时才没有利用ArrayList类实例化List集合，下面看一个模仿经常需要随机访问集合中对象的例子。
在编写该例子时，用到了Java.lang.Math类的random()方法，通过该方法可以得到一个小于10的double型随机数，将该随机数乘以5后再强制转换成整数，将得到一个0到4的整数，并随机访问由ArrayList类实现的List集合中该索引位置的对象，具体代码如下：
src\com\mwq\TestCollection.java关键代码：

```java
public static void main(String[] args) {
String a = "A", b = "B", c = "C", d = "D", e = "E";
List<String> list = new ArrayList<String>();
list.add(a);      // 索引位置为 0
list.add(b);      // 索引位置为 1
list.add(c);      // 索引位置为 2
list.add(d);      // 索引位置为 3
list.add(e);      // 索引位置为 4
System.out.println(list.get((int) (Math.random() * 5)));     // 模拟随机访问集合中的对象
}
```

> 我实际中的练习例子：

```java
package code;  
import java.util.ArrayList;  
import java.util.Iterator;  
public class SimpleTest {  
   
   
 public static void main(String []args){  
    
  ArrayList list1 = new ArrayList();    
  list1.add("one");  
  list1.add("two");  
  list1.add("three");  
  list1.add("four");  
  list1.add("five");  
  list1.add(0,"zero");    
  System.out.println("<--list1中共有>" + list1.size()+ "个元素");    
  System.out.println("<--list1中的内容:" + list1 + "-->");  
    
  ArrayList list2 = new ArrayList();  
  list2.add("Begin");  
  list2.addAll(list1);  
  list2.add("End");  
  System.out.println("<--list2中共有>" + list2.size()+ "个元素");    
  System.out.println("<--list2中的内容:" + list2 + "-->");  
    
  ArrayList list3 =  new ArrayList();  
  list3.removeAll(list1);  
  System.out.println("<--list3中是否存在one: "+ (list3.contains("one")? "是":"否")+ "-->");  
    
  list3.add(0,"same element");  
  list3.add(1,"same element");  
  System.out.println("<--list3中共有>" + list3.size()+ "个元素");    
  System.out.println("<--list3中的内容:" + list3 + "-->");  
  System.out.println("<--list3中第一次出现same element的索引是" + list3.indexOf("same element") + "-->");  
  System.out.println("<--list3中最后一次出现same element的索引是" + list3.lastIndexOf("same element") + "-->");  
    
    
  System.out.println("<--使用Iterator接口访问list3->");  
  Iterator it = list3.iterator();  
  while(it.hasNext()){  
   String str = (String)it.next();  
   System.out.println("<--list3中的元素:" + list3 + "-->");  
  }  
    
  System.out.println("<--将list3中的same element修改为another element-->");  
  list3.set(0,"another element");  
  list3.set(1,"another element");  
     System.out.println("<--将list3转为数组-->");  
    // Object []  array =(Object[]) list3.toArray(new   Object[list3.size()] );  
     Object [] array = list3.toArray();  
     for(int i = 0; i < array.length ; i ++){  
      String str = (String)array[i];  
      System.out.println("array[" + i + "] = "+ str);        
     }       
       
     System.out.println("<---清空list3->");  
     list3.clear();  
     System.out.println("<--list3中是否为空: " + (list3.isEmpty()?"是":"否") + "-->");  
     System.out.println("<--list3中共有>" + list3.size()+ "个元素");   
    
  //System.out.println("hello world!");  
 }  
}  
```