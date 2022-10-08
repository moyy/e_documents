- [Cargo.toml: 清单文件](#cargotoml-清单文件)
  - [1. workspace 工作区](#1-workspace-工作区)
  - [2. crate 包](#2-crate-包)

# Cargo.toml: 清单文件

[Cargo.toml: 清单文件](https://doc.rust-lang.org/cargo/reference/manifest.html)

Cargo.toml 是 manifest清单，toml格式

Cargo.lock 是 记住所有依赖项的版本号（包括补丁版本）

规范：

* 对lib，git管理需要 忽略 Cargo.lock
* 对bin，git管理需要 加上 Cargo.lock

## 1. workspace 工作区

一个工作区包含多个crate

工作区的cargo选项

``` toml

[workspace]
members = ["包目录名 或者 子工作区名"]

```

## 2. crate 包

``` toml

[package]
# 包名
name = "请修改：包名"
# 发布版本
version = "0.1.0"
# 作者，可以写 n个；但一个库 只有一个主要维护者
authors = ["请修改：作者名 moyy <请修改：邮箱地址 moyy@gmail.com>"]
# rust 编译器版本，一般写 2021
edition = "2021"
# 库 作用 描述，一句话概括
description = "请修改：描述信息."
# 仓库对应的 源码地址
repository = "https://github.com/GaiaWorld/请修改：仓库名"
# 开源许可，一般用 双许可
license = "MIT OR Apache-2.0"
# 在 crates.io 搜索的 关键字
keywords = ["pi", "请修改：搜索关键字"]

# links = "链接 C库 的名字"

# build = "build.rs"

# 特性
[features]

# （运行时）依赖库
[dependencies]
time = "0.1" # 一般不填补丁版本，注：一般情况不要填"*"；

# 开发依赖库，比如 test、bench、bin
[dev-dependencies]

# 构建依赖库，build.rs时依赖
[build-dependencies]

```