# [approx crate](https://docs.rs/approx/0.5.1/approx/index.html)

## 作用

主要作用: 判断 f32/f64 近似相等

3种 方法

+ 绝对误差
+ 相对误差
+ `Ulp`: Units in the Last Place

### 1.1 绝对误差

+ a, b 是 f32/f64
+ epsilon 是 比较 的 最小值
+ 公式: $|a - b| \le epsilon$

缺点：浮点数 越大，最小精度 就会越大，因为 指数越大，尾数 对应的误差 就会 放大 到 2的指数次冥。这导致 无法 根据 同一个 epsilon 去判断 所有范围的 浮点数 比较的情况

### 1.2. 相对误差

+ a, b 是 f32/f64
+ epsilon 是 比较 的 最小值
+ epsilon 是 比较 的 最小值
+ max_relative 比值 最大的 数字，(0,1)
+ 公式: $\frac{|a - b|}{max(|a|, |b|)|} \le max\_relative$

### 1.3. `Ulps`

`Ulps`: units in the last place

思路：利用 ieee754标准，从 f32/f64 得到 对应的 二进制 内存编码，然后利用 这个整数 进行判断 

+ 默认 max_ulps = 4
+ |a.to_int() - b.to_int()| < max_ulps

参考: [比较浮点数](https://randomascii.wordpress.com/2012/02/25/comparing-floating-point-numbers-2012-edition/)

## 2. trait

| trait      | 约束      | 说明         |
| ---------- | --------- | ------------ |
| AbsDiffEq  | PartialEq | 绝对误差差的 |
| RelativeEq | AbsDiffEq | 相对误差     |
| UlpsEq     | AbsDiffEq | `ULP`        |

## 3. 宏

+ 绝对误差
  - abs_diff_eq!(1.0, 1.0);
  - abs_diff_eq!(1.0, 1.0, epsilon = f64::EPSILON);
+ 相对误差
  - relative_eq!(1.0, 1.0);
  - relative_eq!(1.0, 1.0, epsilon = f64::EPSILON, max_relative = 1.0);
+ Ulps误差
  - ulps_eq!(1.0, 1.0);
  - ulps_eq!(1.0, 1.0, epsilon = f64::EPSILON, max_ulps = 4);
  - assert_ulps_eq!(1.0, 1.0);

## 4. AbsDiffEq 的 f32 实现

+ |a - b| <= max_relative

``` rs
fn abs_diff_eq(a: &f32, b: &f32, epsilon: f32) -> bool {
    f32::abs(a - b) <= epsilon
}
```

## 5. RelativeEq 的 f32 实现

+ |a - b| / max(|a|, |b|) <= max_relative

``` rs
fn relative_eq(a: &f32, b: &f32, epsilon: f32, max_relative: f32) -> bool {
    if a == b {
        return true;
    }

    // 无穷，肯定不等
    if f32::is_infinite(*a) || f32::is_infinite(*b) {
        return false;
    }

    let abs_diff = f32::abs(a - b);

    // 判断: 绝对误差
    if abs_diff <= epsilon {
        return true;
    }

    abs_diff <= max_relative * f32::max(f32::abs(*b), f32::abs(*a));
}
```

## 6. [UlpsEq 的 f32 实现](https://randomascii.wordpress.com/2012/02/25/comparing-floating-point-numbers-2012-edition/)

`Ulps`: units in the last place

+ 默认 max_ulps = 4
+ |a.to_int() - b.to_int()| < max_ulps

``` rs
fn ulps_eq(a: &f32, b: &f32, epsilon: f32, max_ulps: u32) -> bool {
    // 判断 绝对误差
    if f32::abs_diff_eq(a, b, epsilon) {
        return true;
    }

    // 判断 正负号
    if self.signum() != b.signum() {
        return false;
    }

    // 变 整数
    let a: u32 = a.to_bits();
    let b: u32 = b.to_bits();

    if a <= b {
        b - a <= max_ulps as u32
    } else {
        a - b <= max_ulps as u32
    }
}
```