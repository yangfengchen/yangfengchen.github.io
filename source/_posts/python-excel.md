---
title: python操作excel入门
date: 2020-06-06 20:04:15
tags:
categories: 
- python	
---

​	风尘：小二，跟你打听一件事，python操作excel用什么工具。

​	小二：客官，python操作excel有pandas、xlrd+xlwt、Grid studio、Dask等，听说Pandas是最好的探索性数据分析工具之一。



1、计划

​	python通过pandas简单操作excel的读取、获取列最大值、取某几列生成新的excel、循环遍历excel行列的一些基础操作

2、磨刀

​	pip install -U pandas

​	#openpyxl不安装操作某些会失败

​	pip install -U openpyxl

3、结果

```
# -*- coding: utf-8 -*-
#openpyxl pandas
import pandas as pd


def read_excel():
    # 方法二：通过指定表单名的方式来读取
    df = pd.read_excel('D:/TestUnit/统计模板数.xlsx',sheet_name="Sheet1", header=None)  # 可以通过sheet_name来指定读取的表单,  header=None 无标题
    data = df.head()  # 默认读取前5行的数据 head
    print("第一行数据:\n{0}".format(df.loc[0].values))
    # 第n-n行的数据
    print("获取到所有的值1:\n{0}".format(df.loc[[0, 1]].values))
    print("获取到所有的值:\n{0}".format(data))  # 格式化输出
    print("获取到所有的值2:\n{0}".format(df))  # 格式化输出
    #读取每行数据
    for i,r in df.iterrows():
        #print(i,'-->',r,type(r))
        print("每行总长度:{0}".format(r.size))
        print(r.loc[0],r.loc[1],r.loc[2],r.loc[3])
    #读取每行数据 大数据 itertuples比iterrows效率高
    for r in df.itertuples():
        #print(i,'-->',r,type(r))
        print("每行总长度:\n{0}".format(r), r[1])
    #读取zip

def search_excel():
    df = pd.read_excel('D:/TestUnit/统计模板数.xlsx',sheet_name="Sheet1")  # 可以通过sheet_name来指定读取的表单,  header=None 无标题
    #获取病人数列值
    #print(df[["病人数"]])
    #print(df['科室'].dtype)
    #查找病人数列最大值
    print(df['病人数'].max())
    #查找病人数大于10的
    print(df[df['病人数']>10])
    #分组查找最大值
    print(df.groupby("科室").apply(
        lambda x: x.loc[x["评价分"].idxmax()]
    ))


def write_excel():
    d = {"科室":["呼吸","呼吸","内科","内科","外科"],"医生":["张三","李四","王五","赵六","张三"],
         "病人数":[10,20,30,40,50],"评价分":[1,3,4,2,5]}
    df = pd.DataFrame(data=d)
    df.to_excel('D:/TestUnit/统计模板数.xlsx', index=False)
def replace_excel():
    df = pd.DataFrame(pd.read_excel('D:/TestUnit/测试.xlsx'))
    df1 = df[['email']]
    df1.to_excel('D:/TestUnit/c测试1.xlsx', index=False)


if __name__ == '__main__':
    #写数据到excel里面
    #write_excel()
    #读excel数据
    #read_excel()
    #查找
    search_excel()
    #replace_excel()
```

​	