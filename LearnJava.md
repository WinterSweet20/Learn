# JVM详解

JVM(Java Virtual Mechine)Java虚拟机的缩写

Java虚拟机包括一套字节码指令集、一组寄存器、一个栈、一个垃圾回收堆和一个存储方法域





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


