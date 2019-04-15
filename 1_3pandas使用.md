<h1>pandas包的使用</h1>
pandas的两个数据结构：Series和DataFrame

Series:
>一种类似于以为NumPy数组的对象，它由一组数据（各种NumPy数据类型）和与之相关的一组数据标签（即索引）组成的。
可以用index和values分别规定索引和值。如果不规定索引，会自动创建 0 到 N-1 索引。

DataFrame:
>一种表格型结构,含有一组有序的列，每一列可以是不同的数据类型。
既有行索引，又有列索引。

<!-- TOC -->

- [一:DataFrame的基本使用](#一dataframe的基本使用)
    - [1.创建DataFrame(csv,excel,数据)](#1创建dataframecsvexcel数据)
    - [2.数据表信息查看](#2数据表信息查看)
        - [纬度 shape](#纬度-shape)
        - [基本信息(维度、列名称、数据格式、所占空间等)](#基本信息维度列名称数据格式所占空间等)
        - [列的数据类型 dtypes](#列的数据类型-dtypes)
        - [空值查看(全部、列) isnull](#空值查看全部列-isnull)
        - [某一列的唯一值 unique](#某一列的唯一值-unique)
        - [查看数据表的值和列名称 values columns](#查看数据表的值和列名称-values-columns)
        - [查看前10行数据、后10行数据](#查看前10行数据后10行数据)
    - [3.数据表清洗](#3数据表清洗)
        - [填充空值NaN fillna](#填充空值nan-fillna)
        - [使用列price的均值对NA进行填充 fillna](#使用列price的均值对na进行填充-fillna)
        - [清除city字段的字符空格 .map(str.strip)](#清除city字段的字符空格-mapstrstrip)
        - [大小写转换 .str.upper()](#大小写转换-strupper)
        - [更改列数据格式和名称 astype rename](#更改列数据格式和名称-astype-rename)
        - [删除先出现或后出现的重复值 drop_duplicates](#删除先出现或后出现的重复值-drop_duplicates)
        - [数据替换 replace](#数据替换-replace)
    - [4.数据预处理](#4数据预处理)
        - [数据表合并 merge 并交连接](#数据表合并-merge-并交连接)
        - [设置索引列 set_index](#设置索引列-set_index)
        - [按照特定列的值排序 sort_values  sort_index](#按照特定列的值排序-sort_values--sort_index)
        - [对列设置条件 np.where(df['price']>2000)](#对列设置条件-npwheredfprice2000)
        - [对复合多个条件的数据进行分组标记](#对复合多个条件的数据进行分组标记)
    - [5.数据提取](#5数据提取)
        - [提取行数据](#提取行数据)
        - [提取4日之前的所有数据](#提取4日之前的所有数据)
        - [提取行列数据](#提取行列数据)
        - [使用ix按索引标签和位置混合提取数据](#使用ix按索引标签和位置混合提取数据)
        - [判断city列的值是否为北京](#判断city列的值是否为北京)
        - [判断city列里是否包含beijing和shanghai，然后将符合条件的数据提取出来](#判断city列里是否包含beijing和shanghai然后将符合条件的数据提取出来)
        - [提取某一列的前3个字符并生成数据表](#提取某一列的前3个字符并生成数据表)
    - [6.数据筛选](#6数据筛选)
        - [与 &](#与-)
        - [或 |](#或-)
        - [非 !=](#非-)
        - [对筛选后的数据按city列进行计数](#对筛选后的数据按city列进行计数)
        - [使用query函数进行筛选  好用!](#使用query函数进行筛选--好用)
        - [对筛选后的结果按price进行求和](#对筛选后的结果按price进行求和)
    - [7.数据汇总](#7数据汇总)
        - [对所有的列进行计数汇总](#对所有的列进行计数汇总)
        - [按城市对id字段进行计数](#按城市对id字段进行计数)
        - [对两个字段进行汇总计数](#对两个字段进行汇总计数)
        - [对city字段进行汇总，并分别计算price的合计和均值 len sum mean](#对city字段进行汇总并分别计算price的合计和均值-len-sum-mean)
        - [数据采样](#数据采样)
        - [数据表描述性统计](#数据表描述性统计)
        - [计算列的标准差](#计算列的标准差)
        - [计算两个字段间的协方差](#计算两个字段间的协方差)
        - [两个字段的相关性分析](#两个字段的相关性分析)
    - [8.数据输出](#8数据输出)
        - [写入Excel](#写入excel)
        - [写入到CSV](#写入到csv)

<!-- /TOC -->

#### 一:DataFrame的基本使用
##### 1.创建DataFrame(csv,excel,数据)
```python
import numpy as np      #导入numpy包和pandas包
import pandas as pd 

csvDf = pd.DataFrame(pd.read_csv('name.csv',header=1))
excelDf = pd.DataFrame(pd.read_excel('name.excel'))

df = pd.DataFrame({"id": [1001, 1002, 1003, 1004, 1005, 1006],
                       "date": pd.date_range('20130102', periods=6),
                       "city": ['Beijing ', 'SH', ' guangzhou ', 'Shenzhen', 'shanghai', 'BEIJING '],
                       "age": [23, 44, 54, 32, 34, 32],
                       "category": ['100-A', '100-B', '110-A', '110-C', '210-A', '130-F'],
                        "price": [1200, np.nan, 2133, 5433, np.nan, 4432]},
                      columns=['id', 'date', 'city', 'category', 'age', 'price'])
print(df)
```

##### 2.数据表信息查看
###### 纬度 shape
>df.shape
###### 基本信息(维度、列名称、数据格式、所占空间等)
>df.info()
###### 列的数据类型 dtypes
> DataFrame中每一列都可以是不同的数据类型
```python
df.dtypes         #每一列的数据类型

df['id'].dtypes   #某一列的数据类型
```
###### 空值查看(全部、列) isnull
```python
df.isnull         #查看全部数据是否为null，空的地方显示为NaN
 
df['id'].isnull   #查看某一列是否有空值
```
###### 某一列的唯一值 unique
```python
df['id'].unique()
```
###### 查看数据表的值和列名称 values columns
```python
df.values        #显示所有值
 
df.columns       #显示所有列名
```

###### 查看前10行数据、后10行数据
```python
df.head()        #默认为前10行,可以添加n  df.head(3)

df.tail()        #默认为后10行,可以添加n  df.tail(3)
```

##### 3.数据表清洗 
###### 填充空值NaN fillna
```python
df.fillna(value=0)       #将所有空值填充为0

df['price'].fillna(0)    #将指定列空值 填充为0
```
###### 使用列price的均值对NA进行填充 fillna
```python
df['price'].fillna(df['price'].mean())
```
 
###### 清除city字段的字符空格 .map(str.strip)
```python
df['city'] = df['city'].map(str.strip)    #使用map迭代每个元素
print(df['city'])
```

###### 大小写转换 .str.upper()
```python
df['city']=df['city'].str.lower()    #.upper()
```

###### 更改列数据格式和名称 astype rename
```python
df['id'].astype('float')

print(df.rename(columns={'id': 'ids'}))  
```

###### 删除先出现或后出现的重复值 drop_duplicates
```python
print(df['city'].drop_duplicates())            #删除后出现的重复值

print(df['city'].drop_duplicates(keep='last')) #删除先出现的重复值
```

###### 数据替换 replace
```python
df['city'].replace('sh', 'shanghai')
```

##### 4.数据预处理
```python
df1=pd.DataFrame({"id":[1001,1002,1003,1004,1005,1006,1007,1008], 
                  "gender":['male','female','male','female','male','female','male','female'],
                  "pay":['Y','N','Y','Y','N','Y','N','Y',],
                  "m-point":[10,12,20,40,40,40,30,20]})
``` 

###### 数据表合并 merge 并交连接
```python
df_inner=pd.merge(df,df1,how='inner')  # 匹配合并，交集
df_left=pd.merge(df,df1,how='left')    # 左连接
df_right=pd.merge(df,df1,how='right')  #右连接
df_outer=pd.merge(df,df1,how='outer')  #并集！
```

###### 设置索引列 set_index
```python
df.set_index('id')
```
###### 按照特定列的值排序 sort_values  sort_index
```python
df.sort_values(by=['age'])

#按照索引列排序
df.sort_index
```

###### 对列设置条件 np.where(df['price']>2000)
```python
# 如果price列的值>300，group列显示high，否则显示low
 df['price'] = np.where(df['price']>2000,"yes","no")
```

###### 对复合多个条件的数据进行分组标记
```python
df.loc[(df['city'] == 'beijing') & (df['price'] >= 4000), 'sign'] = 1
```

##### 5.数据提取
主要用到的三个函数：loc,iloc和ix，loc函数按标签值进行提取，iloc按位置进行提取，ix可以同时按标签和位置进行提取

###### 提取行数据
```python
#提取单行数据
df.loc[2]
df[2:3]

#提取区域行数据
print(df.iloc[0:5])      #只提取数值数据

print(df[0:5])           #同时有表头信息
```
###### 提取4日之前的所有数据
```python
df[:'2013-01-04']
```

###### 提取行列数据
```python
df.iloc[:3,:2]          #前3行2列

df.iloc[[0,2,4],[1,3]]  #提取 0，2，4行，1，3列
``` 

###### 使用ix按索引标签和位置混合提取数据
```python
df.ix[:'2013-01-04',:4]   #2013-01-04号之前，前四列数据
```

###### 判断city列的值是否为北京
```python
df['city'].isin['beijing']
```
###### 判断city列里是否包含beijing和shanghai，然后将符合条件的数据提取出来
```python
print(df.loc[df['city'].isin(['Beijing','Shenzhen'])])
```
###### 提取某一列的前3个字符并生成数据表
```python
print(pd.DataFrame(df['category'].str[:3]))
```


##### 6.数据筛选
使用与、或、非三个条件配合大于、小于、等于对数据进行筛选，并进行计数和求和。 

###### 与 & 
```python
#使用与号进行条件筛选,并且提取出数据
print(df.loc[(df['age'] >= 22) & (df['city'] == 'Beijing ')])
```

###### 或 |  
```python
print(df.loc[(df['age'] >= 35) | (df['city'] == 'Beijing ')])
```

###### 非 !=
```python
print(df.loc[(df['city'] != 'beijing')].sort(['id']))
``` 

###### 对筛选后的数据按city列进行计数
```python
print(df.loc[(df['city'] != 'Beijing')].city.count())    # 6
```

###### 使用query函数进行筛选  好用!
```python
 print(df.query('city == ["Beijing","Shenzhen"]'))
```
###### 对筛选后的结果按price进行求和
```python
 print(df.query('city == ["Beijing","Shenzhen"]').price.sum)
```

##### 7.数据汇总
主要函数是groupby和pivote_table 
###### 对所有的列进行计数汇总
```python
print(df.groupby('city').count())
```

###### 按城市对id字段进行计数
```python
print(df.groupby('city')['id'].count())
```

###### 对两个字段进行汇总计数
```python
df.groupby(['city','size'])['id'].count()
```

###### 对city字段进行汇总，并分别计算price的合计和均值 len sum mean
```python
df.groupby('city')['price'].agg([len,np.sum, np.mean]) 
```

数据统计，数据采样，计算标准差，协方差和相关系数 
###### 数据采样
```python
#随机采样
df.sample(n=3)

#手动设置采样权重
weights = [0, 0, 0, 0, 0.5, 0.5]
df.sample(n=2, weights=weights) 

#采样后不放回
df.sample(n=6, replace=False)          #设置为True时,表示放回
```

###### 数据表描述性统计describe(count、mean、std、min、25%、50%、75%、max)
```python
df.describe().round(2).T               #round函数设置显示小数位，T表示转置
```

###### 计算列的标准差
```python
df['price'].std()
```

###### 计算两个字段间的协方差
```python
df['price'].cov(df['m-point']) 

#数据表中所有字段间的协方差
df.cov()
```

###### 两个字段的相关性分析
```python
df['price'].corr(df['m-point'])    #相关系数在-1到1之间，接近1为正相关，接近-1为负相关，0为不相关

#数据表的相关性分析
df.corr()
```

##### 8.数据输出
###### 写入Excel
```python
df.to_excel('excel.xlsx', sheet_name='sheet_name') 
```
###### 写入到CSV
```python
df.to_csv('pythoncsv.csv') 
```


