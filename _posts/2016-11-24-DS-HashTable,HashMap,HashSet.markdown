---
layout:     post
title:      "<>DS - HashTable, HashMap and HashSet"
subtitle:   "DS Note"
date:       2016-11-24
author:     "Zexi"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Algorithms
    - Data Structure
---



## HashTable, HashMap and HashSet

http://blog.csdn.net/speedme/article/details/22485681

### 1. HashTable和HashMap的区别

相信这个是大家最容易混淆的。

HashMap和Hashtable都实现了Map接口，但决定用哪一个之前先要弄清楚它们之间的分别。主要的区别有：**线程安全性，同步(synchronization)，以及速度**。

HashMap几乎可以等价于Hashtable，除了HashMap是非synchronized的，并可以接受null(HashMap allows one null key and any number of null values.，而Hashtable则不行)。这就是说，HashMap中如果在表中没有发现搜索键，或者如果发现了搜索键，但它是一个空的值，那么get()将返回null。如果有必要，用containKey()方法来区别这两种情况。

HashMap是非synchronized，而Hashtable是synchronized，这意味着Hashtable是线程安全的，多个线程可以共享一个Hashtable；而如果没有正确的同步的话，多个线程是不能共享HashMa的。 即是说，在多线程应用程序中，不用专门的操作就安全地可以使用Hashtable了；而对于HashMap，则需要额外的同步机制。但HashMap的同步问题可通过Collections的一个

静态方法得到解决：

```java
    Map Collections.synchronizedMap(Map m)
```

这个方法返回一个同步的Map，这个Map封装了底层的HashMap的所有方法，使得底层的HashMap即使是在多线程的环境中也是安全的。                                                                                              而而且Java 5提供了ConcurrentHashMap，它是HashTable的替代，比HashTable的扩展性更好。
    
    要详细了解ConcurrentHashMap见《构建一个更好的 HashMap---ConcurrentHashMap》

另一个区别是HashMap的迭代器(Iterator)是fail-fast迭代器，而Hashtable的enumerator迭代器不是fail-fast的。所以当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException，但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。这条同样也是Enumeration和Iterator的区别。

由于Hashtable是线程安全的也是synchronized，所以在单线程环境下它比HashMap要慢。如果你不需要同步，只需要单一线程，那么使用HashMap性能要好过Hashtable。

HashMap不能保证随着时间的推移Map中的元素次序是不变的。

哈希值的使用不同，HashTable直接使用对象的hashCode，代码是这样的：

```java      
int hash = key.hashCode();
int index = (hash & 0x7FFFFFFF) % tab.length;
```

而HashMap重新计算hash值，而且用与代替求模：

```java
int hash = hash(k);
int i = indexFor(hash, table.length);
```
要注意的一些重要术语：

1. sychronized意味着在一次仅有一个线程能够更改Hashtable。就是说任何线程要更新Hashtable时要首先获得同步锁，其它线程要等到同步锁被释放之后才能再次获得同步锁更新Hashtable。
2. Fail-safe和iterator迭代器相关。如果某个集合对象创建了Iterator或者ListIterator，然后其它的线程试图“结构上”更改集合对象，将会抛出ConcurrentModificationException异常。但其它线程可以通过set()方法更改集合对象是允许的，因为这并没有从“结构上”更改集合。但是假如已经从结构上进行了更改，再调用set()方法，将会抛出IllegalArgumentException异常。
3. 结构上的更改指的是删除或者插入一个元素，这样会影响到map的结构。

### 2. HashSet和HashMap的区别
---------------------------------------------------------
在分析他们的区别之前，我们首先分别来简单介绍一下他们俩。（后面我会详细的结合源码分析他俩）

* **什么是HashSet？**

HashSet实现了Set接口，它不允许集合中有重复的值，当我们提到HashSet时，第一件事情就是在将对象存储在HashSet之前，要先确保对象重写equals()和hashCode()方法，这样才能比较对象的值是否相等，以确保set中没有储存相等的对象。如果我们没有重写这两个方法，将会使用这个方法的默认实现。详见《探索equals()和hashCode()方法》。

public boolean add(Object o)方法用来在Set中添加元素，当元素值重复时则会立即返回false，如果成功添加的话会返回true。

* **什么是HashMap？**

HashMap实现了Map接口，Map接口对键值对进行映射。Map中不允许重复的键。Map接口有两个基本的实现，HashMap和TreeMap。TreeMap保存了对象的排列次序，而HashMap则不能。HashMap允许键和值为null。HashMap是非synchronized的，但collection框架提供方法能保证HashMap synchronized，这样多个线程同时访问HashMap时，能保证只有一个线程更改Map。

public Object put(Object Key,Object value)方法用来将元素添加到map中。

* **HashSet和HashMap的区别**

*HashMap*	*HashSet*

HashMap实现了Map接口	HashSet实现了Set接口

HashMap储存键值对	HashSet仅仅存储对象（且无重复对象）

使用put()方法将元素放入map中	使用add()方法将元素放入set中

HashMap中使用键对象来计算hashcode值	HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false

HashMap比较快，因为是使用唯一的键来获取对象	HashSet较HashMap来说比较慢

### 3. HashMap工作原理

实际上，HashSet 和 HashMap 之间有很多相似之处，对于 HashSet 而言，系统采用 Hash 算法决定集合元素的存储位置，这样可以保证能快速存、取集合元素；对于 HashMap 而言，系统 key-value 当成一个整体进行处理，系统总是根据 Hash 算法来计算 key-value 的存储位置，这样可以保证能快速存、取 Map 的 key-value 对。

在介绍集合存储之前需要指出一点：虽然集合号称存储的是 Java 对象，但实际上并不会真正将 Java 对象放入 Set 集合中，只是在 Set 集合中保留这些对象的引用而言。也就是说：Java 集合实际上是多个引用变量所组成的集合，这些引用变量指向实际的 Java 对象。就像引用类型的数组一样，当我们把 Java 对象放入数组之时，并不是真正的把 Java 对象放入数组中，只是把对象的引用放入数组中，每个数组元素都是一个引用变量。

**HashMap存储的实现（put()方法）**

当程序试图将多个key-value放入HashMap中是，以如下代码片段为例：

```java
HashMap<String , Double> map = new HashMap<String , Double>();   
map.put("语文" , 80.0);   
map.put("数学" , 89.0);   
map.put("英语" , 78.2);  
```

HashMap采用了一种所谓的“Hash算法”来决定每个元素的存储位置。

当程序执行map.put("语文",80.0)时，系统将调用"语文"（即Key）的hashCode()方法得到其hashCode值---每个java对象都有hashCode()方法，都可以通过该方法获得它的hashCode值。得到这个对象的hashCode值之后，系统根据hashCode值来决定 该元素的存储位置。

我们可以看HashMap类的put(K key,V value)方法的源代码：

```java
public V put(K key, V value)   
{   
    // 如果 key 为 null，调用 putForNullKey 方法进行处理  
    if (key == null)   
        return putForNullKey(value);   
    // 根据 key 的 keyCode 计算 Hash 值  
    int hash = hash(key.hashCode());   
    // 搜索指定 hash 值在对应 table 中的索引  
    int i = indexFor(hash, table.length);  
    // 如果 i 索引处的 Entry 不为 null，通过循环不断遍历 e 元素的下一个元素  
    for (Entry<K,V> e = table[i]; e != null; e = e.next)   
    {   
        Object k;   
        // 找到指定 key 与需要放入的 key 相等（hash 值相同  
        // 通过 equals 比较放回 true）  
        if (e.hash == hash && ((k = e.key) == key   
            || key.equals(k)))   
        {   
            V oldValue = e.value;   
            e.value = value;   
            e.recordAccess(this);   
            return oldValue;   
        }   
    }   
    // 如果 i 索引处的 Entry 为 null，表明此处还没有 Entry   
    modCount++;   
    // 将 key、value 添加到 i 索引处  
    addEntry(hash, key, value, i);   
    return null;   
}  
```

上面程序中用到了一个重要的内部接口：Map.Entry，每个 Map.Entry 其实就是一个 key-value 对。从上面程序中可以看出：当系统决定存储 HashMap 中的 key-value 对时，完全没有考虑 Entry 中的 value，仅仅只是根据 key 来计算并决定每个 Entry 的存储位置。这也说明了前面的结论：我们完全可以把 Map 集合中的 value 当成 key 的附属，当系统决定了 key 的存储位置之后，value 随之保存在那里即可。

上面方法提供了一个根据 hashCode() 返回值来计算 Hash 码的方法：hash()，这个方法是一个纯粹的数学计算，其方法如下：

```java
static int hash(int h) 
{ 
    h ^= (h >>> 20) ^ (h >>> 12); 
    return h ^ (h >>> 7) ^ (h >>> 4); 
}
```

对于任意给定的对象，只要它的 hashCode() 返回值相同，那么程序调用 hash(int h) 方法所计算得到的 Hash 码值总是相同的。接下来程序会调用 indexFor(int h, int length) 方法来计算该对象应该保存在 table 数组的哪个索引处。indexFor(int h, int length) 方法的代码如下：

```java
static int indexFor(int h, int length) 
{ 
    return h & (length-1); 
}
```

这个方法非常巧妙，它总是通过 h &(table.length -1) 来得到该对象的保存位置——而 HashMap 底层数组的长度总是 2 的 n 次方，这一点可参看后面关于 HashMap 构造器的介绍。

当 length 总是 2 的倍数时，h & (length-1)将是一个非常巧妙的设计：假设 h=5,length=16, 那么 h & length - 1 将得到 5；如果 h=6,length=16, 那么 h & length - 1 将得到 6 ……如果 h=15,length=16, 那么 h & length - 1 将得到 15；但是当 h=16 时 , length=16 时，那么 h & length - 1 将得到 0 了；当 h=17 时 , length=16 时，那么 h & length - 1 将得到 1 了……这样保证计算得到的索引值总是位于 table 数组的索引之内。

根据上面 put 方法的源代码可以看出，当程序试图将一个 key-value 对放入 HashMap 中时，程序首先根据该 key 的 hashCode() 返回值决定该 Entry 的存储位置：如果两个 Entry 的 key 的 hashCode() 返回值相同，那它们的存储位置相同。如果这两个 Entry 的 key 通过 equals 比较返回 true，新添加 Entry 的 value 将覆盖集合中原有 Entry 的 value，但 key 不会覆盖。如果这两个 Entry 的 key 通过 equals 比较返回 false，新添加的 Entry 将与集合中原有 Entry 形成 Entry 链，而且新添加的 Entry 位于 Entry 链的头部——具体说明继续看 addEntry() 方法的说明。

当向 HashMap 中添加 key-value 对，由其 key 的 hashCode() 返回值决定该 key-value 对（就是 Entry 对象）的存储位置。当两个 Entry 对象的 key 的 hashCode() 返回值相同时，将由 key 通过 eqauls() 比较值决定是采用覆盖行为（返回 true），还是产生 Entry 链（返回 false）。

上面程序中还调用了 addEntry(hash, key, value, i); 代码，其中 addEntry 是 HashMap 提供的一个包访问权限的方法，该方法仅用于添加一个 key-value 对。下面是该方法的代码：

```java
void addEntry(int hash, K key, V value, int bucketIndex) 
{ 
    // 获取指定 bucketIndex 索引处的 Entry 
    Entry<K,V> e = table[bucketIndex]; 	 // ①
    // 将新创建的 Entry 放入 bucketIndex 索引处，并让新的 Entry 指向原来的 Entry 
    table[bucketIndex] = new Entry<K,V>(hash, key, value, e); 
    // 如果 Map 中的 key-value 对的数量超过了极限
    if (size++ >= threshold) 
        // 把 table 对象的长度扩充到 2 倍。
        resize(2 * table.length); 	 // ②
}
```

上面方法的代码很简单，但其中包含了一个非常优雅的设计：系统总是将新添加的 Entry 对象放入 table 数组的 bucketIndex 索引处——如果 bucketIndex 索引处已经有了一个 Entry 对象，那新添加的 Entry 对象指向原有的 Entry 对象（产生一个 Entry 链），如果 bucketIndex 索引处没有 Entry 对象，也就是上面程序①号代码的 e 变量是 null，也就是新放入的 Entry 对象指向 null，也就是没有产生 Entry 链。

**什么是Map.Entry？**

```java
Map是java中的接口，Map.Entry是Map的一个内部接口。  
         Map提供了一些常用方法，如keySet()、entrySet()等方法，keySet()方法返回值是Map中key值的集合；entrySet()的返回值也是返回一个Set集合，此集合的类型为Map.Entry。  
         Map.Entry是Map声明的一个内部接口，此接口为泛型，定义为Entry<K,V>。它表示Map中的一个实体（一个key-value对）。接口中有getKey(),getValue方法。  
           
        由以上可以得出，遍历Map的常用方法：  
       1.  Map map = new HashMap();  
           Irerator iterator = map.entrySet().iterator();  
           while(iterator.hasNext()) {  
                   Map.Entry entry = iterator.next();  
                   Object key = entry.getKey();  
                   //  
           }  
       2.Map map = new HashMap();   
           Set  keySet= map.keySet();  
           Irerator iterator = keySet.iterator;  
           while(iterator.hasNext()) {  
                   Object key = iterator.next();  
                   Object value = map.get(key);  
                   //  
           }  
   
       另外，还有一种遍历方法是，单纯的遍历value值，Map有一个values方法，返回的是value的Collection集合。通过遍历collection也可以遍历value,如  
      Map map = new HashMap();  
      Collection c = map.values();  
      Iterator iterator = c.iterator();  
      while(iterator.hasNext()) {  
             Object value = iterator.next();   
} 
```

Map.Entry是Map内部定义的一个接口，专门用来保存key→value的内容。Map.Entry的定义如下：

    public static interface Map.Entry<K,V> 

Map.Entry是使用static关键字声明的内部接口，此接口可以由外部通过"外部类.内部类"的形式直接调用。在本接口中提供了如表13-12所示的方法。

表13-12  Map.Entry接口的常用方法

1. public boolean equals(Object o) 对象比较
2. public K getKey() 取得key
3. public V getValue() 取得value
4. public int hashCode() 返回哈希码
5. public V setValue(V value) 设置value的值

从之前的内容可以知道，在Map的操作中，所有的内容都是通过key→value的形式保存数据的，那么对于集合来讲，实际上是将key→value的数据保存在了Map.Entry的实例之后，再在Map集合中插入的是一个Map.Entry的实例化对象，如图13-4所示。
 

图13-4  Map与Map.Entry
【图】

U提示：Map.Entry在集合输出时会使用到。

在一般的Map操作中（例如，增加或取出数据等操作）不用去管Map.Entry接口，但是在将Map中的数据全部输出时就必须使用Map.Entry接口

**Hash 算法的性能选项**

根据上面代码可以看出，在同一个 bucket 存储 Entry 链的情况下，新放入的 Entry 总是位于 bucket 中，而最早放入该 bucket 中的 Entry 则位于这个 Entry 链的最末端。

上面程序中还有这样两个变量：

size：该变量保存了该 HashMap 中所包含的 key-value 对的数量。

threshold：该变量包含了 HashMap 能容纳的 key-value 对的极限，它的值等于 HashMap 的容量乘以负载因子（load factor）。

从上面程序中②号代码可以看出，当 size++ >= threshold 时，HashMap 会自动调用 resize 方法扩充 HashMap 的容量。每扩充一次，HashMap 的容量就增大一倍。

上面程序中使用的 table 其实就是一个普通数组，每个数组都有一个固定的长度，这个数组的长度就是 HashMap 的容量。HashMap 包含如下几个构造器：

HashMap()：构建一个初始容量为 16，负载因子为 0.75 的 HashMap。

HashMap(int initialCapacity)：构建一个初始容量为 initialCapacity，负载因子为 0.75 的 HashMap。

HashMap(int initialCapacity, float loadFactor)：以指定初始容量、指定的负载因子创建一个 HashMap。

当创建一个 HashMap 时，系统会自动创建一个 table 数组来保存 HashMap 中的 Entry，下面是 HashMap 中一个构造器的代码：

```java 
 // 以指定初始化容量、负载因子创建 HashMap 
 public HashMap(int initialCapacity, float loadFactor) 
 { 
	 // 初始容量不能为负数
	 if (initialCapacity < 0) 
		 throw new IllegalArgumentException( 
		"Illegal initial capacity: " + 
			 initialCapacity); 
	 // 如果初始容量大于最大容量，让出示容量
	 if (initialCapacity > MAXIMUM_CAPACITY) 
		 initialCapacity = MAXIMUM_CAPACITY; 
	 // 负载因子必须大于 0 的数值
	 if (loadFactor <= 0 || Float.isNaN(loadFactor)) 
		 throw new IllegalArgumentException( 
		 loadFactor); 
	 // 计算出大于 initialCapacity 的最小的 2 的 n 次方值。
	 int capacity = 1; 
	 while (capacity < initialCapacity) 
		 capacity <<= 1; 
	 this.loadFactor = loadFactor; 
	 // 设置容量极限等于容量 * 负载因子
	 threshold = (int)(capacity * loadFactor); 
	 // 初始化 table 数组
	 table = new Entry[capacity]; 			 // ①
	 init(); 
 }
 ```

上面代码中粗体字代码包含了一个简洁的代码实现：找出大于 initialCapacity 的、最小的 2 的 n 次方值，并将其作为 HashMap 的实际容量（由 capacity 变量保存）。例如给定 initialCapacity 为 10，那么该 HashMap 的实际容量就是 16。

initialCapacity 与 HashTable 的容量

创建 HashMap 时指定的 initialCapacity 并不等于 HashMap 的实际容量，通常来说，HashMap 的实际容量总比 initialCapacity 大一些，除非我们指定的 initialCapacity 参数值恰好是 2 的 n 次方。当然，掌握了 HashMap 容量分配的知识之后，应该在创建 HashMap 时将 initialCapacity 参数值指定为 2 的 n 次方，这样可以减少系统的计算开销。

程序①号代码处可以看到：table 的实质就是一个数组，一个长度为 capacity 的数组。

对于 HashMap 及其子类而言，它们采用 Hash 算法来决定集合中元素的存储位置。当系统开始初始化 HashMap 时，系统会创建一个长度为 capacity 的 Entry 数组，这个数组里可以存储元素的位置被称为“桶（bucket）”，每个 bucket 都有其指定索引，系统可以根据其索引快速访问该 bucket 里存储的元素。

无论何时，HashMap 的每个“桶”只存储一个元素（也就是一个 Entry），由于 Entry 对象可以包含一个引用变量（就是 Entry 构造器的的最后一个参数）用于指向下一个 Entry，因此可能出现的情况是：HashMap 的 bucket 中只有一个 Entry，但这个 Entry 指向另一个 Entry ——这就形成了一个 Entry 链。如图 1 所示：

图 1. HashMap 的存储示意

key相同的则产生链。

**HashMap 的读取实现**

当 HashMap 的每个 bucket 里存储的 Entry 只是单个 Entry ——也就是没有通过指针产生 Entry 链时，此时的 HashMap 具有最好的性能：当程序通过 key 取出对应 value 时，系统只要先计算出该 key 的 hashCode() 返回值，在根据该 hashCode 返回值找出该 key 在 table 数组中的索引，然后取出该索引处的 Entry，最后返回该 key 对应的 value 即可。看 HashMap 类的 get(K key) 方法代码：

```java
 public V get(Object key) 
 { 
	 // 如果 key 是 null，调用 getForNullKey 取出对应的 value 
	 if (key == null) 
		 return getForNullKey(); 
	 // 根据该 key 的 hashCode 值计算它的 hash 码
	 int hash = hash(key.hashCode()); 
	 // 直接取出 table 数组中指定索引处的值，
	 for (Entry<K,V> e = table[indexFor(hash, table.length)]; 
		 e != null; 
		 // 搜索该 Entry 链的下一个 Entr 
		 e = e.next) 		 // ①
	 { 
		 Object k; 
		 // 如果该 Entry 的 key 与被搜索 key 相同
		 if (e.hash == hash && ((k = e.key) == key 
			 || key.equals(k))) 
			 return e.value; 
	 } 
	 return null; 
 }
```

从上面代码中可以看出，如果 HashMap 的每个 bucket 里只有一个 Entry 时，HashMap 可以根据索引、快速地取出该 bucket 里的 Entry；在发生“Hash 冲突”的情况下，单个 bucket 里存储的不是一个 Entry，而是一个 Entry 链，系统只能必须按顺序遍历每个 Entry，直到找到想搜索的 Entry 为止——如果恰好要搜索的 Entry 位于该 Entry 链的最末端（该 Entry 是最早放入该 bucket 中），那系统必须循环到最后才能找到该元素。

归纳起来简单地说，HashMap 在底层将 key-value 当成一个整体进行处理，这个整体就是一个 Entry 对象。HashMap 底层采用一个 Entry[] 数组来保存所有的 key-value 对，当需要存储一个 Entry 对象时，会根据 Hash 算法来决定其存储位置；当需要取出一个 Entry 时，也会根据 Hash 算法找到其存储位置，直接取出该 Entry。由此可见：HashMap 之所以能快速存、取它所包含的 Entry，完全类似于现实生活中母亲从小教我们的：不同的东西要放在不同的位置，需要时才能快速找到它。

当创建 HashMap 时，有一个默认的负载因子（load factor），其默认值为 0.75，这是时间和空间成本上一种折衷：增大负载因子可以减少 Hash 表（就是那个 Entry 数组）所占用的内存空间，但会增加查询数据的时间开销，而查询是最频繁的的操作（HashMap 的 get() 与 put() 方法都要用到查询）；减小负载因子会提高数据查询的性能，但会增加 Hash 表所占用的内存空间。

掌握了上面知识之后，我们可以在创建 HashMap 时根据实际需要适当地调整 load factor 的值；如果程序比较关心空间开销、内存比较紧张，可以适当地增加负载因子；如果程序比较关心时间开销，内存比较宽裕则可以适当的减少负载因子。通常情况下，程序员无需改变负载因子的值。

如果开始就知道 HashMap 会保存多个 key-value 对，可以在创建时就使用较大的初始化容量，如果 HashMap 中 Entry 的数量一直不会超过极限容量（capacity * load factor），HashMap 就无需调用 resize() 方法重新分配 table 数组，从而保证较好的性能。当然，开始就将初始容量设置太高可能会浪费空间（系统需要创建一个长度为 capacity 的 Entry 数组），因此创建 HashMap 时初始化容量设置也需要小心对待。


### 4. HashSet工作原理

对于 HashSet 而言，它是基于 HashMap 实现的，HashSet 底层采用 HashMap 来保存所有元素，因此 HashSet 的实现比较简单，查看 HashSet 的源代码，可以看到如下代码：

```java 
 public class HashSet<E> 
	 extends AbstractSet<E> 
	 implements Set<E>, Cloneable, java.io.Serializable 
 { 
	 // 使用 HashMap 的 key 保存 HashSet 中所有元素
	 private transient HashMap<E,Object> map; 
	 // 定义一个虚拟的 Object 对象作为 HashMap 的 value 
	 private static final Object PRESENT = new Object(); 
	 ... 
	 // 初始化 HashSet，底层会初始化一个 HashMap 
	 public HashSet() 
	 { 
		 map = new HashMap<E,Object>(); 
	 } 
	 // 以指定的 initialCapacity、loadFactor 创建 HashSet 
	 // 其实就是以相应的参数创建 HashMap 
	 public HashSet(int initialCapacity, float loadFactor) 
	 { 
		 map = new HashMap<E,Object>(initialCapacity, loadFactor); 
	 } 
	 public HashSet(int initialCapacity) 
	 { 
		 map = new HashMap<E,Object>(initialCapacity); 
	 } 
	 HashSet(int initialCapacity, float loadFactor, boolean dummy) 
	 { 
		 map = new LinkedHashMap<E,Object>(initialCapacity 
			 , loadFactor); 
	 } 
	 // 调用 map 的 keySet 来返回所有的 key 
	 public Iterator<E> iterator() 
	 { 
		 return map.keySet().iterator(); 
	 } 
	 // 调用 HashMap 的 size() 方法返回 Entry 的数量，就得到该 Set 里元素的个数
	 public int size() 
	 { 
		 return map.size(); 
	 } 
	 // 调用 HashMap 的 isEmpty() 判断该 HashSet 是否为空，
	 // 当 HashMap 为空时，对应的 HashSet 也为空
	 public boolean isEmpty() 
	 { 
		 return map.isEmpty(); 
	 } 
	 // 调用 HashMap 的 containsKey 判断是否包含指定 key 
	 //HashSet 的所有元素就是通过 HashMap 的 key 来保存的
	 public boolean contains(Object o) 
	 { 
		 return map.containsKey(o); 
	 } 
	 // 将指定元素放入 HashSet 中，也就是将该元素作为 key 放入 HashMap 
	 public boolean add(E e) 
	 { 
		 return map.put(e, PRESENT) == null; 
	 } 
	 // 调用 HashMap 的 remove 方法删除指定 Entry，也就删除了 HashSet 中对应的元素
	 public boolean remove(Object o) 
	 { 
		 return map.remove(o)==PRESENT; 
	 } 
	 // 调用 Map 的 clear 方法清空所有 Entry，也就清空了 HashSet 中所有元素
	 public void clear() 
	 { 
		 map.clear(); 
	 } 
	 ... 
 }
```

由上面源程序可以看出，HashSet 的实现其实非常简单，它只是封装了一个 HashMap 对象来存储所有的集合元素，所有放入 HashSet 中的集合元素实际上由 HashMap 的 key 来保存，而 HashMap 的 value 则存储了一个 PRESENT，它是一个静态的 Object 对象。

HashSet 的绝大部分方法都是通过调用 HashMap 的方法来实现的，因此 HashSet 和 HashMap 两个集合在实现本质上是相同的。

**HashMap 的 put 与 HashSet 的 add**

由于 HashSet 的 add() 方法添加集合元素时实际上转变为调用 HashMap 的 put() 方法来添加 key-value 对，当新放入 HashMap 的 Entry 中 key 与集合中原有 Entry 的 key 相同（hashCode() 返回值相等，通过 equals 比较也返回 true），新添加的 Entry 的 value 将覆盖原来 Entry 的 value，但 key 不会有任何改变，因此如果向 HashSet 中添加一个已经存在的元素，新添加的集合元素（底层由 HashMap 的 key 保存）不会覆盖已有的集合元素。

掌握上面理论知识之后，接下来看一个示例程序，测试一下自己是否真正掌握了 HashMap 和 HashSet 集合的功能。

下面这个程序其实，我在上篇博客《探索equals()和hashCode()方法》中已经讲得很清楚了，但是由于比较重要，我就再把他写一遍。主要说明的就是重写equals()方法时，就必须重写hashCode()方法。

```java
 class Name
{
    private String first; 
    private String last; 
    
    public Name(String first, String last) 
    { 
        this.first = first; 
        this.last = last; 
    } 

    public boolean equals(Object o) 
    { 
        if (this == o) 
        { 
            return true; 
        } 
        
	if (o.getClass() == Name.class) 
        { 
            Name n = (Name)o; 
            return n.first.equals(first) 
                && n.last.equals(last); 
        } 
        return false; 
    } 
}

public class HashSetTest
{
    public static void main(String[] args)
    { 
        Set<Name> s = new HashSet<Name>();
        s.add(new Name("abc", "123"));
        System.out.println(
            s.contains(new Name("abc", "123")));
    }
}
```

上面程序中向 HashSet 里添加了一个 new Name("abc", "123") 对象之后，立即通过程序判断该 HashSet 是否包含一个 new Name("abc", "123") 对象。粗看上去，很容易以为该程序会输出 true。

实际运行上面程序将看到程序输出 false，这是因为 HashSet 判断两个对象相等的标准除了要求通过 equals() 方法比较返回 true 之外，还要求两个对象的 hashCode() 返回值相等。而上面程序没有重写 Name 类的 hashCode() 方法，两个 Name 对象的 hashCode() 返回值并不相同，因此 HashSet 会把它们当成 2 个对象处理，因此程序返回 false。

由此可见，当我们试图把某个类的对象当成 HashMap 的 key，或试图将这个类的对象放入 HashSet 中保存时，重写该类的 equals(Object obj) 方法和 hashCode() 方法很重要，而且这两个方法的返回值必须保持一致：当该类的两个的 hashCode() 返回值相同时，它们通过 equals() 方法比较也应该返回 true。通常来说，所有参与计算 hashCode() 返回值的关键属性，都应该用于作为 equals() 比较的标准。

**hashCode() 和 equals()** 详见《探索equals()和hashCode()方法》

如下程序就正确重写了 Name 类的 hashCode() 和 equals() 方法，程序如下：

```java
class Name 
{ 
    private String first;
    private String last;
    public Name(String first, String last)
    { 
        this.first = first; 
        this.last = last; 
    } 
    // 根据 first 判断两个 Name 是否相等
    public boolean equals(Object o) 
    { 
        if (this == o) 
        { 
            return true; 
        } 
        if (o.getClass() == Name.class) 
        { 
            Name n = (Name)o; 
            return n.first.equals(first); 
        } 
        return false; 
    } 
	 
    // 根据 first 计算 Name 对象的 hashCode() 返回值
    public int hashCode() 
    { 
        return first.hashCode(); 
    }

    public String toString() 
    { 
        return "Name[first=" + first + ", last=" + last + "]"; 
    } 
 } 
 
 public class HashSetTest2 
 { 
    public static void main(String[] args) 
    { 
        HashSet<Name> set = new HashSet<Name>(); 
        set.add(new Name("abc" , "123")); 
        set.add(new Name("abc" , "456")); 
        System.out.println(set); 
    } 
}
```

上面程序中提供了一个 Name 类，该 Name 类重写了 equals() 和 toString() 两个方法，这两个方法都是根据 Name 类的 first 实例变量来判断的，当两个 Name 对象的 first 实例变量相等时，这两个 Name 对象的 hashCode() 返回值也相同，通过 equals() 比较也会返回 true。

程序主方法先将第一个 Name 对象添加到 HashSet 中，该 Name 对象的 first 实例变量值为"abc"，接着程序再次试图将一个 first 为"abc"的 Name 对象添加到 HashSet 中，很明显，此时没法将新的 Name 对象添加到该 HashSet 中，因为此处试图添加的 Name 对象的 first 也是" abc"，HashSet 会判断此处新增的 Name 对象与原有的 Name 对象相同，因此无法添加进入，程序在①号代码处输出 set 集合时将看到该集合里只包含一个 Name 对象，就是第一个、last 为"123"的 Name 对象。

### 5. 常见问题

**“你知道HashMap的工作原理吗？” “你知道HashMap的get()方法的工作原理吗？”**

答：“HashMap是基于hashing的原理，我们使用put(key, value)存储对象到HashMap中，使用get(key)从HashMap中获取对象。当我们给put()方法传递键和值时，我们先对键调用hashCode()方法，返回的hashCode用于找到bucket位置来储存Entry对象。”这里关键点在于指出，HashMap是在bucket中储存键对象和值对象，作为Map.Entry。这一点有助于理解获取对象的逻辑。如果你没有意识到这一点，或者错误的认为仅仅只在bucket中存储值的话，你将不会回答如何从HashMap中获取对象的逻辑。这个答案相当的正确，也显示出面试者确实知道hashing以及HashMap的工作原理。

**“当两个对象的hashcode相同会发生什么？”**

从这里开始，真正的困惑开始了，一些面试者会回答因为hashcode相同，所以两个对象是相等的，HashMap将会抛出异常，或者不会存储它们。然后面试官可能会提醒他们有equals()和hashCode()两个方法，并告诉他们两个对象就算hashcode相同，但是它们可能并不相等。一些面试者可能就此放弃，而另外一些还能继续挺进，他们回答“因为hashcode相同，所以它们的bucket位置相同，‘碰撞’会发生。因为HashMap使用链表存储对象，这个Entry(包含有键值对的Map.Entry对象)会存储在链表中。”这个答案非常的合理，虽然有很多种处理碰撞的方法，这种方法是最简单的，也正是HashMap的处理方法。但故事还没有完结，面试官会继续问：

**“如果两个键的hashcode相同，你如何获取值对象？”**

面试者会回答：当我们调用get()方法，HashMap会使用键对象的hashcode找到bucket位置，然后获取值对象。面试官提醒他如果有两个值对象储存在同一个bucket，他给出答案:将会遍历链表直到找到值对象。面试官会问因为你并没有值对象去比较，你是如何确定确定找到值对象的？除非面试者直到HashMap在链表中存储的是键值对，否则他们不可能回答出这一题。

其中一些记得这个重要知识点的面试者会说，找到bucket位置之后，会调用keys.equals()方法去找到链表中正确的节点，最终找到要找的值对象。完美的答案！

许多情况下，面试者会在这个环节中出错，因为他们混淆了hashCode()和equals()方法。因为在此之前hashCode()屡屡出现，而equals()方法仅仅在获取值对象的时候才出现。一些优秀的开发者会指出使用不可变的、声明作final的对象，并且采用合适的equals()和hashCode()方法的话，将会减少碰撞的发生，提高效率。不可变性使得能够缓存不同键的hashcode，这将提高整个获取对象的速度，使用String，Interger这样的wrapper类作为键是非常好的选择。

如果你认为到这里已经完结了，那么听到下面这个问题的时候，你会大吃一惊。

**“如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？”**

除非你真正知道HashMap的工作原理，否则你将回答不出这道题。默认的负载因子大小为0.75，也就是说，当一个map填满了75%的bucket时候，和其它集合类(如ArrayList等)一样，将会创建原来HashMap大小的两倍的bucket数组，来重新调整map的大小，并将原来的对象放入新的bucket数组中。这个过程叫作rehashing，因为它调用hash方法找到新的bucket位置。如果你能够回答这道问题，下面的问题来了：

**“你了解重新调整HashMap大小存在什么问题吗？”**

你可能回答不上来，这时面试官会提醒你当多线程的情况下，可能产生条件竞争(race condition)。

当重新调整HashMap大小的时候，确实存在条件竞争，因为如果两个线程都发现HashMap需要重新调整大小了，它们会同时试着调整大小。在调整大小的过程中，存储在链表中的元素的次序会反过来，因为移动到新的bucket位置的时候，HashMap并不会将元素放在链表的尾部，而是放在头部，这是为了避免尾部遍历(tail traversing)。如果条件竞争发生了，那么就死循环了。这个时候，你可以质问面试官，为什么这么奇怪，要在多线程的环境下使用HashMap呢？：）


**“为什么String, Interger这样的wrapper类适合作为键？”**

 String, Interger这样的wrapper类作为HashMap的键是再适合不过了，而且String最为常用。因为String是不可变的，也是final的，而且已经重写了equals()和hashCode()方法了。其他的wrapper类也有这个特点。不可变性是必要的，因为为了要计算hashCode()，就要防止键值改变，如果键值在放入时和获取时返回不同的hashcode的话，那么就不能从HashMap中找到你想要的对象。不可变性还有其他的优点如线程安全。如果你可以仅仅通过将某个field声明成final就能保证hashCode是不变的，那么请这么做吧。因为获取对象的时候要用到equals()和hashCode()方法，那么键对象正确的重写这两个方法是非常重要的。如果两个不相等的对象返回不同的hashcode的话，那么碰撞的几率就会小些，这样就能提高HashMap的性能。

**“我们可以使用自定义的对象作为键吗？ ”**

这是前一个问题的延伸。当然你可能使用任何对象作为键，只要它遵守了equals()和hashCode()方法的定义规则，并且当对象插入到Map中之后将不会再改变了。如果这个自定义对象时不可变的，那么它已经满足了作为键的条件，因为当它创建之后就已经不能改变了。

**“我们可以使用CocurrentHashMap来代替Hashtable吗？”**

这是另外一个很热门的面试题，因为ConcurrentHashMap越来越多人用了。我们知道Hashtable是synchronized的，但是ConcurrentHashMap同步性能更好，因为它仅仅根据同步级别对map的一部分进行上锁。ConcurrentHashMap当然可以代替HashTable，但是HashTable提供更强的线程安全性。看看查看《HashMap Vs ConcurrentHashMap》Hashtable和

ConcurrentHashMap的区别。

这些问题设计哪些知识点：

* hashing的概念
* HashMap中解决碰撞的方法
* equals()和hashCode()的应用，以及它们在HashMap中的重要性
* 不可变对象的好处
* HashMap多线程的条件竞争
* 重新调整HashMap的大小

好吧今天的HashSet和HashMap就告一段落了，明天讲TreeSet和TreeMap。顺便介绍介绍一篇博客给大家《20道最常见的java问题(电子商务方向)》。改天我也研究研究。
