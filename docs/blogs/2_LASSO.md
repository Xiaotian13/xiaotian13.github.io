
> date: 2020-04-16



## 零、简介

lasso回归（Least Absolute Shrinkage and Selection Operator Regression）与岭回归（Ridge Regression）以线性回归为基础，在其之上做出了一定的改进。

<!--more-->

## 一、Preliminaries and Notation：线性回归

对于一个线性回归模型，我们有：

假设数据集中共有 $n$ 个变量，包含 $m$ 个数据。数据集形似

$$
x = 
\left [
\begin{matrix}
x_1^{(1)} & x_1^{(2)} & \cdots & x_1^{(n)}\\
 x_2^{(1)} & x_2^{(2)} & \cdots & x_2^{(n)}\\
 \vdots & \vdots & \ddots & \vdots \\
 x_m^{(1)} & x_m^{(2)} & \cdots & x_m^{(n)}\\
\end{matrix}
\right ]_{m \times n}
\tag{1}
$$

数据集 $x$ 中每一列代表一个变量，每一行代表一个数据。矩阵 $x$ 可被进一步表示为：
$$
x = [x^{(1)}, x^{(2)}, \cdots, x^{(n)}] = [x_1, x_2, \cdots, x_m]^T \\
\tag{2}
$$

其中
$$
x^{(i)} = 
\left [
\begin{matrix}
x_1^{(i)} \\
 x_2^{(i)} \\
 \vdots \\
 x_m^{(i)} \\
\end{matrix}
\right ]
, \  \ 
x_i = 
\left [
\begin{matrix}
x_i^{(1)} \\
 x_i^{(2)} \\
 \vdots \\
 x_i^{(n)} \\
\end{matrix}
\right ].
\tag{3}
$$

并取
$$
X  = [1, x] = \left [\begin{matrix}1 & x_1^{(1)} & \cdots & x_1^{(n)}\\ 1 & x_2^{(1)} & \cdots & x_2^{(n)}\\ \vdots & \vdots & \ddots & \vdots \\ 1 & x_m^{(1)} & \cdots & x_m^{(n)}\\\end{matrix} \right ]_{m \times (n+1)}, \hat x_i = \left [\begin{matrix}1 \\x_i \end{matrix}\right].
\tag{4}
$$
已知每个数据对应的真实值，并用 $\hat .$ 来表示对应的预测值或估计值：
$$
\begin{aligned}
& Y = \left [
\begin{matrix}
y_1 \\
 y_2 \\
 \vdots \\
 y_m
\end{matrix}
\right ], 

\end{aligned}

\hat Y = \left [
\begin{matrix}
\hat y_1 \\
 \hat y_2 \\
 \vdots \\
 \hat y_m
\end{matrix}
\right ]. \tag{5}
$$
取系数向量为
$$
\beta = \left [
\begin{matrix}
\beta_0 \\
\beta_1 \\
\vdots \\
\beta_n \\
\end{matrix}
\right ]
\tag{6}
$$
则我们对应的线性模型即为
$$
\begin{aligned}
& \hat y_i = \beta^T\hat x_i \\
& \hat Y = X\beta
\end{aligned}
\tag{7}
$$
对于线性回归，其目标（成本）函数为
$$
J(\beta) = \frac{1}{m}\sum_{i=1}^m(y_i - \hat y_i)^2 = \frac{1}{m}\sum_{i=1}^m(y_i - \hat x_i \beta)^2 = \frac{1}{m}||Y-X\beta||_2^2
\tag{8}
$$
对应最优的系数估计 $\hat \beta$ 即为
$$
\hat \beta = \mathop{\arg\min}_{\beta} J(\beta) = \mathop{\arg\min}_{\beta} \frac{1}{m}||Y-X\beta||_2^2
\tag{9}
$$
我们之前已经推导过，线性回归最优的 $\beta$ 为：
$$
\hat \beta = (X^TX)^{-1}X^TY \tag{10}
$$


## 二、LASSO回归与岭回归

lasso回归与岭回归建立在原线性回归的基础上。为了防止系数向量中某些系数很大，而另一些系数又很小，我们希望在成本函数中加入系数的惩罚项，即将成本函数做出以下变化：
$$
\begin{aligned}
& C_1(\beta) = J(\beta) + \lambda ||\beta||_1 = \frac{1}{m}||Y-X\beta||_2^2 + \lambda||\beta||_1, \\

& C_2(\beta) = J(\beta) + \lambda ||\beta||_2 = \frac{1}{m}||Y-X\beta||_2^2 + \lambda||\beta||_2^2
\end{aligned}
\tag{11}
$$
其中，$\lambda >0$； $||\beta||_1, ||\beta||_2$ 分别为系数向量 $\beta$ 的L1范数与L2范数，具体为
$$
\begin{aligned}
& ||\beta||_1 = \sum_{i=0}^m |\beta_i|, \\

& ||\beta||_2 = \sqrt{\sum_{i=0}^m \beta_i^2}
\end{aligned}
\tag{12}
$$
如果在成本函数中加入的是L1范数，则其对应的模型即为**LASSO回归**；如果在成本函数中加入的是L2范数，则其对应的模型即为**岭回归**。

我们一方面想要使得拟合出的直线有着最小的拟合误差，同时又希望拟合得到的系数不能太大，通过将两部分整合至一个成本函数中，可以实现这样一个目标。可以注意到，成本函数中，第一部分代表着拟合误差，第二部分代表着系数向量的大小，而超参数 $\lambda$ 作为它们的桥梁，平衡两者在成本函数中各自的权重。

在之前推导线性回归的最优系数解时，我们采用了对其目标函数求导取极值的方式，这样做的前提是目标函数连续可微，且应当是（下）凸函数。显而易见，成本函数 $C_1(\beta)$ 并不满足这一性质，因为绝对值项是连续、不可导的；而 $C_2(\beta)$ 是满足的。因此，我们可以采用同样的思路去求解岭回归，但对于LASSO回归，我们需要采用其他的方法求解。本次仅给出岭回归的求解。

## 三、岭回归的求解

岭回归对应的目标（成本）函数为
$$
\begin{aligned} 
C_2(\beta) = \frac{1}{m}||Y-X\beta||_2^2 + \lambda||\beta||_2^2
& = \frac{1}{m} (Y -X \beta)^{T} (Y - X \beta) + \lambda\beta^T\beta\\ 
& = \frac{1}{m} (Y^{T} - \beta^{T} X^{T}) (Y - X \beta)  + \lambda\beta^T\beta\\ 
& = \frac{1}{m}(Y^{T} Y - Y^{T} X \beta - \beta^{T} X^{T} Y + \beta^{T} X^{T} X \beta)  + \lambda\beta^T\beta
\end{aligned}
\tag{13}
$$
将成本函数 $C_2(\beta)$ 对 $\beta$ 求导并取0，有
$$
\frac{\rm d C_2}{\rm d \beta}=2 X^{T} X \beta-2 X^{T} Y+2 \lambda I \beta=0 \tag{14}
$$
即
$$
\left(X^{T} X+\lambda I\right) \beta=X^{T} Y \tag{15}
$$
解析解即为
$$
\hat \beta = (X^TX + \lambda I)^{-1}X^TY \tag{16}
$$
这里，需要指出的是，**矩阵 $(X^TX + \lambda I)$ 一定可逆**，这就避免了线性回归最优解 $(10)$ 的潜在问题：$X^TX$ 可能不可逆。下面给出证明。



> > $\mathbf{Lemma \ 1:}$  对任意实矩阵 $D \in \mathbb R^{m\times n}$ ， $D^TD$ 是半正定的。
> >
> > $\mathbf{Proof^*}$ 任取实向量 $x \in \mathbb R^n$ ，有 $x^T(D^TD)x = (Dx)^TDx = ||Dx||_2^2 \ge 0$ .
>
> 
>
> >$\mathbf{Lemma \ 2:}$ 如果矩阵 $M \in \mathbb R^{n\times n}$ 是半正定的，则其特征值非负。
>
> 
>
> > $\mathbf{Lemma \ 3:}$ 对于齐次线性方程 $Ax = 0$ ，若方程只有零解，则矩阵 $A$ 可逆；如果方程有无穷解，则矩阵 $A$ 不可逆。
>
> 
>
> $\mathbf{Proof:}$
>
> 构造以下方程组：
> $$
> (X^TX + \lambda I) x = 0 \tag{17}
> $$
> 移项，可得
> $$
> X^TX x = -\lambda x \tag{18}
> $$
> 根据特征向量的定义，可知 $x$ 即为矩阵 $X^TX$ 对应于特征值 $-\lambda$ 的特征向量。又因为矩阵 $X^TX$ 是半定的，上面的引理1、2指出其特征值非负；而 $-\lambda < 0$ ，故 $x = 0$ 。
>
> 因此，方程组 $(17)$ 仅有零解，由引理3可知矩阵 $(X^TX + \lambda I)$ 可逆。**Q.E.D.**

## 四、编程实现

下面我们用Python来做简单实现。

首先使用`sklearn.datasets.make_regression`函数来生成我们的数据集。我们生成了1000个数据，自变量与因变量的个数均为1，真实截距设置为2，噪音系数设置为75。

最后将lasso回归、岭回归与线性回归放在一起作对比。

```python
#-*- coding = UTF-8 -*-

import numpy as np
import matplotlib.pyplot as plt

from sklearn.datasets import make_regression

from sklearn.linear_model import LinearRegression, Lasso, Ridge
from sklearn.metrics import r2_score, mean_squared_error


x, y, coef = make_regression(n_samples=1000, n_features=1, n_targets=1, coef=True, bias=2, noise=75, shuffle=True, random_state=13)

model = LinearRegression()
model_1 = Lasso(alpha=20, random_state=13)
model_2 = Ridge(alpha=20, random_state=13)

model.fit(x, y)
model_1.fit(x, y)
model_2.fit(x, y)

print('Linear Regression R^2:   {:.6f}', r2_score(model.predict(x), y))
print('LASSO Regression R^2:   {:.6f}', r2_score(model_1.predict(x), y))
print('Ridge Regression R^2:   {:.6f}', r2_score(model_2.predict(x), y))
print('\n\n')
print('Linear Regression MSE:   {:.6f}', mean_squared_error(model.predict(x), y))
print('LASSO Regression MSE:   {:.6f}', mean_squared_error(model_1.predict(x), y))
print('Ridge Regression MSE:   {:.6f}', mean_squared_error(model_2.predict(x), y))

#print(model.coef_, model.intercept_)
#print(coef)


plt.scatter(x, y, s=8)
ls = np.array([i/100-3 for i in range(0, 600)]).reshape(-1, 1)
plt.plot(ls, model.predict(ls), color='blue', label='Linear')
plt.plot(ls, model_1.predict(ls), color='red', label='LASSO')
plt.plot(ls, model_2.predict(ls), color='green', label='Ridge')
plt.legend()
plt.show()

```

可以看到，岭回归与线性回归几乎重合，而LASSO回归则呈现出与它们不一样的结果。事实上，LASSO回归所使用的L1正则有着稀疏诱导的性质，虽然L1正则与L2正则均能够起到收缩系数的作用，但L1正则的收缩效果更强，它能使得系数收缩为0，而L2正则仅能使系数收缩至一个较小的数，不能收缩为0。因此，在有着多个输入变量时，LASSO回归能够起到变量选择的作用，将其中某些变量的系数收缩至0，即使得系数向量具有稀疏性。

## 五、总结与反思

下面我们提出几点思考。

- lasso回归与岭回归中，均包含一个超参数 $\lambda$ ，这个超参数应该如何选择？
- lasso回归如何高效求解？