
> date: 2020-05-18



## 简介

在无监督学习中，常见的两种任务为聚类与降维。这里给出三个聚类效果评价指标：互信息，标准化互信息，调整互信息(MI, NMI, AMI)，分别给出它们的计算方法与代码。需要指出的是，这三个指标均需要已知数据点的真实标签。

<!--more-->

## Preliminaries and Notation

已知 $N$ 个 $D$ 维的数据，构成数据矩阵 $\mathbf{X} = [x_1, x_2, \cdots, x_N] \in \mathbb{R}^{D\times N}$ ；

对每个数据 $x_i \in \mathbb{R}^{D\times 1}$ ，其对应标签(label)为 $u_i \in \mathbb{R}$ ；

标签中共有 $R$ 个不同的取值，或者说数据共有 $R$ 类，其对应的标签组成向量 $U = [u_1, u_2, \cdots, u_N] \in \mathbb{R}^N $ ；

我们取自然数 $1$ 至 $N$ 来为这 $N$ 个数据点依次编号，并取 $S = \left\{1, 2, \cdots, N\right\}$ ，则有
$$
S = \mathop{\cup}\limits_{i=1}^R U_i = \left\{U_1, U_2, \cdots, U_R\right\}
$$
 其中 $U_i$ 代表归属于第 $i$ 类的数据集合；如对向量 $U = [1, 1, 2, 2, 3, 3, 3]$ ，则有
$$
N = 7, R = 3, U_1 = \left\{1, 2\right\}, U_2 = \left\{3, 4\right\}, U_3 = \left\{5, 6, 7 \right\}
$$
> 这里需要指出的是，如果整个数据集共有5类，则我们默认用自然数1至5来代表这5类；
>
> 对于 $U$ 而言，其包含 $R$ 类，则用自然数 $1$ 至 $R$ 来代表这 $R$ 个类别。

所谓聚类问题，即为对原始数据集 $\mathbf{X}$ 进行一种划分；通过某种聚类算法（如DBSCAN、谱聚类等），我们可以得到一个对 $\mathbf{X}$ 的划分，即 $V = [v_1, v_2, \cdots, v_N] \in \mathbb{R}^N$ ；

类似地，我们有
$$
S = \mathop{\cup}\limits_{i=1}^C V_i = \left\{V_1, V_2, \cdots, V_C \right\}
$$
其中 $V_i$ 代表聚类结果中归属于第 $i$ 类的数据集合。



## 信息熵与列联表(contingency table)

在介绍互信息之前，首先介绍一下香农熵（信息熵）和列联表(contingency table)。

对上述标签向量 $U \in \mathbb{R}^N$ ，其香农熵（信息熵）可以被计算为：
$$
\text{H}(U) = -\sum_{i=1}^R p_i \log p_i \tag{1}
$$
其中对数函数的底常取 $2$ 或自然对数 $e$；

 $p_i$ 为归属于第 $i$ 类的数据个数占数据总量的比例，即
$$
p_i = \frac{\left| U_i \right|}{N} \tag{2}
$$

如对向量 $U = [1, 1, 1]$ ，则有 $p_1 = 1$ ，$\text{H} (U) = - 1 \cdot \log(1) = 0$ ；

对向量 $U = [1, 2, 3]$ ，则有 $p_1 = p_2 = p_3 = \frac{1}{3}$ ，$\text{H} (U) = - 3 \cdot \frac{1}{3} \cdot \log(\frac{1}{3}) = \log 3$ ；

对向量 $U = [1, 2, 2]$ ，则有 $p_1 = \frac{1}{3}, p_2 = \frac{2}{3}$ ，$\text{H} (U) = - \frac{1}{3} \cdot \log(\frac{1}{3}) - \frac{2}{3} \cdot \log(\frac{2}{3}) = \log 3 - \frac{2}{3} \cdot \log2$ ；

不难发现，信息熵是非负的，因为 $0 < p_i \leq 1$ 。



我们取矩阵 $M \in \mathbb{R}^{R\times C} $ 为真实标签向量 $U$ 与预测标签向量 $V$ 的列联表(contingency table)，满足
$$
m_{ij} = \left|U_i \cap V_j\right| \tag{3}
$$
其中 $i = 1, 2, \cdots, R$ ，$j = 1, 2, \cdots, C$ 。

如对向量 $U = [1, 1, 2, 2], V = [1, 1, 1, 2]$ ，有
$$
\begin{aligned}U_1 &= \left\{1, 2\right\} & U_2 &= \left\{3, 4\right\} \\V_1 &= \left\{1, 2, 3\right\} & V_2 &= \left\{4\right\}\end{aligned}
$$
因此，
$$
\begin{aligned}
m_{1 1} & = \left|U_1 \cap V_1\right| = \left|\left\{1, 2\right\}\right| = 2 \\
m_{1 2} & = \left|U_1 \cap V_2\right| = \left|\varnothing\right| = 0 \\
m_{2 1} & = \left|U_2 \cap V_1\right| = \left|\left\{3 \right\}\right| = 1 \\
m_{2 2} & = \left|U_2 \cap V_2\right| = \left|\left\{4 \right\}\right| = 1 \\
\end{aligned}
$$
列联表为
$$
M = 
\left[
\begin{array}{l}
2 & 0 \\
1 & 1
\end{array}
\right]
$$



## 互信息(Mutual information)

互信息的计算公式如下：

$$
\text{MI}(U, V) = \sum_{i = 1}^R\sum_{j=1}^C p_{i,j} \log\left(\frac{p_{i, j}}{p_i \times p_j}\right) \tag{4}
$$
其中
$$
p_{i,j} = \frac{\left|U_i \cap V_j \right|}{N} = \frac{m_{ij}}{N} \tag{5}
$$

$$
p_i = \frac{\left| U_i \right|}{N}, 
p_j = \frac{\left| V_j \right|}{N}
$$

如对向量 $U = [1, 1, 2, 2], V = [1, 1, 1, 2]$ ，有
$$
\begin{aligned}
p_{1, 1} & = m_{11} / N = 0.5 \\
p_{1, 2} & = m_{12} / N = 0 \\
p_{2, 1} & = m_{21} / N = 0.25 \\
p_{2, 2} & = m_{22} / N = 0.25 \\
\end{aligned}
$$


## 标准化互信息(NMI, Normalized Mutual Information)

通常采用NMI和AMI来作为衡量聚类效果的指标。

标准化互信息的计算方法如下：

$$
\text{NMI}(U, V) = \frac{\text{MI}(U, V)}{F\left(\text{H}\left(U\right), \text{H}\left(V\right)\right)} \tag{6}
$$

其中 $F(x_1, x_2)$ 可以为 $\min$/$\max$ 函数；可以为几何平均，即 $F(x_1, x_2) = \sqrt{x_1x_2}$ ；可以为算术平均，即 $F(x_1, x_2) = \frac{x_1 + x_2}{2}$ 。

通常我们选取算术平均，则标准化互信息即可被计算为
$$
\text{NMI}(U, V) = 2 \frac{\text{MI}(U, V)}{\text{H}\left(U\right) + \text{H}\left(V\right)} \tag{7}
$$



## 调整互信息(AMI, Adjusted Mutual Information)

调整互信息的计算要复杂一些，其计算方法如下：

$$
\text{AMI}(U, V) = \frac{\text{MI}(U, V) - \mathbb E\left\{ \text{MI}(U, V) \right\}}{F\left(\text{H}\left(U\right), \text{H}\left(V\right)\right) - \mathbb E\left\{ \text{MI}(U, V) \right\}} \tag{8}
$$

其中，$\mathbb E\left\{ \text{MI}(U, V) \right\}$ 为互信息 $\text{MI}(U, V)$ 的期望，计算方法为
$$
\begin{aligned}
\mathbb E\left\{ \text{MI}(U, V) \right\} &= \\
\sum_{i=1}^R \sum_{j=1}^C &\sum_{k = \left(a_i + b_j - N \right)^+}^{\min \left(a_i, b_j\right)} \frac{k}{N} \log\left(\frac{N \times k}{a_i \times b_j}\right)\frac{a_i!b_j!\left(N - a_i\right)!\left(N - b_j\right)!}{N!k!\left(a_i - k\right)!\left(b_j - k\right)!\left(N - a_i - b_j + k\right)!} 
\end{aligned}
\tag{9}
$$
其中 $\left(a_i + b_j - N \right)^+$ 为 $\max \left(1, a_i + b_j - N \right)$ ；

$a_i, b_j$ 分别为列联表 $M$ 的第 $i$ 行和与第 $j$ 列和，具体为
$$
\begin{aligned}
a_i = \sum_{j=1}^C m_{ij} \\
b_j = \sum_{i=1}^R m_{ij}
\end{aligned}
\tag{10}
$$
如果我们选取函数 $F(x_1, x_2)$ 为 $\max$ 函数，则调整互信息可被计算为
$$
\text{AMI}(U, V) = \frac{\text{MI}(U, V) - \mathbb E\left\{ \text{MI}(U, V) \right\}}{\max\left(\text{H}\left(U\right), \text{H}\left(V\right)\right) - \mathbb E\left\{ \text{MI}(U, V) \right\}} \tag{11}
$$

如果我们选取函数 $F(x_1, x_2)$ 为几何平均，则调整互信息可被计算为

$$
\text{AMI}(U, V) = \frac{\text{MI}(U, V) - \mathbb E\left\{ \text{MI}(U, V) \right\}}{\sqrt{\text{H}\left(U\right) \cdot \text{H}\left(V\right)} - \mathbb E\left\{ \text{MI}(U, V) \right\}} \tag{12}
$$

如果我们选取函数 $F(x_1, x_2)$ 为算术平均，则调整互信息可被计算为

$$
\text{AMI}(U, V) = \frac{\text{MI}(U, V) - \mathbb E\left\{ \text{MI}(U, V) \right\}}{\frac{1}{2}\left(\text{H}\left(U\right) + \text{H}\left(V\right)\right) - \mathbb E\left\{ \text{MI}(U, V) \right\}} \tag{13}
$$



## 编程实现

Python中的 `sklearn` 库里有这三个指标的类，可以直接调用；

matlab中似乎没有找到现成的包，因此自己编写。

### python

这里NMI和AMI的计算均采用算术平均；$\log$ 函数的底为自然对数 $e$ 。

```python
from sklearn.metrics.cluster import entropy, mutual_info_score, normalized_mutual_info_score, adjusted_mutual_info_score

MI = lambda x, y: mutual_info_score(x, y)
NMI = lambda x, y: normalized_mutual_info_score(x, y, average_method='arithmetic')
AMI = lambda x, y: adjusted_mutual_info_score(x, y, average_method='arithmetic')

A = [1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 3]
B = [1, 2, 1, 1, 1, 1, 1, 2, 2, 2, 2, 3, 1, 1, 3, 3, 3]
#print(entropy(A))
#print(MI(A, B))
print(NMI(A, B))
print(AMI(A, B))

C = [1, 1, 2, 2, 3, 3, 3]
D = [1, 1, 1, 2, 1, 1, 1]
print(NMI(C, D))
print(AMI(C, D))
```



### matlab

这里的NMI与AMI的代码实现均采用的算术平均， $\log$ 函数的底采用自然对数 $e$ 。

函数文件：

`NMI_AMI.m` 

```matlab
function [NMI, AMI] = NMI_AMI(X, Y)
%NMI_AMI return NMI, AMI
% MI: mutual information
% H; entropy
% NMI: normalized mutual infomation
% AMI: adjusted mutual infomation
% NMI(X, Y) = MI(X, Y) / F(H(X), H(Y))
% AMI(X, Y) = (MI(X, Y) - EMI(X, Y)) / (F(H(X) + H(Y)) - EMI(X, Y))
% F(x, y) is a function, can be "mean", "max", "geometric", "arithmetic"
% here we both use arithmetric

NMI = 2 * MI(X, Y) / (H(X) + H(Y));

AMI = (MI(X, Y) - EMI(X, Y)) / (1/2 * (H(X) + H(Y)) - EMI(X, Y));

end



function [res] = MI(X, Y)
%MI mutual infomation

n = length(X);
X_list = unique(X);
Y_list = unique(Y);
res = 0;
for x = X_list
    for y = Y_list
        loc_x = find(X == x);
        loc_y = find(Y == y);
        loc_xy = intersect(loc_x, loc_y);
        res = res + length(loc_xy) / n * log(length(loc_xy) / n / ((length(loc_x) / n) * (length(loc_y) / n)) + eps);
    end
end

end



function [res] = H(X)
%H information entropy

n = length(X);
X_list = unique(X);
res = 0;

for x = X_list
    loc = find(X == x);
    px = length(loc) / n;
    res = res - px * log(px);
end

end



function [res] = f(a, b)
% F calculate a! / b!
% sometimes a and b can be very large, hence, directly calculate a! or b! is not
% suitable; but maybe a-b is small; 
% a,b should both be positive integers

res = 1;
if a > b
    for i = b+1:a
        res = res * i;
    end
elseif a < b
    for i = a+1:b
        res = res / i;
    end
else
    res = 1;
end

end



function [res] = EMI(U, V)
% EMI expected mutual information, E[MI(X, Y)]

N = length(U);

U_list = unique(U);
V_list = unique(V);
R = length(U_list);
C = length(V_list);

M = zeros(R, C);
for i = 1:R
    for j = 1:C
        U_loc = find(U == U_list(i));
        V_loc = find(V == V_list(j));
        M(i, j) = length(intersect(U_loc, V_loc));
    end
end

a = sum(M, 2);
b = sum(M, 1);
res = 0;

for i = 1:R
    for j = 1:C
        for nij = max(a(i) + b(j) - N, 1):min(a(i), b(j))
            res = res + nij / N * log(N * nij / (a(i) * b(j)) + eps) * f(a(i), a(i) - nij) * f(b(j), b(j) - nij) * f(N - a(i), N) * f(N - b(j), N - a(i) - b(j) + nij) / factorial(nij);
        end
    end
end

end

```

主文件（或脚本）：

```matlab
clc;
format long

A = [1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 3];
B = [1, 2, 1, 1, 1, 1, 1, 2, 2, 2, 2, 3, 1, 1, 3, 3, 3];
[NMI, AMI] = NMI_AMI(A, B)

C = [1, 1, 2, 2, 3, 3, 3];
D = [1, 1, 1, 2, 1, 1, 1];
[NMI, AMI] = NMI_AMI(C, D)
```



### 结果对比

python代码的运行结果为：

> 0.36456177185718985
>
> 0.26018122538925054
>
> 0.28483386264113447
>
> 0.056748831755324296



matlab代码的运行结果为：

> NMI =
>
>    0.364561771857190
>
>
> AMI =
>
>    0.260181225389251
>
>
> NMI =
>
>    0.284833862641135
>
>
> AMI =
>
>    0.056748831755324



## 总结与反思

这里仅给出了三个指标的计算方法，方便直接使用；对于各个指标的优缺点、构造思想等，并没有研究。



## References

1. https://en.wikipedia.org/wiki/Adjusted_mutual_information （科学上网）

2. Vinh, Nguyen Xuan; Epps, Julien; Bailey, James (2010), "Information Theoretic Measures for Clusterings Comparison: Variants, Properties, Normalization and Correction for Chance" (PDF), The Journal of Machine Learning Research, 11 (oct): 2837–54

   论文链接：http://jmlr.csail.mit.edu/papers/volume11/vinh10a/vinh10a.pdf

