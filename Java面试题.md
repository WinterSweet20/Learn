![æ¨¡åå¾](.\img\面试题技能书.png)

[CSDN](https://gitchat.csdn.net/activity/5c6cf6044bb44360f3370255)



# Java基础

## JDK和JRE

- `JDK`：`Java Development Kit` 的简称，Java 开发工具包，提供了 Java 的开发环境和运行环境。
- `JRE`：`Java Runtime Environment `的简称，Java 运行环境，为 Java 的运行提供了所需环境。

JDK包括JRE，当然还包括java源码编译器javac等其他java调试工具<br/>如果只是运行java程序，只需要安装JRE就行了，要开发java程序就需要安装JDK

## ==和equals()

### == 

- 对于基本类型是值比较
- 对于引用类型是引用比较

### equals()

`equals()`本质上就是`==`，但是`String`和`Integer`重写（重写事也必须重写`hashCode`方法）了`equals()`方法，使之变成了值比较

**所以**：`equals()`本质上就是`==`，只是对于少数引用类型重写了`equals()`方法使之变成了值比较，比如*String,Integer*

## hashCode

`hashCode`相等，`equals()`不一定相等，因为就算`hashCode`相等，不代表在散列表中的键值对相等

## final

- final 修饰的类叫最终类，该类不能被继承。
- final 修饰的方法不能被子类重写（被声明为final类的方法自动声明为final）
- final 修饰的变量（包括基本类型和实例变量）叫常量，常量必须初始化，不能被修改。

##Math.round()

`Math.round(-1.5) = -1`<br/>`Math.round(1.5) = 2`

`Math.round()`向坐标轴右取整

## String、StringBuffer和StringBuilder

`String`和其他的区别，`String`是不可修改的，修改都是产生新的字符串对象，然后修改指针指向它

`StringBuffer`和`StringBuilder`最大的区别是`StringBuffer`是线程安全的，`StringBuilder`的性能比`StringBuffer`高

### new String

`String str = “Hello”`和`String str = new String("Hello")`不一样<br/>前一个是把它分配到常量池，后一个则是堆内存中

### 字符串反转

```java
StringBuilder stringBuilder = new StringBuilder;
stringBuilder.append("hello");
System.out.println(stringBuilder.reverse());//olleh

//StringBuffer同
```

### String常用方法

| 方法 | 效果 |
| ---- | ---- |
|  indexOf()|返回指定字符的索引|
|charAt()|返回指定索引处的字符|
|replace()|字符串替换|
|trim()|去除字符串两端空白|
|split()|分割字符串，返回一个分割后的字符串数组|
|getBytes()|返回字符串的 byte 类型数组|
|length()|返回字符串长度|
|toLowerCase()|将字符串转成小写字母|
| toUpperCase()|将字符串转成大写字符|
|substring()|截取字符串|
|equals()|字符串比较。|

## 抽象类

- 抽象类不一定要有抽象方法
- 普通类和抽象类的区别：
  - 普通类不可以包含抽象方法
  - 普通类可以直接实例化
- 抽象类不能用`final`修饰
  - 抽象类就是让其他类继承，`final`不能被继承
- 接口和抽象类的区别：
  - 实现不同：抽象类用继承`extends`，接口用实现`impements`
  - 抽象类可以有构造函数，接口不能有
  - 类可以实现多个接口，只能继承一个抽象类
  - 接口中的方法默认public，抽象类的可以是任意修饰符

## IO

### 分类

按功能分：输入流（input），输出流（output）

按类型分：字节流和字符流

字节流和字符流的区别：字节流按 8 位传输以字节为单位输入输出数据，字符流按 16 位

### BIO,NIO&AIO

- BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。一个事件对应一个线程，需要IO操作时阻塞，直到IO完成之后才继续执行
- NIO：Non IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用，需要IO操作的时候请求，然后程序继续做自己的事情，轮询，当IO操作的时候处理数据
- AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制，需要IO操作的时候请求，然后程序继续做自己的事情，当IO操作完成的时候，os发送信息高速程序完成IO操作，程序再处理数据

BIO无法处理高并发事件，NIO和AIO设计复杂，能使用简单技术解决问题就是用简单技术

###Files的常用方法

| 方法                     | 作用                   |
| ------------------------ | ---------------------- |
| Files. exists()          | 检测文件路径是否存在。 |
| Files. createFile()      | 创建文件               |
| Files. createDirectory() | 创建文件夹             |
| Files. delete()          | 删除一个文件或目录     |
| Files. copy()            | 复制文件               |
| Files. move()            | 移动文件               |
| Files. size()            | 查看文件个数           |
| Files. read()            | 读取文件               |
| Files. write()           | 写入文件               |

# 容器

Java 容器分为 Collection 和 Map 两大类，其下又有很多子类：

- Collection
  - List
    - ArrayList
    - LinkedList
    - Vector
  	- Stack
  - Set
  	- HashSet
  	- LinkedHashSet
  	- TreeSet
- Map
	- HashMap
  		- LinkedHashMap
	- TreeMap
	- ConcurrentHashMap
	- Hashtable

## Collection

- Collection 是一个集合接口，它提供了对集合对象进行基本操作的通用接口方法，所有集合都是它的子类，比如 List、Set 等。

- Collections 是一个包装类，包含了很多静态方法，不能被实例化，就像一个工具类，比如提供的排序方法： Collections. sort(list)。

### List、Set

这个世界上本没有集合<br/>有人想要回自动拓展的数组，于是有了List<br/>有人想要不重复的数组，于是有了Set<br/>有人想要自动排序的数组，于是有了TreeSet，TreeList，Tree***

所以：<br/>Array固定大小，Array的内置方法没有ArrayList多(因为更多的封装)

但是：<br/>Array可以存储基本类型和对象，ArrayList只能存储对象

#### ArrayList&LinkedList

ArrayList通过动态数组实现（*动态数组：每次当大小不够用的时候，扩容**1.5**倍*）<br/>LinkedList通过双向链表实现
> 在插入大量数据之前推荐使用`list.ensureCapacity(int num)`方法，因为每次迭代都是数组的新建和拷贝，会浪费大量的时间和空间

所以：<br/>ArrayList的随机访问速度更快<br/>LinkedList的插入和删除更快（*ArrayList插入或者删除要修改之后的每个元素的下标*）

#### Vector

Vector是线程安全的，而ArrayList不是<br/>但ArrayList性能要优于Vector

## HashMap

实现：通过`put(key,value)`和`get(key)`插入和获取值，通过传入的`key`计算`Hash`值，根据`Hash`值把数据保存在`bucket`里面，或者从中取出数据<br/>当计算出来的`Hash`值相同时，称为*Hash冲突*，个数比较少的时候使用链表存储相同的，较多的时候使用红黑树

## HashSet

HashSet是基于HashMap实现的，只是HashSet不允许重复值

##HashMap&***

### HashMap&TreeMap

HashMap插入删除定位一个元素比较快，HashMap是无序的，所以有有序遍历的需求的时候，用TreeMap

### HashMap&HashTable

- HashMap允许key，value为空，HashTable不允许
- HashMap不是线程安全，HashTable是
- HashTable是保留类，需要多线程用ConcurrentHashMap替代

## Iterator

```java
List<String> list = new ArrayList<>;
Iterator<String> it = list.iterator();
while(it.hasNext()){
    System.out.println(it.next());
}
```

Iterator更加安全，因为可以确保当前元素在被更改的时候抛出ConcurrentModificationException

### Iterator与ListIterator的区别

ListIterator只能遍历List，可以双向遍历，添加了一些额外的功能，比如*添加一些元素、删除一些元素*

## 怎么创建一个不可修改的集合

```java
List<String> list = new ArrayList<>;
Collection<string> col = Collection.unmodifiableCollection(list);
```



# 多线程

并发和并行的区别：

并行：多个处理器或者多核处理器同时处理多个任务<br/>串行：多个任务在同一个CPU核上，按照细分的时间片交替执行，在逻辑上看起来像是在同时执行的

并行：两个咖啡机两条队列<br/>串行：一台咖啡机两条队列（轮流服务一个）

一个进程由至少一个线程组成

## 守护进程

守护线程是运行在后台的一种特殊进程。它独立于控制终端并且周期性地执行某种任务或等待处理某些发生的事件。在 Java 中垃圾回收线程就是特殊的守护线程。

## 创建线程的三种方式

- 继承Thread类重写run()方法
- 实现Runnable接口
- 实现Callable接口

Runnable和Callable的区别：<br/>Callable有返回值，创建的步骤方法不同

## 线程的几种状态

- 新建状态:NEW
- 就绪状态
- 运行状态:RUNNABLE
- 阻塞状态:BLOCKED,WAITING,TIMED_WAITING
- 死亡状态:TERMINATED

## sleep() 和 wait() 的区别

- 类的不同：sleep() 来自 Thread，wait() 来自 Object。
- 释放锁：sleep() 不释放锁；wait() 释放锁。
- 用法不同：sleep() 时间到会自动恢复；wait() 可以使用 notify()/notifyAll()直接唤醒。

## notify()和notifyAll()的区别

notify()唤醒一个线程，唤醒哪个线程由虚拟机控制<br/>notifyAll()唤醒所有线程参与CPU和锁的竞争，竞争成功的线程继续执行

## run()和start()的区别

run()中定义的是线程执行的代码块，可以执行多次<br/>start()用于启动线程，只能执行一次

## 线程池

创建线程池的几种方法：

> 线程池创建有七种方式，最核心的是最后一种：
>
> - newSingleThreadExecutor()：它的特点在于工作线程数目被限制为 1，操作一个无界的工作队列，所以它保证了所有任务的都是被顺序执行，最多会有一个任务处于活动状态，并且不允许使用者改动线程池实例，因此可以避免其改变线程数目；
> - newCachedThreadPool()：它是一种用来处理大量短时间工作任务的线程池，具有几个鲜明特点：它会试图缓存线程并重用，当无缓存线程可用时，就会创建新的工作线程；如果线程闲置的时间超过 60 秒，则被终止并移出缓存；长时间闲置时，这种线程池，不会消耗什么资源。其内部使用 SynchronousQueue 作为工作队列；
> - newFixedThreadPool(int nThreads)：重用指定数目（nThreads）的线程，其背后使用的是无界的工作队列，任何时候最多有 nThreads 个工作线程是活动的。这意味着，如果任务数量超过了活动队列数目，将在工作队列中等待空闲线程出现；如果有工作线程退出，将会有新的工作线程被创建，以补足指定的数目 nThreads；
> - newSingleThreadScheduledExecutor()：创建单线程池，返回 ScheduledExecutorService，可以进行定时或周期性的工作调度；
> - newScheduledThreadPool(int corePoolSize)：和newSingleThreadScheduledExecutor()类似，创建的是个 ScheduledExecutorService，可以进行定时或周期性的工作调度，区别在于单一工作线程还是多个工作线程；
> - newWorkStealingPool(int parallelism)：这是一个经常被人忽略的线程池，Java 8 才加入这个创建方法，其内部会构建ForkJoinPool，利用Work-Stealing算法，并行地处理任务，不保证处理顺序；
> - ThreadPoolExecutor()：是最原始的线程池创建，上面1-3创建方式都是对ThreadPoolExecutor的封装。

线程池有哪些状态：

> - RUNNING：接收新的任务，处理等待队列中的任务
> - SHUTDOWN：不再接收新的任务，等待正在处理的和等待队列中的任务全部处理完成
> - STOP：不接收新的任务，结束正在执行的任务，清空等待队列
> - TIDYING：所有线程销毁，队列为空，在向TERMINATED转变，会执行钩子方法 terminated()
> - TERMINATED：所有工作线程已经销毁，等待队列为空

线程池中execute(),submiit()的区别：

> execute()：只能执行Runnable类型的任务，无返回值
>
> submit()：可以执行Runnable和Callable类型的任务。有返回值

Java中怎么保证线程安全：

> - 使用线程安全类，比如java.util.concurrent下的类
> - 使用自动锁synchronized
> - 使用手动锁Lock

多线程中 synchronized 锁升级的原理是什么:

> synchronized 锁升级原理：在锁对象的对象头里面有一个 threadid 字段，在第一次访问的时候 threadid 为空，jvm 让其持有偏向锁，并将 threadid 设置为其线程 id，再次进入的时候会先判断 threadid 是否与其线程 id 一致，如果一致则可以直接使用此对象，如果不一致，则升级偏向锁为轻量级锁，通过自旋循环一定次数来获取锁，执行一定次数之后，如果还没有正常获取到要使用的对象，此时就会把锁从轻量级升级为重量级锁，此过程就构成了 synchronized 锁的升级。
> 

synchronized 底层实现原理:

>synchronized 是由一对 monitorenter/monitorexit 指令实现的，monitor 对象是同步的基本实现单元。在 Java 6 之前，monitor 的实现完全是依靠操作系统内部的互斥锁，因为需要进行用户态到内核态的切换，所以同步操作是一个无差别的重量级操作，性能也很低。但在 Java 6 的时候，Java 虚拟机 对此进行了大刀阔斧地改进，提供了三种不同的 monitor 实现，也就是常说的三种不同的锁：偏向锁（Biased Locking）、轻量级锁和重量级锁，大大改进了其性能。

synchronized 和 volatile 的区别是什么:

>- volatile 是变量修饰符；synchronized 是修饰类、方法、代码段。
>- volatile 仅能实现变量的修改可见性，不能保证原子性；而 synchronized 则可以保证变量的修改可见性和原子性。
>- volatile 不会造成线程的阻塞；synchronized 可能会造成线程的阻塞。

synchronized 和 Lock 的区别:

>- synchronized 可以给类、方法、代码块加锁；而 lock 只能给代码块加锁。
>- synchronized 不需要手动获取锁和释放锁，使用简单，发生异常会自动释放锁，不会造成死锁；而 lock 需要自己加锁和释放锁，如果使用不当没有 unLock()去释放锁就会造成死锁。
>- 通过 Lock 可以知道有没有成功获取锁，而 synchronized 却无法办到。

synchronized 和 ReentrantLock 区别:

>synchronized 早期的实现比较低效，对比 ReentrantLock，大多数场景性能都相差较大，但是在 Java 6 中对 synchronized 进行了非常多的改进。

主要区别如下：

>- ReentrantLock 使用起来比较灵活，但是必须有释放锁的配合动作；
>- ReentrantLock 必须手动获取与释放锁，而 synchronized 不需要手动释放和开启锁；
>- ReentrantLock 只适用于代码块锁，而 synchronized 可用于修饰方法、代码块等。
>- volatile 标记的变量不会被编译器优化；synchronized 标记的变量可以被编译器优化。

锁的升级的目的：

> 锁升级是为了减低了锁带来的性能消耗。在 Java 6 之后优化 synchronized 的实现方式，使用了偏向锁升级为轻量级锁再升级到重量级锁的方式，从而减低了锁带来的性能消耗。

##死锁

什么是死锁：

> 当线程 A 持有独占锁a，并尝试去获取独占锁 b 的同时，线程 B 持有独占锁 b，并尝试获取独占锁 a 的情况下，就会发生 AB 两个线程由于互相持有对方需要的锁，而发生的阻塞现象，我们称为死锁。

怎么防止死锁:

> 尽量使用 tryLock(long timeout, TimeUnit unit)的方法(ReentrantLock、ReentrantReadWriteLock)，设置超时时间，超时可以退出防止死锁。
>尽量使用 Java. util. concurrent 并发类代替自己手写锁。
>尽量降低锁的使用粒度，尽量不要几个功能用同一把锁。
>尽量减少同步的代码块。

ThreadLocal ,有哪些使用场景:

>- ThreadLocal 为每个使用该变量的线程提供独立的变量副本，所以每一个线程都可以独立地改变自己的副本，而不会影响其它线程所对应的副本。

>- ThreadLocal 的经典使用场景是数据库连接和 session 管理等。


atomic 的原理

>atomic 主要利用 CAS (Compare And Wwap) 和 volatile 和 native 方法来保证原子操作，从而避免 synchronized 的高开销，执行效率大为提升。

# 反射(Reflect)

Java反射机制在程序==运行时==<br/>对于任意一个类，都能够知道这个类的所有属性和方法<br/>对于任意一个对象，都能调用它的任意一个方法和属性<br/>这种 **动态的获取信息** 以及 **动态调用对象的方法** 的功能称为 **java 的反射机制**。

[拓展](https://juejin.im/post/598ea9116fb9a03c335a99a4)

## Java序列化

Java 序列化是为了保存各种对象在内存中的状态，并且可以把保存的对象状态再读出来。

什么情况下需要Java序列化：

> - 想把内存中的对象保存到一个文件中或者数据库中的时候
> - 想用套接字在网络上传送对象的时候
> - 想通过RMI（远程方法调用）传输对象的时候

## 动态代理

动态代理是运行时动态生成代理类。

用到动态代理的实例：

>  spring aop、hibernate 数据查询、测试框架的后端 mock、rpc，Java注解对象获取等。

怎么实现动态代理：

> JDK 原生动态代理和 cglib 动态代理。JDK 原生动态代理是基于接口实现的，而 cglib 是基于继承当前类的子类实现的。

# 对象拷贝

为什么要使用克隆：

> new出来的对象值的属性都是初始化时候的值，当需要一个对象来保存当前对象的状态的时候就需要克隆了

怎么实现克隆：

> - 实现Cloneable接口并重写Object类中的Clone()方法<br/>
> - 实现Serializable接口，通过对象的序列化和反序列化实现克隆（真正的深克隆）

深拷贝和浅拷贝的区别是什么：

> 浅克隆：只克隆它所包含的所有对象的引用地址
>
> 深克隆：克隆包括自身所包含的所有对象实例

另外，对于基本数据类型，无论是深克隆还是浅克隆，都会进行复制（因为不存储在堆上）

# Java Web

 JSP 和 servlet的区别：

> 

# 异常

# 网络

#设计模式

# Spring

## Spring MVC

## SpringBoot

## Spring Cloud

# Hibernate

#Mybatis

# 中间件

## RabbitMQ

## Kafka

## Zookeeper

#数据

## MySQL

## Redis

#JVM

