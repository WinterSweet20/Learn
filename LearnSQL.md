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

### CREATE INDEX

CREATE INDEX 语句用于在表中创建索引。<br/>在不读取整个表的情况下，索引使数据库应用程序可以更快地查找数据。

```sql
CREATE UNIQUE INDEX Index_Name ON Table_Name (column_one, clounm_two);
```

在表中创建一个列1和列2的唯一`INUQUE`索引`Index_Name`

==注意==：更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。

### DROP

```SQL
ALTER TABLE Table_Name DROP INDEX Indec_Name;
DROP TABLE Table_Name;
DROP DATABASE Database_Name;

TRUNCATE TABLE Table_Name;
```

删除表的索引，删除表，删除数据库

清空表的数据（但是不删除表）

### ALTER TABLE

`ALTER TABLE `语句用于在已有的表中添加、删除或修改列。

```sql
ALTER TABLE Table_Name ADD Column_Name datatype;
ALTER TABLE Table_Name DROP Column_Name;

ALTER TABLE Table_Name MODIFY COLUMN Column_Name datatype;
```

添加/删除列

更改列的数据类型

### AUTO INCREMENT

Auto-increment 会在新记录插入表中时生成一个唯一的数字

```sql
CREATE TABLE Table_Name(
ID int NOT NULL AUTO_INCREMENT=100)
```

在创建表时如果没有自动添加一个数字，从100开始，每次加一（默认从1开始）

在执行INSERT语句的时候如果没有指定id则自动加一，并且下一个自增从这个指定的id开始

==注意==：指定了AUTO_INCREMENT的列必须要建索引，不然会报错（索引可以不为主键索引）

在建表之后亦可以设定自增属性，使用ALTER TABLE，例：

`ALTER TABLE student CHANGE id id INT( 11 ) NOT NULL AUTO_INCREMENT;`

### 视图 VIEW

在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。<br/>视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。<br/>你可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，也可以呈现数据，就像这些数据来自于某个单一的表一样。

```SQL
CREATE VIEW View_Name AS 
SELECT Column_Names FROM Table_Name WHERE ....;
```

视图可以操作（查询，从一张视图生成另一张视图等）

暂时没用到很多，看不懂，[详情](https://www.runoob.com/sql/sql-view.html)

撤销视图：`DROP VIEW View_Name`

### DATA

#### DATA常用函数

| 函数                                                         | 描述                                |
| :----------------------------------------------------------- | :---------------------------------- |
| [NOW()](https://www.runoob.com/sql/func-now.html)            | 返回当前的日期和时间                |
| [CURDATE()](https://www.runoob.com/sql/func-curdate.html)    | 返回当前的日期                      |
| [CURTIME()](https://www.runoob.com/sql/func-curtime.html)    | 返回当前的时间                      |
| [DATE()](https://www.runoob.com/sql/func-date.html)          | 提取日期或日期/时间表达式的日期部分 |
| [EXTRACT()](https://www.runoob.com/sql/func-extract.html)    | 返回日期/时间的单独部分             |
| [DATE_ADD()](https://www.runoob.com/sql/func-date-add.html)  | 向日期添加指定的时间间隔            |
| [DATE_SUB()](https://www.runoob.com/sql/func-date-sub.html)  | 从日期减去指定的时间间隔            |
| [DATEDIFF()](https://www.runoob.com/sql/func-datediff-mysql.html) | 返回两个日期之间的天数              |
| [DATE_FORMAT()](https://www.runoob.com/sql/func-date-format.html) | 用不同的格式显示日期/时间           |

#### Date数据类型

|类型|效果|
|-----------|----------------|
|DATE - 格式|YYYY-MM-DD|
|DATETIME - 格式|YYYY-MM-DD HH:MM:SS|
|TIMESTAMP - 格式|YYYY-MM-DD HH:MM:SS|
|YEAR - 格式|YYYY 或 YY|

完整的 [数据类型参考手册](https://www.runoob.com/sql/sql-datatypes.html)

### NULL值

NULL 值代表遗漏的未知数据<br/>默认的列可以存放NULL值，如果不允许，需要加NOT NULL约束

==注意==：NULL和0是不等价的，并且无法比较<br/>因此一个`int`或者`float`类型与`NULL`值进行算术运算，结果都是`NULL`<br/>判断一个值是不是NULL需要用：`SELECT Columns FROM Table_Name WHERE Column_Name IS(OR IS NOT) NULL`

#### NULL相关函数

`ISNULL(Column_Name， 0)`当值为`NULL`的时候返回0，如果不是，则返回原值，就可以进行操作了

在不同是数据库中`ISNULL(),NVL(),IFNULL(),COALESCE()`,都是同样的用法和同样的效果

# SQL的通用数据类型

| 数据类型                           | 描述                                                         |
| :--------------------------------- | :----------------------------------------------------------- |
| CHARACTER(n)                       | 字符/字符串。固定长度 n。                                    |
| VARCHAR(n) 或 CHARACTER VARYING(n) | 字符/字符串。可变长度。最大长度 n。                          |
| BINARY(n)                          | 二进制串。固定长度 n。                                       |
| BOOLEAN                            | 存储 TRUE 或 FALSE 值                                        |
| VARBINARY(n) 或 BINARY VARYING(n)  | 二进制串。可变长度。最大长度 n。                             |
| INTEGER(p)                         | 整数值（没有小数点）。精度 p。                               |
| SMALLINT                           | 整数值（没有小数点）。精度 5。                               |
| INTEGER                            | 整数值（没有小数点）。精度 10。                              |
| BIGINT                             | 整数值（没有小数点）。精度 19。                              |
| DECIMAL(p,s)                       | 精确数值，精度 p，小数点后位数 s。例如：decimal(5,2) 是一个小数点前有 3 位数，小数点后有 2 位数的数字。 |
| NUMERIC(p,s)                       | 精确数值，精度 p，小数点后位数 s。（与 DECIMAL 相同）        |
| FLOAT(p)                           | 近似数值，尾数精度 p。一个采用以 10 为基数的指数计数法的浮点数。该类型的 size 参数由一个指定最小精度的单一数字组成。 |
| REAL                               | 近似数值，尾数精度 7。                                       |
| FLOAT                              | 近似数值，尾数精度 16。                                      |
| DOUBLE PRECISION                   | 近似数值，尾数精度 16。                                      |
| DATE                               | 存储年、月、日的值。                                         |
| TIME                               | 存储小时、分、秒的值。                                       |
| TIMESTAMP                          | 存储年、月、日、小时、分、秒的值。                           |
| INTERVAL                           | 由一些整数字段组成，代表一段时间，取决于区间的类型。         |
| ARRAY                              | 元素的固定长度的有序集合                                     |
| MULTISET                           | 元素的可变长度的无序集合                                     |
| XML                                | 存储 XML 数据                                                |

然而，不同的数据库对数据类型定义提供不同的选择。

下面的表格显示了各种不同的数据库平台上一些数据类型的通用名称：

| 数据类型            | Access                  | SQLServer                                            | Oracle           | MySQL       | PostgreSQL       |
| :------------------ | :---------------------- | :--------------------------------------------------- | :--------------- | :---------- | :--------------- |
| *boolean*           | Yes/No                  | Bit                                                  | Byte             | N/A         | Boolean          |
| *integer*           | Number (integer)        | Int                                                  | Number           | Int Integer | Int Integer      |
| *float*             | Number (single)         | Float Real                                           | Number           | Float       | Numeric          |
| *currency*          | Currency                | Money                                                | N/A              | N/A         | Money            |
| *string (fixed)*    | N/A                     | Char                                                 | Char             | Char        | Char             |
| *string (variable)* | Text (<256) Memo (65k+) | Varchar                                              | Varchar Varchar2 | Varchar     | Varchar          |
| *binary object*     | OLE Object Memo         | Binary (fixed up to 8K) Varbinary (<8K) Image (<2GB) | Long Raw         | Blob Text   | Binary Varbinary |



==注意==：在不同的数据库中，同一种数据类型可能有不同的名称。即使名称相同，尺寸和其他细节也可能不同！ ==请总是检查文档！==

#DB数据类型

在 MySQL 中，有三种主要的类型：Text（文本）、Number（数字）和 Date/Time（日期/时间）类型。

**Text 类型：**

| 数据类型         | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| CHAR(size)       | 保存固定长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的长度。最多 255 个字符。 |
| VARCHAR(size)    | 保存可变长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的最大长度。最多 255 个字符。**注释：**如果值的长度大于 255，则被转换为 TEXT 类型。 |
| TINYTEXT         | 存放最大长度为 255 个字符的字符串。                          |
| TEXT             | 存放最大长度为 65,535 个字符的字符串。                       |
| BLOB             | 用于 BLOBs（Binary Large OBjects）。存放最多 65,535 字节的数据。 |
| MEDIUMTEXT       | 存放最大长度为 16,777,215 个字符的字符串。                   |
| MEDIUMBLOB       | 用于 BLOBs（Binary Large OBjects）。存放最多 16,777,215 字节的数据。 |
| LONGTEXT         | 存放最大长度为 4,294,967,295 个字符的字符串。                |
| LONGBLOB         | 用于 BLOBs (Binary Large OBjects)。存放最多 4,294,967,295 字节的数据。 |
| ENUM(x,y,z,etc.) | 允许您输入可能值的列表。可以在 ENUM 列表中列出最大 65535 个值。如果列表中不存在插入的值，则插入空值。**注释：**这些值是按照您输入的顺序排序的。可以按照此格式输入可能的值： ENUM('X','Y','Z') |
| SET              | 与 ENUM 类似，不同的是，SET 最多只能包含 64 个列表项且 SET 可存储一个以上的选择。 |

**Number 类型：**

| 数据类型        | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| TINYINT(size)   | 带符号-128到127 ，无符号0到255。                             |
| SMALLINT(size)  | 带符号范围-32768到32767，无符号0到65535, size 默认为 6。     |
| MEDIUMINT(size) | 带符号范围-8388608到8388607，无符号的范围是0到16777215。 size 默认为9 |
| INT(size)       | 带符号范围-2147483648到2147483647，无符号的范围是0到4294967295。 size 默认为 11 |
| BIGINT(size)    | 带符号的范围是-9223372036854775808到9223372036854775807，无符号的范围是0到18446744073709551615。size 默认为 20 |
| FLOAT(size,d)   | 带有浮动小数点的小数字。在 size 参数中规定显示最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DOUBLE(size,d)  | 带有浮动小数点的大数字。在 size 参数中规显示定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DECIMAL(size,d) | 作为字符串存储的 DOUBLE 类型，允许固定的小数点。在 size 参数中规定显示最大位数。在 d 参数中规定小数点右侧的最大位数。 |

> **注意：**以上的 size 代表的并不是存储在数据库中的具体的长度，如 int(4) 并不是只能存储4个长度的数字。
>
> 实际上int(size)所占多少存储空间并无任何关系。int(3)、int(4)、int(8) 在磁盘上都是占用 4 btyes 的存储空间。就是在显示给用户的方式有点不同外，int(M) 跟 int 数据类型是相同的。
>
> 例如：
>
> 1、int的值为10 （指定zerofill）
>
> ```
> int（9）显示结果为000000010
> int（3）显示结果为010
> ```
>
> 就是显示的长度不一样而已 都是占用四个字节的空间

**Date 类型：**

| 数据类型    | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| DATE()      | 日期。格式：YYYY-MM-DD**注释：**支持的范围是从 '1000-01-01' 到 '9999-12-31' |
| DATETIME()  | *日期和时间的组合。格式：YYYY-MM-DD HH:MM:SS**注释：**支持的范围是从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' |
| TIMESTAMP() | *时间戳。TIMESTAMP 值使用 Unix 纪元('1970-01-01 00:00:00' UTC) 至今的秒数来存储。格式：YYYY-MM-DD HH:MM:SS**注释：**支持的范围是从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC |
| TIME()      | 时间。格式：HH:MM:SS**注释：**支持的范围是从 '-838:59:59' 到 '838:59:59' |
| YEAR()      | 2 位或 4 位格式的年。**注释：**4 位格式所允许的值：1901 到 2155。2 位格式所允许的值：70 到 69，表示从 1970 到 2069。 |

*即便 DATETIME 和 TIMESTAMP 返回相同的格式，它们的工作方式很不同。在 INSERT 或 UPDATE 查询中，TIMESTAMP 自动把自身设置为当前的日期和时间。TIMESTAMP 也接受不同的格式，比如 YYYYMMDDHHMMSS、YYMMDDHHMMSS、YYYYMMDD 或 YYMMDD。



# SQL 函数

------

SQL 拥有很多可用于计数和计算的内建函数。

------

## SQL Aggregate 函数

SQL Aggregate 函数计算从列中取得的值，返回一个单一的值。

有用的 Aggregate 函数：

- AVG() - 返回平均值
- COUNT() - 返回行数
- FIRST() - 返回第一个记录的值
- LAST() - 返回最后一个记录的值
- MAX() - 返回最大值
- MIN() - 返回最小值
- SUM() - 返回总和

------

## SQL Scalar 函数

SQL Scalar 函数基于输入值，返回一个单一的值。

有用的 Scalar 函数：

- UCASE() - 将某个字段转换为大写
- LCASE() - 将某个字段转换为小写
- MID() - 从某个文本字段提取字符，MySql 中使用
- SubString(字段，1，end) - 从某个文本字段提取字符
- LEN() - 返回某个文本字段的长度
- ROUND() - 对某个数值字段进行指定小数位数的四舍五入
- NOW() - 返回当前的系统日期和时间
- FORMAT() - 格式化某个字段的显示方式