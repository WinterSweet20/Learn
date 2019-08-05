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



<div STYLE="page-break-after: always;"></div>



# MySQL

##基础

###基础操作(一)

```sql
SELECT DISTINCT columns FROM Table_Name WHERE column_one LIKE "word" AND column_two = "0" OR column_two = "1" ORDER BY column_one, column_two DESC;

INSERT INTO Table_Name (columns)
VALUES (dates);

UPDATE Table_Name
SET column_one = 'date_one', column_two = 'date_two'
WHERE id = 'id';

DELETE FROM Table_Name
WHERE ID = 'id';
```

从`FROM`一张叫做`Table_Name`的表中查询`SELECT`,`columns`列，相同的只展示一条数据`DINSTINCT`，条件`WHERE`：列1`column_one`像`LIKE`字符串`word`并且`AND`列2`column_two`等于1或者`OR`等于0，按照列1和列2排序`ORDER BY`，排序方式为降序`DESC`（默认升序）

往一张叫做`Table_Name`的表中插入列名`columns`和值`dates`对应的一条数据（`columns`可以缺省，但是缺省的时候必须插入表中完整的字段）

更新`UPDATE`表`Table_Name`中id等于‘id’的一条数据，设置`SET`列1`column_one`等于`'date_one'`，<br/>列2<`column_two`等于`'date_two'`<br/>==WHERE很重要！！！！没有WHERE会更新表中的所有数据==

从表`Table_Name`中删除`DELETE`ID为`'id'`的数据

###WHERE子句中的运算符

| 运算符  | 描述                                                       |
| :------ | :--------------------------------------------------------- |
| =       | 等于                                                       |
| <>      | 不等于。**注释：**在 SQL 的一些版本中，该操作符可被写成 != |
| >       | 大于                                                       |
| <       | 小于                                                       |
| >=      | 大于等于                                                   |
| <=      | 小于等于                                                   |
| BETWEEN | 在某个范围内                                               |
| LIKE    | 搜索某种模式                                               |
| IN      | 指定针对某个列的多个可能值                                 |

###基础操作(二)

```sql
CREATE DATABASE my_db;

CREATE TABLE table_name
(
column_name1 data_type(size),
column_name2 data_type(size),
column_name3 data_type(size),
....
);

CREATE TABLE table_name
(
column_name1 data_type(size) constraint_name,
column_name2 data_type(size) constraint_name,
column_name3 data_type(size) constraint_name,
....
);
```

创建一个名为`my_db`的数据库

创建一个名字为`table_name`的表，数据类型为`data_type`，大小为`size`

创建一个名字为`table_name`的表，数据类型为`data_type`，大小为`size`，约束条件为`constraint_name`

#### 约束

| 约束名称   | 约束效果                       |
| ---------- | ------------------------------ |
| *NOT NULL* | 不接受空值（包括插入和更新时） |
|*UNIQUE*|只能有唯一值|
|*PRIMARY KEY*|主键|
|*FOREIGN KEY*|外键|
|*CHECK*|限制值的范围|
|*DEFAULT*|设置默认值|

##### UNIQUE

##### PRIMARY KEY

##### FOREIGN KEY

##### CHECK

##### DEFAULT



<div STYLE="page-break-after: always;"></div>




## 进阶

[菜鸟](https://www.runoob.com/sql/sql-top.html)

### LIMIT,LIKE,REGEXP,IN,BETWEEN

```sql
SELECT columns FROM TableName LIMIT num ORDER BY id DESC;
```

从表`Table_Name`中倒叙`DESC`查找`SELECT`一定`LIMIT`数量`num`条数据（默认前`num`条）

```sql
SELECT columns FROM Table_Name WHERE clomn_one LIKE '%part';
```

LIKE子句与通配符

通配符

|通配符|描述|
| ------------------------------ | -------------------------- |
| %                              | 替代 0 个或多个字符        |
| _                              | 替代一个字符               |
| [*charlist*]                   | 字符列中的任何单一字符     |
| [^*charlist*] 或 [!*charlist*] | 不在字符列中的任何单一字符 |

```sql
SELECT columns FROM Table_Name WHERE clomn_one REGEXP '^[^a-Z]';
```

查询`Table_Name`中`column_one`不以`a-Z`开头的

```sql
SELECT columns FROM Table_Name WHERE clomn_one IN (0,1,2,3)
```

查询`Table_Name`中`column_one`为0或1或2或3的

```sql
SELECT columns FROM Table_Name WHERE (clomn_one NOT BETWEEN 0 AND 3) AND column_two NOT IN (one,two) 
```

查询表`Table_Name`中`column_one`在0到3之间`column_two`不是`one`和`two`的

`BETWEEN`可以操作文本，数字和日期

###sql别名

```sql
SELECT one.id,CONCAT(one.name, one.age, one.phone, two.address) AS other_mes
FROM student as one, people as two
WHERE one.id = two.id;
```

从`student`和`people`中查找`student`里面的`id,name,age,phone,people`里面的`address`,并且打包命名为`other_mes`，中，`student`的`id`等于`people`的`id`的

在下面的情况下，使用别名很有用：

- 在查询中涉及超过一个表
- 在查询中使用了函数
- 列名称很长或者可读性差
- 需要把两个列或者多个列结合在一起

### JOIN

INNER JION 和 WHERE的区别：

WHERE子句中使用的连接语句，在数据库语言中，被称为隐性连接。INNER JOIN……ON子句产生的连接称为显性连接。

四种JION:

- **INNER JOIN**：如果表中有至少一个匹配，则返回行
- **LEFT JOIN**：即使右表中没有匹配，也从左表返回所有的行
- **RIGHT JOIN**：即使左表中没有匹配，也从右表返回所有的行
- **FULL JOIN**：只要其中一个表中存在匹配，则返回行



<img src = "https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png" alt="无法显示图片"/>

#### INNER JOIN

```SQL
SELECT one.name,one.age
FROM Table_one as one
INNER JOIN Table_two
ON one.id = two.id;
```

查询表1中的`id`和表2中的`id`一样的数据，展示表1的`name,age`

#### LEFT JOIN

```sql
SELECT one.name, one.age, two.addr 
FROM Table_one as one
LEFT JOIN Table_two
ON one.id = two.id;
```

查询表1中的`id`和表2中的`id`一样的数据，展示表1的`name,age`，表2的`addr`，<br/>和表1中的id表2里面没有的数据，展示表1的`name,age`，表2的`addr`为NULL<br/>或者说查询表1里面的所有数据，表1的id表2没有的，表2的字段值为NULL

#### RIGHT JOIN

```sql
SELECT one.name, one.age, two.addr 
FROM Table_one as one
RIGHT JOIN Table_two
ON one.id = two.id;
```

查询表1中的`id`和表2中的`id`一样的数据，展示表1的`name,age`，表2的`addr`，<br/>和表2中的id表1里面没有的数据，展示表2的`addr`，表1的`name,age`为NULL<br/>或者说查询表2里面的所有数据，表2的id表1没有的，表1的字段值为NULL

==<del>不算</del>神奇的发现==

把`LEFT JOIN`和`RIGHT JOIN`，表1和表2的位置互换之后能的到相同的效果<br/>别人又不是傻的，为什么要分`LEFT JOIN &RIGHT JOIN`呢<br/>因为大表在前和小表在前的效率不同？<br/>而且暂时还没搞清楚多表关联这样是不是相同的

#### FULL OUTER JOIN

```sql
SELECT *
FROM Table_one AS one
FULL OUTER JOIN Table_two AS two
ON one.id = two.id;
```

查询表1和表2所有的内容，表1的`id`表2没有的，表2中的内容为`NULL`，表2的`id`表1没有的，表1的字段值为`NULL`



一般要使得数据库查询语句性能好点应遵循一下==原则==: 

- 在做表与表的连接查询时，大表在前，小表在后 
- 不使用表别名，通过字段前缀区分不同表中的字段 
- 查询条件中的限制条件要写在表连接条件前 
- 尽量使用索引的字段做为查询条件 





### UNION

SQL UNION 操作符合并两个或多个 SELECT 语句的结果。

```sql
SELECT name_one FROM Table_one WHERE ID="1"
UNION
SELECT name_two FROM Table_two WHERE ID="1"
ORDER BY name_one
```

选取表1和表2中`ID="1"`的`name`，列名为`name_one`（第一个`SELECT`语句中的列名），按照`name_one`排序，不包含重复值，如果需要重复值，使用`UNION ALL`

### SELECT INTO

从一个表复制数据到另一个表

####SELECT INTO FROM

从一个表里复制数据到另一个新表

```sql
SELECT Table_one.name, Table_one.count, Table_two.date
INTO New_Tabel
FROM Table_one
LEFT JOIN Table_two
ON Table_one.id=Table_two.id
```

复制表1中`name='CN'`的`name,count`，表2中的`date`到新表`New_Table`中

SELECT INTO还可以包含WHERE子句，如

```sql
SELECT * INTO New_Table WHERE name='CN';
```

当原表中没有数据满足WHERE子句中的条件时，创建一个==表结构相同==的空表，如：

```sql
SELECT * INTO New_Table WHERE 0=1;
```

#### INSERT INTO SELECT

从一个表复制数据到另一个已经存在的表

```sql
INSERT INTO Table_two
(Name, Age)
SELECT name, age 
FROM Table_one
WHERE age<=18;
```

从表1复制`age<=18`的数据的`name,age`到表2的`Name,Age`中<br/>如果()缺省，则复制为新的列

INSERT INTO SELECT不会改变目标表已经存在的行

#### 区别

- SELECT INTO FROM要求目标表不存在，INSERT INTO SELECT要求存在

- SELECT INTO FROM用于创建拷贝的新表，INSERT INTO SELECT用于批量导入数据