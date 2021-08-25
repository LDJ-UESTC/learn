# Java基础、中间件基础

## 为什么重写equals()方法就必须重写hashCode()方法?
+ equals()方法是用于比较对象引用是否指向同一块内存空间，对象重写equals()方法通常是为了在业务逻辑中比较对象值是否相同；hashcode()方法具有散列表能快速检索的特性，因此引入来提高程序运行的效率。在使用的时候，先比较两个对象的hashcode()方法返回值，如果不同，则不需要再使用equals()方法进行比较，如果相同，再使用equal()方法继续比较。这里需要保证一个原则，equals()方法返回false时，hashcode()方法返回值也一定不同（比如对于存储散列表的集合set（去重）来说，equal的两个对象是不能放入其中的，但是因为hashcode()值不同，会被保存进去，这样就会引起混乱），如何保证？------->重写hashcode()方法
## Object有哪些方法?
**jdk1.5中部分方法如下：**
+ **getclass()方法**，返回一个对象的运行时类。该 Class 对象是由所表示类的 static synchronized 方法锁定的对象。
+ hashCode()方法，返回该对象的哈希码值。支持该方法是为哈希表提供一些优点，例如，java.util.Hashtable 提供的哈希表。
+ **equals()方法**，如果此对象与 obj 参数相同，则返回 true；否则返回 false。
+ **clone()方法**，返回此实例的一个克隆。
+ **toString()方法**，Object 类的 toString 方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at 标记符“@”和此对象哈希码的无符号十六进制表示组成。换句话说，该方法返回一个字符串，它的值等于：getClass().getName() + '@' + Integer.toHexString(hashCode())，返回该对象的字符串表示形式。
+ **notify()方法**，唤醒在此对象监视器上等待的单个线程。如果所有线程都在此对象上等待，则会选择唤醒其中一个线程。选择是任意性的，并在对实现做出决定时发生。线程通过调用其中一个 wait 方法，在对象的监视器上等待。直到当前的线程放弃此对象上的锁定，才能继续执行被唤醒的线程。被唤醒的线程将以常规方式与在该对象上主动同步的其他所有线程进行竞争；例如，唤醒的线程在作为锁定此对象的下一个线程方面没有可靠的特权或劣势。
+ **notifyAll()方法**，唤醒在此对象监视器上等待的所有线程。线程通过调用其中一个 wait 方法，在对象的监视器上等待。直到当前的线程放弃此对象上的锁定，才能继续执行被唤醒的线程。被唤醒的线程将以常规方式与在该对象上主动同步的其他所有线程进行竞争；例如，唤醒的线程在作为锁定此对象的下一个线程方面没有可靠的特权或劣势。
+ **finalize()方法**，当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。子类重写 finalize 方法，以配置系统资源或执行其他清除。
+ **wait()方法**，导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量。当前的线程必须拥有此对象监视器。
## 接口和抽象类的区别，什么情况下用接口或抽象类?
### 区别
+ 抽象类是用来捕捉子类的通用特性的 。它不能被实例化，只能被用作子类的（父类）超类。抽象类是被用来创建继承层级里子类的模板。接口是抽象方法的集合。如果一个类实现了某个接口，那么它就继承了这个接口的抽象方法。这就像契约模式，如果实现了这个接口，那么就必须确保使用这些方法。接口只是一种形式，接口自身不能做任何事情。
### 什么时候使用抽象类和接口？
抽象类是对一种事物的抽象，即对**类**抽象，而接口是对**行为**的抽象。
+ 如果你拥有一些方法并且想让它们中的一些有*默认实现*，那么使用抽象类吧。
+ 如果你想*实现多重继承*，那么你必须使用接口。由于Java不支持多继承，子类不能够继承多个类，但可以实现多个接口。因此你就可以使用接口来解决它。
+ 如果基本功能在不断改变，那么就需要使用抽象类。如果不断改变基本功能并且使用接口，那么就需要改变所有实现了该接口的类。
## 为什么String设计成不可变？String 和 StringBuilder、StringBuffer 的区别？
[参考链接](https://blog.csdn.net/huaye502/article/details/6603592)
### 设计成不可变
+ 可变性：String类使用final关键字字符数组保存字符串，所以String的对象时不可变的。而StringBuilder与StringBuffer都继承自AbstractStringBuilder类，在AbstractStringBuilder中使用char[] value但是没有用final关键字修饰，所以这两种对象都是可变的。
+ 线程安全性：String的对象是不可变的，也就可以理解为常量，线程安全。AbstractStringBuilder是StringBuilder与StringBuffer的公共父类，定义了一些字符串的基本操作。StringBuffer对方法加了同步锁或者对调用方法加了同步锁，所以是线程安全的。StringBuilder并没有对方法进行同步锁，所以是非线程安全的。
+ 性能：每次都String类型进行改变的时候，都会生成一个新的String对象，然后将新的指针指向新的String对象。StringBuilder相比使用StringBuffer仅能提升10%-15%的性能提升，但却要冒着多线程不安全的风险。
+ 对于三者使用的总结：
  + 操作少量数据String
  + 单线程操作字符串缓冲区下操作大量数据=StringBuilder
  + 多线程操作字符串缓冲区下操作大量数据=StringBuffer
## 使用String s=new String(“abc”) 和 String s="abc" 的区别?
`不要使用new String("some string") 方式构造字符串`
+ 要点说明:
创建字符串时不要使用new String("some string")，此处会产生`2`个对象，在Java中，"some String"默认就已经创建了一个字符串，如果再用new String("some string")，将会创建出另外一个字符串，造成浪费。
"abc"本身就是一个字符串对象，而在运行时执行String s = new String("abc")时，"abc"这个对象被复制一份到heap中，并且把heap中的对象引用交给s持有。这条语句创建了两个String对象。
## Arraylist、HashMap的初始容量、加载因子、扩容增量？
+ ArrayList()：可以构造一个**默认初始容量为10**的空列表，每次扩容后容量**增长50%**
+ HashMap()：构建一个**默认初始容量为 16**，**负载因子为 0.75**的 HashMap。
+ 当HashMap中的元素个数超过数组大小 $*$ loadFactor时，就会进行数组扩容，loadFactor的默认值为0.75，这是一个折中的取值。也就是说，默认情况下，数组大小为16，那么当HashMap中元素个数超过16 $*$ 0.75=12的时候，就把数组的大小扩展为 2 $*$ 16=32，即**扩大一倍**，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以如果我们已经预知HashMap中元素的个数，那么预设元素的个数能够有效的提高HashMap的性能。
## 有序的Map有哪些？为什么TreeMap是有序的？哪些集合是线程安全的？
+ 有序Map：Map主要用于存储健值对，根据键得到值，因此不允许键重复，但允许值重复。使用的时候key和value的数据类型可以相同。也可以不同。Map接口的主要实现类有HashMap（无序）、LinkedHashMap（有序）、TreeMap（有序，按照key的字典升序排序）、ConcurrentHashMap。
+ 为什么有序：TreeMap底层是基于红黑树实现的排序Map，为了进行排序，TreeMap要求放入的key必须实现Comparable接口，作为Value的对象则没有任何要求。如果作为key的类没有实现Comparable接口，必须在创建TreeMap时同时自定义Comparator比较器。
  + TreeMap的增删改查和统计相关的操作的时间复杂度都为 O(logn) 。由于实现了Map接口，则key的值不允许重复（重复则覆盖），也不允许为null，按照key的自然顺序排序或者Comparator接口指定的排序方法进行排序。value允许重复，也允许为null，当key重复时，会覆盖此value值。
  + TreeMap的特殊操作，如获取第一个key、更大的key、更小的key、key的子区间，优势较大。如果你需要得到一个有序的结果时就应该使用TreeMap（因为HashMap中元素的排列顺序是不固定的）。除此之外，由于HashMap在增删改查有更好的性能，所以大多不需要排序的时候我们会使用HashMap
+ 线程安全的集合：Vector、statck、HashTable、Properties、concurrent包下面的类（如ConcurrentHashMap）线程安全。[参考链接](https://blog.csdn.net/mypersonalsong/article/details/83416393)
## HashMap的底层数据结构，是如何插入的？哈希冲突解决方案？为什么是非线程安全的？
+ JDK1.8之前HashMap底层是基于数组和链表结合在一起使用，也就是链表散列。HashCode经过扰动函数处理过后得到hash值，然后通过（n-1）&hash判断当前元素存放的位置（这里n指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素hash值以及key是否相等， 如果相同的话，直接覆盖，不同的话就通过拉链法解决冲突。所谓扰动函数就是HashMap中的hash方法。使用hash方法是为了防止一些实现比较差的hashCode()方法，换句话说使用扰动函数之后可以减少碰撞。相比于之前的版本，JDK1.8之后在解决哈希冲突时有了较大的变化，当链表大于阀值（默认为8）时，将链表转化为红黑树，以减少搜索时间
## HashMap为什么初始容量总是2的n次方？
+ 对于任意给定的对象，只要它的 hashCode() 返回值相同，那么程序调用 hash(int h) 方法所计算得到的 hash 码值总是相同的。我们首先想到的就是把hash值对数组长度取模运算，这样一来，元素的分布相对来说是比较均匀的。但是，“模”运算的消耗还是比较大的。`当length总是 2 的n次方时，h& (length-1)运算等价于对length取模，也就是h%length，但是&比%具有更高的效率`，所以说，当数组长度为2的n次幂的时候，不同的key算得得index相同的几率较小，那么数据在数组上分布就比较均匀，也就是说碰撞的几率小，相对的，查询的时候就不用遍历某个位置上的链表，这样查询效率也就较高了。

## ConcurrentHashMap 和 Hashtable 的区别？
## synchronized的使用方式、底层实现以及JDK1.6的优化？
## 谈谈 synchronized和ReentrantLock 的区别？
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

