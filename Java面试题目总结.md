# Java基础、中间件基础

## 为什么重写equals()方法就必须重写hashCode()方法?
+ equals()方法是用于比较对象引用是否指向同一块内存空间，对象重写equals()方法通常是为了在业务逻辑中比较对象值是否相同；hashcode()方法具有散列表能快速检索的特性，因此引入来提高比较对象时程序运行的效率。在使用的时候，先比较两个对象的hashcode()方法返回值，如果不同，则不需要再使用equals()方法进行比较，如果相同，再使用equal()方法继续比较。这里需要保证一个原则，hashcode()方法和equals()方法返回结果要一致（比如对于存储散列表的集合set（去重效果）来说，如果不重写hashcode()方法，因为hashcode()值不同，对象会被保存进去，但是业务逻辑上相等的两个对象是不能放入其中的，这样就会引起混乱），如何保证？------->重写hashcode()方法

## Object有哪些方法?
**jdk1.5中部分方法如下：**
+ **getclass()方法**，`返回一个对象的运行时类`。该 Class 对象是由所表示类的 static synchronized 方法锁定的对象。
+ **hashCode()方法**，`返回该对象的哈希码值`。支持该方法是为哈希表提供一些优点，例如，java.util.Hashtable 提供的哈希表。
+ **equals()方法**，如果此对象与 obj 参数相同，则返回 true；否则返回 false。
+ **clone()方法**，`返回此实例的一个克隆`。
+ **toString()方法**，Object 类的 toString 方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at 标记符“@”和此对象哈希码的无符号十六进制表示组成。换句话说，该方法返回一个字符串，它的值等于：getClass().getName() + '@' + Integer.toHexString(hashCode())，返回该对象的字符串表示形式。
+ **notify()方法**，`唤醒在此对象监视器上等待的单个线程`。如果所有线程都在此对象上等待，则会选择唤醒其中一个线程。选择是任意性的，并在对实现做出决定时发生。线程通过调用其中一个 wait 方法，在对象的监视器上等待。直到当前的线程放弃此对象上的锁定，才能继续执行被唤醒的线程。被唤醒的线程将以常规方式与在该对象上主动同步的其他所有线程进行竞争；例如，唤醒的线程在作为锁定此对象的下一个线程方面没有可靠的特权或劣势。
+ **notifyAll()方法**，`唤醒在此对象监视器上等待的所有线程`。线程通过调用其中一个 wait 方法，在对象的监视器上等待。直到当前的线程放弃此对象上的锁定，才能继续执行被唤醒的线程。被唤醒的线程将以常规方式与在该对象上主动同步的其他所有线程进行竞争；例如，唤醒的线程在作为锁定此对象的下一个线程方面没有可靠的特权或劣势。
+ **finalize()方法**，当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。子类重写 finalize 方法，以配置系统资源或执行其他清除。
+ **wait()方法**，导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量。当前的线程必须拥有此对象监视器。

## 接口和抽象类的区别，什么情况下用接口或抽象类?
### 区别
+ 抽象类是用来`捕捉子类的通用特性`的 ,它`不能被实例化`，只能被用作子类的（父类）超类，抽象类`被用来作为子类的模板使用`。接口是抽象方法的集合,如果一个类实现了某个接口，那么它就继承了这个接口的抽象方法，这就像契约模式，如果实现了这个接口，那么就`必须确保使用这些方法。接口只是一种形式，接口自身不能做任何事情`。
### 什么时候使用抽象类和接口？
抽象类是对一种事物的抽象，即对**类**抽象，而接口是对**行为**的抽象。
+ 如果你拥有一些方法并且想让它们中的一些有*默认实现*，那么使用抽象类吧。
+ 如果你想*实现多重继承*，那么你必须使用接口。由于Java不支持多继承，子类不能够继承多个类，但可以实现多个接口。因此你就可以使用接口来解决它。
+ 如果基本功能在不断改变，那么就需要使用抽象类。如果不断改变基本功能并且使用接口，那么就需要改变所有实现了该接口的类。

## 为什么String设计成不可变？String 和 StringBuilder、StringBuffer 的区别？
[java中String、StringBuffer和StringBuilder的区别(简单介绍)](https://www.cnblogs.com/weibanggang/p/9455926.html)
### 设计成不可变的好处（设计考虑、效率优化、安全性）
`String类使用final关键字字符数组保存字符串，所以String的对象是不可变的`。
+ 便于实现字符串池
  + 在Java中，由于会大量的使用String常量，`如果每一次声明一个String都创建一个String对象，那将会造成极大的空间资源的浪费`。Java提出了String pool的概念，在堆中开辟一块存储空间String pool，当初始化一个String变量时，如果该字符串已经存在了，就不会去创建一个新的字符串变量，而是会返回已经存在了的字符串的引用。如果字符串是可变的，某一个字符串变量改变了其值，那么其指向的变量的值也会改变，String pool将不能够实现！
+ 使多线程安全
  + `不可变对象不能被写，所以保证了多线程的安全`。
+ 避免安全问题
  + `在网络连接和数据库连接中字符串常常作为参数`，例如，网络连接地址URL，文件路径path，反射机制所需要的String参数。`其不可变性可以保证连接的安全性`。如果字符串是可变的，黑客就有可能改变字符串指向对象的值，那么会引起很严重的安全问题。因为String是不可变的，所以它的值是不可改变的。但由于String不可变，也就没有任何方式能修改字符串的值，每一次修改都将产生新的字符串，如果使用char[]来保存密码，仍然能够将其中所有的元素设置为空和清零，也不会被放入字符串缓存池中，用字符串数组来保存密码会更好。
+ 加快字符串处理速度
  + 由于String是不可变的，保证了hashcode的唯一性，于是在创建对象时其hashcode就可以放心的缓存了，不需要重新计算。这也就是Map喜欢将String作为Key的原因，处理速度要快过其它的键对象。所以HashMap中的键往往都使用String。
### 联系和区别：
+ 三者共同之处:
  都是final类,不允许被继承，主要是从性能和安全性上考虑的，因为这几个类都是经常被使用着，且考虑到防止其中的参数被参数修改影响到其他的应用
  + StringBuffer是线程安全，可以不需要额外的同步用于多线程中;
  + StringBuilder是非同步,运行于多线程中就需要使用着单独同步处理，但是速度就比StringBuffer快多了;
  + StringBuffer与StringBuilder两者共同之处:可以通过append、indert进行字符串的操作。
  + String实现了三个接口:Serializable、Comparable<String>、CarSequence
  + StringBuilder只实现了两个接口Serializable、CharSequence，相比之下String的实例可以通过compareTo方法进行比较，其他两个不可以。
+ 区别：
  + 首先说运行速度，或者说是执行速度，在这方面运行速度快慢为：StringBuilder > StringBuffer > String；String最慢的原因：String为字符串常量，而StringBuilder和StringBuffer均为字符串变量，即String对象一旦创建之后该对象是不可更改的，但后两者的对象是变量，是可以更改的。
  + 再来说线程安全，在线程安全上，StringBuilder是线程不安全的，而StringBuffer是线程安全的。如果一个StringBuffer对象在字符串缓冲区被多个线程使用时，StringBuffer中很多方法可以带有synchronized关键字，所以可以保证线程是安全的，但StringBuilder的方法则没有该关键字，所以不能保证线程安全，有可能会出现一些错误的操作。所以如果要进行的操作是多线程的，那么就要使用StringBuffer，但是在单线程的情况下，还是建议使用速度比较快的StringBuilder。（一个线程访问一个对象中的synchronized(this)同步代码块时，其他试图访问该对象的线程将被阻塞）
  + String：适用于`少量的字符串操作`的情况
  + StringBuilder：适用于`单线程下在字符缓冲区进行大量操作`的情况
  + StringBuffer：适用`多线程下在字符缓冲区进行大量操作`的情况

## 使用String s=new String(“abc”) 和 String s="abc" 的区别?
`不要使用new String("some string") 方式构造字符串`
+ 要点说明:
  + 创建字符串时不要使用new String("some string")，此处`会产生2个对象`，在Java中，"some String"默认就已经创建了一个字符串，如果再用new String("some string")，将会创建出另外一个字符串，造成浪费。
  + "abc"本身就是一个字符串对象，而在运行时执行String s = new String("abc")时，"abc"这个对象被复制一份到heap中，并且把heap中的对象引用交给s持有。这条语句创建了两个String对象。

## Arraylist、HashMap的初始容量、加载因子、扩容增量？
+ ArrayList()：可以构造一个**默认初始容量为10**的空列表，每次扩容后容量**增长50%**
+ HashMap()：构建一个**默认初始容量为 16**，**加载因子为 0.75**的 HashMap。
+ 当HashMap中的元素个数超过数组大小 $*$ loadFactor时，就会进行数组扩容，loadFactor的默认值为0.75，这是一个折中的取值。也就是说，默认情况下，数组大小为16，那么当HashMap中元素个数超过16 $*$ 0.75=12的时候，就把数组的大小扩展为 2 $*$ 16=32，即**扩大一倍**，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以如果我们已经预知HashMap中元素的个数，那么预设元素的个数能够有效的提高HashMap的性能。

## 有序的Map有哪些？为什么TreeMap是有序的？哪些集合是线程安全的？
+ 有序Map：Map主要用于存储健值对，根据键得到值，因此不允许键重复，但允许值重复。使用的时候key和value的数据类型可以相同。也可以不同。Map接口的主要实现类有HashMap（无序）、**LinkedHashMap（有序）、TreeMap（有序，按照key的字典升序排序）**、ConcurrentHashMap。
+ 为什么TreeMap有序：**TreeMap底层是基于红黑树实现的排序Map**，为了进行排序，TreeMap要求放入的key必须实现Comparable接口，作为Value的对象则没有任何要求。如果作为key的类没有实现Comparable接口，必须在创建TreeMap时同时自定义Comparator比较器。
+ 线程安全和线程不安全的集合
  + Vector、HashTable、Properties、Collections包装方法、java.util.concurrent包中的集合都是是线程安全的（详见*Java线程安全的集合详解*）
    + Vector
      + Vector和ArrayList类似，是长度可变的数组，与ArrayList不同的是，Vector是线程安全的，它给几乎所有的public方法都加上了synchronized关键字。由于加锁导致性能降低，在不需要并发访问同一对象时，这种强制性的同步机制就显得多余，所以现在Vector已被弃用
    + HashTable
      + HashTable和HashMap类似，不同点是HashTable是线程安全的，它给几乎所有public方法都加上了synchronized关键字，还有一个不同点是HashTable的K，V都不能是null，但HashMap可以，它现在也因为性能原因被弃用了
    + Collections包装方法
      + Vector和HashTable被弃用后，它们被ArrayList和HashMap代替，但它们不是线程安全的，所以Collections工具类中提供了相应的包装方法把它们包装成线程安全的集合。Collections针对每种集合都声明了一个线程安全的包装类，`在原集合的基础上添加了锁对象`，集合中的每个方法都通过这个锁对象实现同步
    + java.util.concurrent包中的集合
      + ConcurrentHashMap
        + ConcurrentHashMap和HashTable都是线程安全的集合，它们的不同主要是加锁粒度上的不同。HashTable的加锁方法是给每个方法加上synchronized关键字，这样锁住的是整个Table对象。而ConcurrentHashMap是更细粒度的加锁。在JDK1.8之前，ConcurrentHashMap加的是分段锁，也就是Segment锁，每个Segment含有整个table的一部分，这样不同分段之间的并发操作就互不影响。JDK1.8对此做了进一步的改进，它取消了Segment字段，直接在table元素上加锁，实现对每一行进行加锁，进一步减小了并发冲突的概率。
      + CopyOnWriteArrayList和CopyOnWriteArraySet
        + 它们是加了写锁的ArrayList和ArraySet，锁住的是整个对象，但读操作可以并发执行
      + 包下的其他类
        + 除此之外还有ConcurrentSkipListMap、ConcurrentSkipListSet、ConcurrentLinkedQueue、ConcurrentLinkedDeque等，至于为什么没有ConcurrentArrayList，原因是无法设计一个通用的而且可以规避ArrayList的并发瓶颈的线程安全的集合类，只能锁住整个list，这用Collections里的包装类就能办到
  + ArrayList、LinkedList、HashSet、TreeSet、HashMap、TreeMap等都是线程不安全的。
  + [Java线程安全的集合详解](https://blog.csdn.net/lixiaobuaa/article/details/79689338)
  + [JDK 提供的并发容器总结](https://github.com/Snailclimb/JavaGuide/blob/master/docs/java/multi-thread/%E5%B9%B6%E5%8F%91%E5%AE%B9%E5%99%A8%E6%80%BB%E7%BB%93.md)

## HashMap的底层数据结构，是如何插入的？哈希冲突解决方案？为什么是非线程安全的？
+ [HashMap(JDK1.8)源码+底层数据结构分析](https://github.com/Snailclimb/JavaGuide/blob/master/docs/java/collection/HashMap(JDK1.8)%E6%BA%90%E7%A0%81+%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E5%88%86%E6%9E%90.md)
+ 底层数据结构分析和解决冲突方法
  + HashMap 可以存储 null 的 key 和 value，但 null 作为键只能有一个，null 作为值可以有多个。JDK1.8 之前 HashMap 由 数组+链表 组成的，**使用“拉链法”解决冲突**。JDK1.8 以后，当**链表长度大于阈值（默认为 8）**（将链表转换成红黑树前会判断，如果**当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树**）时，将链表转化为红黑树，以减少搜索时间。HashMap 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。并且， HashMap 总是使用 2 的幂作为哈希表的大小。
+ 如何插入？
  + `JDK1.8 之前`， HashMap 通过 key.hashCode()方法，经过扰动函数处理过后得到 hash 值，然后通过 (n - 1) & hash 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突，**链表头部插入**。所谓扰动函数指的就是 HashMap 的 hash 方法。使用 hash 方法是为了防止一些实现比较差的 hashCode() 方法，换句话说使用扰动函数之后可以减少碰撞。
  ```java
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);//一次扰动：(h >>> 16)
    }
  ```
  + `JDK1.8 以后`，在解决哈希冲突时有了较大的变化。`当链表长度大于阈值（默认为 8）时，会首先调用 treeifyBin()方法。这个方法会根据 HashMap 数组来决定是否转换为红黑树。只有当数组长度大于或者等于 64 的情况下，才会执行转换红黑树操作，以减少搜索时间。否则，就是只是执行 resize() 方法对数组扩容。`**插入的是链表尾部**
+ 为什么线程不安全
  + [图解HashMap为什么线程不安全](https://blog.csdn.net/zzu_seu/article/details/106669757)
  + HashMap的线程不安全主要体现在下面两个方面：
    + 在JDK1.7中，当`并发执行扩容操作时会造成环形链和数据丢失`的情况。
    + 在JDK1.8中，在`并发执行put操作时会发生数据覆盖`的情况。

## HashMap为什么初始容量总是2的n次方？
+ 对于任意给定的对象，只要它的 hashCode() 返回值相同，那么程序调用 hash(int h) 方法所计算得到的 hash 码值总是相同的。我们首先想到的就是把hash值对数组长度取模运算，这样一来，元素的分布相对来说是比较均匀的。但是，“模”运算的消耗还是比较大的。`当length总是 2 的n次方时，h& (length-1)运算等价于对length取模，也就是h%length，但是&比%具有更高的效率`，所以说，当数组长度为2的n次幂的时候，不同的key算得得index相同的几率较小，那么数据在数组上分布就比较均匀，也就是说碰撞的几率小，相对的，查询的时候就不用遍历某个位置上的链表，这样查询效率也就较高了。


# <font color='red'>===================修改到这里了========================</font>

## ConcurrentHashMap 和 Hashtable 的区别？
+ 底层数组+链表实现，无论key还是value都不能为null，线程安全，实现线程安全的方式是**在修改数据时锁住整个HashTable**，`效率低`，ConcurrentHashMap做了相关优化
+ ConcurrentHashMap是使用了**锁分段技术**来保证线程安全的。锁分段技术：首先将数据分成一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问

## synchronized的使用方式、底层实现以及JDK1.6的优化？
+ 使用方式
  + synchronized(this) 等价于 synchronized (instance) 等价于 synchronized method，影响范围是单个实例，上述写法的锁，都是同一把锁，即this。
  + synchronized(*.class) 等价于 static synchronized method 相似于 synchronized (static object)，其锁的影响范围都是同一个classloader下的所有实例，即所有实例共用同一把锁。
  + 绝大多数的Java应用，同一个class只会被同一个classloader加载一次，此种情况下，应用内的所有实例共用同一把锁。其中synchronized(*.class) 与 static synchronized method是同一把锁，即都是*.class。static object的锁是object lock。
+ 底层实现
  + CAS（compare and swap）比较并且交换
    + 从内存读取原值
    + 计算结果
    + 将结果写回内存中时先对比该此时内存中的值是否与之前取出的值是否一致，一致则将结果写入内存，不 一致（中途线程B修改了内存中的值）则重复三个步骤直到写入结果成功为止
    + 注：ABA问题，**其他线程修改多次之后的最终值与原值相同**（线程B在A进行步骤三之前多次修改了内存值，但修改完的最终值仍然是1，此时A在步骤三**进行比较时判定结果未被其他线程修改，直接将结果写入**），在一些特殊的场景是需要区分内存是否被其他线程修改的情况，可采用`增加版本号或者时间戳`的方式进行判断
    + CAS底层实现，在Java中CAS最底层实现是 lock cmpxchg。cpu中有一条汇编指令是 cmpxchg，cpu底层支持compare and exchange操作，**但该指令不是原子的**，cpu在执行这条命令的中途也会被打断，所以`在多核场景下需要使用指令lock将锁定一个北桥信号（可以简单理解为内存总线）锁住，使得cmpxchg不会被其他cpu打断`
  + Java对象的内存分布
    + 如下图所示，一个普通的Java对象由四部分组成
      + markword
        + 主要用来表示对象的线程锁状态，另外还可以用来配合GC、存放该对象的hashCode
      + Klass pointer
        + 是一个指向方法区中Class信息的指针，意味着该对象可随时知道自己是哪个Class的实例
      + instance data
        + 用于保存对象属性和值的主体部分，占用内存空间取决于对象的属性数量和类型
      + 对齐字节是为了减少堆内存的碎片空间
      + markword的细节
        + 针对synchronized来说，synchronized本质上就是对markword里面的值进行操作，最需要关注的是最后三位的值，不同组合用于表示不同级别的锁
+ JDK1.6的优化
  + synchronized的升级过程（无锁态->偏向锁->轻量级锁->重量级锁）
    +  synchronized在JDK1.6以前，一旦使用就必须**向操作系统内核申请锁资源**，这样会造成严重的性能损耗，之后对其进行了一系列优化，在大部分情况下使用synchronized就能满足需要，下面是锁的大致升级过程：
       + 无锁
         + **无锁没有对资源进行锁定，所有的线程都能访问并修改同一个资源，但同时只有一个线程能修改成功**。无锁的特点就是修改操作在循环内进行，线程会不断的尝试修改共享资源。如果没有冲突就修改成功并退出，否则就会继续循环尝试。如果有多个线程修改同一个值，必定会有一个线程能修改成功，而其他修改失败的线程会不断重试直到修改成功。`CAS原理及应用即是无锁的实现`。无锁无法全面代替有锁，但无锁在某些场合下的性能是非常高的。
       + 偏向锁
         + 偏向锁是指一段同步代码一直被一个线程所访问，那么该线程会自动获取锁，降低获取锁的代价。在大多数情况下，锁总是由同一线程多次获得，不存在多线程竞争，所以出现了偏向锁。其目标就是在只有一个线程执行同步代码块时能够提高性能。当一个线程访问同步代码块并获取锁时，会在Mark Word里存储锁偏向的线程ID。在线程进入和退出同步块时不再通过CAS操作来加锁和解锁，而是检测Mark Word里是否存储着指向当前线程的偏向锁。引入偏向锁是为了在无多线程竞争的情况下尽量减少不必要的轻量级锁执行路径，因为轻量级锁的获取及释放依赖多次CAS原子指令，而偏向锁只需要在置换ThreadID的时候依赖一次CAS原子指令即可。偏向锁只有遇到其他线程尝试竞争偏向锁时，持有偏向锁的线程才会释放锁，线程不会主动释放偏向锁。偏向锁的撤销，需要等待全局安全点（在这个时间点上没有字节码正在执行），它会首先暂停拥有偏向锁的线程，判断锁对象是否处于被锁定状态。撤销偏向锁后恢复到无锁（标志位为“01”）或轻量级锁（标志位为“00”）的状态。偏向锁在JDK 6及以后的JVM里是默认启用的。可以通过JVM参数关闭偏向锁：-XX:-UseBiasedLocking=false，关闭之后程序默认会进入轻量级锁状态。
       + 轻量级锁
         + 是指当锁是偏向锁的时候，被另外的线程所访问，偏向锁就会升级为轻量级锁，其他线程会通过自旋的形式尝试获取锁，不会阻塞，从而提高性能。在代码进入同步块的时候，如果同步对象锁状态为无锁状态（锁标志位为“01”状态，是否为偏向锁为“0”），虚拟机首先将在当前线程的栈帧中建立一个名为锁记录（Lock Record）的空间，用于存储锁对象目前的Mark Word的拷贝，然后拷贝对象头中的Mark Word复制到锁记录中。拷贝成功后，虚拟机将使用CAS操作尝试将对象的Mark Word更新为指向Lock Record的指针，并将Lock Record里的owner指针指向对象的Mark Word。如果这个更新动作成功了，那么这个线程就拥有了该对象的锁，并且对象Mark Word的锁标志位设置为“00”，表示此对象处于轻量级锁定状态。如果轻量级锁的更新操作失败了，虚拟机首先会检查对象的Mark Word是否指向当前线程的栈帧，如果是就说明当前线程已经拥有了这个对象的锁，那就可以直接进入同步块继续执行，否则说明多个线程竞争锁。若当前只有一个等待线程，则该线程通过自旋进行等待。但是当自旋超过一定的次数，或者一个线程在持有锁，一个在自旋，又有第三个来访时，轻量级锁升级为重量级锁。

## 谈谈 synchronized和ReentrantLock 的区别？
  + 如何确保线程安全
    + 
## Java内存模型及volatile实现原理？
## volatile和synchronized的区别，volatile一定能替代synchronized吗？
## CountDownLatch、CyclicBarrier、Semaphore、LockSupport和Exchanger？
## ThreadLocal实现原理强引用、软引用、弱引用、虚引用？
## JDK提供的并发容器CopyOnWrite、ArrayListBlockingQueue（阻塞队列FIFO）ConcurrentLinkedQueue（非阻塞队列）等实现方式？
## CAS
## ReentrantLock的实现？
## 多线程的实现方式，start()是立刻启动吗？
## ThreadPoolExecutor的重要参数？执行顺序？如何设置参数？
## 什么是死锁，死锁的四个必要条件？
## Java内存区域？
## 对象的访问定位有哪两种方式？
## 如何判断对象是否死亡？(两种方法)
## 垃圾收集算法？常见的垃圾回收器？
## JVM参数？JVM调优？Full GC的触发条件？
## 一个线程OOM后，其他线程还能正常运行吗？
## 类加载过程？类加载器？Java字节码文件结构？
## BIO（Blocking I/O）、伪异步IO、NIO（New I/O）、AIO？
## 数据库查询缓慢是什么原因，如何优化？
## 索引实现？
## 大表优化？
## 如何实现MySQL的读写分离？
## 数据库的乐观锁和悲观锁？
## MySQL的锁机制？
## 数据库事务的特性（ACID）和隔离级别？
## MVCC实现可重复读？
## RC隔离级别下，读A行数据，updateA 行数据，读B行数据，updateB行数据，再updateA行数据，请问加了几次锁？对A加锁是在什么时候加了几次锁？
## MySQL的事务实现？何时加行锁？
## Redis数据结构及各结构的内部实现？
## Redis为什么快？
## Redis通信协议？
## Redis事务？
## Redis 的过期策略以及内存淘汰机制？
## Redis持久化方式？
## Redis主从复制机制redis 集群模式的工作原理能说一下么？在集群模式下，redis 的 key 是如何寻址的？分布式寻址都有哪些算法？了解一致性 hash 算法吗？
## Redis 和 DB 一致性？缓存穿透？缓存击穿？缓存雪崩？缓存清洗？并发竞争Key？
## Spring的优点，用到了哪些设计模式，IOC和AOP的理解？
## Bean的生命周期？
## 解释Spring支持的几种bean的作用域？
## BeanFactory、FactoryBean的区别？
## Spring如何处理单例Bean的循环依赖？
## SpringMVC、Mybatis 执行过程？
Spring事务？
SpringBoot最大的优势（三连问）？
Spring Cloud Eureka 服务发现？
Ribbon负载均衡？
为什么使用Dubbo？
Dubbo服务注册原理？
Dubbo提供的负载均衡策略？
如何基于 dubbo 进行服务治理、服务降级、失败重试以及超时重试？
RabbitMQ Exchange 类型？
如何保证消息顺序性？
为什么使用消息队列？消息队列的缺点？常见的消息队列组件比较？
MQ如何保证高可用？
MQ如何处理重复消费？
RabbitMQ消息堆积？
MQ如何保证可靠传输？
ES的分布式架构原理？
ES写入数据的工作原理是什么啊？
底层的 lucene 介绍一下呗？倒排索引了解吗？
ES在数据量很大的情况下（数十亿级别）如何提高查询效率？
ES 生产集群的部署架构是什么？
分布式事务？
使用哪些组件或者方法可以提升网站性能、可用性以及并发量？
设计高可用（系统7×24小时不间断服务）系统的常用手段？
如何设计高并发系统？
CAP和BASE理论？
分布式 ID 生成器？
基于 Redis 的分布式锁？
Redis分布式锁过期了但业务没执行完？
内存溢出排查方法？CPU高负载排查方法？
如何保障双11狂欢下的99.99%高可用？
秒杀系统设计？
以上目录能覆盖一大部分考题，更加深入的底层原理、场景题等需要平时工作的沉淀思考以及框架源码的阅读总结。

建议平时工作之余，坚持阅读优秀的框架源码，这对面试答题以及日后编码具有极大的效应。以下列出部分推荐的源码：

Java基础：集合、JUC
DB：Redis、MongoDB、MySQL（任选其一，推荐Redis，相对简单）
ORM：Mybatis、Spring Data Jpa、Hibernate （任选其一，选平时工作中使用频率来选即可）
MQ：RabbitMQ、Kafka、RocketMQ、ActiveMQ（任选其一，选平时工作中使用频率来选即可，但还是推荐Kafka）
J2EE：Spring、Spring Boot
Web框架：Spring MVC、Spring Security、Spring Webflux（一般了解mvc即可，其他看兴趣）
注册中心：Eureka、Zookeeper（任选其一，推荐Eureka）
分布式事务：TCC Transaction、Seata、Fesar（任选其一，主要了解实现思路）
其他：Zuul、Hystrix、Redisson、Netty、Dubbo、Ribbon、ES、HBase
常考的已加粗，重点阅读总结。
