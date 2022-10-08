- [API指南：命名](#api指南命名)
  - [1. 判断有无](#1-判断有无)
  - [2. 构造](#2-构造)
  - [3. 转换](#3-转换)

# API指南：命名

## 1. 判断有无

|方法名称|参数说明|备注|举例|
|--|--|--|--|
|`has/is_...`|`&self`（或无）|期望返回 bool|[slice::is_empty](https://doc.rust-lang.org/std/primitive.slice.html#method.is_empty)、[Result::is_ok](https://doc.rust-lang.org/std/result/enum.Result.html#method.is_ok)|

## 2. 构造

|方法名称|参数说明|备注|举例|
|--|--|--|--|
|`new`|无 `self`，通常 >= 1|构造器，另参见 [Default](https://doc.rust-lang.org/std/default/trait.Default.html)|[Box::new](https://doc.rust-lang.org/std/boxed/struct.Box.html#method.new)、[std::net::Ipv4Addr::new](https://doc.rust-lang.org/std/net/struct.Ipv4Addr.html#method.new)|
|`with_...`|无 `self`，>= 1|其他构造器|[Vec::with_capacity](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.with_capacity)|

## 3. 转换

|方法名称|参数说明|备注|举例|
|--|--|--|--|
|`from_...`|1|参考：[转换](/docs/rust_tech/rust_tech-1cl8ac2mp9p5u)|[String::from_utf8_lossy](https://doc.rust-lang.org/std/string/struct.String.html#method.from_utf8_lossy)|
|`as_...`|`&self`|无成本，& -> &, 返回视图|[str::as_bytes](https://doc.rust-lang.org/std/primitive.str.html#method.as_bytes)|
|`to_...`|`&self`|高开销 的转换|[str::to_string](https://doc.rust-lang.org/std/primitive.str.html#method.to_string)、[std::fs::File::into_raw_fd](https://doc.rust-lang.org/std/fs/struct.File.html#method.into_raw_fd)|
|`into_...`|`self`|所有权 --> 所有权|[str::to_string](https://doc.rust-lang.org/std/primitive.str.html#method.to_string)、[std::fs::File::into_raw_fd](https://doc.rust-lang.org/std/fs/struct.File.html#method.into_raw_fd)|