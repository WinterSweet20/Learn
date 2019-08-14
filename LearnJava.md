[菜鸟](https://www.runoob.com/java/java-tutorial.html)、[JavaSchool](http://www.51gjie.com/)



# JVM详解

JVM(Java Virtual Mechine)Java虚拟机的缩写

Java虚拟机包括一套字节码指令集、一组寄存器、一个栈、一个垃圾回收堆和一个存储方法域



# Java基本数据类型

Java8大基本数据类型：

- 整数类 byte, short, int, long
- 浮点类 double, float
- 逻辑类 boolean
- 文本类 char

默认值：

| 名称 | 默认值 |
| ---- | ------ |
| byte | 0      |
|short|0|
|int|0|
|long|0L|
|float|0.0f|
|double|0.0d|
|char|'u0000'|
|boolean|false|



# StringBuffer和StringBuilder

相比String，StringBuffer和StringBuilder最大的区别就是能够在不产生新对象的情况下改变字符串

所以StringBuffer和StringBuilder的主要方法有：

| 名称 | 功能 |
| ---- | ---- |
|      StringBuffer.append(String s)|把s字符串加到现有里面      |
|StringBuffer.reverse()|反转字符串|
|StringBuffer.delete(int satrt,int end)|删除一段字符串|
|StringBuffer.replace(int start,int end,String s)|替换一段字符串|

StringBuilder==方法==同StringBuffer

StingBuilder性能比StringBuffer高，所以在通常情况下推荐用TStringBuilder<br/>StringBuffer是线程安全的，StringBuilder不是，所以在需要线程安全的情况下必须使用StringBuffer



# Java面向对象

## 抽象类

- 并不是所有的类都是面向对象的，如果一个类没有足够的信息来描绘一个对象，那就是抽象类
- 除了不能实例之外，成员变量、方法，构造方法都存在，访问方式和普通类相同
- 因为不能被实例化，所以抽象类必须被继承才能被使用
- 父类包括子类共有的常用方法，因为父类是抽象的，所以不能直接使用
- 抽象类表示的是一种继承关系，一个类只能继承一个抽象类

语法：

```java
public abstract class Animal{
    private String name;
    public Animal(String name){
        this.name = name;
    }
    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
    public abstract void helloWorld();
}

public class Cat extends Animal{
    private String food;
    public Cat(String name,String food){
        super(name);
        this.food = food;
    }
    public String getFood(){
        return food;
    }
    public void setFood(String food){
        this.food = food;
    }
    public void helloWorld(){
        System.out.println("Cat:"+getName()+" say 'Hello World'");
    }
}

public static void main(String[] args){
    Animal kitty = new Cat("Kitty", "Fish");
    kitty.helloWorld();//Cat:Kitty say 'Hello World'
}
```

子类会继承抽象类的方法和成员变量<br/>子类调用父类构造方法用`super()`<br/>子类必须实现父类的抽象方法，除非该子类也是抽象类<br/>有抽象方法的一定是抽象类，抽象类不一定有抽象方法<br/>构造方法和类方法（含`static`）不能是抽象方法

## 接口<del>类</del>

- 接口不是类
- 在JAVA编程语言中是一个抽象类型，是抽象方法的集合，接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。
- 类描述对象的属性和方法。接口则包含类要实现的方法
- 除非实现接口的是抽象类，否则要实现接口中的所有方法
- 接口无法被实例化，但是可以被实现
- 在 Java 中，接口类型可用来声明一个变量，他们可以成为一个空指针，或是被绑定在一个以此接口实现的对象
- 接口不能包含成员变量，==除了==**static 和 final 变量**
- 接口支持多继承



```java
interface InterfaceName extends oneInterface,twoInterface{ //接口可多继承
    public void methordOne();//没有方法体的方法
    public int methordTwo();//省略abstract，隐式虚拟
}

public class ClassName implements InterfaceName,OtherInterfaceName{
    public void methordOne(){
        System.out.println("HelloWorld!");
    }
    public int methordTwo(){
        return 0;
    }//必须实现所有接口
}
```



### 标记接口

- 最常用的继承接口是没有包含任何方法的接口。

- 标记接口是没有任何方法和属性的接口.它仅仅表明它的类属于一个特定的类型

- 标记接口作用：简单形象的说就是给某个对象打个标（盖个戳），使对象拥有某个或某些特权。

```java
public interface InterfaceName{}
```






# 遍历List的四种方法

##  1. for each循环

```java
List<Integer> list = new ArrayList<Integer>();
for(Integer j :list){
    System.out.println(j);
}
```

## 2. 迭代器循环

```java
List<Integer> list = new ArrayList<Integer>();
Iterator<Integer> iterator = list.iterator();
while(iterator.hasNext()){
    System.out.println(iterator.next());
}
```

## 3. 下标递增循环遍历

```java
List<Integer> list = new ArrayList<Integer>();
for(int num = 0; num < list.size(); num++){
    System.out.println(list.get(num));
}
```

## 4. 下标循环递减遍历

```java
List<Integer> list = new ArrayList<Integer>();
for(int num = list.size()-1; num>=0; num--){
    System.out.println(list.get(num));
}
```

- List除了插入删除比较多的，建议使用ArrayList
- List遍历建议使用foreach
- LinkedList建议少使用get()



<div STYLE="page-break-after: always;"></div>
# 合并List

```java
//合并list1和list2得到list3
List list1 = new ArrayList();
List list2 = new ArrayList();
List list3 = new ArrayList();

//不去重
list3.addAll(list1);
list3.addAll(list2);

//去重
list temp = new ArrayList(list1);//temp = list1
temp.retainAll(list2);//temp=list1和list2的公共部分
list1.removeAll(temp);//list1去掉了公共部分
list3.addAll(list1);
list3.addAll(list2);

//我也不知道为什么要写上一个
//简单去重
list3.addAll(list1.removeAll(list2));
list3.addAll(list2)
```

# 空指针异常的原因

空指针异常：`java.lang.NullPointerException`

1. 字符串变量未初始化
2. 接口类型的对象没有用具体的类初始化
   1. new出来的对象无法调用@Autowired注入的Spring Bean，否则报空指针异常[详情](https://blog.csdn.net/u011464124/article/details/88633765)

# Java方法调用

1. 非静态方法
   1. 如果在本类中，直接调用
   2. 在不同类中，非静态方法调用其他类的静态方法，需要通过导入该类中的包，并且需要通过**类名**来调用
   3. 在不用类中，非静态方法调用其他类的非静态方法时，需要导入该类中的包，还需要通过**创建对象**来调用。
2. 静态方法
   1. 在本类中，直接调用
   2. 不在本类中，导入包，`类名.方法名()`



<div STYLE="page-break-after: always;"></div>
#  Java中常用的Page类

Page类一般自己编写，不同的项目和要求可能需要不同的Page类

## 实例代码

<ins>参考**文思海辉**Page类</ins>

```java
/*
 * The MIT License (MIT)
 *
 * Copyright (c) 2014-2017 abel533@gmail.com
 *
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the "Software"), to deal
 * in the Software without restriction, including without limitation the rights
 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 * copies of the Software, and to permit persons to whom the Software is
 * furnished to do so, subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in
 * all copies or substantial portions of the Software.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 * THE SOFTWARE.
 */

package com.github.pagehelper;

import java.io.Closeable;
import java.util.ArrayList;
import java.util.List;

/**
 * Mybatis - 分页对象
 *
 * @author liuzh/abel533/isea533
 * @version 3.6.0
 *          项目地址 : http://git.oschina.net/free/Mybatis_PageHelper
 */
public class Page<E> extends ArrayList<E> implements Closeable {
    private static final long serialVersionUID = 1L;

    /**
     * 页码，从1开始
     */
    private int pageNum;
    /**
     * 页面大小
     */
    private int pageSize;
    /**
     * 起始行
     */
    private int startRow;
    /**
     * 末行
     */
    private int endRow;
    /**
     * 总数
     */
    private long total;
    /**
     * 总页数
     */
    private int pages;
    /**
     * 包含count查询
     */
    private boolean count = true;
    /**
     * 分页合理化
     */
    private Boolean reasonable;
    /**
     * 当设置为true的时候，如果pagesize设置为0（或RowBounds的limit=0），就不执行分页，返回全部结果
     */
    private Boolean pageSizeZero;
    /**
     * 进行count查询的列名
     */
    private String countColumn;
    /**
     * 排序
     */
    private String orderBy;
    /**
     * 只增加排序
     */
    private boolean orderByOnly;

    public Page() {
        super();
    }

    /**
     *一系列的构造函数
     */
    public Page(int pageNum, int pageSize) {
        this(pageNum, pageSize, true, null);
    }

    public Page(int pageNum, int pageSize, boolean count) {
        this(pageNum, pageSize, count, null);
    }

    private Page(int pageNum, int pageSize, boolean count, Boolean reasonable) {
        super(0);
        if (pageNum == 1 && pageSize == Integer.MAX_VALUE) {
            pageSizeZero = true;
            pageSize = 0;
        }
        this.pageNum = pageNum;
        this.pageSize = pageSize;
        this.count = count;
        calculateStartAndEndRow();
        setReasonable(reasonable);
    }

    /**
     * int[] rowBounds
     * 0 : offset
     * 1 : limit
     */
    public Page(int[] rowBounds, boolean count) {
        super(0);
        if (rowBounds[0] == 0 && rowBounds[1] == Integer.MAX_VALUE) {
            pageSizeZero = true;
            this.pageSize = 0;
        } else {
            this.pageSize = rowBounds[1];
            this.pageNum = rowBounds[1] != 0 ? (int) (Math.ceil(((double) rowBounds[0] + rowBounds[1]) / rowBounds[1])) : 0;
        }
        this.startRow = rowBounds[0];
        this.count = count;
        this.endRow = this.startRow + rowBounds[1];
    }
    /**
     *构造函数结束
     */
    
    /**
     *Setter and Getter
     */
    public List<E> getResult() {
        return this;
    }

    public int getPages() {
        return pages;
    }

    public Page<E> setPages(int pages) {
        this.pages = pages;
        return this;
    }

    public int getEndRow() {
        return endRow;
    }

    public Page<E> setEndRow(int endRow) {
        this.endRow = endRow;
        return this;
    }

    public int getPageNum() {
        return pageNum;
    }

    public Page<E> setPageNum(int pageNum) {
        //分页合理化，针对不合理的页码自动处理
        this.pageNum = ((reasonable != null && reasonable) && pageNum <= 0) ? 1 : pageNum;
        return this;
    }

    public int getPageSize() {
        return pageSize;
    }

    public Page<E> setPageSize(int pageSize) {
        this.pageSize = pageSize;
        return this;
    }

    public int getStartRow() {
        return startRow;
    }

    public Page<E> setStartRow(int startRow) {
        this.startRow = startRow;
        return this;
    }

    public long getTotal() {
        return total;
    }

    public void setTotal(long total) {
        this.total = total;
        if (total == -1) {
            pages = 1;
            return;
        }
        if (pageSize > 0) {
            pages = (int) (total / pageSize + ((total % pageSize == 0) ? 0 : 1));
        } else {
            pages = 0;
        }
        //分页合理化，针对不合理的页码自动处理
        if ((reasonable != null && reasonable) && pageNum > pages) {
            pageNum = pages;
            calculateStartAndEndRow();
        }
    }

    public Boolean getReasonable() {
        return reasonable;
    }

    public Page<E> setReasonable(Boolean reasonable) {
        if (reasonable == null) {
            return this;
        }
        this.reasonable = reasonable;
        //分页合理化，针对不合理的页码自动处理
        if (this.reasonable && this.pageNum <= 0) {
            this.pageNum = 1;
            calculateStartAndEndRow();
        }
        return this;
    }

    public Boolean getPageSizeZero() {
        return pageSizeZero;
    }

    public Page<E> setPageSizeZero(Boolean pageSizeZero) {
        if (pageSizeZero != null) {
            this.pageSizeZero = pageSizeZero;
        }
        return this;
    }
    public String getOrderBy() {
        return orderBy;
    }

    public <E> Page<E> setOrderBy(String orderBy) {
        this.orderBy = orderBy;
        return (Page<E>) this;
    }

    public boolean isOrderByOnly() {
        return orderByOnly;
    }

    public void setOrderByOnly(boolean orderByOnly) {
        this.orderByOnly = orderByOnly;
    }
    /**
     *End
     */
    
    
    
    /**
     *各种逻辑操作
     */
    /**
     * 计算起止行号
     */
    private void calculateStartAndEndRow() {
        this.startRow = this.pageNum > 0 ? (this.pageNum - 1) * this.pageSize : 0;
        this.endRow = this.startRow + this.pageSize * (this.pageNum > 0 ? 1 : 0);
    }

    public boolean isCount() {
        return this.count;
    }

    public Page<E> setCount(boolean count) {
        this.count = count;
        return this;
    }

    /**
     * 设置页码
     *
     * @param pageNum
     * @return
     */
    public Page<E> pageNum(int pageNum) {
        //分页合理化，针对不合理的页码自动处理
        this.pageNum = ((reasonable != null && reasonable) && pageNum <= 0) ? 1 : pageNum;
        return this;
    }

    /**
     * 设置页面大小
     *
     * @param pageSize
     * @return
     */
    public Page<E> pageSize(int pageSize) {
        this.pageSize = pageSize;
        calculateStartAndEndRow();
        return this;
    }

    /**
     * 是否执行count查询
     *
     * @param count
     * @return
     */
    public Page<E> count(Boolean count) {
        this.count = count;
        return this;
    }

    /**
     * 设置合理化
     *
     * @param reasonable
     * @return
     */
    public Page<E> reasonable(Boolean reasonable) {
        setReasonable(reasonable);
        return this;
    }

    /**
     * 当设置为true的时候，如果pagesize设置为0（或RowBounds的limit=0），就不执行分页，返回全部结果
     *
     * @param pageSizeZero
     * @return
     */
    public Page<E> pageSizeZero(Boolean pageSizeZero) {
        setPageSizeZero(pageSizeZero);
        return this;
    }

    /**
     * 指定 count 查询列
     *
     * @param columnName
     * @return
     */
    public Page<E> countColumn(String columnName) {
        this.countColumn = columnName;
        return this;
    }

    public PageInfo<E> toPageInfo() {
        PageInfo<E> pageInfo = new PageInfo<E>(this);
        return pageInfo;
    }

    public PageSerializable<E> toPageSerializable() {
        PageSerializable<E> serializable = new PageSerializable<E>(this);
        return serializable;
    }

    public <E> Page<E> doSelectPage(ISelect select) {
        select.doSelect();
        return (Page<E>) this;
    }

    public <E> PageInfo<E> doSelectPageInfo(ISelect select) {
        select.doSelect();
        return (PageInfo<E>) this.toPageInfo();
    }

    public <E> PageSerializable<E> doSelectPageSerializable(ISelect select) {
        select.doSelect();
        return (PageSerializable<E>) this.toPageSerializable();
    }

    public long doCount(ISelect select) {
        this.pageSizeZero = true;
        this.pageSize = 0;
        select.doSelect();
        return this.total;
    }

    public String getCountColumn() {
        return countColumn;
    }

    public void setCountColumn(String countColumn) {
        this.countColumn = countColumn;
    }

    @Override
    public String toString() {
        return "Page{" +
                "count=" + count +
                ", pageNum=" + pageNum +
                ", pageSize=" + pageSize +
                ", startRow=" + startRow +
                ", endRow=" + endRow +
                ", total=" + total +
                ", pages=" + pages +
                ", reasonable=" + reasonable +
                ", pageSizeZero=" + pageSizeZero +
                '}' + super.toString();
    }

    @Override
    public void close() {
        PageHelper.clearPage();
    }
}

```

##  <del>XJB</del>总结

Page类最基础的需要

```java
PageSize //一页的数据大小
PageIndex //当前所在页数
totalPage; //总页数
totalNum; //总数据量

//构造函数，初始化以上的属性
//并且处理上面属性参数缺省的时候设定默认值

//分页查询时应该还有偏移量，可以通过pageIndex和pageSize得到
```

<ins>	实例代码</ins>中还有`查询列名`，用于查询，还有`Page`中的`doSelect`方法，然后执行一个通用的接口`Iselect`的方法`doSelect`，并且`Page`类中的很多属性都有逻辑关联的，比如`当前页数`和`偏移量`，在设置的时候需要注意

​	除此之外，gitHub或者其他地方都有很多自己写的通用分页类，可以删删改改用

#Java Stream、File&IO

![img](https://www.runoob.com/wp-content/uploads/2013/12/iostream2xx.png)

流：输入流从一个源读取数据、输出流向一个目标写数据

Java.io 包中的流支持很多种格式，比如：基本类型、对象、本地化字符集等

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
System.out.println(char()br.read());//读一个字符
System.out.println(br.readline());//读一行字符

String s = new String("Hello Ws.20")
System.out.println(s);//输出s
System.out.write(s);//输出s
//到控制台

File f = new File(FilePath);
InputStream in = new FileInputStream(f);
OutputStream out = new FileOutputStream(f);
```

除了 InputStream 外，还有一些其他的输入流，更多的细节参考下面链接：

- [ByteArrayInputStream](https://www.runoob.com/java/java-bytearrayinputstream.html)
- [DataInputStream](https://www.runoob.com/java/java-datainputstream.html)

除了OutputStream外，还有一些其他的输出流，更多的细节参考下面链接：

- [ByteArrayOutputStream](https://www.runoob.com/java/java-bytearrayoutputstream.html)
- [DataOutputStream](https://www.runoob.com/java/java-dataoutputstream.html)

更多用法[参考](https://www.runoob.com/java/java-files-io.html)

## File和I/O

一些关于文件和I/O的类：

- [File Class(类)](https://www.runoob.com/java/java-file.html)
- [FileReader Class(类)](https://www.runoob.com/java/java-filereader.html)
- [FileWriter Class(类)](https://www.runoob.com/java/java-filewriter.html)

```java
String dirName = new String("Ws");
String dirsName = new String("\user\Ws");

File dir = new File(dirName);
dir.mkdir();//创建文件夹(Ws)
File dirs = new File(dirsName);
dirs.mkdirs;//创建文件及其父文件夹（user和Ws都会创建）
```

File常见操作：

```java
File f = new File(FilePath);
```



| 方法 | 作用 |
| ---- | ---- |
|      Files. exists()|检测文件路径是否存在。|
| Files. createFile()|创建文件|
|Files. createDirectory()|创建文件夹|
|Files. delete()|删除一个文件或目录|
| Files. copy()|复制文件|
|Files. move()|移动文件|
|Files. size()|查看文件个数|
|Files. read()|读取文件|
|Files. write()|写入文件|

## Scanner类

java.util.Scanner 是 Java5 的新特征，我们可以通过 Scanner 类来获取用户的输入

```java
Scanner scan = new Scanner(System.in);
while(scan.hasNext()){
    String s = scan.next();
}
scan.close();
//还有scan.nextLine(),用法同上
```

next()&nextLine()的区别：

next():

- 1、一定要读取到有效字符后才可以结束输入。
- 2、对输入有效字符之前遇到的空白，next() 方法会自动将其去掉。
- 3、只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
- 4、next() 不能得到带有空格的字符串。

nextLine()：

- 1、以Enter为结束符,也就是说 nextLine()方法返回的是输入回车之前的所有字符。
- 2、可以获得空白。

除此之外还有`Scanner.hasNextInt()`和`Scanner.hasNextFloat()`

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

# 多线程

线程：一条线程是指进程中的一个单一顺序的控制流，一个进程中可以并发多个线程，每个线程并行执行不同的任务<br/>**满足编写高效率的程序来充分达到应用CPU的目的**

线程的状态：

- 新建状态
- 就绪状态
- 运行状态
- 阻塞状态
- 死亡状态

## 线程的优先级

每一个Java线程都有一个优先级，Java线程的优先级是一个数字，从1到10<br/>默认状态下分配的优先级是5<br/>线程的优先级并不能**保证**线程的执行顺序，而且非常依赖于平台

## 创建一个线程

创建线程的三种方法：

- 实现Runnable接口
- 通过继承Thread类本身
- 通过Callable和Future创建线程

## 通过实现Runnable

实现Runnable需要实现一个方法调用`public void run()`<br/>run()可以调用其他方法，使用其他类，并声明变量，就像主线程一样  ==理解==

Thread定义了几个构造方法，下面这个是常用的：

```java
Thread(Runnable threadOb,String threadName);
```

threadOb是一个实现了Runnable接口方法的一个类实例，threadName指定新线程的名字<br/>新线程创建之后你可以使用`void run()`运行它

### 实例

```java
class RunnableDemo implements Runnable {
   private Thread t;
   private String threadName;
   
   RunnableDemo( String name) {
      threadName = name;
      System.out.println("Creating " +  threadName );
   }
   
   public void run() {
      System.out.println("Running " +  threadName );
      try {
         for(int i = 4; i > 0; i--) {
            System.out.println("Thread: " + threadName + ", " + i);
            // 让线程睡眠一会
            Thread.sleep(50);
         }
      }catch (InterruptedException e) {
         System.out.println("Thread " +  threadName + " interrupted.");
      }
      System.out.println("Thread " +  threadName + " exiting.");
   }
   
   public void start () {
      System.out.println("Starting " +  threadName );
      if (t == null) {
         t = new Thread (this, threadName);
         t.start ();
      }
   }
}
 
public class TestThread {
 
   public static void main(String args[]) {
      RunnableDemo R1 = new RunnableDemo( "Thread-1");
      R1.start();
      
      RunnableDemo R2 = new RunnableDemo( "Thread-2");
      R2.start();
   }   
}
```

run()定义了线程需要执行的操作，start()定义了这个实现类开始运行的方法<br/>在上面这个例子是调用Thread的构造方法，然后将自身作为参入传入Thread中<br/>当调用这个实现类的start()的时候，实例化了一个Thread，然后调用自身的run()方法（*因为将自身作为参数传入了Thread的构造方法*）

我的理解：

自己编写一个实现类实现Runnable接口<br/>需要重写run方法，用于自定义这个线程需要做的是什么<br/>需要重写start()方法，用于怎么开始<br/>讲不清楚了，看==例子==

## 通过继承Thread

```java
class ThreadDemo extends Thread {
   private Thread t;
   private String threadName;
   
   ThreadDemo( String name) {
      threadName = name;
      System.out.println("Creating " +  threadName );
   }
   public void run() {
      System.out.println("Running " +  threadName );
      try {
         for(int i = 4; i > 0; i--) {
            System.out.println("Thread: " + threadName + ", " + i);
            // 让线程睡眠一会
            Thread.sleep(50);
         }
      }catch (InterruptedException e) {
         System.out.println("Thread " +  threadName + " interrupted.");
      }
      System.out.println("Thread " +  threadName + " exiting.");
   }
   public void start () {
      System.out.println("Starting " +  threadName );
      if (t == null) {
         t = new Thread (this, threadName);
         t.start ();
      }
   }
} 
public class TestThread {
 public static void main(String args[]) {
     ThreadDemo T1 = new ThreadDemo( "Thread-1");
     T1.start();
     ThreadDemo T2 = new ThreadDemo( "Thread-2");
     T2.start();
   }   
}
```

创建一个新的类，继承Thread，然后创建一个该类的实例<br/>必须重写run()方法，也必须需要start()方法才能执行<br/>本质上也是实现了Runnable的一个实例类

==同上==

## Thread 方法

下表列出了Thread类的一些重要方法：

| **序号** |                         **方法描述**                        |
| :------- | :-------------------------------------------------------: |
| 1        | **public void start()** 使该线程开始执行；**Java** 虚拟机调用该线程的 run 方法。 |
| 2        | **public void run()** 如果该线程是使用独立的 Runnable 运行对象构造的，则调用该 Runnable 对象的 run 方法；否则，该方法不执行任何操作并返回。 |
| 3        | **public final void setName(String name)** 改变线程名称，使之与参数 name 相同。 |
| 4        | **public final void setPriority(int priority)**  更改线程的优先级。 |
| 5        | **public final void setDaemon(boolean on)** 将该线程标记为守护线程或用户线程。 |
| 6        | **public final void join(long millisec)** 等待该线程终止的时间最长为 millis 毫秒。 |
| 7        |            **public void interrupt()** 中断线程。            |
| 8        | **public final boolean isAlive()** 测试线程是否处于活动状态。 |

测试线程是否处于活动状态。 上述方法是被Thread对象调用的。下面的方法是Thread类的静态方法。

| **序号** |                         **方法描述**                         |
| :------- | :----------------------------------------------------------: |
| 1        | **public static void yield()** 暂停当前正在执行的线程对象，并执行其他线程。 |
| 2        | **public static void sleep(long millisec)** 在指定的毫秒数内让当前正在执行的线程休眠（暂停执行），此操作受到系统计时器和调度程序精度和准确性的影响。 |
| 3        | **public static boolean holdsLock(Object x)** 当且仅当当前线程在指定的对象上保持监视器锁时，才返回 true。 |
| 4        | **public static Thread currentThread()** 返回对当前正在执行的线程对象的引用。 |
| 5        | **public static void dumpStack()** 将当前线程的堆栈跟踪打印至标准错误流。 |

## Callable和Future

```java
public class CallableThreadTest implements Callable<Integer> {
    public static void main(String[] args){  
        CallableThreadTest ctt = new CallableThreadTest();  
        FutureTask<Integer> ft = new FutureTask<>(ctt);  
        for(int i = 0;i < 100;i++){  
            System.out.println(Thread.currentThread().getName()+" 的循环变量i的值"+i);  
            if(i==20){  
                new Thread(ft,"有返回值的线程").start();}  
        }  
        try {  
            System.out.println("子线程的返回值："+ft.get());} 
        catch (InterruptedException e){  
            e.printStackTrace();} 
        catch (ExecutionException e){  
            e.printStackTrace(); }  
  }
    @Override  
    public Integer call() throws Exception{  
        int i = 0;  
        for(;i<100;i++){  
            System.out.println(Thread.currentThread().getName()+" "+i);}  
        return i;  
    }  
}
```

- 实现Callable接口，并实现call()方法
- call()方法中是线程的执行体
- 使用FutureTask类来包装对象，该对象封装了call()的返回数据类型
- 使用Future的实例作为Thread创建实例的参数
- 调用FutureTask.get()获得线程执行后的返回值

## 三种方法的对比

实现Callable和Runnable时，只是实现了对应的接口，还可以继承其他类

使用Thread的时候，编写简单，访问当前线程简单（*直接使用this*）

##需要了解的

- 线程同步
- 线程间通信
- 线程死锁
- 线程控制：挂起、停止和恢复

有限理解多线程的关键是理解程序是并行执行的而不是串行执行的，通过对多线程的使用，可以编写出非常高效的程序，不过如果创建太多的线程，实际上是变慢了，因为：**切换上下文的开销（时间内存等）大于程序执行的开销**

## 守护进程

守护进程是运行在后台的一种特殊的进程。它独立于控制终端周期性的某个任务或者等待处理某些任务<br/>不会因为关闭窗口等原因结束进程，常见于服务器等，java 的垃圾回收进程也是守护进程

守护进程的特点：<br/>守护进程必须与其运行前的环境隔离开来。这些环境包括未关闭的文件描述符、[控制终端](https://baike.baidu.com/item/控制终端)、[会话](https://baike.baidu.com/item/会话)和[进程组](https://baike.baidu.com/item/进程组)、工作目录以及文件创建[掩码](https://baike.baidu.com/item/掩码)等。这些环境通常是守护进程从执行它的[父进程](https://baike.baidu.com/item/父进程)(特别是shell)继承下来的

守护进程和普通进程的本质是一样的，所以，创建守护进程的方法就是一一实现它的那些特征：

1. 创建子进程，终止父进程（使程序以僵尸进程的形式运行，从一定程度上做到了与控制终端脱离）
2. 在子进程中创建新会话，并担任该会话组的组长，使用系统函数setsid()，setsid()的三个作用：
  1. 摆脱原会话的控制
  2. 摆脱原进程组的控制
  3. 摆脱原终端的控制
3. 改变工作目录
4. 重设文件创建掩码
5. 关闭文件描述符

## sleep(),wait()&yeild()

![img](https://upload-images.jianshu.io/upload_images/66827-780462c52b8f5a83.png)

### sleep()

- Thread.sleep()方法用来暂停线程的执行，将CPU放给线程调度器。
- Thread.sleep()方法是一个静态方法，它暂停的是当前执行的线程。
- Java有两种sleep方法，一个只有一个毫秒参数，另一个有毫秒和纳秒两个参数。
- 与wait方法不同，sleep方法不会释放锁
- 如果其他的线程中断了一个休眠的线程，sleep方法会抛出Interrupted Exception。
- 休眠的线程在唤醒之后不保证能获取到CPU，它会先进入就绪态，与其他线程竞争CPU。
- 有一个易错的地方，当调用t.sleep()的时候，会暂停线程t。这是不对的，因为Thread.sleep是一个静态方法，它会使当前线程而不是线程t进入休眠状态。

==注意==：sleep()和yeild()都是作用于**当前进程**

###wait()

wait()用于线程间通信，来源于Object类<br/>在调用的时候会释放所持有对象的管程和锁（sleep()不会)<br/>在调用之后会等待某种条件(自定义)满足之后调用notify()函数使之进入就绪状态参与CPU竞争

### yeild()

只是释放CPU资源，让其他线程有运行的机会<br/>哪个线程能获得CPU完全取决于调度器，有时候甚至是调用yeild()方法的线程再次获得CPU

## 线程池

###ThreadPoolExecutor

ThreadPoolExecutor、AbstractExecutorService、ExecutorService和Executor之间的关系：

interface ExccutorService extends interface Excutor;

abstract class AbstractExcutorService implements ExcutorService;

class ThreadPoolExcutor extends AbstractExcutorService;

ThreadPoolExecutor、AbstractExecutorService、ExecutorService和Executor

ThreadPoolExcutor的几个关键参数：

- corePoolSize：核心池大小
- maximumPoolSize ：线程池最大线程数量
- keepAliveTime ：当线程空闲的时候允许的最长存活时间
- allowCoreThreadTimeOut：是否允许设置线程存活时间
- unit：keepAliveTime的单位
- workQueue：阻塞队列，核心池满的时候新来的任务放在阻塞队列
- threadFactory：线程工厂，主要用来创建线程
- handle：拒绝任务处理的时的策略
  - 丢弃任务并抛出RejectedExecutionException异常
  - 丢弃任务但不抛出异常
  - 丢弃队列最前面的任务并尝试重新执行（重复此过程）
  - 由调用线程处理该任务

corePoolSize和maximumPoolSize的区别：

> corePoolSize是线程池的最大容量<br/>当线程池的里面线程的数量达到corePoolSize的时候，会放到缓冲队列里面<br/>当缓冲队列为空并且线程数量小于corePoolSize时会销毁线程
>
> maximumPoolSize是线程池允许的最大容量<br/>当线程池满并且缓冲队列满的时候会创建新的线程来执行任务（此时线程数量已经大于corePoolSize）<br>但是线程池容量最大不可以超过maximumPoolSize，否则抛出异常

ThreadPoolExcutor的几个关键方法：

- execute()：向线程池提交一个任务，交由线程池执行
- submit()：也是提交任务，但是会返回结果
- shutdown()：关闭线程池（等待已经存在任务执行完）
- shutdownNow：立即关闭线程池

### 线程池状态

- RUNNING：接收新的任务，处理等待队列中的任务
- SHUTDOWN：不再接收新的任务，等待正在处理的和等待队列中的任务全部处理完成
- STOP：不接收新的任务，结束正在执行的任务，清空等待队列
- TIDYING：所有线程销毁，队列为空，在向TERMINATED转变，会执行钩子方法 terminated()
- TERMINATED：所有工作线程已经销毁，等待队列为空

### 线程池大小

如何合理配置线程池的大小：

> 如果是CPU密集型
>
> &blablabla.....

线程池请看这篇==[Blog](https://www.cnblogs.com/dolphin0520/p/3932921.html)==

##Java锁机制

Java多线程加锁机制：

- Synchronized
- 显示Lock

### Synchronized

Synchronized是一个关键字，用来将代码锁起来：

```java
public synchronized void methordName(){}
```

- synchronized是一种互斥锁

  > 一次只允许一个线程进入被锁住的代码块

- synchronized是一种内置锁/监视器锁

  > Java每个对象中都有一个内置锁，synchronized关键字就是激活这种内置锁的

Synchronized的作用：

> - 原子性：没有线程能够同时访问被保护的代码块
> - 可见性：修改后的变量对其他线程是可见的

#### 原理

妈呀，写的啥啊，懵逼来懵逼去

具体可参考：

- [blog.csdn.net/chenssy/art…](https://link.juejin.im/?target=https%3A%2F%2Fblog.csdn.net%2Fchenssy%2Farticle%2Fdetails%2F54883355)
- [blog.csdn.net/u012465296/…](https://link.juejin.im/?target=https%3A%2F%2Fblog.csdn.net%2Fu012465296%2Farticle%2Fdetails%2F53022317)

#### 使用

```java
public class testClass{
    public synchronized void test(){
    //方法体
	}

	public void test(){
    	synchronized(this){
    	    //代码块
    	}
	}
}
```

上面的例子是修饰方法和代码块的使用方法

修饰静态方法时：

> 因为静态方法属于类方法，所以获得的锁是属于类的锁（类的字节码文件对象）

修饰普通方法或者代码块时：

>  获取的是对象锁

**它们是不冲突的**

重入性:

> 例如对象锁<br/>当你因为要使用某一代码块已经获得了一个实例对象的对象锁，再访问其他代码块的内容，也是可以的！！！

因为锁的持有者是线程，一个线程持有一个实例对象的对象锁，自然可以访问它的所有锁定的代码块

释放锁：

> 被锁定的方法/代码块执行完毕之后会自动释放锁不需要其他操作<br/>当执行的代码块出现异常的时候其持有的所有锁都会自动释放<br/>程序进入WAITING状态时（比如调用了wait()方法）
>
> 所以：不会出现异常而导致死锁的情况

### 显式锁Lock

https://blog.csdn.net/a78270528/article/details/79896466自己看

# Java序列化

Java的序列化机制：

> 用于保存对象在内存中的各种状态和属性，并且可以再读出来<br/>一个对象可以被表示为一个字节序列，该字节序列包括该对象的数据、有关对象的类型信息和存储在对象中数据的类型<br/>将序列化对象写入文件之后可以从文件中读取出来并且进行反序列化<br/>整个过程是JVM独立的，即序列化和反序列化不受平台影响

一个类想要序列化成功，必须满足两个条件：

> - 该类必须实现`implements java.io.Serializable`对象
> - 该类的属性必须都是可序列化的，如果不可，必须注明是短暂的`public transient ...`
>
> 如果想要知道一个Java的标准类是否可序列化，只需要看它有没有实现`java.io.Serizlizable`

序列化与反序列化：

```java
import java.io.*

public class outTest{
	public static void main(String[] args){
		Student s = new Student();
		s.setName("shuyang.lei");
		try{
			FileOutputStream fileOut = new FileOutputStream("user/output/stu.ser");
			ObjectOutputStream out = new ObjectOutputStream(fileOut);
			out.writeObject(s);
			out.close();
			fileOut.close();
		}catch(IOException i){
			i.printStackTrace;
		}
	}
}
```



```java
import java.io.*

public class inTest{
	public static void main(String[] args){
		Student s = null;
		try{
			FileInputStream fileIn = new FileInputStream("user/output/stu.ser");
			ObjectInputStream in = new ObjectInputStream(fileIn);
			e = (Student)in.readObject;
			in.close();
			fileIn.close();
		}catch(IOException i){
         	i.printStackTrace();
         	return;
      }catch(ClassNotFoundException c){
         System.out.println("Student class not found");
         c.printStackTrace();
         return;
      }
      System.out.println("Name:"+s.getName());
	}
}
```

==注意==:

- 记得关闭`FileOutputStream,ObjectOutputStream`等
- 系列化输出的文件后缀名标准约定为.ser
- 记得`catch  IOException&ClassNotFoundException`
- 如果因为标注了`transient`而没有序列化的属性反序列化之后是改类型的默认值int(0),Object(null)

# Java代理

代理类，已经与之对应的委托类

优点：

> 可以隐藏委托类的实现<br/>可以实现客户与委托类之间的解耦，可以在不修改委托类的情况下额外做一些处理（修改隐藏类）

## 静态代理

```java
public class StudentAgent implements People{
    private Student stu;
    
    public StudentAgent(Student stu){
        this.stu = stu;
    }
    public void learn(Object IT){
        if(ITStudent){
        	stu.learn(math);
        }
    }
    public void run(){
        stu.run();
    }
}
```

大概就是Student类的代理：

> 存储了一个student类型的对象<br/>方法调用存储的student对象的方法<br/>还可以在不修改Student类的情况下加（或者修改）一些逻辑，比如上面的如果不是IT学生不学IT<br/>一般情况下委托类和代理类都实现统一接口或者派生自相同的父类

静态代理类需要在程序执行之前已经存在（手动编写）

## 动态代理

代理类在程序运行时被创建，也就是说代理类并不是在Java代码中定义的，而是根据我们在Java代码中的逻辑自动生成的

相比静态代理的优势：

> 可以很方便的对代理类的函数进行统一处理

### 使用动态代理

####JDK原生代理

```java
public class DynamicProxy implements InvocationHandler{
    private Object obj;
    public DynamicProxy(Object obj){
        this.obj = obj;
    }
    @Override
    public Object invoke(Object proxy, Method method,Object[] args) throws Throwable{
        System.out.println("Before");
        Object result = method.invoke(obj, args);
        Sys.out.println("After");
        return result;
    }
}
```

`method.invoke`：method类的方法，反射调用

`DynamicProxy`：自己编写的中介类，实现的是`InvocationHandler`接口，必须要重写`invoke`方法，在invote中调用委托类的方法，并且可以统一编写自己的逻辑。

```java
public class Main{
    public static void main(String[] args){
        DynamicProxy inter = new DynamicProxy(new Student());
        //执行下面这条语句会产生一个$Proxy0.class的文件，即动态生成的代理类文件
        System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles","true");
        Student stu = (Student)(Proxy.newProxyInstance(Student.class.getClassLoader(),new class[] {Student.class},inter));
        stu.run();
        stu.learn(IT);
    }
}
```

首先实例化一个中介类，传入委托类<br>然后调用`Proxy.newProxyInstance()`来获取一个代理类实例<br/>再然后就可以使用了

`public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h) throws IllegalArgumentException `

方法参数列表：

> loader：定义了代理类的ClassLoder<br/>interfaces：代理类实现的接口列表<br/>h：调用处理器，也就是我们上面定义的实现了InvocationHandler接口的类实例

####cglib代理

cglib是一种第三方代理模式，在使用的时候需要导入以下三方jar包：

1. asm-2.2.3.jar
2. asm-commons-2.2.3.jar
3. asm-util-2.2.3.jar
4. cglib-nodep-2.1_3.jar

CGLIB动态代理模式不需要想JDK动态代理模式那样使用接口<br/>一个非抽象类就可以<br/>这个非抽象类需要实现MethodInterceptor接口，并重写intercept方法

```java
public class Hello{
    public void sayHello(){
        System.out.println("HelloWorld");
    }
}

public class CglibProxy implements MethodInterceptor{
    public Object getProxy(Class cls){
        Enhancer en = new Enhancer();
        en.SetSuperclass(cls);
        en.SetCallback(this);
        Object proxy = en.create();
        return proxy;
    }
    @Override
    public Object intercept(Object proxy,Method method,Object[] args,MethodProxy methodPrxy) throws Throwable{
        System.out.println("Before");
        Object result = methodProxy.invokeSuper(proxy, args);
        System.out.println("After");
        return result;
    }
}

public class test{
    public static void main(String[] args){
        CglibProxy cg = new CglibProxy();
        Hello h = (Hello)cg.getProxy(Hello.class);
        h.sayHello();
    }
}
```

在一般的例子中，getProxy里面的步骤并没有写到中介类里面（我觉得包装之后更优雅）<br/>区别就是在`setCallback`的时候传的参数就不是`this`<br/>而是`en.setCallback(new yourProxy)`

我觉得上面的代码还是蛮简单的，可以理解，不可以理解的都时不变的，copy就好了

#### 对比

原生代理不需要导入包，依靠java原生的类就可以实现<br/>原生代理委托类必须要实现至少一个接口，cglib代理不用

