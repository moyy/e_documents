- [rustc 多库 编译 & 链接](#rustc-多库-编译--链接)
  - [1. 需求](#1-需求)
  - [2. 新建一个 wasm_engine 的 rust项目](#2-新建一个-wasm_engine-的-rust项目)
    - [2.1. build.rs](#21-buildrs)
    - [2.2. cargo.toml 如下](#22-cargotoml-如下)
    - [2.3. src/lib.rs](#23-srclibrs)
    - [2.4. 构建](#24-构建)

# rustc 多库 编译 & 链接

## 1. 需求

每个项目根据自己的需要，在 wasm-bindgen 中，打包下面的模块:

+ gui
+ res
+ astc
+ A*
+ 八叉
+ 四叉
+ 行为树
+ ...

## 2. 新建一个 wasm_engine 的 rust项目

### 2.1. build.rs

输入一堆 库的路径：

+ 在 Cargo.toml 添加依赖项；
+ src/lib.rs 中，添加导出方法；

### 2.2. cargo.toml 如下

``` toml

[package]
name = "wasm_engine"
version = "1.0.0"
edition = "2018"

[lib]
crate-type = ["cdylib"]

[dependencies]
wee_alloc = { version = "0.4", optional = true }
# 这里需要填依赖的工具库

[package.metadata.wasm-pack.profile.release]
wasm-opt = ['-O4']

```

### 2.3. src/lib.rs

``` rs

#[cfg(feature = "wee_alloc")]
#[global_allocator]
static ALLOC: wee_alloc::WeeAlloc = wee_alloc::WeeAlloc::INIT;

pub use 依赖工具库::*;

```

### 2.4. 构建

``` rs

wasm-pack build

wasm-pack build --target no-modules

wasm-pack build --target no-modules -- --features wee_alloc

```