**CLassification**

* 显式
* 隐式
    * pro：利于判断点是否在曲线上
    * con：难以绘制
* 参数

## Parametric Curves



## Interpolation

### Cubic Hermite Interpolation

输入：P(0), P'(0), P(1), P'(1)

Cubic：三次方，四个自由度，因此可解



写出四元一次方程组，写成矩阵形式$P = MX$，用逆矩阵$X = M^{-1}P$可解出X



$\displaystyle P(t) = \sum_{i=1}^3 h_iH_i(t)$，其中$h_i = ..., H_i = ...$

### Catmull-Rom Interpolation

输入：四个值（无导数）P(0), P(1), P(2), P(3)

trick：用 (P2-P0)/2作为P'(1)，(P3-P1)/2作为P'(2)

为什么要用左右：

* $f'(x) = \lim_{\Delta x \rightarrow0}\frac{f(x+\Delta x)-f(x)}{\Delta x}$的误差为$O(\Delta x)$ (泰勒展开)
* $f'(x) = \lim_{\Delta x \rightarrow0}\frac{f(x+\Delta x)-f(x-\Delta x)}{2\Delta x}$的误差为$O(\Delta x^2)$



有N个点时，有N-1个sep，中间N-3个都是可以的，仅左右两个由于P'(0)和P'(N-1)得不到所以无法用Cubic Hermite Interpolation。一个可行的方法是用单边导数进行估计（个人想法是利用割线斜率）

## Bzier Curve

[Bézier curve - Wikipedia](https://en.wikipedia.org/wiki/B%C3%A9zier_curve)

一阶：${\displaystyle \mathbf {B} (t)=\mathbf {P} _{0}+(\mathbf {P} _{1}-\mathbf {P} _{0})t=(1-t)\mathbf {P} _{0}+t\mathbf {P} _{1}{\mbox{ , }}t\in [0,1]}$

二阶：${\displaystyle \mathbf {B} (t)=(1-t)^{2}\mathbf {P} _{0}+2t(1-t)\mathbf {P} _{1}+t^{2}\mathbf {P} _{2}{\mbox{ , }}t\in [0,1]}$

三阶：${\displaystyle \mathbf {B} (t)=\mathbf {P} _{0}(1-t)^{3}+3\mathbf {P} _{1}t(1-t)^{2}+3\mathbf {P} _{2}t^{2}(1-t)+\mathbf {P} _{3}t^{3}{\mbox{ , }}t\in [0,1]}$

一般：${\displaystyle \mathbf {B} (t)=\sum _{i=0}^{n}{n \choose i}\mathbf {P} _{i}(1-t)^{n-i}t^{i}={n \choose 0}\mathbf {P} _{0}(1-t)^{n}t^{0}+{n \choose 1}\mathbf {P} _{1}(1-t)^{n-1}t^{1}+\cdots +{n \choose n-1}\mathbf {P} _{n-1}(1-t)^{1}t^{n-1}+{n \choose n}\mathbf {P} _{n}(1-t)^{0}t^{n}{\mbox{ , }}t\in [0,1]}$



**性质**

https://blog.csdn.net/xuehuafeiwu123/article/details/54584339



**Rational Bezier Curve**

不能画圆



**Disadvantage**

* 阶数和控制点个数要一样
* global，每个点对整个图形都有影响



## B-spline

局部控制



**性质**

* localization
* normalization
    * 加权平均
* 阶数可控
* differential：不同阶的N之间的递归关系



## NURBS

Non-Uniform Rational B-Spline



# Summary

$\large\displaystyle P(t) = \sum_i P_i B_i(t)$