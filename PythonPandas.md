#Python Pandas

导读：Pandas是日常数据分析师使用最多的分析和处理库之一，本篇文章总结了常用的46个Pandas数据工作方法，包括创建数据对象、查看数据信息、数据切片和切块、数据筛选和过滤、数据预处理操作、数据合并和匹配、数据分类汇总以及map、apply和agg高级函数的使用方法。

##创建数据对象


Pandas最常用的数据对象是数据框（DataFrame）和Series。数据框与R中的DataFrame格式类似，都是一个二维数组。Series则是一个一维数组，类似于列表。数据框是Pandas中最常用的数据组织方式和对象。有关更多数据文件的读取将在第三章介绍，本节介绍从对象和文件创建数据框的方式，具体如表1所示。

表1 Pandas创建数据对象

| 方法                                                         | 用途                                            | 示例                                                         | 示例说明                                                     |
| ------------------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| read_tableread_csvread_excel                                 | 从文件创建数据框                                | In: import  pandas as pd   In: data1  = pd.read_table('table_data.txt',sep=';') | 读取table_data.txt文件，数据分隔符是;                        |
| DataFrame.from_dictDataFrame.from_itemsDataFrame.from_records | 从其他对象例如Series、Numpy数组、字典创建数据框 | In: data_dict  = {'col1': [2, 1, 0], 'col2': ['a', 'b', 'a'], 'col3': [True, True, False]}In: data2  = pd.DataFrame.from_dict(data_dict) | 基于字典创建数据框，列名为字典的3个key，每一列的值为key对应的value值 |

##查看数据信息


查看信息常用方法包括对总体概况、描述性统计信息、数据类型和数据样本的查看，具体如表2所示：

表2 Pandas常用查看数据信息方法汇总

| 方法     | 用途                                                 | 示例                                                         | 示例说明                                              |
| -------- | ---------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| info     | 查看数据框的索引和列的类型、费控设置和内存用量信息。 | In: print(data2.info())Out:  <class 'pandas.core.frame.DataFrame'>RangeIndex:  3 entries, 0 to 2Data  columns (total 3 columns):col1    3 non-null int64col2    3 non-null objectcol3    3 non-null booldtypes:  bool(1), int64(1), object(1)memory  usage: 131.0+ bytesNone | 返回对象的所有信息                                    |
| describe | 显示描述性统计数据，包括集中趋势、分散趋势、形状等。 | In: print(data2.describe())Out:    col1count   3.0mean    1.0std     1.0min     0.025%     0.550%     1.075%     1.5max     2.0 | 默认查看数值型列，使用include=  'all'查看所有类型数据 |
| dtype    | 查看数据框每一列的数据类型                           | In: print(data2.dtypes)Out:  col1     int64col2    objectcol3      booldtype:  objectt | 结果是Series类型                                      |
| head     | 查看前N条结果                                        | In: print(data2.head(2))Out:   col1 col2   col30     2     a  True1     1     b  True | 从第一行开始取前2行                                   |
| tail     | 查看后N条结果                                        | In: print(data2.tail(2))Out:   col1 col2    col31     1     b   True2     0     a  False | 从最后一行开始取后2行                                 |
| index    | 查看索引                                             | In:  print(data2.index)Out: RangeIndex(start=0,  stop=3, step=1) | 结果是一个类列表的对象，可用列表方法操作对象          |
| columns  | 查看列名                                             | In:  print(data2.columns)Out: Index(['col1',  'col2', 'col3'], dtype='object') |                                                       |
| shape    | 查看形状，记录有多少行多少列                         | In: print(data2.shape)Out:  (3,3)                            | 形状为元组类型                                        |
| isnull   | 查看每个值是否为空值                                 | In:  print(data2.isnull())Out:    col1    col2   col30  False   False  False1  False   False  False2  False   False  False | 数据中没有空值，因此都是False                         |
| unique   | 查看特定列的唯一值                                   | In:  print(data2['col2'].unique())Out:  ['a' 'b']            | 查看col2列的唯一值                                    |

注意 在上述查看方法中，除了info方法外，其他方法返回的对象都可以直接赋值给变量，然后基于变量对象做二次处理。例如可以从dtype的返回值中仅获取类型为bool的列。


##数据切片和切块


数据切片和切块是使用不同的列或索引切分数据，实现从数据中获取特定子集的方式。常见的数据切片和切换的方式如表3所示：

表3 Pandas常用数据切分方法

| 方法                            | 用途                                                         | 示例                                                         | 示例说明                                                     |
| ------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [['列名1', '列名2',…]]          | 按列名选择单列或多列                                         | In: print(data2[['col1','col2']])Out:    col1 col20     2     a1     1     b2     0     a | 选择data2的col1和col3两列                                    |
| [m:n]                           | 选择行索引在m到n间的记录                                     | In:  print(data2[0:2])Out:       col1 col2  col30     2     a  True1     1     b  True | 选取行索引在[0:2)中间的记录，不包含2                         |
| iloc[m:n]                       | In: print(data2.iloc[0:2])Out:       col1 col2  col30     2     a  True1     1     b  True |                                                              |                                                              |
| iloc[m:n,j:k]                   | 选择行索引在m到n且列索引在j到k间的记录                       | In:  print(data2.iloc[0:2,0:1])Out:          col10     21     1 | 选取行索引在[0:2)列索引在[0:1)中间的记录，行索引不包含2，列索引不包含1 |
| loc[m:n,[  '列名1', '列名2',…]] | 选择行索引在m到n间且列名为列名1、列名2的记录                 | In:  print(data2.loc[0:2,['col1','col2']])Out:    col1 col20     2     a1     1     b2     0     a | 选取行索引在[0:2)之间，列名为'col1'和'col2'的记录，行索引不包含2 |

提示 如果选择特定索引的数据，直接写索引值即可。例如data2.loc[2,['col1','col2']]为选择第三行且列名为'col1'和'col2'的记录。



##数据筛选和过滤



数据筛选和过滤是基于条件的数据选择，本章2.6.3提到的比较运算符都能用于数据的筛选和选择条件，不同的条件间的逻辑不能直接用and、or来实现且、或的逻辑，而是要用&和|实现。常用方法如表4所示：

表4 Pandas常用数据筛选和过滤方法

| 方法             | 用途                                           | 示例                                                         | 示例说明                            |
| ---------------- | ---------------------------------------------- | ------------------------------------------------------------ | ----------------------------------- |
| 单列单条件       | 以单独列为基础选择符合条件的数据               | In:  print(data2[data2['col3']==True])Out:    col1 col2   col30     2     a  True1     1    b   True | 选择col3中值为True的所有记录        |
| 多列单条件       | 以所有的列为基础选择符合条件的数据             | In:  print(data2[data2=='a'])Out:    col1 col2   col30   NaN     a   NaN1   NaN   NaN   NaN2   NaN     a   NaN | 选择所有值为a的数据                 |
| 使用“且”进行选择 | 多个筛选条件，且多个条件的逻辑为“且”，用&表示  | In:  print(data2[(data2['col2']=='a') & (data2['col3']==True)])Out:    col1 col2   col30     2     a  True | 选择col2中值为a且col3值为True的记录 |
| 使用“或”进行选择 | 多个筛选条件，且多个条件的逻辑为“或”，用\|表示 | In:  print(data2[(data2['col2']=='a') \| (data2['col3']==True)])Out:      col1 col2   col30     2     a   True1     1     b   True2     0     a  False | 选择col2中值为a或col3值为True的记录 |
| 使用isin查找范围 | 基于特定值的范围的数据查找                     | In:  print(data2[data2['col1'].isin([1,2])])Out:    col1 col2   col30     2     a  True1     1     b  True | 筛选col1列值为1或2的记录            |
| query            | 按照类似sql的规则筛选数据                      | In:  print(data2.query('col2=="b"'))Out:    col1 col2   col31     1     b     1 | 筛选数据中col2值为b的记录           |



##数据预处理操作



Pandas的数据预处理基于整个数据框或Series实现，整个预处理工作包含众多项目，本节列出通过Pandas实现的场景功能。本节功能具体如表5所示：

表5 Pandas常用预处理方法

| 方法            | 用途                                                         | 示例                                                         | 示例说明                     |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------------------- |
| T               | 转置数据框，行和列转换                                       | In:  print(data2.T)Out:      0   1  2col1  2   1  0col2  a   b  a | 行索引、列名以及数据相互调换 |
| sort_values     | 按值排序，默认为正序，可通过ascending=False指定倒序排序      | In: print(data2.sort_values(['col1']))Out:   col1 col22     0     a1     1     b0     2     a | 按colo1列排序                |
| sort_index      | 按索引排序，默认为正序，可通过ascending=False指定倒序排序    | In:  print( data2.sort_index(ascending=False))Out:    col1 col2   col32     0     a     01     1     b     10     2     a     1 | 按索引倒序排序               |
| dropna          | 去掉缺失值，可通过axis设置为0或  index、1或columns丢弃带有缺失值的行或列 | In:  print(data2.dropna())Out:    col1 col2   col30     2     a   True1     1     b   True2     0     a  False | 直接丢弃带有缺失值的行       |
| fillna          | 填充缺失值，可设置为固定值以及不同的填充方法                 | In:  print(data2.fillna(method='bfill'))Out:    col1 col2   col30     2     a   True1     1     b   True2     0     a  False | 使用下一个有效记录填充缺失值 |
| astype          | 转换特定列的类型                                             | In:  data2['col3'] = data2['col3'].astype(int)In:  print(data2.dtypes)Out:  col1     int64col2    objectcol3     int32dtype:  object | 将col3转换为int型            |
| rename          | 更新列名                                                     | In:  print(data2.rename(columns=  {'col1':'A','col2':'B','col3':'C'}))Out:    A   B  C0  2   a  11  1   b  12  0   a  0 | 将data2的列名更新为A、B、C   |
| drop_duplicates | 去重重复项，通过指定列设置去重的参照                         | In:  print(data2.drop_duplicates(['col3']))Out:    col1 col2   col30     2     a     12     0     a     0 | 按col3列去重重复记录         |
| replace         | 查找替换                                                     | In:  print(data2.replace('a','A'))Out:    col1 col2   col30     2     A     11     1     b     12     0     A     0 | 将小写字符a替换为大些字母A   |
| sample          | 抽样                                                         | In:  print(data2.sample(n=2))Out:    col1 col2   col30     2     a     11     1     b     1 | 从data2中随机抽取2条数据     |



##数据合并和匹配



数据合并和匹配是将多个数据框做合并或匹配操作。具体实现如表6所示：

表6 Pandas常用数据合并和匹配方法

| 方法   | 用途                           | 示例                                                         | 示例说明                                                   |
| ------ | ------------------------------ | ------------------------------------------------------------ | ---------------------------------------------------------- |
| merge  | 关联并匹配两个数据框           | In:  print(data2.merge(data1,on='col1',how='inner'))Out:    col1 col2_x  col3_x   col2_y  col3_y  col40    1      b      1      2      3   4 | 关联data1和data2，主键分别为a列和col1列，内关联方式        |
| concat | 合并两个数据框，可按行或列合并 | In: print(pd.concat((data1,data2),axis=1))Out:  col1 col2   col3  col4  col1 col2 col30   1    2   3   4      2    a     11   6    7   8   9      1    b     12  11   12  13  14      0    a     0 | 按列合并data1和data2，可通过指定axis=0按行合并             |
| append | 按行追加数据框                 | In:  print(data1.append(data2))Out:   col1 col2   col3  col40     1     2     3   4.01     6     7     8   9.02    11    12    13  14.00     2     a     1   NaN1     1     b     1   NaN2     0     a     0   NaN | 将data2追加到data，等价于pd.concat((data1,data2),  axis=0) |
| join   | 关联并匹配两个数据框           | In:  print(data1.join(data2,lsuffix='_d1', rsuffix='_d2'))Out:  col1_d1   col2_d1  col3_d1  col4   col1_d2 col2_d2  col3_d20   1    2   3   4     2  a  11   6    7   8   9     1  b  12  11   12  13  14     0  a  0 | 将data1和data2关联，设置关联后的列名前缀分别为d1和d2       |



##数据分类汇总




数据分类汇与Excel中的概念和功能类似。具体实现如表7所示：

表7 Pandas常用数据分类汇总方法

| 方法        | 用途                 | 示例                                                         | 示例说明                                           |
| ----------- | -------------------- | ------------------------------------------------------------ | -------------------------------------------------- |
| groupby     | 按指定的列做分类汇总 | In:  print(data2.groupby(['col2'])['col1'].sum())Out:  col2a    2b    1Name:  col1, dtype: int64 | 以col2列为维度，以col1列为指标求和                 |
| pivot_table | 建立数据透视表视图   | In: print(pd.pivot_table(data2,index=['col2']))Out:       col1   col3col2a        1    0.5b        1    1.0Name:  col1, dtype: int64 | 以col2列为索引建立数据透视表，默认计算方式为求均值 |



##高级函数使用

Pandas能直接实现数据框级别高级函数的应用，而不用写循环遍历每条记录甚至每个值后做计算，这种方式能极大提升计算效率，具体如表8所示：

表8 Pandas常用高级函数

| 方法  | 用途                                             | 示例                                                         | 示例说明                                      |
| ----- | ------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| map   | 将一个函数或匿名函数应用到Series或数据框的特定列 | In:  print(data2['col3'].map(lambda x:x*2))Out:  0    21    22    0Name:  col3, dtype: int64 | 对data2的col3的每个值乘2                      |
| apply | 将一个函数或匿名函数应用到Series或数据框         | In:  print(data2.apply(pd.np.cumsum))Out:    col1 col2   col30     2    a      11     3    ab     22     3   aba     2 | 将data2的所有列按行（默认）做累加             |
| agg   | 一次性对多个列做聚合操作                         | In:  import numpy as npIn:  print(data2.groupby(['col2']).agg(  {'col1':np.sum,'col3':np.mean}))Out:    col1   col3col2a     2    0.5b     1    1.0 | 在data2中以col2为维度，对col1求和，col3求均值 |

