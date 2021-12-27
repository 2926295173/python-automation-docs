## Pandas 快速开始

安装 pandas 以及相关依赖
> 请确认已经安装了xlrd, xlwt, openpyxl 这三个包

```shell
pip install pandas==1.3.0
pip install xlrd # xlrd 版本不得高于2.0.0
pip install xlwt 
pip install openpyxl
```

```python
import pandas as pd

pd.__version__  # 1.3.0
```

## 基本的文件读取和写入

### 数据读取

pandas支持多种文件格式的读取、这里主要介绍读取csv, excel, txt 文件格式、其余格式请自行扩展。

```python
import pandas as pd

df_csv = pd.read_csv('../data/xhs_chengdu.csv')
print(df_csv.head(10))
# print(df_csv.head(10).to_markdown())
```

```python
import pandas as pd

df_ = pd.read_excel('../data/xhs_chengdu.xlsx')
print(df_.head(10).to_markdown())
```

这里有一些常用的公共参数，header=None表示第一行不作为列名，index_col表示把某一列或几列作为索引，索引的内容将会在第三章进行详述，usecols表示读取列的集合，默认读取所有的列，parse_dates表示需要转化为时间的列，关于时间序列的有关内容将在第十章讲解，nrows表示读取的数据行数。上面这些参数在上述的三个函数里都可以使用。
比如:

```python
import pandas as pd

df_ = pd.read_excel('../data/xhs_chengdu.xlsx',header=None) # 忽略表头
# df_ = pd.read_excel('../data/xhs_chengdu.xlsx',usecols=['city', 'title']) # 只读取 col1,col2 列
# df_ = pd.read_excel('../data/xhs_chengdu.xlsx',parse_dates=['pub_time']) # 需要转化为时间的列(pub_time)
# df_ = pd.read_excel('../data/xhs_chengdu.xlsx', nrows=2) # 只读取 2 行
print(df_.head(10).to_markdown()) # 输出前 10 行并转化为 markdown 语法
```

如果想要把表格快速转换为markdown和latex语言，可以使用to_markdown和to_latex函数，此处需要安装tabulate包。

```shell
pip install tabulate
```

这里笔者比较提前安装完毕了。

|    | city   |   collects |   comments | pub_time         | title                                    | update_time         | username          |
|---:|:-------|-----------:|-----------:|:-----------------|:-----------------------------------------|:--------------------|:------------------|
|  0 | 成都   |        998 |         10 | 2020-06-11 14:26 | 成都重庆旅游攻略｜超详细，假期出游必备   | 2021-08-26 16:47:27 | Starman           |
|  1 | 成都   |        160 |         57 | 2021-08-11 20:34 | 夏日出行｜超详细的成都融创乐园攻略！     | 2021-08-26 16:47:31 | 荔枝太郎          |
|  2 | 成都   |       2127 |        137 | 2020-06-11 19:49 | 成都夜市最强攻略合集，总有一个能打动你   | 2021-08-26 16:47:36 | 金克丝            |
|  3 | 成都   |       2403 |        152 | 2019-08-16 21:49 | 五一小长假怎么花1500元玩转重庆和成都👇   | 2021-08-26 16:47:40 | 旅拍摄影师小周周  |
|  4 | 成都   |        568 |         53 | 2021-07-27 17:21 | 重庆成都5天4夜一站式旅游攻略🔥人均1000   | 2021-08-26 16:47:44 | 不眠的小嗷娇      |
|  5 | 成都   |       1302 |         20 | 2021-04-26 12:31 | 2021年成都私藏攻略㊙️五一小长假旅游必看   | 2021-08-26 16:47:48 | 150斤少女         |
|  6 | 成都   |        700 |          7 | 2021-04-25 08:40 | 成都三天两晚保姆级攻略🔵人均1000💰快收藏 | 2021-08-26 16:47:52 | 雪琪小仙女        |
|  7 | 成都   |        564 |         36 | 2021-03-25 23:04 | 成都必去的12个景点门票攻略～             | 2021-08-26 16:47:57 | 雪琪小仙女        |
|  8 | 成都   |       3190 |        122 | 2021-04-21 16:59 | 成都旅游攻略|网红美食避坑|本地人推荐‼️    | 2021-08-26 16:48:01 | 破产兄弟BrokeBros |
|  9 | 成都   |        963 |         39 | 2021-06-11 11:35 | 成都旅游‼️本地人整理三天两夜吃喝全攻略    | 2021-08-26 16:48:05 | 150斤少女         |

> 如表格格式有错乱、笔者推荐使用 jupyter notebook

```python
import pandas as pd

df_csv = pd.read_table('../data/xhs_chengdu.txt')
print(df_csv.head(5))
```

在读取txt文件时，经常遇到分隔符非空格的情况，read_table 有一个分割参数sep，
它使得用户可以自定义分割符号，进行txt数据的读取。例如，下面的读取的表以 abc 为分割：

这里有一份 txt 文件格式的数据、它以 abc 为分割符：
```text
col1 abc col2
TS abc This is an apple.
GQ abc My name is Bob.
WT abc Well done!
PT abc May I help you?
```


```python
import pandas as pd
df_ = pd.read_table('../data/my_table_special_sep.txt')
```

|    | col1 abc col2            |
|---:|:-------------------------|
|  0 | TS abc This is an apple. |
|  1 | GQ abc My name is Bob.   |
|  2 | WT abc Well done!        |
|  3 | PT abc May I help you?   |

上面的结果显然不是理想的，这时可以使用sep，同时需要指定引擎为python：

```python
import pandas as pd
df_ = pd.read_table('../data/my_table_special_sep.txt', sep=' abc ', engine='python')
```

得到:

|    | col1   | col2              |
|---:|:-------|:------------------|
|  0 | TS     | This is an apple. |
|  1 | GQ     | My name is Bob.   |
|  2 | WT     | Well done!        |
|  3 | PT     | May I help you?   |

> 【WARNING】sep是正则参数
在使用read_table的时候需要注意，参数sep中使用的是正则表达式，因此需要对|进行转义变成\|，否则无法读取到正确的结果。有关正则表达式的基本内容可以参考第八章或者其他相关资料。


### 数据写入
一般在数据写入中，最常用的操作是把index设置为False，特别当索引没有特殊意义的时候，这样的行为能把索引在保存的时候去除。

[数据准备csv](data/数据准备csv.md ':include')

将读取到的 csv 文件数据写入到 xlsx 文件(反之亦然)
```python
import pandas as pd
df_ = pd.read_excel('../data/xhs_chengdu.xlsx') # 忽略表头
df_.to_excel('../data/my_csv_saved.xlsx', index=False)
```

## 常用方法和基本数据结构
- `pd.Series` 方法: 一维的行数据结构
- `pd.DataFrame` 方法: 二维的表格结构(常用于读写转换)

构建 Series
```python
import pandas as pd
s = pd.Series(data = [100, 'a', {'dic1':5}],
              index = pd.Index(['id1', 20, 'third'], name='my_idx'),
              dtype = 'object',
              name = 'my_name')

```
object代表了一种混合类型，正如上面的例子中存储了整数、字符串以及Python的字典数据结构。此外，目前pandas把纯字符串序列也默认认为是一种object类型的序列

DataFrame在Series的基础上增加了列索引，一个数据框可以由二维的data与行列索引来构造
```python
import pandas as pd
data = [[1, 'a', 1.2], [2, 'b', 2.2], [3, 'c', 3.2]]
df = pd.DataFrame(data = data,
                  index = ['row_%d'%i for i in range(3)],
                  columns=['col_0', 'col_1', 'col_2'])
```

## 缺失值的处理

## pandas 进阶使用

## 文件转储(与数据库交互)