# 类型系统 术语：型变

型变：分为 协变、不变、逆变

+ `covariance` 协变: 如果 A 是 B 的 SubType，那么 Wrapper< A > 是 Wrapper< B > 的 SubType。
+ `invariance` 不变：如果 A 是 B 的 SubType，那么 Wrapper< A > 和 Wrapper< B > 没有任何关系。
+ `contravariance` 逆变：如果 A 是 B的SubType，那么 Wrapper< B > 是 Wrapper< A > 的 SubType。

## 1. Rust中 的 型变

### 1.1. 生命周期

+ 'long  是 'short 的 子类型，写作 'long:'short
+ &'static str 是 &'a str 的 子类型，写作 &'static str < &'a str

### 1.2. 闭包：参数逆变，返回值协变

+ Fn(T) ：在T上是逆变的
+ Fn() -> T ：在T上是协变的
+ Fn(T) -> T ：在T上是不变的

### 1.3. 普通类型

+ T：在T上是协变的
+ &'a T：在’a和T上是协变的
+ &'a mut T：在’a上是协变的，在T上是不变
+ *const T：在T上是协变的
+ *mut T：在T上是不变