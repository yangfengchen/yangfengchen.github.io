---
title: python使用matplotlib生成图标
date: 2020-06-06 20:34:35
tags:
categories: 
- python
---

​	风尘：小二，来一份python操作数据生成图表工具

​	小二：来嘞，客官请看：

​				matplotlib：是python的2D绘图库

​				pyecharts：是一个用于生成 Echarts 图表的类库

​				seaborn：Seaborn 建立在 matplotlib 的基础之上

​				.............

1、计划

​	python使用matplotlib生成图表，只生成柱状图，其余图形直接官网看

1、磨刀

​	pip install -U matplotlib

2、结果

```
import pandas as pd

from matplotlib import font_manager as fm, rcParams
import matplotlib.pyplot as plt

d = {"科室":["呼吸","呼吸","内科","内科","外科"],
     "病人数":[10,20,30,40,50]}
#该处可以直接读取excel，因为简单起见只是自定义了数据
df = pd.DataFrame(data=d)
#设置坐标
df.plot.bar(x="科室",y="病人数")
plt.rcParams['font.sans-serif']=['SimHei'] #显示中文标签,如果不设置中文会乱码
plt.rcParams['axes.unicode_minus'] = False
#显示图形
plt.show()
```