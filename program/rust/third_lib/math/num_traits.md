- [num_traits crate](#num_traits-crate)
  - [1. 基础](#1-基础)
  - [2. 转换](#2-转换)
  - [3. 主要](#3-主要)
    - [3.1. Real 数学函数 封装](#31-real-数学函数-封装)
    - [3.2. Float](#32-float)

# [num_traits crate](https://docs.rs/num-traits/latest/num_traits/)

## 1. 基础

|Trait|约束|作用|
|--|--|--|
|[Zero](https://docs.rs/num-traits/0.2.12/num_traits/identities/trait.Zero.html)||Zero::zero()|
|[One](https://docs.rs/num-traits/0.2.12/num_traits/identities/trait.One.html)||One::one()|
|[Signed / Unsigned](https://docs.rs/num-traits/latest/num_traits/sign/trait.Signed.html)||正负号相关|
|[Bounded](https://docs.rs/num-traits/latest/num_traits/bounds/trait.Bounded.html)||最大 最小 值|
|[NumOps](https://docs.rs/num-traits/0.2.12/num_traits/trait.NumOps.html)|Add + Sub + Mul + Div + Rem|四则运算|
|[FloatCore](https://docs.rs/num-traits/latest/num_traits/float/trait.FloatCore.html)|Num + NumCast + Copy + Neg + PartialOrd|最基本的浮点方法，非 std 也可用|

## 2. 转换

|cast trait|方法|例子|
|--|--|--|
|[AsPrimitive < T >](https://docs.rs/num-traits/0.2.12/num_traits/cast/trait.AsPrimitive.html)|T 是 基本数字类型|let t = self.as_()|
|[`FromPrimitive`](https://docs.rs/num-traits/0.2.12/num_traits/cast/trait.FromPrimitive.html)|一堆 from_i8() -> Option< Self >|
|[ToPrimitive](https://docs.rs/num-traits/0.2.12/num_traits/cast/trait.ToPrimitive.html)|一堆 to_i8() -> Option< i8 >|
|[`NumCast`](https://docs.rs/num-traits/0.2.12/num_traits/cast/trait.NumCast.html)|Sized + ToPrimitive|数字转换|

## 3. 主要

|trait|约束|作用|
|--|--|--|
|[`Num`](https://docs.rs/num-traits/latest/num_traits/trait.Num.html)|NumOps + Zero + One + PartialEq|数字|
|[`Real`](https://docs.rs/num-traits/latest/num_traits/real/trait.Real.html)|Num + NumCast + Copy + Neg + PartialOrd|数学上的 实数的 函数，std 可用|
|[`FloatConst`](https://docs.rs/num-traits/latest/num_traits/float/trait.FloatConst.html)|Num + NumCast + Copy + Neg + PartialOrd|浮点 常量|
|[`Float`](https://docs.rs/num-traits/latest/num_traits/float/trait.Float.html)|Num + NumCast + Copy + Neg + PartialOrd|浮点，除了Real的所有方法，还加上 ieee754 特征，std 可用|
|[`PrimInt`](https://docs.rs/num-traits/latest/num_traits/int/trait.PrimInt.html)|Num + NumCast + Copy + Sized + Bounded + PartialOrd + Ord + Eq + 位运算 + check/saturating 四则运算|整数|

### 3.1. [Real](https://docs.rs/num-traits/latest/num_traits/real/trait.Real.html) 数学函数 封装

纯数学上的 实数 的 数学运算，看不到 浮点数 的 特征（如：NaN 和 Inf）

### 3.2. [Float](https://docs.rs/num-traits/latest/num_traits/float/trait.Float.html)

Float 除了 Real的所有方法外，还包括 表示数特征的方法，如下：

|方法|说明|
|--|--|
|integer_decode|将 f 分成 3个部分（符号，指数，位数）|
|classify|
|nan|
|is_nan|
|is_normal|
|neg_zero| 
|is_finite|
|infinity| 
|neg_infinity| 
|is_infinite|