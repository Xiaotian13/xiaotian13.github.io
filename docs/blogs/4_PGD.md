
> date: 2020-05-09




## 简介

对于包含不可微部分的优化目标如lasso回归的求解，梯度下降等算法已不再适用。近端梯度下降(Proximal Gradient Descent)即被用来解决包含不可导项的优化问题。

<!--more-->

## 回顾梯度下降

给定一个无约束优化问题形如
$$
\min_\mathbf{x} f(\mathbf{x})
\tag{1}
$$

其中 $\mathbf{x} \in \mathbb R^n, f:\mathbb R^{n} \rightarrow \mathbb R$ ；

函数 $f(\mathbf{x})$ 连续可微，且关于 $\mathbf x$ 是（下）凸函数。需要指出的是，在下面的推导中，如未特殊说明，则所使用的向量均为列向量。

对于函数 $f(\mathbf{x})$ ，我们在点 $\mathbf x_0$ 做二阶泰勒展开，将其用来近似原函数：
$$
\begin{aligned} 
f(\mathbf{x})
& \approx f(\mathbf{x}_0) + \nabla f(\mathbf{x}_0)^{T}(\mathbf{x} - \mathbf{x}_0) + \frac{1}{2} (\mathbf{x} - \mathbf{x}_0)^T\nabla^{2} f(\mathbf{x})(\mathbf{x} - \mathbf{x}_0) \\ 
\end{aligned}
\tag{2}
$$
其中，$\nabla f(\mathbf x_0)$ 为函数 $f(\mathbf x)$ 在点 $\mathbf x_0$ 处的梯度；$\nabla^2 f(\mathbf x_0)$ 为函数 $f(\mathbf x)$ 在点 $\mathbf x_0$ 处的Hessian矩阵；对于上式中的 $\nabla f(\mathbf{x}_0)^{T}(\mathbf{x} - \mathbf{x}_0)$ 项，通常也可以用内积的形式表示，即 $<\nabla f(\mathbf{x}_0), (\mathbf{x} - \mathbf{x}_0)>$ 。
$$
\nabla f(\mathbf x_0) = \left [
\begin{matrix}
\frac{\partial f}{\partial x_{1}} \\
\frac{\partial f}{\partial x_{2}} \\
\vdots \\
\frac{\partial f}{\partial x_{n}} \\
\end{matrix}
\right ]_{\mathbf x = \mathbf x_0} \ ; 
\nabla^2 f(\mathbf x_0) = Hf(\mathbf x_0) =\left[\begin{array}{cccc}
\frac{\partial^{2} f}{\partial x_{1}^{2}} & \frac{\partial^{2} f}{\partial x_{1} \partial x_{2}} & \cdots & \frac{\partial^{2} f}{\partial x_{1} \partial x_{n}} \\
\frac{\partial^{2} f}{\partial x_{2} \partial x_{1}} & \frac{\partial^{2} f}{\partial x_{2}^{2}} & \cdots & \frac{\partial^{2} f}{\partial x_{2} \partial x_{n}} \\
\vdots & \vdots & \ddots & \vdots \\
\frac{\partial^{2} f}{\partial x_{n} \partial x_{1}} & \frac{\partial^{2} f}{\partial x_{n} \partial x_{2}} & \cdots & \frac{\partial^{2} f}{\partial x_{n}^{2}}
\end{array}\right]_{\mathbf x = \mathbf x_0}
$$
二阶导相对一阶导要难求很多，并且从时间复杂度的角度上来讲，求二阶导信息即Hessian矩阵要花去许多时间。我们将二阶导用 $\frac{1}{t} I_n$ 代替，其中 $t > 0$ ，$I_n$ 为 $n$ 阶单位矩阵，便有
$$
\begin{aligned} 
f(\mathbf{x})
& \approx f(\mathbf{x}_0) + \nabla f(\mathbf{x}_0)^{T}(\mathbf{x} - \mathbf{x}_0) + \frac{1}{2t} \|\mathbf{x} - \mathbf{x}_0 \|_{2}^{2} \\ 
\end{aligned}
\tag{3}
$$
我们求解上述 $(3)$ 对原函数 $f(\mathbf{x})$ 的近似式，并用 $\mathbf{x}^+$ 来表示最优解，即
$$
\mathbf{x}^+ = \mathop{\arg\min}_\mathbf{x} \{f(\mathbf{x}_0) + \nabla f(\mathbf{x}_0)^{T}(\mathbf{x} - \mathbf{x}_0) + \frac{1}{2t} \|\mathbf{x} - \mathbf{x}_0 \|_{2}^{2} \}
\tag{4}
$$
由于 $f(\mathbf{x}_0)$ 是常数，对该优化问题没有影响，故上式等价于
$$
\mathbf{x}^+ = \mathop{\arg\min}_\mathbf{x} \{\nabla f(\mathbf{x}_0)^{T}(\mathbf{x} - \mathbf{x}_0) + \frac{1}{2t} \|\mathbf{x} - \mathbf{x}_0 \|_{2}^{2} \}
\tag{5}
$$
上面这个目标函数是关于自变量 $\mathbf x$ 的“二次函数”，能够轻松的得到其最小值。我们再做一定化简：
$$
\begin{aligned} 
\mathbf{x}^+ &= \mathop{\arg\min}_\mathbf{x} \{\nabla f(\mathbf{x}_0)^{T}(\mathbf{x} - \mathbf{x}_0) + \frac{1}{2t} \|\mathbf{x} - \mathbf{x}_0 \|_{2}^{2} \} \\
& = \mathop{\arg\min}_\mathbf{x} \{\nabla f(\mathbf{x}_0)^{T}(\mathbf{x} - \mathbf{x}_0) + \frac{1}{2t} (\mathbf{x} - \mathbf{x}_0)^T(\mathbf{x} - \mathbf{x}_0) \} \\ 
& = \mathop{\arg\min}_\mathbf{x} \{\frac{1}{2t}[\mathbf{x} - (\mathbf{x}_0 - 2t\nabla f(\mathbf{x}_0)]^T(\mathbf{x} - \mathbf{x}_0) \} \tag{6} \\ 
\end{aligned}
$$
化简后，可以明显看出该“二次函数”的两个零点。因此，自变量取两个零点的中点时，函数值最小，即有
$$
\begin{aligned} 
\mathbf{x}^+ &= \frac{1}{2}[\mathbf{x}_0 - 2t\nabla f(\mathbf{x}_0) + \mathbf{x}_0] \\
& = \mathbf{x}_0 - t \nabla f(\mathbf{x}_0)
\end{aligned}
$$
亦或者，我们可将 $(6)$ 式再做化简：
$$
\begin{aligned} 
\mathbf{x}^+ 
& = \mathop{\arg\min}_\mathbf{x} \{\frac{1}{2t}[\mathbf{x} - (\mathbf{x}_0 - 2t\nabla f(\mathbf{x}_0)]^T(\mathbf{x} - \mathbf{x}_0) \} \\ 
& = \mathop{\arg\min}_\mathbf{x} \{\frac{1}{2t}[\mathbf{x} - (\mathbf{x}_0 - t\nabla f(\mathbf{x}_0) + t\nabla f(\mathbf{x}_0)]^T[\mathbf{x} - (\mathbf{x}_0 - t\nabla f(\mathbf{x}_0) - t\nabla f(\mathbf{x}_0)] \} \\
& = \mathop{\arg\min}_\mathbf{x} \{\frac{1}{2t}\|\mathbf{x} - (\mathbf{x}_0 - t\nabla f(\mathbf{x}_0)\|_2^2 - \frac{t}{2}\|\nabla f(\mathbf{x}_0)\|_2^2 \} \\
& = \mathop{\arg\min}_\mathbf{x} \frac{1}{2t}\|\mathbf{x} - (\mathbf{x}_0 - t\nabla f(\mathbf{x}_0)\|_2^2 \tag{7} \\
& = \mathbf{x}_0 - t\nabla f(\mathbf{x}_0) \tag{8}
\end{aligned}
$$
我们将 $(8)$ 式中的符号做出一点小改动，就得到
$$
\mathbf{x}^k = \mathbf{x}^{k-1} - t \nabla f(\mathbf{x}^{k-1}) \tag{9}
$$
这就是梯度下降的迭代公式。

> 直观的看 $(7)$ 式，不难发现，前面的二阶泰勒展开等做法，本质为将原问题转化为求解其二阶近似(quadratic approximation)；
>
> 在得到 $(3)$ 式的过程中，我们用 $\frac{1}{t} I_n$ 去替换 $(2)$ 中的 $\nabla^2 f(\mathbf x_0)$ ，这并不是毫无根据的。通常来说，我们会假设目标函数 $f(\mathbf x)$ 在 $\mathbb R^n$ 满足Lipchitz连续，即 $\forall \mathbf{x}_1, \mathbf{x}_2 \in \mathbb R^n$ ，$\exists \ L > 0$ ， 满足
> $$
> |\nabla f(\mathbf x_1) - \nabla f(\mathbf x_2)| \leq L |\mathbf x_1 - \mathbf x_2|
> $$
> 上式中的 $\leq$ 表示两向量中对应分量均分别满足该关系。直观的看，函数满足Lipchitz连续意味着，导函数曲线上任两点间的连线斜率不能无穷大；也即表明，函数的二阶导是有穷的，我们可以找到一个有穷的大整数来作为二阶导的上界。因此，我们取 $t = \frac{1}{L}$ ，也就是 $L = \frac{1}{t}$ ，我们将 $L I_n$ 或 $\frac{1}{t} I_n$ 视为 $\nabla^2 f(\mathbf x_0)$ 的一个“上界”，并用其替换Hessian矩阵，以简化求解。

## 临近点算子

对于lasso回归，我们的优化问题为
$$
\begin{aligned}
& \min_\beta \frac{1}{2}\|Y - X\beta\|_2^2 + \lambda\|\beta\|_1 \\
& \text { s.t. } \ Y, \beta\in \mathbb R^n, X\in \mathbb R^{m\times(n+1)},\lambda>0
\end{aligned}
\tag{10}
$$
可以看到，由于 $l_1$-norm 的存在，该目标函数不可导。类似地，我们研究这样一种目标函数，即其可拆分为两部分：
$$
f(\mathbf x) = g(\mathbf x) + h(\mathbf x)
$$
$$
\min_{\mathbf x} f(\mathbf x) \Leftrightarrow \min_{\mathbf x} g(\mathbf x) + h(\mathbf x)
$$

其中 $g(\mathbf x)$ 是连续可导的凸函数；$h(\mathbf x)$ 是连续的凸函数，可以是不光滑的（不可导）(not necessarily differentiable)。

对应上面的lasso回归 $(10)$，我们即对应
$$
\begin{aligned}
& f(\beta) = g(\beta) + h(\beta) \\
& g(\beta) = \frac{1}{2}\|Y - X\beta\|_2^2 \\
& h(\beta) = \lambda\|\beta\|_1
\end{aligned}
$$
这样，我们将目标函数不可导的部分与可导的部分拆分开来。对于可导的部分 $g(\mathbf x)$ ，我们可以做类似上面的 $(3) \sim (8)$ 式的处理，有
$$
\begin{align*} 
\mathbf{x}^+ 
& = \mathop{\arg\min}_\mathbf{x} \{g(\mathbf{x}_0) + \nabla g(\mathbf{x}_0)^{T}(\mathbf{x} - \mathbf{x}_0) + \frac{1}{2t} \|\mathbf{x} - \mathbf{x}_0 \|_{2}^{2} +h(\mathbf x) \} \\ 
& = \mathop{\arg\min}_\mathbf{x} \{ \frac{1}{2t}\|\mathbf{x} - (\mathbf{x}_0 - t\nabla g(\mathbf{x}_0)\|_2^2 + h(\mathbf x) \} \tag{12} \\
\end{align*}
$$
这次我们不能直接给出 $\mathbf x^+$ 的显式解了，因为有着不可导项 $h(\mathbf x)$ 项的存在。下面给出**临近点算子/近端算子(proximal operator)**的定义：
$$
\text{prox}_{t,h(\cdot)}(\mathbf z) = \mathop{\arg\min}_\mathbf{x} \{ \frac{1}{2t}\|\mathbf{x} - \mathbf z\|_2^2 + h(\mathbf x) \} \tag{13}
$$

> 需注意，形如
> $$
> \text{prox}_{t,h} (\mathbf z) = \mathop{\arg\min}_{\mathbf x} \{ \frac{1}{2}\|\mathbf x - \mathbf z\|_2^2 + th(\mathbf x) \} \\
> $$
> $$
> \text{prox}_{t,h} (\mathbf x) = \mathop{\arg\min}_{\mathbf z} \{ \frac{1}{2}\|\mathbf x - \mathbf z\|_2^2 + th(\mathbf z) \}
> $$
>
> 与式 $(13)$ 是等价的。

有了 $(13)$ 的符号表示，我们可将式 $(12)$ 等价转化为：
$$
\mathbf x^+ = \text{prox}_{t, h(\cdot)}(\mathbf x_0 - t \nabla g(\mathbf x_0))
$$
对上式做类似 $(8) \sim (9)$ 的符号变动，即有
$$
\mathbf x^{k} = \text{prox}_{t, h(\cdot)}(\mathbf x^{k-1} - t \nabla g(\mathbf x^{k-1})) \tag{14}
$$
$(14)$ 式即为近端梯度下降算法的核心迭代式。对于很多不同的函数形式 $h(\cdot)$ ，如 $l_0$ 范数、$l_1$ 范数、$l_2$ 范数、二次函数等，近端算子都有着对应的闭式解(closed-form)（或者说显式解），下表我们就列出了几个简单的情况。更详细的可参考文末的ref [3]。

|           $h(\cdot)$           |            $\text{prox}_{t,h(\cdot)}(\mathbf z)$             |            Comment            |
| :----------------------------: | :----------------------------------------------------------: | :---------------------------: |
|               0                |                         $\mathbf z$                          |           Obviously           |
| $\vert\vert \cdot\vert\vert_0$ | $z_i = \left\{\begin{array}{ll}z_i & \vert z_i\vert \geq t \\0 & \text { otherwise }\end{array}\right.$ | Elementwise hard-thresholding |
| $\vert\vert\cdot\vert\vert_1$  |      $z_i = \text{sign}(z_i)\max(\vert z_i\vert -t, 0)$      | Elementwise soft-thresholding |

---

### 线性回归

线性回归即对应 $h(\cdot) = 0$ 的情况，此时我们的迭代式即为：
$$
\mathbf x^{k} = \text{prox}_{t, h(\cdot)=0}(\mathbf x^{k-1} - t \nabla g(\mathbf x^{k-1})) = \mathbf x^{k-1} - t \nabla g(\mathbf x^{k-1})
$$

---

### $l_1$ 范数

lasso回归中使用了 $l_1$ 范数，故对应 $h(\cdot) = \lambda\|\cdot\|_1$ 的情况，此时迭代式 $(14)$ 即可化为：
$$
\mathbf x^{k} = \text{prox}_{t, \lambda\|\cdot\|_1}(\mathbf x^{k-1} - t \nabla g(\mathbf x^{k-1})) = S_{\lambda t}(\mathbf x^{k-1} - t \nabla g(\mathbf x^{k-1}))
\tag{15}
$$
其中 $S_{\lambda t}(\cdot)$ 称为**软阈值算子(soft-thresholding operator)**，可以看到该算子即为临近点算子中 $h(\cdot) = \lambda\|\cdot\|_1$ 的情况。考虑标量 $z$ ，我们有
$$
\text{prox}_{t, \lambda\|\cdot\|_1}(z) = S_{\lambda t}(z) = \text{sign}(z)\max(|z|-\lambda t, 0)
= 
\left\{\begin{array}{ll}
z - \lambda t &  z > \lambda t \\
z & -\lambda t \leq z \leq \lambda t \\
z + \lambda t & z < -\lambda t
\end{array}\right.
\tag{16}
$$
其中，$\text{sign}(\cdot)$ 为符号函数，即
$$
\text{sign}(z)
= 
\left\{\begin{array}{ll}
1 &  z > 0 \\
0 & z = 0 \\
-1 & z < 0
\end{array}\right.
$$
考虑向量 $\mathbf z = [z_1, \cdots,z_n]^T \in \mathbb R^n$ ，则 $S_{\lambda t}(\mathbf z) = [S_{\lambda t}(z_1),\cdots, S_{\lambda t}(z_n)]^T$ ，即为对其每个分量均做 $(16)$ 式的操作。我们也可推导出其具体形式，如下：
$$
\begin{aligned} 
S_{\lambda t}(z_i)
& = \text{sign}(z_i)\max(|z_i|-t, 0) \\ 
& = \text{sign}(z_i)|z_i|\max(\frac{|z_i|-t}{|z_i|}, 0) \\
& = z_i\max(\frac{|z_i|-t}{|z_i|}, 0) \\
& = z_i\max(1 - \frac{t}{|z_i|}, 0) \\
\end{aligned} \\
$$

$$
\therefore S_{\lambda t}(\mathbf z) = \mathbf z \circ [1 - \frac{t}{|\mathbf z|}]_+ 
\tag{17}
$$

其中 $|\mathbf z|$ 为对向量 $\mathbf z$ 的每个分量取绝对值后组成的向量；$[\cdot]_+$ 表示取正，即 $\max(\cdot, 0)$ ；$A\circ B$ 代表 $A_{m\times n}$ 与 $B_{m\times n}$ 的Hadamard乘积，即对应元素相乘，满足 $(A\circ B)_{ij} = a_{ij} \times b_{ij}$ 。通常也可用符号 $\odot$ 表示。

> 我们简单推导一下为什么一范数对应的临近点算子的显式解是这个形式，下面的推导不是严格推导。
> $$
> \begin{aligned}
> \text{prox}_{t,\|\cdot\|_1}(\mathbf z)
> & =  \mathop{\arg\min}_\mathbf{x} \{ \frac{1}{2t}\|\mathbf{x} - \mathbf z\|_2^2 + \|\mathbf x\|_1 \} \\ & = \mathop{\arg\min}_\mathbf{x}  \sum_{i=1}^n [\frac{1}{2t}(x_i - z_i)^2 + |x_i|\ ]  
> \end{aligned}
> $$
>
> $$
> \therefore [\text{prox}_{t,\|\cdot\|_1}(\mathbf z)]_i = \mathop{\arg\min}_{x_i} \{ \frac{1}{2t}(x_i - z_i)^2 + |x_i|\ \}= \mathop{\arg\min}_{x_i} f(x_i)
> $$
>
> 对函数 $f(x_i)$ “求导”并令其等于0，
> $$
> \frac{df}{d x{_i}} = \frac{1}{t} (x_i - z_i) + \text{sign}(x_i)  = 0
> $$
> 化简，即有
> $$
> x_i = z_i - t \cdot \text{sign}(x_i)
> $$
> 当 $x_i > 0$ ，上式即为 $x_i = z_i - t > 0 $ ；
>
> 当 $x_i < 0$ ，上式即为 $x_i = z_i + t < 0 $ ；
>
> 当 $x_i = 0$ ，上式即为 $x_i = z_i$ ；
>
> 分别对应上面三种情况，我们即可得到
> $$
> [\text{prox}_{t,\|\cdot\|_1}(\mathbf z)]_i= \left\{\begin{array}{ll}z_i -  t &  z_i > t \\z_i & -t \leq z_i \leq t \\z_i + t & z < -t\end{array}\right.
> $$

---

### $l_2$ 范数

对于 $h(\cdot) = || \cdot ||_2$ 的情况，我们即有
$$
\text{prox}_{t,\|\cdot\|_2}(\mathbf z) = \mathbf z \circ [1 - \frac{t}{\|\mathbf z\|_2}]_+ 
\tag{18}
$$

## 近端梯度下降与软阈值迭代算法

通常我们将 $l_1$ 范数对应的临近点算子称为软阈值，将 $l_0$ 范数对应的临近点算子称为硬阈值。**软阈值迭代算法(ISTA, iterative soft-thresholding algorithm)**，即为用软阈值作为包含 $l_1$ 范数的目标函数的“梯度”，使用“梯度下降”来得到最优值的算法。回顾lasso回归的优化问题 $(10)$ 与迭代式 $(14)$ ，我们可以计算得到
$$
\begin{align*}
\nabla g(\beta) & = \nabla \frac{1}{2}\|Y - X\beta\|_2^2 = X^T(X\beta - Y), \\
\therefore \beta^{k}
& =  \text{prox}_{t, \lambda \|\cdot\|_1}(\beta^{k-1} - t \nabla g(\beta^{k-1})) \\ 
& = S_{\lambda t}(\beta^{k-1} - t X^T(X\beta^{k-1} - Y)) \tag{19} \\
& = \beta^{k-1} \circ [1 - \frac{\lambda t}{|\beta^{k-1}|}]_+ \tag{20}
\end{align*}
$$
下面我们给出完整的ISTA步骤。

>  $\mathbf{Algorithm \ 1. \ \ \ \ ISTA}$ 
>
> input: a small number $\epsilon$ or maxiterations.
>
> output: $\hat \beta$
>
> Initialize $\beta, t, \lambda$ (e.g., set $\beta^0 = \vec{1}_{n\times1}, t=0.1, \lambda = 0.1$), set $k=1$ 
>
> **while** $\left|J^{k}-J^{k-1}\right|<\varepsilon$ or $t<$ maxiterations **do**
>
> ​	update $\beta^k$ using $(19)$ or $(20)$ .
>
> ​    compute $J^k(\beta),$ where $J^k(\beta) = \frac{1}{2}||Y-X\beta^k||_2^2 + \lambda||\beta^k||_1$
>
> ​	$k := k+1$
>
> **end while**
>
>  Output result given by $\hat \beta = \beta^{k-1}$

更一般地，我们给出**近端梯度下降(PGD)的算法框架**如下：

> $\mathbf{Algorithm \ 2. \ \ \ \ PGD}$ 
>
>input: a small number $\epsilon$ or maxiterations.
>
>output: $ \hat \beta $
>
>Initialize $\beta, t, \lambda$ (e.g., set $\beta^0 = \vec{1}_{n\times1}, t=0.1, \lambda = 0.1$), set $k=1$ 
>
>**while** $\left|J^{k}-J^{k-1}\right|<\varepsilon$ or $t<$ maxiterations **do**
>
>​	update $\beta^k$ using $(14)$ , i.e., $\beta^k = \text{prox}_{t, h(\cdot)}(\beta^{k-1} - t \nabla g(\beta^{k-1}))$
>
>​    compute $J^k(\beta),$ where $J^k(\beta) = g(\beta^k) + \lambda h(\beta^k)$
>
>​	$k := k+1$
>
>**end while**
>
>Output result given by $\hat \beta = \beta^{k-1}$



## 编程实现

有时间再补。

## 总结与反思

下面我们提出几点思考。

- 步长参数 $t$ 如何选取？
- 算法的时间复杂度如何？

## References

1. http://stat.cmu.edu/~ryantibs/convexopt/lectures/prox-grad.pdf

2. https://angms.science/doc/CVX/ISTA0.pdf

3. https://www.math.ucla.edu/~wotaoyin/summer2016/4_proximal.pdf

   （各种 $h(\cdot)$ 对应近端算子闭式解）

