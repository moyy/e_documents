# 错误处理

+ [Option](./option.md)
+ [Result](./result.md)
+ [第三方库 either](https://crates.io/crates/either) 枚举，提供了 Left, Right 两个值；

## 概述

| 类型                 | 函数族            |
| -------------------- | ----------------- |
| 拆箱                 | `unwrap`          |
| 箱子 同类型 变换     | `or`, `or_else`   |
| Option <--> Resut    | `ok/err`, `ok_or` |
| 拆箱 - 换类型        | `map_or`          |
| 拆箱 - 换类型 - 装箱 | `map`, `and_then` |

## 参考

* [错误处理：范式](http://www.sheshbabu.com/posts/rust-error-handling/)

![](../../img/m_42db222746caa0d507a0472b54b94a40_r.png)