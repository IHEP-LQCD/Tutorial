# gvar 及其拟合工具lsqfit

## gvar简介

格点计算结果需要同时体现均值和偏差，gvar模组提供了处理高斯型随机变量的工具，详细内容可以查阅[官方文档](https://gvar.readthedocs.io/en/latest)

通过下面的代码可以创建一个gvar.Gvar对象，这里x 表示均值为1，标准偏差为0.2的高斯型随机变量,直接用x()可以根据给定高斯分布参数抽取一个随机变量。
```python
import numpy as np
import gvar as gv
x = gv.gvar(1,0.2)
print(x.mean,'+-',x.sdev)#output:1+-0.2
```
我们也可以直接用数据集创建gvar.Gvar对象，


gvar.Gvar对象之间进行算数运算会得到新的gvar.Gvar对象，得到新变量的均值和标准偏差，值得注意的是，gvar中误差处理只是简单的通过偏导数诱导的误差传递，如果所进行的运算在误差范围内线性性不好，需要使用重抽样方法，如Jacknife 和 Bootstrap。
```python
import gvar as gv
x = gv.gvar(1,0.2)
y = gv.gvar(2,0.1)
z=x+y
print(z) #output:3.00(22)
```

变量之间的协方差矩阵会被保存，这在有关联的变量间进行运算时会起到作用。
```python
print(gv.evalcov([x,z]))
#output:
#[[0.04 0.04]
# [0.04 0.05]]

w=x+y+1
print(w-z) #output:(1(0))
```

我们还可以直接通过数据集创建gvar.Gvar对象。
```python
import random
data=[]

for i in range(10000):
    alist.append(random.gauss(0,0.1))

dataSet=gv.dataset.avg_data(alist)
print(dataSet)
```
## lsqfit
对以gvar.Gvar对象组成的序列进行拟合可以使用模组lsqfit，详细内容参见[官方文档](https://lsqfit.readthedocs.io/en/latest/)。
本文只以一个简单拟合程序来说明其基本使用方式。


```python
import numpy as np
import random
imporrt gvar as gv
import lsqfit

Nt=128
timeArray =np.arrange(128)
yArray
for nt in timeArray:
  
  

```






