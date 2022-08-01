
> date: 2020-04-19




## 简介

梯度下降(Gradient Descent)是常见的优化算法，它是一系列优化算法的基石，是一个良好的思路。

<!--more-->

## Preliminaries and Notation：优化模型

给定一个无约束优化问题形如
$$
\min_\mathbf{x} f(\mathbf{x})
\tag{1}
$$

其中 $\mathbf{x} \in \mathbb R^n, f:\mathbb R^{n} \rightarrow \mathbb R$ ；函数 $f(\mathbf{x})$ 连续可微，且关于 $\mathbf x$ 是（下）凸函数。凸函数可以保证我们得到的最优解是全局最优，即 $\forall \mathbf{x}_1, \mathbf{x}_2 \in \mathbb C, \ \mathbf{x}_1 \neq \mathbf{x}_2$ ，其中 $\mathbb C$ 为函数的定义域且是凸集，有
$$
f(\mathbf{x}_2) \geq f(\mathbf{x}_1) + \nabla f(\mathbf{x}_1)^T(\mathbf{x}_2 - \mathbf{x}_1)
\tag{2}
$$

举个简单的例子，函数 $y = x^2$ 是凸的；函数 $f(x, y) = x^2 - y^2$（马鞍面）是非凸的。对于凸函数，我们可以通过求导的方式来计算得到函数极小值，并且该极小值就是函数的最小值，这是一个非常好的性质。



回顾线性回归模型，我们假设自变量与因变量呈线性，即
$$
y = x\beta = \left [
\begin{matrix}
1 & x_1 &\cdots x_n
\end{matrix}
\right ]
\left [
\begin{matrix}
\beta_0 \\
\beta_1 \\
\vdots \\
\beta_n \\
\end{matrix}
\right ] = \beta_0 + \beta_1x_1 + \cdots +\beta_nx_n
\tag{3}
$$
对线性回归模型中的系数求解，即是一个优化问题。通常，我们有着成本函数形如
$$
J(\beta) = \frac{1}{m}\sum_{i=1}^m(y_i - \hat y_i)^2 = \frac{1}{m}\sum_{i=1}^m(y_i - \ x_i \beta)^2 = \frac{1}{m}||Y-X\beta||_2^2
\tag{4}
$$
对应最优的系数估计 $\beta$ 即为
$$
\hat \beta = \mathop{\arg\min}_{\beta} J(\beta) = \mathop{\arg\min}_{\beta} \frac{1}{m}||Y-X\beta||_2^2
\tag{5}
$$
## 批梯度下降（Batch Gradient Descent）

首先给出我们的问题形式。

已知成本函数 $J(\beta)$ ，其中 $\beta \in \mathbb R^{n}$ ，我们的问题即为：
$$
\begin{aligned}
&\min_\beta J(\beta) \\
&\text { s.t. } \ \beta\in \mathbb R^n
\end{aligned}
\tag{6}
$$
对应的最优解为：
$$
\hat \beta = \mathop{\arg\min}_\beta J(\beta) \tag{7}
$$


在成本函数满足连续可微且是凸函数的条件下，BGD算法的流程即为：

> $\mathbf{Algorithm \ 1. \ \ \ \ BGD}$ 
>
> input: a small number $\epsilon$ or maxiterations.
>
> output: $\hat \beta$
>
> Initialize $\beta, \alpha$ (e.g., set $\beta^0 = \vec{1}_{n\times1}, \alpha=0.1$ ), set $t=1$ 
>
> **while** $\left|J^{t}-J^{t-1}\right|<\varepsilon$ or $t<$ maxiterations **do**
>
> ​    **for** $i = 1:n$ **do**
>
> ​        $\beta_i^t = \beta_i^{t-1} - \alpha \frac{\partial}{\partial \beta_i} J(\beta^{t-1})$
>
> ​    **end for**
>
> ​    compute $J^t(\beta) = J(\beta^t),$ where $\beta^t = [\beta_1^t, \cdots, \beta_n^t]^T$
>
> ​	$t:=t+1$
>
> **end while**
>
> Output result given by $\hat \beta = \beta^{t-1}$

这个算法的思路非常简单直观，我们可以用一个简单的凸函数 $y = x^2$ 为构想。我们的目标是找到这个函数的最小值，也就是函数图像上的最低点；把这个过程类比为，我们向这样一个下凸的山坡上扔一个小球，希望这个小球能够顺利的滚到最低点。为了模拟这个过程，我们首先给定一个初始值，也就是小球在 $t = 0$ 时在山坡上的位置；之后，我们使小球沿着当前位置下降速度最快的方向——也就是该点的梯度方向——也就是山坡最陡的方向前进一小步，前进的步长就是由我们的超参数 $\alpha$ 所控制。

就线性回归问题而言，我们对应的成本函数为上面给出的 $(4)$ 式，我们可以很轻松的求出该函数对各分量的偏导数。
$$
J(\beta) = J(\beta_0, \beta_1, \cdots, \beta_n) = \frac{1}{m}\sum_{i=1}^m(y_i - \beta_0 - \beta_1x_i^{(1)}-\cdots-\beta_nx_i^{(n)})^2 
$$

$$
\begin{aligned}
\therefore \ & \frac{\partial}{\partial \beta_0}J(\beta) = -\frac{2}{m}\sum_{i=1}^m(y_i - \beta_0 - \beta_1x_i^{(1)}-\cdots-\beta_nx_i^{(n)}) \\ 
& \frac{\partial}{\partial \beta_k}J(\beta) = -\frac{2}{m}\sum_{i=1}^mx_i^{(k)}(y_i - \beta_0 - \beta_1x_i^{(1)}-\cdots-\beta_nx_i^{(n)}),k=1,\cdots,n
\end{aligned}
\tag{8}
$$
上式看起来也许有些冗长，我们同样可以用矩阵求导法则来计算出矩阵形式的结果；或者将 $(8)$ 直接改为矩阵形式，是同理的。
$$
J(\beta) = \frac{1}{m}||Y-X\beta||_2^2 = \frac{1}{m}(Y-X\beta)^T(Y-X\beta)
$$

$$
\therefore \frac{\partial J(\beta)}{\partial \beta} = \frac{2}{m}X^T(X\beta - Y)
\tag{9}
$$

因此我们的迭代公式为
$$
\beta^t = \beta^{t-1} - \alpha \frac{2}{m}X^T(X\beta^{t-1} - Y)
\tag{10}
$$

## 随机梯度下降（Stochastic Gradient Descent）

对于上面的批梯度下降，我们可以直观的感受到，在每次迭代更新系数的过程中，我们均使用了所有的数据。这对于数据量较大的情况而言，是非常不利的。随机梯度下降即从这方面入手考虑，每次迭代中，我们仅使用一个数据用于更新系数，这样会使得算法运算大大加快。虽然不能保证每次迭代均是向着最优方向进行的，但总体而言，最终的系数会收敛于最优解。

我们结合线性回归，给出以下SGD的算法流程：

> $\mathbf{Algorithm \ 2. \ \ \ \ SGD}$ 
>
> input: dataset $\{x\}_{i=1}^m$
>
> output: $\hat \beta$
>
> Initialize $\beta, \alpha$ (e.g., set $\beta^0 = \vec{1}_{n\times1}, \alpha=0.1$ ), set $t=1$ 
>
> **while** $\left|J^{t}-J^{t-1}\right|<\varepsilon$ or $t<$ maxiterations **do**
>
> ​	randomly generate $k$ , where $k \in \{1, \cdots, n\}$
>
> ​	$\beta_0^t = \beta_0^{t-1} - \alpha \frac{2}{m}(\beta_0^{t-1} + \beta_1^{t-1}x_k^{(1)} + \cdots+\beta_n^{t-1}x_k^{(n)} - y_k)$
>
> ​    **for** $i = 1:n$ **do**
>
> ​        $\beta_i^t = \beta_0^{t-1} - \alpha \frac{2}{m} x_k^{(i)}(\beta_0^{t-1} + \beta_1^{t-1}x_k^{(1)} + \cdots+\beta_n^{t-1}x_k^{(n)} - y_k)$
>
> ​    **end for**
>
> ​    compute $J^t(\beta) = J(\beta^t),$ where $\beta^t = [\beta_1^t, \cdots, \beta_n^t]^T$
>
> ​	$t:=t+1$
>
> **end while**
>
> Output result given by $\hat \beta = \beta^{t-1}$

可以看到，每次更新中我们仅使用了一个随机挑选出的数据，这就是SGD与BGD最大的区别所在。

## 小批次梯度下降(Mini-Batch Gradient Descent)

小批次梯度下降是对BGD与SGD的一个折中：既不使用全部的数据做更新迭代，也不仅使用一个数据。所以，我们每次从全集中选出一小部分的数据作为一个批次，用于更新迭代中。具体算法流程此处省略。

## 编程实现

编程实现有时间再补。

## 总结与反思

下面我们提出几点思考。

- 步长参数 $\alpha$ 如何选取？如果太大，则有可能导致算法不收敛，或收敛至一个较大的数值；如果太小，则算法的收敛速度可能过慢。
- 如果目标函数是连续凸函数，但不可导，此时上面的算法就已不再适用，因为我们不能求出一阶导。能否在BGD算法的基础上做出改进？









