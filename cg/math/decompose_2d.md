- [2D 变换矩阵 分解](#2d-变换矩阵-分解)
  - [1、目的](#1目的)
  - [2、表示](#2表示)
    - [2.1、平移 Translate](#21平移-translate)
    - [2.2、反射 Refect](#22反射-refect)
    - [2.3、旋转 Rotate](#23旋转-rotate)
    - [2.4、缩放 Scale](#24缩放-scale)
    - [2.5、错切 Shear/Skew](#25错切-shearskew)
  - [3、矩阵分解](#3矩阵分解)
    - [3.1、概述](#31概述)
    - [3.2、结论](#32结论)
    - [3.3、平移分量](#33平移分量)
    - [3.4、QR 分解](#34qr-分解)
    - [3.5、从 S 分解出 反射 Rl](#35从-s-分解出-反射-rl)
  - [4、附录：弧度 和 度 的 转换](#4附录弧度-和-度-的-转换)
  - [5、参考](#5参考)

# 2D 变换矩阵 分解

## 1、目的

将 2D 仿射变换 的 齐次坐标 3*3矩阵，分解为 平移、旋转、错切、缩放 的 组合

## 2、表示

### 2.1、平移 Translate

$\vec{t}=\{t_x, t_y\}$

$$T = \left(
    \begin{matrix}
    1 & 0 & t_x \\
    0 & 0 & t_y \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

### 2.2、反射 Refect

$\vec{rl}=\{r_x, r_y\}$

**注:** $r_x, r_y \in \{1, -1\}$

$$Rl = \left(
    \begin{matrix}
    r_x & 0 & 0 \\
    0 & r_y & 0 \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

### 2.3、旋转 Rotate

$\theta$

+ **单位：** $\theta$ 的单位：弧度
  - 逆时针, $\theta$ 为 正数
  - 顺时针, $\theta$ 为 负数

$$R = \left(
    \begin{matrix}
    cos(\theta) & -sin(\theta) & 0 \\
    sin(\theta) & cos(\theta) & 0 \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

### 2.4、缩放 Scale

$\vec{s}=\{x, y\}$

**注：** $x, y$ 均为 非负数

$$S = \left(
    \begin{matrix}
    x & 0 & 0 \\
    0 & y & 0 \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

### 2.5、错切 Shear/Skew

$\vec{h}=\{h_x, h_y\}$

+ **单位: ** $h_x, h_y$ 单位是 弧度
+ **作用: ** 将 矩阵 变 平行四边形

$$H = \left(
    \begin{matrix}
    1 & tan(h_x) & 0 \\
    tan(h_y) & 1 & 0 \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

## 3、矩阵分解

### 3.1、概述

向量(齐次坐标 表示), $\vec{v} = \{v_x, v_y, 1\}$

矩阵(齐次坐标 表示)

$$M = \left(
    \begin{matrix}
    a & c & e \\
    b & d & f \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

变换后: $\vec{v_1} = M \vec{v}$

任何 2D 可逆变换 的 齐次矩阵  $M_{3 \times 3}$，可以表示成：

$$M = T(t_x, t_y) \times R(\theta) \times H(h_x, 0) \times S(x, y) $$

### 3.2、结论

已知

$$M = \left(
    \begin{matrix}
    a & c & e \\
    b & d & f \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

则: $M = T(t_x, t_y) \times R(\theta) \times H(h_x, 0) \times S(x, y)$

|变换|表达|值|说明|
|--|--|--|--|
|平移|$T(t_x, t_y)$|$t_x=e; t_y=f$||
|旋转|$R(\theta)$|$\theta$ = Math.atan2(b, a)|$\theta \in [-\pi, \pi)$|
|错切|$H(h_x, 0)$|$h_x=Math.atan(\frac{a \times c + b \times d}{a \times d - b \times c})$|$h_x \in (-\frac{\pi}{2}, \frac{\pi}{2})$|
|缩放|$S(x, y)$|$x = \sqrt{a^2 + b^2}; y = \frac{a \times d - b \times c}{x}$|x > 0|

### 3.3、平移分量

$T(t_x, t_y)$

+ $t_x = e$
+ $t_y = f$

### 3.4、QR 分解

已知:

$$M = \left(
    \begin{matrix}
    a & c \\
    b & d \\
    \end{matrix}
\right)$$

$M_1 = R(\theta) \times S(x, y) \times H(h_x, 0)$

令 $s = tan(h_x)$ 则：

$$\left(
    \begin{matrix}
    a & c \\
    b & d \\
    \end{matrix}
\right) = \left(
    \begin{matrix}
    cos\theta & -sin\theta \\
    sin\theta & cos\theta \\
    \end{matrix}
\right) \left(
    \begin{matrix}
    1 & s \\
    0 & 1 \\
    \end{matrix}
\right) \left(
    \begin{matrix}
    x & 0 \\
    0 & y \\
    \end{matrix}
\right) = \left(
    \begin{matrix}
    x \times cos\theta & s \times y \times cos\theta - y \times sin\theta \\
    x \times sin\theta & s \times y \times sin\theta + y \times cos\theta\\
    \end{matrix}
\right)$$

则: 

$$\left\{
    \begin{cases}
        a = x \times cos\theta \\ 
        b = x \times sin\theta \\ 
        c = s \times y \times cos\theta - y \times sin\theta \\
        d = s \times y \times sin\theta + y \times cos\theta
    \end{cases}
\right.$$

2式 / 1式, 得: $tan\theta = \frac{b}{a}$

所以: **$\theta = Math.atan2(b, a)$**

由 1式, 2式, 得: $x^2 = a^2 + b^2$

由 [这篇文章](./atan.md)，当 $\theta$ 取 Math.atan2(b, a)值时，x 永远是 正数，故 $x = \sqrt{a^2 + b^2}$

由 2式，4式，得：

$$
\left\{ 
    \begin{cases}
        y \times (s \times cos\theta - sin\theta) = c \\ 
        y \times (s \times sin\theta + cos\theta) = d
    \end{cases}
\right.
$$

$$\frac{s \times cos\theta - sin\theta}{s \times sin\theta + cos\theta} = \frac{c}{d}$$

则: $s = \frac{a \times c + b \times d}{a \times d - b \times c}$

代入 4式, 得: $y = \frac{a \times d - b \times c}{x}$

### 3.5、从 S 分解出 反射 Rl

$S = Rl \times S_1 = S_1 \times Rl$

$$\left(
    \begin{matrix}
    x & 0 \\
    0 & y \\
    \end{matrix}
\right) = \left(
    \begin{matrix}
    sign(x) & 0 \\
    0 & sign(y) \\
    \end{matrix}
\right) \left(
    \begin{matrix}
    |x| & 0 \\
    0 & |y| \\
    \end{matrix}
\right)$$

$$Rl = \left(
    \begin{matrix}
    sign(x) & 0 \\
    0 & sign(y) \\
    \end{matrix}
\right)$$

$$S_1 = \left(
    \begin{matrix}
    |x| & 0 \\
    0 & |y| \\
    \end{matrix}
\right)$$

其中: $sign(a)$ 是 符号函数，取 a 的 正负号

$$sign(a) = \begin{cases}
    1, x \gt 0 \\
    0, x = 0 \\
    -1, x \lt 0
\end{cases}$$

**注记1**: $S1$ 的 对角线元素都是 非负数

**注记2**: $Rl$ 的 对角线元素都是 1，-1

**注记3**: 对角阵 满足 乘法交换律

$$\left(
    \begin{matrix}
    x & 0 \\
    0 & y \\
    \end{matrix}
\right) \left(
    \begin{matrix}
    a & 0 \\
    0 & b \\
    \end{matrix}
\right) = \left(
    \begin{matrix}
    a & 0 \\
    0 & b \\
    \end{matrix}
\right) \left(
    \begin{matrix}
    x & 0 \\
    0 & y \\
    \end{matrix}
\right) = \left(
    \begin{matrix}
    x \times a & 0\\
    0 & y \times b \\
    \end{matrix}
\right)$$

## 4、附录：弧度 和 度 的 转换

+ 度 变 弧度 $\theta = \pi \frac{x\degree}{180\degree}$
+ 弧度 变 度 $x\degree=180\degree\frac{\theta}{\pi}$

## 5、参考

+ [CSS 2D 变换插值](https://drafts.csswg.org/css-transforms/#mathematical-description)