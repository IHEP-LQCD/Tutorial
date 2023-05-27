# gvar & lsqfit

## 安装

``` bash
pip install gvar
pip install lsqfit
```

> **For Windows users:** 安装过程中如果报错缺少 MSVC 依赖, 请到 [Visual Studio](https://visualstudio.microsoft.com/) 安装，需要至少含有 ```MSVC C++ tools``` 和 ```Windows 11/10 SDK``` 两项（详见 [Visual Studio](./Windows.md)）。

## gvar 简介

格点计算结果需要同时体现均值和偏差，gvar 模组提供了处理高斯型随机变量的工具，详细内容可以查阅 [官方文档](https://gvar.readthedocs.io/en/latest)

通过下面的代码可以创建一个 `gvar.Gvar` 对象，这里 x 表示均值为 1，标准偏差为 0.2 的高斯型随机变量,直接用 `x()` 或 'gvar.boostrap_iter' 可以根据给定高斯分布参数抽取一个随机变量。

```python
import numpy as np
import gvar as gv
x = gv.gvar(1,0.2)
print(x.mean, '+-', x.sdev)  # output: 1+-0.2

for i in range(10):
    print(x())
```

我们还可以直接通过数据集创建 `gvar.Gvar` 对象。

```python
import random
data = []

for i in range(10000):
    data.append(random.gauss(0, 0.1))

dataSet = gv.dataset.avg_data(data)
print(dataSet)
```

`gvar.Gvar` 对象之间进行算数运算会得到新的 `gvar.Gvar` 对象，得到新变量的均值和标准偏差，值得注意的是，gvar 中误差处理只是简单的通过偏导数诱导的误差传递，如果所进行的运算在误差范围内线性性不好，需要使用重抽样方法，如 Jacknife 和 Bootstrap。

```python
import gvar as gv
x = gv.gvar(1, 0.3)
y = gv.gvar(2, 0.4)
z = x + y
print(z)  # output: 3.00 (50)
```

变量之间的协方差矩阵会被记录，这在有关联的变量间进行运算时会起到作用。

```python
print(gv.evalcov([x, z]))
# output:
# [[0.09 0.09]
#  [0.09 0.25]]

w = x + y + 1
print(w - z)  # output: 1(0)
```

> **注意:** (*`gv.dataset.avg_data` 是带有协方差矩阵（covariance matrix）的数据类型*)。以下是一个例子：
> ```python
> # 假设两点关联函数 corr 具有 corr。shape= (Ncfg, Nt)
> A = gv.dataset.avg_data(corr[:,:])
> print(gv.evalcov(A))
> # 可以看到 gv.evalcov(A) 是 (Nt, Nt) 形状的协方差矩阵
> B = gv.gvar(gv.mean(A), gv.sdev(A))
> print(gv.evalcov(B))
> # gv.evalcov(B) 还是是 (Nt, Nt) 形状的协方差矩阵，但是除了对角元外都变为 0 了.
> ```

## lsqfit

对以 `gvar.Gvar` 对象组成的序列进行拟合可以使用 `lsqfit` 模块，详细内容参见 [官方文档](https://lsqfit.readthedocs.io/en/latest/)。
`lsqfit` 的作者是大名鼎鼎的 [Peter Lepage](https://inspirehep.net/authors/1000548)，该程序最初也是为格点 QCD 分析写的包。

本文只以一个简单拟合程序来说明其基本使用方式。

```python
import numpy as np
import gvar as gv
import lsqfit

Nt = 128


def setData():

    def func(x, p):
        return p['w'] * np.cosh(p['h'] *
                                (x - Nt / 2) / 6.894) + p['w2'] * np.cosh(
                                    p['h2'] * (x - Nt / 2) / 6.894)

    xData = np.arange(8, 15)
    prior = {}
    prior['w'] = gv.gvar(2, 1)
    prior['h'] = gv.gvar(3.8, 0.1)
    prior['w2'] = gv.gvar(0.1, 1)
    prior['h2'] = gv.gvar(4.5, 10)
    yData = gv.gvar([
        '34.67(18)', '16.489(93)', '7.910(49)', '3.884(26)', '1.861(14)',
        '0.9110(75)', '0.4476(40)'
    ])

    return xData, yData, func, prior


if __name__ == '__main__':
    xData, yData, func, prior = setData()
    fit = lsqfit.nonlinear_fit(data=(xData, yData), fcn=func, prior=prior)
    print(fit.format(maxline=True))
```

在上面的例子中，lsqfit.nonlinear_fit 得到一个 `lsqfit.nonlinear_fit` 对象。拟合得到的结果都可以在 fit.format 中找到。上面的代码得到的结果如下所示：
```
Least Square Fit:
  chi2/dof [dof] = 2.5 [7]    Q = 0.016    logGBF = -225.8

Parameters:
              w                          2.95(35)e-14                 [  2.0 (1.0) ]  *
              h   3.9712361677636982548733612930 (43)                 [  3.80 (10) ]  *
             w2                         8.038(69)e-17                 [  0.1 (1.0) ]
             h2   5.0774354372295347204158133536 (13)                 [     4 (10) ]

Fit:
     x[k]           y[k]      f(x[k],p)
---------------------------------------
        8     34.67 (18)     34.33 (12)  *
        9    16.489 (93)    16.562 (50)
       10     7.910 (49)     8.000 (21)  *
       11     3.884 (26)    3.8702 (99)
       12     1.861 (14)    1.8753 (60)  *
       13    0.9110 (75)    0.9104 (40)
       14    0.4476 (40)    0.4429 (27)  *

Settings:
  svdcut/n = 1e-12/0    tol = (1e-08*,1e-10,1e-10)    (itns/time = 213/0.0)
  fitter = scipy_least_squares    method = trf

```
第一部分是用于评价拟合质量的参数，第二部分是拟合得到的参数，后面的一个 * 代表期望值与数据相差一个 $\sigma$

## Eaxmple: 两点关联函数拟合（two-state fit）
以下是一个 two-state fit 例子。注意总是把能级最高的态考虑为所有高激发态的污染项。

假设一个两点关联函数的变量 `correlator`, 形状为 `correlator.shape = (Ncfg, Nt)`。

第一步是计算带误差的两点关联函数，误差由统计学中的重抽样方法（resampling）给出。格点中一般使用的重抽样方法包括： Jackknife （刀切法）或 Bootstrap 方法。

调用函数 `gv.dataset.avg_data` 做重抽样给出误差，
```python
corr = gv.dataset.avg_data(correlator[:, :])
print(corr)
```
你会看到输入的 `(Ncfg, Nt)` 后， 输出 `(Nt)` 长度的带误差的两点关联函数。即是我们要拟合的 `ydata`。

定义两点关联函数的拟合函数 `func`，
```python
def func(t, p):
    E1 = p['E1']
    E2 = p['E2']
    Z1 = p['Z1']
    Z2 = p['Z2']

    ans = Z1 * (gv.exp(-E1 * t) + gv.exp(-E1 * (Nt - t)))
    ans += Z2 * (gv.exp(-E2 * t) + gv.exp(-E2 * (Nt - t)))
    return ans
```

接下来要定义一个给程序做拟合用的初值,
```python
prior = {
        'Z1': gv.gvar(1, 10),
        'Z2': gv.gvar(1, 10),
        'E1': gv.gvar(0.55, 0.2),
        'E2': gv.gvar(0.90, 0.2),
}
```

> **注意:**
>  - 初值的输入参数有两个：`prior` 或 `p0`。`prior`是带误差的`gv.Gvar`类型， `p0` 是一个数。
>  - `prior` 和 `p0` 作为输入参数互斥，即二选一输入。
>  - 如果你使用 `prior`, 建议误差给大一些。
>  - *（重要）*：`prior` 或 `p0` 改变可能导致拟合失败。但已经拟合上的前提下，拟合结果不应该依赖 `prior` 或 `p0`。

然后做拟合，
```python
fit_window = np.arange(15, 64)
xdata = np.arange(Nt)[fit_window]
ydata = gv.dataset.avg_data(correlator[:, :])[fit_window]

fit = lsqfit.nonlinear_fit(data=(xdata, ydata),
                                fcn=func,
                                prior=prior,
                            )
print(fit.format(maxline=True))
fit_E = fit.p["E1"] if gv.mean(fit.p["E1"]) < gv.mean(fit.p["E2"]) else fit.p["E2"]
fit_func = fit.fcn(np.arange(Nt), fit.p)
```

## Example: 联合拟合（joint fit）


