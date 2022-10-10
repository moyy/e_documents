# 线性代数 笔记

## 1. 索引

+ [向量-矩阵](./01_vector_matrix.md)
+ [行列式](./05_determinant.md)

## 来源

+ [The Art of Linear Algebra](https://github.com/kenjihiranabe/The-Art-of-Linear-Algebra/blob/main/The-Art-of-Linear-Algebra.pdf)
+ [Six Great Theorems of Linear Algebra](https://math.mit.edu/~gs/linearalgebra/linearalgebra5_6Great.pdf)

## 1、概述

+ 向量 vs 矩阵
+ 线性方程组
+ 线性变换
+ 投影
+ 变换的行列式
+ 特征值 和 特征向量

## 2、6个主要结论

|结论|说明|
|--|--|
|维度|同一个向量空间 所有的基向量 个数 都一样|
|四个子空间|列个数 = 列空间维度 + 零空间维度|
|四个子空间|列空间维度 = 行空间维度 = 秩（Rank）|
|四个子空间|$A_{m \times n}$ 的 列空间 和 零空间 是 $R^n$ 正交互补|
|SVD|存在 正交基 $v_i$, $u_i$ 使得 $A v_i = \sigma_i u_i$|
|谱定理|对称阵 A ($A^T=A$) 有 $A = Q \Lambda Q^T$, 也就是说 存在 正交基 $q_i$, 使得 $A q_i = \lambda_i q_i$|

## 3、最简概念

对 方阵 $A_{n \times n}$

|非奇异阵（NonSigular）|奇异阵（Sigular）|
|--|--|
|A 可逆 invertible|A 不可逆|
|列 线性 无关|列 线性 相关|
|行 线性 无关|行 线性 相关|
|行列式（determinant） 不为0|行列式 不为0|
|$A \vec{x} = \vec{0}$ 仅有一解 $\vec{x} = \vec{0}$|$A \vec{x} = \vec{0}$有无穷多解|
|$A \vec{x} = \vec{b}$ 仅有一解 $\vec{x} = A^{-1} \vec{b}$|$A \vec{x} = \vec{0}$ 无解 或者 有无穷多解|
|A 有 n个 非零 主元（pivots）|A 有 r < n 个 非零 主元|
|A 秩为 n|A 秩为 r < n|
|最简行形式 R = I|R 至少有一行 全为 0|
|列空间 是 $R^n$|列空间 维度 是 r < n|
|行空间 是 $R^n$|行空间 维度 是 r < n|
|所有 特征值（Eigen Vulue） 都 非0|0 是 A 的 特征值|
|$A^TA$ 是 正定 对称阵|$A^TA$ 是 半正定 对称阵|
|A 有 n个 正的 奇异值（Singular Value）|A 有 r (< n) 个 正的 奇异值（Singular Value）|