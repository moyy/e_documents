# 2D 变换矩阵 分解

## 1、目的

将 2D 仿射变换 的 齐次坐标 3*3矩阵，分解为 平移、反射、旋转、缩放、错切 的 组合

## 2、表示

### 2.1、平移 Translate $\vec{t}=\\{t_x, t_y\\}$

$$T = \left(
    \begin{matrix}
    1 & 0 & t_x \\
    0 & 0 & t_y \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

### 2.2、反射 Refect $\vec{rl}=\\{r_x, r_y\\}$

**注：** $r_x, r_y \in \\{1, -1\\}$

$$Rl = \left(
    \begin{matrix}
    r_x & 0 & 0 \\
    0 & r_y & 0 \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

### 2.3、旋转 Rotate $\theta$

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

### 2.4、缩放 Scale $\vec{s}=\\{x, y\\}$

**注：** $x, y$ 均为 非负数

$$S = \left(
    \begin{matrix}
    x & 0 & 0 \\
    0 & y & 0 \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

### 2.5、错切 Shear/Skew,  $\vec{h}=\\{h_x, h_y\\}$

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

向量(齐次坐标 表示), $\vec{v} = \\{v_x, v_y, v_z, 1\\}$

矩阵(齐次坐标 表示), $$M = \left(
    \begin{matrix}
    a & c & e \\
    b & d & f \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

变换后: $v_1 = M \vec{v}$

任何 2D 可逆变换 的 齐次矩阵  $M_{3 \times 3}$，可以表示成：

$$M = T(t_x, t_y) \times R(\theta) \times Rl(r_x, r_y) \times S(x, y) \times H(h_x, 0)$$

### 3.2、结论

已知

$$M = \left(
    \begin{matrix}
    a & c & e \\
    b & d & f \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

|变换|表达|值|说明|
|--|--|--|--|
|平移|$T(t_x, t_y)$|$t_x=e$ $t_y=f$|
|旋转|$R(\theta)$|$\theta$ = Math.atan2(b, a)|
|缩放|$S(x, y)$|$x = \sqrt{a^2 + b^2}; y = \frac{a \times d - b \times c}{x}$|见 3.5，x，y 可正可负，可以进一步分解为：反射 和 纯缩放|
|错切|sHear(h_x, 0), $s=tan(h_x)$|$s=\frac{a \times c + b \times d}{x^2}$|

### 3.3、平移分量 $T(t_x, t_y)$

+ $$t_x = e$$
+ $$t_y = f$$

### 3.4、QR 分解

已知：

$$M_1 = \left(
    \begin{matrix}
    a & c \\
    b & d \\
    \end{matrix}
\right)$$

$$M_1 = R(\theta) \times S(x, y) \times H(h_x, 0)$$

令 $$s = tan(h_x)$$ 则：

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
    x & 0 \\
    0 & y \\
    \end{matrix}
\right)\left(
    \begin{matrix}
    1 & s \\
    0 & 1 \\
    \end{matrix}
\right) = \left(
    \begin{matrix}
    x \times cos\theta & s \times x \times cos\theta - y \times sin\theta \\
    x \times sin\theta & s \times x \times sin\theta + y \times cos\theta\\
    \end{matrix}
\right)$$

则：

$$
\left\{ 
\begin{array}{c}
    a = x \times cos\theta \\ 
    b = x \times sin\theta \\ 
    c = s \times x \times cos\theta - y \times sin\theta \\
    d = s \times x \times sin\theta + y \times cos\theta
\end{array}
\right. 
$$

由 1式，2式，得：$$x = \sqrt{a^2 + b^2}$$

2式 / 1式，得：$$tan\theta = \frac{b}{a}$$

所以：**$$\theta = arctan\frac{b}{a}$$**

由 1式，3式，得：

$$
\left\{ 
\begin{array}{c}
    c = s \times a - y \times sin\theta \\ 
    d = s \times b + y \times cos\theta
\end{array}
\right. 
$$

$$
\left\{ 
\begin{array}{c}
    y \times sin\theta = s \times a - c \\ 
    y \times cos\theta = d - s \times b
\end{array}
\right. 
$$

$$
\frac{s \times a - c}{d - s \times b} = \frac{sin\theta}{cos\theta} = tan\theta = \frac{b}{a}
$$

则：

$$
s = \frac{a \times c + b \times d}{a^2 + b^2} = \frac{a \times c + b \times d}{x^2}
$$

5式，6式，得：

$$
    y^2 = (s \times a - c)^2 + (d - s \times b)^2
$$

$$
    y = \frac{a \times d - b \times c}{\sqrt{a^2+b^2}} = 
    \frac{a \times d - b \times c}{\sqrt{a^2+b^2}}
$$

### 3.5、从 S 分解出 Rl：$S = Rl \times S_1$

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

其中：$sign(a)$ 是 符号函数，取 a 的 正负号

$$sign(a) = \begin{cases}
    1, x \gt 0 \\
    0, x = 0 \\
    -1, x \lt 0
\end{cases}$$

**注记1**：$S1$ 的 对角线元素都是 非负数

**注记2**：$Rl$ 的 对角线元素都是 1，-1

**注记3**：对角阵 满足 乘法交换律

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
