- [Rust 第三方库](#rust-第三方库)
  - [0、索引](#0索引)
  - [1、语言扩展](#1语言扩展)
  - [2、数据结构](#2数据结构)
  - [3、实用库](#3实用库)
  - [4、游戏 | 渲染](#4游戏--渲染)
  - [5、并发 & 异步](#5并发--异步)
  - [6、开发工具](#6开发工具)
  - [7、系统平台](#7系统平台)
  - [8、Rust宏](#8rust宏)
  - [9、参考](#9参考)

# Rust 第三方库

## 0、索引

+ [数学相关](./math/README.md)
+ [算法](./algorithm.md)
+ [array_lit](./array_lit.md)
+ [bitflags](./bitflags.md)
+ [ffmpeg](./ffmpeg.md)

## 1、语言扩展

|库|主要功能|场景说明|
|--|--|--|
|[log](https://crates.io/crates/log)|日志|输出 到控制台，用它 代替 println! 和 dbg!|
|[thiserror](https://crates.io/crates/thiserror) 和 [anyhow](https://crates.io/crates/anyhow)|错误处理||
|[bitflags](https://crates.io/crates/bitflags)|生成 位标记 的 结构体|
|[bytemuck](https://crates.io/crates/bytemuck)|二进制转换|比如 将 8字节 --> u64|
|[static_init](https://crates.io/crates/static_init)|初始化 & 单例模式|代替 [lazy_static](https://crates.io/crates/lazy_static)|
|[downcast-rs](https://crates.io/crates/downcast-rs)|Any 和 向下造型|
|[cfg-if](https://crates.io/crates/cfg-if)|相当于 C语言的 #ifdef ||
|[scopeguard](https://crates.io/crates/scopeguard)|类似于Go语言的lazy!|
|[once_cell](https://crates.io/crates/once_cell)|延迟，单例模式||
|[either](https://crates.io/crates/either)|提供 Left 和 Right 的 枚举|
|[env_logger](https://crates.io/crates/env_logger)|日志环境|简单，自己开发Demo用|
|[log4rs](https://crates.io/crates/log4rs)|日志环境|仿照 log4java, 公司项目 使用|
|[copyless](https://crates.io/crates/copyless)|零拷贝|经测试，这是个鸡肋，不用管了|

## 2、数据结构

|库|主要功能|场景说明|
|--|--|--|
|[smallvec](https://crates.io/crates/smallvec)|分配在栈上的动态数组||
|[hibitset](https://crates.io/crates/hibitset)|分层的位集|快速的知道某个范围的数据是否被设置|
|[slotmap](https://crates.io/crates/slotmap)|各种 基于 Slot 的 数据结构||
|[bytes](https://crates.io/crates/bytes)|二进制视图，如：Uint8Array|请 优先 使用 [bytemuck](https://crates.io/crates/bytemuck)|
|[itertools](https://crates.io/crates/itertools)|标准库 迭代器 的扩展||

## 3、实用库

|库|主要功能|场景说明|
|--|--|--|
|[serde](https://crates.io/crates/serde)|序列化||
|[ron](https://crates.io/crates/ron)|Rust版本的类似Json的数据格式||
|[rand](https://crates.io/crates/rand)|随机|其中 随机数生成器，请配合[pi_wy_rng](https://github.com/GaiaWorld/pi_wy_rng)使用|
|[regex](https://crates.io/crates/regex)|正则表达式||
|[instant](https://crates.io/crates/instant)|代替 std::time::Instant|在 wasm 也能使用|
|[time](https://crates.io/crates/time)|日期 & 时间||
|[num_traits](https://crates.io/crates/num_traits)|对std 数字类型 的 抽象Trait|Num Trait & Float Trait|

## 4、游戏 | 渲染

|库|主要功能|场景说明|
|--|--|--|
|[nalgebra](https://github.com/dimforge/nalgebra)|线性代数 & 图形学|Vector2/3/4, Quaternion, Matrix|
|[parry](https://github.com/dimforge/parry)|碰撞||
|[rapier](https://github.com/dimforge/rapier)|物理||
|[wgpu](https://crates.io/crates/wgpu)|图形 API 封装||
|[naga](https://crates.io/crates/naga)|Shader 转换 和 编译||
|[image](https://crates.io/crates/image)|图片 编码解码||
|[rectangle-pack](https://crates.io/crates/rectangle-pack)|二维 & 三维 装箱||

## 5、并发 & 异步

|库|主要功能|场景说明|
|--|--|--|
|[futures-lite](https://crates.io/crates/futures-lite)|Future的扩展|例：futures_lite::future::Boxed|
|[rayon](https://crates.io/crates/rayon)|数据并行库|
|[crossbeam](https://crates.io/crates/crossbeam)|并发 的 一系列 工具，包括：原子-Cell、Channel、队列、基于 Epoch 的 GC||
|[parking_lot](https://crates.io/crates/parking_lot)|Mutex，Condvar，RwLock|不要用`std`|

## 6、开发工具

|库|主要功能|场景说明|
|--|--|--|
|[autocfg](https://crates.io/crates/autocfg)|构建时，加入 rustc 参数||
|[clap](https://crates.io/crates/bytes)|命令行参数||
|[cargo-make](https://github.com/sagiegurari/cargo-make)|基于 Rust 的 构建工具|
|[xshell](https://rustcc.cn/article?id=5caec6c7-0804-47df-8dcd-9b7428b61bed)|写 命令行工具，记得 用它|

## 7、系统平台

|库|主要功能|场景说明|
|--|--|--|
|[libloading](https://crates.io/crates/libloading)|动态库 运行时 导入|
|[thread_local](https://crates.io/crates/thread_local)|线程局部存储||
|[windows](https://crates.io/crates/windows)|Windows API|微软 出品|
|[winapi](https://crates.io/crates/winapi)|Windows API|出现的早，大家都在用|
|[jni](https://crates.io/crates/jni)|Rust -- Java|Java|
|[ndk](https://crates.io/crates/ndk)|Rust -- ndk|Android|
|[ndk-glue](https://crates.io/crates/ndk-glue)|Rust -- ndk|偏 apk 构建|
|[objc](https://crates.io/crates/objc)|Rust -- ObjC|Apple: MacOS & iOS|
|[wasm-bindgen](https://crates.io/crates/wasm-bindgen)|WASM|WASM|
|[js-sys](https://crates.io/crates/js-sys)|Rust-ESNext 交互|WASM|
|[web-sys](https://crates.io/crates/web-sys)|Rust-Dom 交互|WASM|
|[wasm-bindgen-futures](https://crates.io/crates/wasm-bindgen-futures)|Rust Future 变 JS Promise|WASM|

## 8、Rust宏

请参考：[The Little Book of Rust Macros](https://danielkeep.github.io/tlborm/book/index.html)

|库|主要功能|场景说明|
|--|--|--|
|[syn](https://crates.io/crates/syn)|Rust 源码 --> 语法解析|
|[quote](https://crates.io/crates/quote)|宏解析|
|[proc-macro2](https://crates.io/crates/proc-macro2 )|宏生成|

## 9、参考

+ [Awesome Rust](https://github.com/rust-unofficial/awesome-rust)
+ [中文 Awesome Rust](https://github.com/chinanf-boy/awesome-rust-zh/blob/master/readme.md)