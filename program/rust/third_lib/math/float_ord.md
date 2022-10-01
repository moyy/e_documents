- [float-ord](#float-ord)
  - [1. 封装 FloatOrd<f32>, FloatOrd<f64>](#1-封装-floatordf32-floatordf64)
  - [2. 为 FloatOrd 实现函数 convert() -> u32](#2-为-floatord-实现函数-convert---u32)
  - [3. 利用 convert() 为 FloatOrd 实现 Eq / Ord, Hash](#3-利用-convert-为-floatord-实现-eq--ord-hash)
  - [4、扩充: 如果 仅实现 Eq / Ord，也可以用 f32.total_cmp(other)](#4扩充-如果-仅实现-eq--ord也可以用-f32total_cmpother)

# [float-ord](https://crates.io/crates/float-ord)

作用：让 f32 f64 具有 Ord, Eq, Hash

## 1. 封装 FloatOrd<f32>, FloatOrd<f64>

## 2. 为 FloatOrd 实现函数 convert() -> u32

``` rs

fn convert<T>(f: FloatOrd<f32>) -> u32 {
    let u: u32 = f.to_bits();
    let bit = 1 << (32 - 1);
    if u & bit == 0 {
        u | bit
    } else {
        !u
    }
}
```

## 3. 利用 convert() 为 FloatOrd 实现 Eq / Ord, Hash

从小到大，依次是: 

NaN | -Infinity | x < 0 | -0 | +0 | x > 0 | +Infinity | NaN

## 4、扩充: 如果 仅实现 Eq / Ord，也可以用 f32.total_cmp(other)