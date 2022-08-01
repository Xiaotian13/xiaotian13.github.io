
> date: 2020-04-11




## 零、简介

线性回归（Linear Regression）是一个简单易用且十分强大的工具。

## 一、线性回归

在一些情况下，我们的数据呈现出线性，这时我们通常会尝试用形似

$$
y = a x +b \tag{1}
$$

的线性方程来做拟合、预测。上式还可以被推广至多维，即

$$
y = a_1 x_1 + a_2 x_2 + \cdots + a_n x_n + b \tag{2}
$$

上述两个方程在实际情况中为预测方程。以最简单的情况为例，更合适的写法应为

$$
\hat y = a x + b
$$

还以最简单的情况即仅有一个自变量为例，我们希望拟合得到的误差尽可能小。用一条直线来拟合数据，可以想象为用某种规律来归纳数据集中所蕴含的信息，这种做法尝试提取出数据集主要的信息规律，相应地，必将带来信息损失。为此，我们引入常用的均方差（MSE，Mean Error Square）来作为我们的损失函数，定义如下：

$$
J(a, b) = \frac{1}{m}\sum_{i=1}^m(y_i - \hat y_i)^2 = \frac{1}{m}\sum_{i=1}^m[y_i - (ax_i + b)]^2\tag{3}
$$

需要指出的是，$\{(x_i, y_i)| i=1, \dots, m\}$ 为我们的数据集，是已知量。因此，成本函数应是关于参数 $a, b$ 的函数。参数 $a, b$ 确定了最终拟合出的直线，因此它们应是使得MSE最小所对应的参数，即

$$
a, b = \mathop{\arg\min}_{a, b} J(a, b) \tag{4}
$$

## 二、最小二乘

最小化MSE是一个优化问题，我们需要思考如何才能得到这样一组符合要求的参数。在上述情况下，成本函数是关于参数 $a, b$ 的二元函数，自然地，容易想到将函数对各变量求偏导，并使得偏导为0。即：

$$
\left\{
\begin{aligned}
& \frac{\partial J}{\partial a}  = 2\sum_{i=1}^m(ax_i + b - y_i)  = 0 \\
& \frac{\partial J}{\partial b}  = 2\sum_{i=1}^m(ax_i + b - y_i)x_i  = 0\\
\end{aligned}
\right. \tag{5}
$$

将上式进一步化简得

$$
\left\{
\begin{aligned}
& a\sum_{i=1}^mx_i + mb - \sum_{i=1}^my_i = 0 \\
& a\sum_{i=1}^mx_i^2 + b\sum_{i=1}^mx_i - \sum_{i=1}^mx_iy_i = 0 \\
\end{aligned}
\right. \tag{6}
$$

将 $(6)$ 式中第一个式子带入第二个式子，消去参数 $b$ ，可计算出 $a$ ；将第一个式子左右同时除以 $m$ ，可用已计算得到的 $a$ 来计算出 $b$ 。

$$
\left\{
\begin{aligned}
& a = \frac{m\sum x_iy_i - \sum x_i \sum y_i}{m \sum x_i^2 - (\sum x_i)^2} = \frac{\sum x_iy_i - m\bar x \bar y}{\sum x_i^2 - m\bar x^2} \\
& b = \bar y - a \bar x \\
\end{aligned}
\right. \tag{7}
$$

这就是我们通常所说的最小二乘。然而，现实中的数据往往拥有不止一个自变量，此时上面推导出的式子便已不能使用。下面我们结合线性代数相关知识，给出多元线性回归最小二乘的推导。

假设我们的数据集拥有 $n$ 个变量，包含 $m$ 个数据。数据集形似

$$
x = 
\left [
\begin{matrix}
x_1^{(1)} & x_1^{(2)} & \cdots & x_1^{(n)}\\
 x_2^{(1)} & x_2^{(2)} & \cdots & x_2^{(n)}\\
 \vdots & \vdots & \ddots & \vdots \\
 x_m^{(1)} & x_m^{(2)} & \cdots & x_m^{(n)}\\
\end{matrix}
\right ]_{m \times n} \tag{8}
$$

数据集 $x$ 中每一列代表一个变量，每一行代表一个数据。矩阵 $x$ 可被进一步表示为：

$$
x = [x^{(1)}, x^{(2)}, \cdots, x^{(n)}] = [x_1, x_2, \cdots, x_m]^T \tag{9} \\
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
\right ]. \tag{10}
$$

回顾 $(2)$ 式的形式，采取另外的符号做等价替代：

$$
\hat y_i = \theta_0 + \theta_1 x_i^{(1)} + \theta_2 x_i^{(2)} + \cdots + \theta_n x_i^{(n)} \tag{11}
$$

若取

$$
\theta = \left [
\begin{matrix}
\theta_1 \\
\theta_2 \\
\vdots \\
\theta_n \\
\end{matrix}
\right ], \tag{12}
$$

则有

$$
\hat y_i = \theta^T x_i + \theta_0 \tag{13}
$$

这里，我们引入 $x_0$ 并使其恒为1，同时将参数 $\theta_0$ 并入参数向量，即取

$$
\begin{aligned}
& \hat x_i = \left [
\begin{matrix}
x_0 \\
x_i 
\end{matrix}
\right ] = \left [
\begin{matrix}
1 \\
x_i 
\end{matrix}
\right ], \\

& \Theta = \left [
\begin{matrix}
\theta_0 \\
\theta
\end{matrix}
\right ],
\end{aligned} \tag{14}
$$

则 $(11)$ 式可被进一步表示为

$$
\hat y_i = \Theta^T \hat x_i \tag{15}
$$

为推导起见，我们将数据集矩阵 $x$ 的最左侧添加一列元素全部为1的向量，并用符号表示出预测值组成的向量，如下：

$$
\begin{aligned}
& X  = [1, x]_{m \times (n + 1)}, \\

& Y = \left [
\begin{matrix}
y_1 \\
 y_2 \\
 \vdots \\
 y_m
\end{matrix}
\right ]_{m \times 1}.

\end{aligned}
 \tag{16}
$$

这样，我们便可以矩阵的形式来表示出损失函数：

$$
\begin{aligned} 
J(\Theta) = \frac{1}{m} \sum_{i=1}^m {(y_i - \hat y_i)}^2
& = \frac{1}{m} \sum_{i=1}^m {(y_i - \Theta^T \hat x_i)}^2 \\ 
& = \frac{1}{m} \|Y - X \Theta \|_2^{2} \\ 
& = \frac{1}{m} (Y -X \Theta)^{T} (Y - X \Theta) \\ 
& = \frac{1}{m} (Y^{T} - \Theta^{T} X^{T}) (Y - X \Theta) \\ 
& = \frac{1}{m}(Y^{T} Y - Y^{T} X \Theta - \Theta^{T} X^{T} Y + \Theta^{T} X^{T} X \Theta) 
\end{aligned}
$$

将成本函数 $J(\Theta)$ 对 $\Theta$ 求导并取0，有

$$
\begin{aligned}
\frac{d J(\Theta)}{d \Theta}
= \frac{1}{m}(-X^{T}Y - X^{T}Y + 2X^{T} X \Theta)
= 0
\end{aligned}
$$
即
$$
X^{T} X \Theta = X^{T} Y \tag{17}
$$

如果 $X^{T} X$ 可逆，则有
$$
\Theta = (X^{T} X)^{-1} X^{T} Y \tag{18}
$$

到此为止，我们已经推出了多元线性回归的参数表达式。

细心的读者可能会发现，上述求偏导的做法不一定能找到函数的最小值。或者说，如果目标函数是非凸的，求偏导的做法仅能得到一个局部最小值或鞍点，不能保证得到全局最优值。举个简单的例子，对于函数 $z = x^2 - y^2$ ，求偏导的做法会使你得到点 $(0, 0)$ 。但当 $x = 0$，$y$ 取任意正值时，目标函数值均要更小，点 $(0, 0)$ 只是原函数的一个鞍点。实际上，这个函数没有最小值，其取值范围是 $\mathbb{R}$ 。

所幸，我们的损失函数是凸函数，采用上述推导得到的结果是正确的。但也还有着其他问题存在：如果数据集的维度较大，矩阵求逆等运算可能会变得十分消耗算力，套用推导出的参数表达式可能无法满足需求；如果 $X^TX$ 不可逆，推导式便直接失效。结合上面提到的全局最优问题，后面便衍生出了诸如rmsprop、Adam等优化算法及其他措施来解决对应的问题。

## 三、编程实现

下面我们用Python来做简单实践。

### （一）code 1

下面这段代码给出了如何用sklearn做线性回归，并查看其回归参数和评估指标MSE、R$^2$ 。

```python
#-*- coding = UTF-8 -*-

import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

X = [[10], [10], [11], [12], [13], [14], [15], [16], [17], [18], [19], [20], [21], [22], [23], [24], [25], [26], [27], [28]]
Y = [[7.7], [9.87], [11.18], [10.43], [12.36], [14.15], [15.73], [16.4], [18.86], [16.13], [18.21], [18.37], [22.61], [19.83], [22.67], [22.7], [25.16], [25.55], [28.21], [28.12]]

model = LinearRegression()
model.fit(X, Y)
a = float(model.coef_) #线性模型的系数
b = float(model.intercept_) #截距
print('a = {}, b = {}\n'.format(a, b))

f = lambda x: a * x + b
predict = [f(k[0]) for k in X]

R2 = r2_score(Y, predict)
MSE = mean_squared_error(Y, predict)
print('R2 = {} \n MSE = {}'.format(R2, MSE))



plt.scatter(X, Y, color='blue')
plt.plot([i for i in range(9, 31, 2)], [a*i+b for i in range(9, 31, 2)], color='red')
plt.show()

```

### （二）code 2

下面这段代码中使用了推导式来计算参数，并将其与使用sklearn得到的参数进行了对比。事实证明这两种操作得到的结果完全一样。

```python
#-*- coding = UTF-8 -*-

import numpy as np
from numpy.linalg import inv
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression

x = [0.03, 0.04, 0.05, 0.07, 0.09, 0.1, 0.12, 0.15, 0.17,   0.2]
y = [40.5, 39.5, 41, 41.5, 43, 42, 45, 47.5, 53, 56]

'''首先采用推导式求系数做线性回归：'''

X = np.ones((10, 2))
Y = np.ones((10, 1))
for i in range(10):
	X[i, 1] = x[i]
	Y[i, 0] = y[i]
B = inv(X.transpose().dot(X)).dot(X.transpose()).dot(Y)
print('B = {}\n'.format(B)) # 回归参数组成的向量


'''下面采用sklearn方法做线性回归：'''

f = lambda x: round(x, 8)

X = np.ones((10, 1))
for i in range(10):
	X[i, 0] = x[i]

model = LinearRegression()
model.fit(X, Y)
theta_0 = float(model.intercept_)
theta_1 = float(model.coef_)
print(theta_0, '\n', theta_1)
# 由于浮点数精度的原因，两个小数不能直接做比较，
# 将不同方法计算得到的参数取8位小数后再做比较
print(f(theta_0) == f(B[0, 0]))
print(f(theta_1) == f(B[1, 0]))


plt.plot([0.02, 0.22], B[0, 0] + B[1, 0] * np.array([0.02, 0.22]), color='red')
plt.scatter(x, y)
plt.show()

```

为方便代码运行，这里将实例数据直接放入了代码中。还可以尝试sklearn中的make_regression函数来生成回归数据。

```python
from sklearn.datasets import make_regression
```

## 四、总结与反思

没有永远滴神，只有暂时滴神。线性回归模型也有着缺点，我们提出几点思考。

- 对于解析解 $(X^TX)^{-1}X^TY$ ，是可能存在这样一种情况的，即矩阵 $X^TX$ 不可逆，从而使得该式不再适用。这也许是因为数据集 $X$ 中有着太多的重复、冗余项；也许是因为噪音太大；也许是因为 $X$ 包含了较多的 0，即 $X$ 是稀疏的。无论如何，这是一个潜在的问题。
- 如果矩阵 $X^TX$ 维度较高，求逆的速度如何？或者说，这样一个解析解的计算时间复杂度怎样。
- 如果数据集中有着离群点，即有着一个与其他数据点相距非常远的数据，那么这样一个数据是否会对模型的拟合结果造成较大的影响呢？即线性回归模型的鲁棒性(robust)如何？
- 是否可能出现一种情况，即得到的系数中有些很大，而另一些很小，从而使我们得到的预测函数中一些系数基本发挥不了作用？比如一个自变量的数量级是 $10^3$，另一个自变量的数量级是 $10^{-3}$ ，这时它们各自对应的系数就可能出现上述情况。

