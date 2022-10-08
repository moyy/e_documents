- [Rust 惯用法](#rust-惯用法)
  - [1. 索引](#1-索引)
  - [2. 说明](#2-说明)
  - [3. 转换 std::convert](#3-转换-stdconvert)
  - [4. API: 面向Trait编程](#4-api-面向trait编程)
  - [5. 参考](#5-参考)

# Rust 惯用法

## 1. 索引

+ [命名规范](./01_name.md)
+ [迭代 vec](./02_iter_vec.md)
+ [内存布局](./03_memory_layout.md)
+ [根据场景选择数据类型](./04_choose_type.md)

## 2. 说明

[Rust 惯用法](https://cheats.rs/)

| 习惯                                  | 说明                              | 代码                                                                                                                                                             |
| ------------------------------------- | --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 善用表达式                            |                                   | x = if x { a } else { b }; <br> x = loop { break 5 }; <br> fn f() -> u32 { 0 }                                                                                   |
| 善用迭代器                            |                                   | (1..10).map(f).collect() <br> names.iter().filter(\|x\|>names.iter().filter(\|x\| x.starts_with("A"))                                                            |
| 用?处理错误                           |                                   | x = try_something()?; <br> get_option()?.run()?                                                                                                                  |
| 用强类型表示语义                      |                                   | 要：enum E { Invalid, Valid { ... } }，不要：ERROR_INVALID = -1 <br> 要：enum E { Visible, Hidden }，不要： visible: bool <br> 要：struct Charge(f32)，不要：f32 |
| 构造 参数 很多时，用`Builder`模式     |                                   | Car::new("Model T").hp(20).build();                                                                                                                              |
| 避免使用 Unsafe                       | FFI 和 C 交互 例外                |
| include_str!() 和 include_bytes!()    | 编译时将文件内容放到 二进制代码中 |                                                                                                                                                                  |
| use std::panic::{set_hook, take_hook} | Rust 捕获 Panic 的 钩子           |                                                                                                                                                                  |
| 使用 Cow<str> 作为返回类型            | 减少拷贝                          |                                                                                                                                                                  |

## 3. 转换 std::convert

| Trait   | 说明                                           |
| ------- | ---------------------------------------------- |
| AsMut   | 低消耗、可变引用到可变引用的转换               |
| AsRef   | 低消耗，引用到引用的转换                       |
| From    | 通过转换来构造自身                             |
| Into    | 一个消耗会自身的转换，可能会比较昂贵（高开销） |
| TryFrom | 尝试通过转换来构造自身                         |
| TryInto | 尝试消耗自身转的换，可能会比较昂贵             |

## 4. API: 面向Trait编程

API 宽进--严出，通过Trait给出最小约束，而不是约定具体的数据结构

+ 参数：Trait + 泛型；为 每个T 实现泛型：S< T >
+ Rust没有OO，但通过分离实现，你可以得到具体的类型

例子：

+ 输入参数，不要用 `&String`，要用 `&str`
	- 最好是：AsStr< T >
+ 输入参数，不要用 `&Vec<T>`，要用 `&[T]`

## 5. 参考

+ [Effective Rust](https://www.lurklurk.org/effective-rust/intro.html)
+ [API设计指南](https://rust-lang.github.io/api-guidelines/about.html)
	- [中文版：实现Rust库API](https://www.aloxaf.com/2019/11/elegant_apis_in_rust/)
+ [Cookbook](https://rust-lang-nursery.github.io/rust-cookbook/)
+ [惯用法：文章收集](https://github.com/mre/idiomatic-rust)