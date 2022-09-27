- [CSS Transform2D 动画插值](#css-transform2d-动画插值)
  - [1、操作](#1操作)
    - [1.1、基本变换](#11基本变换)
    - [1.2、衍生变换](#12衍生变换)
  - [2、变换序列](#2变换序列)
    - [2.1、情况1：操作序列 全相同](#21情况1操作序列-全相同)
    - [2.2、情况2：前面的 操作序列 相同，但是 有一个短](#22情况2前面的-操作序列-相同但是-有一个短)
    - [2.3、情况3：前面的 操作序列 不尽相同](#23情况3前面的-操作序列-不尽相同)
  - [3、不可逆](#3不可逆)
  - [4、序列插入 和 变为矩阵插入的 差异](#4序列插入-和-变为矩阵插入的-差异)
  - [5、矩阵 插值](#5矩阵-插值)
    - [5.1、矩阵 分解](#51矩阵-分解)
    - [5.2、矩阵 插值](#52矩阵-插值)
  - [6、3D 变换 插值](#63d-变换-插值)
    - [6.1、4 * 4 矩阵分解](#614--4-矩阵分解)
    - [6.2、插值](#62插值)
    - [6.3、将 P * T * R * S * K 变回 矩阵](#63将-p--t--r--s--k-变回-矩阵)

- [CSS Transform2D 动画插值](#css-transform2d-动画插值)
  - [1、操作](#1操作)
    - [1.1、基本变换](#11基本变换)
    - [1.2、衍生变换](#12衍生变换)
  - [2、变换序列](#2变换序列)
    - [2.1、情况1：操作序列 全相同](#21情况1操作序列-全相同)
    - [2.2、情况2：前面的 操作序列 相同，但是 有一个短](#22情况2前面的-操作序列-相同但是-有一个短)
    - [2.3、情况3：前面的 操作序列 不尽相同](#23情况3前面的-操作序列-不尽相同)
  - [3、不可逆](#3不可逆)
  - [4、序列插入 和 变为矩阵插入的 差异](#4序列插入-和-变为矩阵插入的-差异)
  - [5、矩阵 插值](#5矩阵-插值)
    - [5.1、矩阵 分解](#51矩阵-分解)
    - [5.2、矩阵 插值](#52矩阵-插值)
  - [6、3D 变换 插值](#63d-变换-插值)
    - [6.1、4 * 4 矩阵分解](#614--4-矩阵分解)
    - [6.2、插值](#62插值)
    - [6.3、将 P * T * R * S * K 变回 矩阵](#63将-p--t--r--s--k-变回-矩阵)

# CSS Transform2D 动画插值

## 1、操作

+ 变换的原点：默认在 物体 中心点，可以改变
+ 右结合
  - 比如 R(30°) T(tx, ty)；固定坐标系下，物体 先做 T，在做 R

### 1.1、基本变换

|变换|作用|恒等|逆变换|
|--|--|--|--|
|R(r°)|旋转 a度（a>0 逆时针；否则顺时针）|R(0°)|R(-r°)|
|T(tx, ty)|平移 (x, y)|T(0, 0)|T(-tx, -ty)|
|S(x, y)|缩放 (x, y)|S(1, 1)|S(1/x, 1/y)|
|Skew(s°, t°)|斜切变换，变换值分别为 tan(s°), tan(t°)|Skew(0°, 0°)|Skew(-s°, -t°)|
|matrix(a, b, c, d, e, f)|3*3的矩阵，从上往下，每一行依次为：(a, c, e), (b, d, f), (0, 0, 1)|matrix(1, 0, 0, 1, 0, 0)|根据公式求|

### 1.2、衍生变换

+ 默认值
  - translate(12px) 等价于 translate(12px, 0px);
  - scale(3) 等价于 scale(3, 1);
+ 特殊版本
  - translateY(12px) 等价于 translate(0px, 12px);
+ 单位不同
  - translate(12px) 和 translate(20%)

## 2、变换序列

from, to 两个关键帧 序列 如下：

+ from = F1, F2, F3, ...
+ to = T1, T2, T3, ...

### 2.1、情况1：操作序列 全相同

+ from = R1 S1 T1 R1 S1
+ to   = R2 S2 T2 R2 S2

这里的全相同，指得是：参数个数，版本名称，单位 都 完全相同

如果 仅仅是 种类一样，处理如下：

+ 第1种：变换名 不同，比如 TranslateX(2px) 和 Translate(10px)，将 TranslateX(2px) 变成 Translate(2px, 0px)
+ 第2种：参数个数 不同，比如 Translate(2px) 和 Translate(10px, 5px)，将 Translate(2px) 变成 Translate(2px, 0px)
+ 第3种：单位不同，比如 Translate(2px) 和 Translate(10%)，将 Translate(10%) 变成 Translate(? px)

得到：参数个数，版本名称，单位 都 完全相同 的 序列

然后 对每个操作 独立插值，得到

+ r = R S T R S，作用到 动画中即可；

### 2.2、情况2：前面的 操作序列 相同，但是 有一个短

+ from = R S T R S
+ to   = R S T R

则 to 最后一个补上 一个恒等操作，就变成 `情况1`

### 2.3、情况3：前面的 操作序列 不尽相同

+ from = R S T R S
+ to   = R S R T

前两个操作相同，第三个操作开始不同：

注：对 R，S，T，Skew 如何 变矩阵，[请参考这里](../transform/decompose_2d.md)

+ 将 from 的 最后三个 操作相乘，变成 矩阵 T R S = M
+ 将 to 的 最后两个 操作相乘，变成 矩阵 R T = M

结果 回到 `情况1`，如下：

+ from = R S M
+ to   = R S M

## 3、不可逆

+ 当 变换成 2.4 的 `情况1` 之后，对于 matrix(a, b, c, d, e, f)，如 a * d - b * c = 0，则 不 播 放 动 画
+ 插值后的操作序列，以下条件成立，则 这一帧 不 渲 染 物体：
    - Scale(x, y) 的 x = 0 或者 y = 0
    - matrix(a, b, c, d, e, f) 的 a * d - b * c = 0

## 4、序列插入 和 变为矩阵插入的 差异

下面情况下，效果不对等：

+ 2.1 情况1 下，对于 Scale(0, 0)，可以插值；变矩阵后矩阵不可分解，就无法做动画插值；
+ 2.1 情况1 下，对于 Rotate(1000°)，对Rotate插值，能转好几圈；但变成矩阵后，就 只能转 0-360°

## 5、矩阵 插值

+ 输入：
  - from = M
  -   to = M
+ 输出：
  - 插值后的矩阵 matrix(a, b, c, d, e, f)

### 5.1、矩阵 分解

参考：[推导见这里](../transform/decompose_2d.md)

输入：matrix(a, b, c, d, e, f) 

$$M = \left(
    \begin{matrix}
    a & c & e \\
    b & d & f \\
    0 & 0 & 1 \\
    \end{matrix}
\right)$$

条件: $(a \times d - b \times c) \ne 0$

输出: 6 个 互相独立的数字 $(t_x, t_y, \theta, h_x, x, y)$

意义: $M = T(t_x, t_y) \times R(\theta) \times H(h_x, 0) \times S(x, y)$

|变换|表达|值|说明|
|--|--|--|--|
|平移|$T(t_x, t_y)$|$t_x=e; t_y=f$||
|旋转|$R(\theta)$|$\theta$ = Math.atan2(b, a)|$\theta \in [-\pi, \pi)$|
|错切|$H(h_x, 0)$|$h_x=Math.atan(\frac{a \times c + b \times d}{a \times d - b \times c})$|$h_x \in (-\frac{\pi}{2}, \frac{\pi}{2})$|
|缩放|$S(x, y)$|$x = \sqrt{a^2 + b^2}; y = \frac{a \times d - b \times c}{x}$|x > 0|

### 5.2、矩阵 插值

操作：

+ 1、分解
  - $from = $t_{x1}, t_{y1}, r_1, h_{x1}, x_1, y_1$
  - $to = $t_{x2}, t_{y2}, r_2, h_{x2}, x_2, y_2$
+ 2、逐个数字 依次 插值
  - $r = t_x, t_y, r, h_x, x, y$
+ 3、将 r 还原为 矩阵 matrix(a, b, c, d, e, f)

令 $s=tan(h_x)$

$$
\left(
    \begin{matrix}
    x \times cos\theta & s \times y \times cos\theta - y \times sin\theta \\
    x \times sin\theta & s \times y \times sin\theta + y \times cos\theta\\
    \end{matrix}
\right)
$$

## 6、3D 变换 插值

### 6.1、4 * 4 矩阵分解

参考：[推导见这里](../transform/decompose_3d.md)

用 QR变换 分解为 P * T * R * S * K

+ Translation = (tx, ty, tz)
+ Scale = (x, y, z)
+ sKew = (sx, sy, sz)
+ Perspective = (px, py, pz, pw)
+ R(quaternion) = (qx, qy, qz, qw)

### 6.2、插值

from = P1 * T1 * R1 * S1 * K1

to = P2 * T2 * R2 * S2 * K2

+ R的插值：对 四元数，用 线性代数库 的 quaternion.slerp 进行 **球面插值** 得到 结果
+ 其他按 每数字 独立插值 即可
+ 得到 插值后的 P * T * R * S * K

### 6.3、将 P * T * R * S * K 变回 矩阵