- [3*3 矩阵的 QR 分解](#33-矩阵的-qr-分解)
  - [1、概述](#1概述)
  - [2、方法1：Gram–Schmidt 正交化](#2方法1gramschmidt-正交化)
  - [3、方法2：矩阵相乘](#3方法2矩阵相乘)
    - [3.1、确定 平方根 的 正负](#31确定-平方根-的-正负)

# 3*3 矩阵的 QR 分解

## 1、概述

$$
A = \left(
    \begin{matrix}
    a_{00} & a_{01} & a_{02} \\
    a_{10} & a_{11} & a_{12} \\
    a_{20} & a_{21} & a_{22}
    \end{matrix}
\right)
$$

+ 如 $|A| \ne 0$, 则 $A = Q \times R$
+ Q 正交矩阵, $QQ^T = Q^TQ = I$
    - |Q| = 1, 让 Q 表示 纯旋转
+ R 上三角矩阵

## 2、方法1：Gram–Schmidt 正交化

$$A = \left(
    \begin{matrix}
    a_{00} & a_{01} & a_{02} \\
    a_{10} & a_{11} & a_{12} \\
    a_{20} & a_{21} & a_{22}
    \end{matrix}
\right) = (\vec{a}_1, \vec{a}_2, \vec{a}_3)$$

$$
Q = \left(
    \begin{matrix}
    q_{00} & q_{01} & q_{02} \\
    q_{10} & q_{11} & q_{12} \\
    q_{20} & q_{21} & q_{22}
    \end{matrix}
\right) = (\vec{q}_1, \vec{q}_2, \vec{q}_3)
$$
 
$$
R = \left(
    \begin{matrix}
    r_{00} & r_{01} & r_{02} \\
    0 & r_{11} & r_{12} \\
    0 & 0 & r_{22}
    \end{matrix}
\right) = (\vec{r}_1, \vec{r}_2, \vec{r}_3)
$$

$$
A = (\vec{a}_1, \vec{a}_2, \vec{a}_3) = Q \times R = (Q \vec{r}_1, Q \vec{r}_2, Q\vec{r}_3)
$$

$$\left\{
    \begin{cases}
        \vec{a}_1 = Q \vec{r}_1 = (\vec{q}_1, \vec{q}_2, \vec{q}_3) \vec{r}_1\\ 
        \vec{a}_1 = Q \vec{r}_2 = (\vec{q}_1, \vec{q}_2, \vec{q}_3) \vec{r}_2\\
        \vec{a}_1 = Q \vec{r}_3 = (\vec{q}_1, \vec{q}_2, \vec{q}_3) \vec{r}_3
    \end{cases}
\right.$$

$$\left\{
    \begin{cases}
        \vec{a}_1 = r_{00} \vec{q}_1 \\ 
        \vec{a}_2 = r_{01} \vec{q}_1 + r_{11} \vec{q}_2 \\
        \vec{a}_3 = r_{02} \vec{q}_1 + r_{12} \vec{q}_2  + r_{22} \vec{q}_3 
    \end{cases}
\right.$$

例子: 求 $r_{01}$, 点乘 $\vec{q}_1$ 即可

$$\vec{a}_2 \cdot \vec{q}_1 = \vec{q}_1 \cdot (r_{01} \vec{q}_1 + r_{11} \vec{q}_2)$$

得到 R:

$$
R = \left(
    \begin{matrix}
    r_{00} & r_{01} & r_{02} \\
    0 & r_{11} & r_{12} \\
    0 & 0 & r_{22}
    \end{matrix}
\right) = \left(
    \begin{matrix}
    \vec{q}_1 \cdot \vec{a}_1 & \vec{q}_1 \cdot \vec{a}_2 & \vec{q}_1 \cdot \vec{a}_3 \\
    0 & \vec{q}_2 \cdot \vec{a}_2 & \vec{q}_2 \cdot \vec{a}_3 \\
    0 & 0 & \vec{q}_3 \cdot \vec{a}_3
    \end{matrix} 
\right)
$$

因为 Q 是正交阵, 所以 $(\vec{q}_1, \vec{q}_2, \vec{q}_3)$ 每个向量都是 单位向量, 而且 $\vec{q}_i \cdot \vec{q}_j = 0$

Gram–Schmidt 正交化，具体算法如下：

第1步：Q 第1列, R 第1行

+ $\vec{q}_1 = \vec{a}_1 / |\vec{a}_1|$
+ $r_{00} = \vec{q}_1 \cdot \vec{a}_1 = |\vec{a}_1|$
+ $r_{01} = \vec{q}_1 \cdot \vec{a}_2$
+ $r_{02} = \vec{q}_1 \cdot \vec{a}_3$

第2步：Q 第2列, R 第2行

+ $\vec{u}_2 = \vec{a}_2 - (\vec{a}_2 \cdot \vec{q}_1)\vec{q}_1$
+ $\vec{q}_2 = \vec{u}_2 / |\vec{u}_2|$
+ $r_{11} = \vec{q}_2 \cdot \vec{a}_2$
+ $r_{12} = \vec{q}_2 \cdot \vec{a}_3$

第3步：Q 第3列, R 第3行

+ $\vec{u}_3 = \vec{a}_3 - (\vec{a}_3 \cdot \vec{q}_1)\vec{q}_1 - (\vec{a}_3 \cdot \vec{q}_2)\vec{q}_2$
+ $\vec{q}_3 = \vec{u}_3 / |\vec{u}_3|$
+ $r_{22} = \vec{q}_3 \cdot \vec{a}_3$

第4步：让 |Q| 为 正数，总是代表 纯旋转

$|Q| = \frac{|A|}{|R|} = \frac{|A|}{r_{00} \times r_{11} \times r_{22}}\gt 0$, |A|, |R| 同号

如 $|A| \gt 0$ 选择正负

$\vec{q}_{1}$ 

使得 

$r_{1}$

均为 正数

如 $|A| \lt 0$ 选择正负 

$$
\vec{q}_{1}
$$ 

使得 

$$
r_{1}
$$

均为 负数

## 3、方法2：矩阵相乘

注: Q 是 正交阵, 则 $QQ^T=Q^TQ=I$

$$A = QR$$

$$A^TA = R^TQ^TQR = R^TR$$

令：

$$
R = \left(
    \begin{matrix}
    r_{00} & r_{01} & r_{02} \\
    0 & r_{11} & r_{12} \\
    0 & 0 & r_{22}
    \end{matrix}
\right)
$$

$$
A^TA = \left(
    \begin{matrix}
    a_{00} & a_{01} & a_{02} \\
    a_{10} & a_{11} & a_{12} \\
    a_{20} & a_{21} & a_{22}
    \end{matrix}
\right) = R^TR = \left(
    \begin{matrix}
    r_{00}^2 & r_{00} \times r_{01} & r_{00} \times r_{02} \\
    r_{00} \times r_{01} & r_{01}^2 + r_{11}^2 & r_{01} \times r_{02} + r_{11} \times r_{12} \\
    r_{00} \times r_{02} & r_{01} \times r_{02} + r_{11} \times r_{12} & r_{02}^2 + r_{12}^2 + r_{22}^2
    \end{matrix}
\right)
$$

解方程，得R的元素

$$\left\{
    \begin{cases}
        r_{00}^2 = a_{00} \\
        r_{01} = \frac{a_{01}}{r_{00}} \\ 
        r_{02} = \frac{a_{02}}{r_{00}} \\ 
        r_{11}^2 = a_{11} - r_{01}^2 \\ 
        r_{12} = \frac{a_{12}-r_{01}r_{02}}{r_{11}} \\ 
        r_{22}^2 = a_{22} - r_{02}^2 - r_{12}^2
    \end{cases}
\right.$$

$$
R = \left(
    \begin{matrix}
    r_{00} & \frac{a_{01}}{r_{00}} & \frac{a_{02}}{r_{00}} \\
    0 & r_{11} & \frac{a_{12} - r_{01}r_{02}}{r_{11}} \\
    0 & 0 & r_{22} \\
    \end{matrix}
\right)
$$

### 3.1、确定 平方根 的 正负

下面 $Q = R^{-1}A$, 求出来的 Q 的行列式 为正值

也就是 $|Q| = \frac{|A|}{|R|} \gt 0$, 则 |R| 与 |A| 同号

$|R| = r_{00} \times r_{11} \times r_{22}$

+ 如 |A| > 0, 则 $r_{00}, r_{11}, r_{22}$ 开平方根时，取 正值
+ 如 |A| < 0, 则 $r_{00}, r_{11}, r_{22}$ 开平方根时，取 负值
