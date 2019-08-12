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

