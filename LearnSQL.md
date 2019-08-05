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

| 约束名称      | 约束效果                       |
| ------------- | ------------------------------ |
| *NOT NULL*    | 不接受空值（包括插入和更新时） |
| *UNIQUE*      | 只能有唯一值                   |
| *PRIMARY KEY* | 主键                           |
| *FOREIGN KEY* | 外键                           |
| *CHECK*       | 限制值的范围                   |
| *DEFAULT*     | 设置默认值                     |

##### UNIQUE

表示只能有唯一值`id INT UNIQUE`

建表时多个列的唯一值`CONSTRAINT UNIQUE (id, age)`

建表之后定义一个名为PersonID的多个列的唯一值约束`ALTER TABLE Table_Name ADD CONSTRAINT PersonID UNIQUE (id, age)`

撤销UNIQUE，`AFTER TABLE Table_Name DROP CONSTRAINT PersonID UNIQUE (id, age)`

##### PRIMARY KEY

表示主键 `PRIMARY KEY id`

建表时多个列共同定义主键：`CONSTRAINT pk_PersonID PRIMARY KEY (id, name)`

建表之后定义和撤销主键同上

撤销主键`ALTER TABLE Table_Name DROP PRIMARY KEY`<br/>撤销多列主键`ALTER TABLE Table_Name DROP CONSTRAIT pk_PersonID)`

==主键必须为不为NULL==

##### FOREIGN KEY

指向另一个表中的`UNIQUE KEY`（唯一约束的键）:`FOREIGN KEY this_id REFERENCES OtherTable(that_id)`

FOREIGN KEY 约束用于预防破坏表之间连接的行为<br/>FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一

表创建之后创建以及撤销外键参照上

##### CHECK

用于限制列中值的范围：`CHECK (id>0)`

创建多列的CHECK和在表创建之后CHECK参上

##### DEFAULT

用于设定默认值：`name varchar(64) DEFAULT 'LeiShuyang'`

表创建之后：`ALTER TABLE Table_Name ALTER name SET DEFAULT 'LeiShuyang'`

撤销：`ALTER TABLE Table_Name ALTER name DROP DEFAULT`



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

| 通配符                         | 描述                       |
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