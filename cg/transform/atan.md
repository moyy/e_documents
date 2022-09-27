- [反正切函数 atan](#反正切函数-atan)
  - [1. y = Math.atan(x)](#1-y--mathatanx)
  - [2. Math.atan2(y, x)](#2-mathatan2y-x)
  - [3. 应用](#3-应用)
    - [3.1. 用 Math.atan](#31-用-mathatan)
    - [3.2. 用 Math.atan2](#32-用-mathatan2)

# 反正切函数 atan

## 1. y = Math.atan(x)

+ 定义域: $(-\infty, \infty)$
+ 值域: $(-\frac{\pi}{2}, \frac{\pi}{2})$
+ x < 0, y < 0
+ x > 0, y > 0
+ x = 0, y = 0

## 2. Math.atan2(y, x)

$\theta$ = Math.atan2(y, x)

+ 定义域: $x, y \in (-\infty, \infty)$
+ 值域: $z \in (-\pi, \pi)$
+ x > 0, y > 0, 第1象限, $\theta \in (0, \frac{\pi}{2})$
+ x < 0, y > 0, 第2象限, $\theta \in (\frac{\pi}{2}, \pi)$
+ x < 0, y < 0, 第3象限, $\theta \in (-\pi, -\frac{\pi}{2})$
+ x > 0, y < 0, 第4象限, $\theta \in (-\frac{\pi}{2}, 0)$
+ x = 0, y > 0, $\theta = \frac{\pi}{2}$
+ x = 0, y < 0, $\theta = -\frac{\pi}{2}$
+ x > 0, y = 0, $\theta = 0$
+ x < 0, y = 0, $\theta = \pi$

## 3. 应用

已知: $a \times b \ne 0$, 求 x, $\theta$

$$\left\{
    \begin{cases}
        x \times cos\theta = a\\ 
        x \times sin\theta = b\\ 
    \end{cases}
\right.$$

+ $x^2=a^2+b^2$
+ $tan\theta = \frac{b}{a}$

x的正负号 取决于 如何求 $\theta$

### 3.1. 用 Math.atan

$\theta = Math.atan(\frac{b}{a})$, $\theta \in  (-\frac{\pi}{2}, \frac{\pi}{2})$

+ $b = 0$, $\theta = 0$, $cos\theta = 1$, x = a
+ $a \times b$ 异号, $\theta \lt 0$, $sin\theta \lt 0$, x 与 b 异号
+ $a \times b$ 同号, $\theta \gt 0$, $sin\theta \gt 0$, x 与 b 同号

### 3.2. 用 Math.atan2

$\theta = Math.atan2(b, a)$, $\theta \in  (-\pi, \pi]$

如下表，枚举所有情况，x > 0

|a|b|$\theta$|$cos\theta$|$sin\theta$|x|
|--|--|--|--|--|--|
|> 0|> 0|$(0, \frac{\pi}{2})$|> 0|> 0|> 0|
|< 0|> 0|$(\frac{\pi}{2}, \pi)$|< 0|> 0|> 0|
|< 0|< 0|$(-\pi, -\frac{\pi}{2})$|< 0|< 0|> 0|
|> 0|< 0|$(-\frac{\pi}{2}, 0)$|> 0|< 0|> 0|
|= 0|> 0|$\frac{\pi}{2}$|0|1|> 0|
|= 0|< 0|$-\frac{\pi}{2}$|0|-1|> 0|
|> 0|= 0|0|1|0|> 0|
|< 0|= 0|$\pi$|-1|0|> 0|
