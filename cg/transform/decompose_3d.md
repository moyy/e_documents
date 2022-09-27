- [3D 变换矩阵 分解](#3d-变换矩阵-分解)
  - [1、目的](#1目的)
  - [2、表示](#2表示)
    - [2.1、透视 Perspective](#21透视-perspective)
    - [2.2、平移 Translate](#22平移-translate)
    - [2.3、旋转 Rotate](#23旋转-rotate)
    - [2.4、错切 Shear / Skew](#24错切-shear--skew)
    - [2.5、缩放 Scale](#25缩放-scale)
  - [3、4*4 矩阵 分解](#344-矩阵-分解)
    - [3.1、概述](#31概述)
    - [3.2. 结论](#32-结论)
    - [3.3. 从 M 分解出 透视](#33-从-m-分解出-透视)
    - [3.4. 从 B 中分解出 平移](#34-从-b-中分解出-平移)
    - [3.5. 从 A 分解出 旋转](#35-从-a-分解出-旋转)
      - [3.5.1. 确定 平方根 的 正负](#351-确定-平方根-的-正负)
      - [3.5.2. 求 四元数](#352-求-四元数)
    - [3.6. 从 R 分解出 错切 和 缩放](#36-从-r-分解出-错切-和-缩放)
      - [3.6.1 求 S 的 逆矩阵](#361-求-s-的-逆矩阵)
      - [3.6.2 求 H 的 逆矩阵](#362-求-h-的-逆矩阵)
    - [3.7、从 S 分解出 反射 Rl](#37从-s-分解出-反射-rl)
  - [4、参考](#4参考)

# 3D 变换矩阵 分解

## 1、目的

将 3D 齐次坐标下，可逆的 线性变换（4*4矩阵），分解为 透视、平移、旋转、错切、缩放 的 组合

## 2、表示

### 2.1、透视 Perspective

$P(p_x, p_y, p_z, p_w)$

$$P = \left(
    \begin{matrix}
    1 & 0 & 0 & 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 1 & 0 \\
    p_x & p_y & p_z & p_w
    \end{matrix}
\right)$$

CSS 3D的 Perspective(d)

$$P = \left(
    \begin{matrix}
    1 & 0 & 0 & 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & -\frac{1}{d} & 1
    \end{matrix}
\right)$$

### 2.2、平移 Translate

$T(t_x, t_y, t_z)$

$$T = \left(
    \begin{matrix}
    1 & 0 & 0 & t_x \\
    0 & 1 & 0 & t_y \\
    0 & 0 & 1 & t_z \\
    0 & 0 & 0 & 1
    \end{matrix}
\right)$$

### 2.3、旋转 Rotate

+ 轴-角 表示 绕 单位向量 $\vec{r}$ 旋转 $\alpha$ 弧度
+ 欧拉角 表示 $\alpha_x, \beta_y, \gamma_z$
+ 单位四元数 表示 $R(q_x, q_y, q_z, q_w)$
+ 正交矩阵 表示 $Q$, 注 $QQ^T=I, |Q|=1$

### 2.4、错切 Shear / Skew

$H(h_x, h_y, h_z)$

$$H = \left(
    \begin{matrix}
    1 & h_x & h_y & 0 \\
    0 & 1 & h_z & 0 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1
    \end{matrix}
\right)$$

### 2.5、缩放 Scale

$S(x, y, z)$

$$S = \left(
    \begin{matrix}
    x & 0 & 0 & 0 \\
    0 & y & 0 & 0 \\
    0 & 0 & z & 0 \\
    0 & 0 & 0 & 1
    \end{matrix}
\right)$$

## 3、4*4 矩阵 分解

### 3.1、概述

向量(齐次坐标 表示), $\vec{v} = \{v_x, v_y, v_z, 1\}$

矩阵(齐次坐标 表示)

$$M = \left(
    \begin{matrix}
    m_{00} & m_{01} & m_{02} & m_{03} \\
    m_{10} & m_{11} & m_{12} & m_{13} \\
    m_{20} & m_{21} & m_{22} & m_{23} \\
    m_{30} & m_{31} & m_{32} & m_{33}
    \end{matrix}
\right)$$

变换后: $\vec{v_1} = M \vec{v}$

可逆的 （齐次坐标下的）线性变换矩阵 $M_{4 \times 4}$ 表示成:

$$M = P(p_x, p_y, p_z, p_w) \times T(t_x, t_y, t_z) \times R(q_x, q_y, q_z, q_w) \times H(h_x, h_y, h_z) \times S(x, y, z)$$

### 3.2. 结论

$$
A = A_{3 \times 3} = \left(
    \begin{matrix}
    m_{00} & m_{01} & m_{02} \\
    m_{10} & m_{11} & m_{12} \\
    m_{20} & m_{21} & m_{22}
    \end{matrix}
\right)
$$

+ 条件: $|A| \ne 0$ 且 $m_{33} \ne 0$
+ 前置: 将 M 的 每个元素 都 除以 $m_{33}$, 则 $m_{33}$ 变成 1
+ 第1步: 根据 3.5, 从 A 中 分解 R
+ 第2步: 根据 3.6, 从 R 中 分解 $R=H \times S$
+ 第3步: 根据 3.6.2, 得到 $R^{-1} = S^{-1} \times H^{-1}$
+ 第4步: 根据 3.5, 得到 $Q = R^{-1} \times A$, 从 Q 得到 四元数
+ 第5步: 根据 3.5, $A = Q \times R$, 得到 $A^{-1} = R^{-1} \times Q^{-1} = R^{-1} \times Q^T$
+ 第6步: 根据 3.3 得到 P
  - 如 $\vec{m_p} = \vec{0}$, 则 $(p_x,p_y,p_z,p_w)=(0, 0, 0, 1)$
  - 否则
      * $\vec{p} = (A^{-1})^T \times \vec{m_p}$
      * $p_w = m_{33} - dot(\vec{p}, \vec{m_t})$

### 3.3. 从 M 分解出 透视

$P(p_x, p_y, p_z, p_w)$

将 M 分成 如下 4部分

+ $A_{3 \times 3}$ 是 M 左上角 3*3 的 子矩阵
+ $\vec{m_t}$ 是 $\{m_{03}, m_{13}, m_{23}\}$ 组成的 列向量
+ $\vec{m_p}^T$ 是 $\{m_{30}, m_{31}, m_{32}\}$ 组成的 行向量
+ 最后是 右下角的 $m_{33}$

$$
A_{3 \times 3} = \left(
    \begin{matrix}
    m_{00} & m_{01} & m_{02} \\
    m_{10} & m_{11} & m_{12} \\
    m_{20} & m_{21} & m_{22}
    \end{matrix}
\right)
$$

+ 条件: $|A_{3 \times 3}| \ne 0$ 且 $m_{33} \ne 0$
+ 如 $\vec{m_p}^T = (0, 0, 0)$, 则 $(p_x, p_y, p_z, p_w) = (0, 0, 0,  m_{33})$
+ 否则，往下继续 计算

$$M = \left(
    \begin{matrix}
    A_{3 \times 3} & \vec{m_t} \\
    \vec{m_p}^T & m_{33}
    \end{matrix}
\right)$$


透视矩阵 P 也分为 4个部分, 注: $I_3$ 是 3*3 的 单位阵

$$P = \left(
    \begin{matrix}
    I_3 & \vec{0} \\
    \vec{p}^T & p_w
    \end{matrix}
\right)$$

矩阵 B，同样也分为 4部分：

$$B = \left(
    \begin{matrix}
    B_{3 \times 3} & \vec{b_t} \\
    \vec{0}^T & 1
    \end{matrix}
\right)$$

现在想要 $P \times B = M$

$$
\left(
    \begin{matrix}
    B_{3 \times 3} & \vec{b_t} \\
    \vec{p}^TB_{3 \times 3} & p_w + \vec{p}^T\vec{b_t}
    \end{matrix}
\right) = \left(
    \begin{matrix}
    A_{3 \times 3} & \vec{m_t} \\
    \vec{m_p}^T & m_{33}
    \end{matrix}
\right)
$$

则 

$$\left\{
    \begin{cases}
        B_{3 \times 3} = A_{3 \times 3} \\ 
        \vec{b_t} = \vec{m_t} \\ 
        \vec{p}^TB_{3 \times 3} = \vec{m_p}^T \\
        p_w + \vec{p}^T\vec{b_t} = m_{33}
    \end{cases}
\right.$$

$$\left\{
    \begin{cases}
        \vec{p} = (A^T)^{-1} \times \vec{m_p} \\
        p_w = m_{33} - dot(\vec{p}, \vec{m_t})
    \end{cases}
\right.$$

结论: $(p_x, p_y, p_z, p_w) = (\vec{p}, p_w)$

同时：

$$B = \left(
    \begin{matrix}
    A_{3 \times 3} & \vec{m_t} \\
    \vec{0}^T & 1
    \end{matrix}
\right)$$

### 3.4. 从 B 中分解出 平移 

$T(t_x, t_y, t_z)$

$$
\left(
    \begin{matrix}
    I_{3} & \vec{t} \\
    \vec{0}^T & 1
    \end{matrix}
\right) \times \left(
    \begin{matrix}
    C_{3 \times 3} & \vec{0} \\
    \vec{0}^T & 1
    \end{matrix}
\right) = \left(
    \begin{matrix}
    A_{3 \times 3} & \vec{m_t} \\
    \vec{0}^T & 1
    \end{matrix}
\right)
$$

$$\left\{
    \begin{cases}
        C_{3 \times 3} = A_{3 \times 3} \\ 
        \vec{t} = \vec{m_t}
    \end{cases}
\right.$$

结论: $(t_x, t_y, t_z) = (m_{03}, m_{13}, m_{23})$

同时：

$$C = \left(
    \begin{matrix}
    A_{3 \times 3} & \vec{0} \\
    \vec{0}^T & 1
    \end{matrix}
\right)$$

下面 就是 将 $A_{3 \times 3}$ 分解为 旋转，错切，缩放 的 组合

$$
A_{3 \times 3} = \left(
    \begin{matrix}
    m_{00} & m_{01} & m_{02} \\
    m_{10} & m_{11} & m_{12} \\
    m_{20} & m_{21} & m_{22}
    \end{matrix}
\right)
$$

### 3.5. 从 A 分解出 旋转

+ $A_{3 \times 3}$
+ $R(q_x, q_y, q_z, q_w)$
+ A = Q R
+ Q 正交阵，且 行列式为1（否则就不是 纯旋转）
    - $QQ^T=I$
+ R 上三角阵

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

#### 3.5.1. 确定 平方根 的 正负

$r_{00}, r_{11}, r_{22}$

目标: 使得 下面 $Q = R^{-1}A$, 求出来的 Q 的行列式 为正值

也就是 $|Q| = \frac{|A|}{|R|} \gt 0$, 则 |R| 与 |A| 同号

$|R| = r_{00} \times r_{11} \times r_{22}$

+ 如 |A| > 0, 则 $r_{00}, r_{11}, r_{22}$ 开平方根时，取 正值
+ 如 |A| < 0, 则 $r_{00}, r_{11}, r_{22}$ 开平方根时，取 负值

#### 3.5.2. 求 四元数

由 Q R = A 得到 $Q = R^{-1}A$

结论: 旋转矩阵 $Q_{3 \times 3}$ 转换成 四元数 $(q_x, q_y, q_z, q_w)$ 即为所求

### 3.6. 从 R 分解出 错切 和 缩放

+ 错切 $H(h_x, h_y, h_z)$
+ 缩放 $S(x, y, z)$ 

$$
R = \left(
    \begin{matrix}
    r_{00} & r_{01} & r_{02} \\
    0 & r_{11} & r_{12} \\
    0 & 0 & r_{22}
    \end{matrix}
\right) = \left(
    \begin{matrix}
    1 & \frac{r_{01}}{r_{11}} & \frac{r_{02}}{r_{22}} \\
    0 & 1 & \frac{r_{12}}{r_{22}} \\
    0 & 0 & 1
    \end{matrix}
\right) \left(
    \begin{matrix}
    r_{00} & 0 & 0 \\
    0 & r_{11} & 0 \\
    0 & 0 & r_{22}
    \end{matrix}
\right)
$$

结论

+ $(h_x, h_y, h_z) = (\frac{r_{01}}{r_{11}}, \frac{r_{02}}{r_{22}}, \frac{r_{12}}{r_{22}})$
+ $(x, y, z) = (r_{00}, r_{11}, r_{22})$

#### 3.6.1 求 S 的 逆矩阵

$$
S = \left(
    \begin{matrix}
    r_{00} & 0 & 0 \\
    0 & r_{11} & 0 \\
    0 & 0 & r_{22}
    \end{matrix}
\right)
$$

$$
S^{-1} = \left(
    \begin{matrix}
    \frac{1}{r_{00}} & 0 & 0 \\
    0 & \frac{1}{r_{11}} & 0 \\
    0 & 0 & \frac{1}{r_{22}}
    \end{matrix}
\right)
$$

#### 3.6.2 求 H 的 逆矩阵

$$
X = \left(
    \begin{matrix}
    1 & a & b \\
    0 & 1 & c \\
    0 & 0 & 1
    \end{matrix}
\right)
$$

$$
X^{-1} = \left(
    \begin{matrix}
    1 & -a & a \times c - b \\
    0 & 1 & -c \\
    0 & 0 & 1
    \end{matrix}
\right)
$$

得到 $H^{-1}$

$$
H = \left(
    \begin{matrix}
    1 & \frac{r_{01}}{r_{11}} & \frac{r_{02}}{r_{22}} \\
    0 & 1 & \frac{r_{12}}{r_{22}} \\
    0 & 0 & 1
    \end{matrix}
\right)
$$

$$
H^{-1} = \left(
    \begin{matrix}
    1 & -\frac{r_{01}}{r_{11}} & \frac{r_{01} \times r_{12}}{r_{11} \times r_{22}} - \frac{r_{02}}{r_{22}} \\
    0 & 1 & -\frac{r_{12}}{r_{22}} \\
    0 & 0 & 1
    \end{matrix}
\right)
$$

### 3.7、从 S 分解出 反射 Rl

$S = Rl \times S_1 = S_1 \times Rl$

$$\left(
    \begin{matrix}
    x & 0 & 0 \\
    0 & y & 0 \\
    0 & 0 & z
    \end{matrix}
\right) = \left(
    \begin{matrix}
    sign(x) & 0 & 0 \\
    0 & sign(y) & 0 \\
    0 & 0 & sign(z)
    \end{matrix}
\right) \left(
    \begin{matrix}
    |x| & 0 & 0 \\
    0 & |y| & 0 \\
    0 & 0 & |z|
    \end{matrix}
\right)$$

$$Rl = \left(
    \begin{matrix}
    sign(x) & 0 & 0 \\
    0 & sign(y) & 0 \\
    0 & 0 & sign(z)
    \end{matrix}
\right)$$

$$S_1 = \left(
    \begin{matrix}
    |x| & 0 & 0 \\
    0 & |y| & 0 \\
    0 & 0 & |z|
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
    x & 0 & 0 \\
    0 & y & 0 \\
    0 & 0 & z 
    \end{matrix}
\right) \left(
    \begin{matrix}
    a & 0 & 0 \\
    0 & b & 0 \\
    0 & 0 & c 
    \end{matrix}
\right) = \left(
    \begin{matrix}
    a & 0 & 0 \\
    0 & b & 0 \\
    0 & 0 & c
    \end{matrix}
\right) \left(
    \begin{matrix}
    x & 0 & 0 \\
    0 & y & 0 \\
    0 & 0 & z 
    \end{matrix}
\right) = \left(
    \begin{matrix}
    x \times a & 0 & 0 \\
    0 & y \times b & 0 \\
    0 & 0 & z \times c 
    \end{matrix}
\right)$$

## 4、参考

+ [4*4 矩阵分解](https://caff.de/posts/4X4-matrix-decomposition/decomposition.pdf)
+ [CSS 3D 变换插值](https://drafts.csswg.org/css-transforms-2/#funcdef-translate3d)
