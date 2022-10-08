- [运算符重载 std::ops & std::cmp](#运算符重载-stdops--stdcmp)
  - [1. 比较运算 Eq & Ord](#1-比较运算-eq--ord)
  - [2. 算术运算](#2-算术运算)
  - [3. 二进制运算](#3-二进制运算)
  - [4. 区间运算](#4-区间运算)
  - [5. 索引运算](#5-索引运算)
  - [6. 调用运算：函数 & 闭包](#6-调用运算函数--闭包)

# 运算符重载 std::ops & std::cmp

+ [std::ops](https://doc.rust-lang.org/std/ops/index.html)
+ [std::cmp](https://doc.rust-lang.org/std/cmp/index.html)


下面运算符，皆可通过实现相关的`trait`重载

## 1. 比较运算 Eq & Ord 

|Trait|约束|使用|
|--|--|--|
|[Eq](https://doc.rust-lang.org/std/cmp/trait.Eq.html)|[PartialEq](https://doc.rust-lang.org/std/cmp/trait.PartialEq.html)|==, !=|
|[Ord](https://doc.rust-lang.org/std/cmp/trait.Ord.html)|Eq + [PartialOrd](https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html)|<, <=, >, >=|

## 2. 算术运算

|Trait|约束|使用|
|--|--|--|
|[Neg](https://doc.rust-lang.org/std/ops/trait.Neg.html)||取反, -a|
|[Add & AddAssign](https://doc.rust-lang.org/std/ops/trait.Add.html)||加 a + b|
|[Sub & SubAssign](https://doc.rust-lang.org/std/ops/trait.Sub.html)||减 a - b|
|[Mul & MulAssign](https://doc.rust-lang.org/std/ops/trait.Mul.html)||乘 a * b|
|[Div & DivAssign](https://doc.rust-lang.org/std/ops/trait.Div.html)||除 a / b|
|[Rem & RemAssign](https://doc.rust-lang.org/std/ops/trait.Rem.html)||取余 a % b|
|[Shl & ShlAssign](https://doc.rust-lang.org/std/ops/trait.Shl.html)||左移 a << n|
|[Shr & ShrAssign](https://doc.rust-lang.org/std/ops/trait.Shr.html)||右移 a >> n|


## 3. 二进制运算

|Trait|约束|使用|
|--|--|--|
|[Not](https://doc.rust-lang.org/std/ops/trait.Not.html)||按位取反 !a|
|[BitAnd & BitAndAssign](https://doc.rust-lang.org/std/ops/trait.BitAnd.html)||按位与 a & b|
|[BitOr & BitOrAssign](https://doc.rust-lang.org/std/ops/trait.BitOr.html)||按位或 a | b|
|[BitXor & BitXorAssign](https://doc.rust-lang.org/std/ops/trait.BitXor.html)||按位异或 a ^ b|

## 4. 区间运算 

+ ..
+ a..
+ ..b
+ ..=c
+ d..e
+ f..=g

|Trait|约束|使用|
|--|--|--|
|[RangeBounds](https://doc.rust-lang.org/std/ops/trait.RangeBounds.html)|||

## 5. 索引运算

index，可以是任何编译时知道大小的类型

|Trait|约束|使用|
|--|--|--|
|[Index](https://doc.rust-lang.org/std/ops/trait.Index.html)||let a = &v[3]|
|[IndexMut](https://doc.rust-lang.org/std/ops/trait.IndexMut.html)||v[3] = 4;|

## 6. 调用运算：函数 & 闭包

|Trait|约束|使用|
|--|--|--|
|[fn, 非 trait](https://doc.rust-lang.org/std/primitive.fn.html)||函数指针类型，不是trait|
|[Fn](https://doc.rust-lang.org/std/ops/trait.Fn.html)|||
|[FnMut](https://doc.rust-lang.org/std/ops/trait.FnMut.html)|||
|[FnOnce](https://doc.rust-lang.org/std/ops/trait.FnOnce.html)|||