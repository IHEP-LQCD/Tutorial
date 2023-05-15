# gvar 及其拟合工具lsqfit

格点计算结果需要同时体现均值和偏差，gvar模组提供了处理高斯型随机变量的工具，详细内容可以查阅[官方文档](https://gvar.readthedocs.io/en/latest/overview.html)

通过下面的代码可以创建一个gvar.Gvar对象，这里x 表示均值为1，标准偏差为0.2的高斯型随机变量。
```python
import gvar as gv
x = gv.gvar(1,0.2)
print(x.mean,'+-',x.sdev)#output:1+-0.2
```

gvar.Gvar对象之间进行算数运算会得到新的gvar.Gvar对象，得到新变量的均值和方差。
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
