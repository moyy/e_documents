- [Rust 构建](#rust-构建)
  - [1. 索引](#1-索引)
  - [2. 参考 Cargo之书](#2-参考-cargo之书)
  - [3. 入门](#3-入门)
    - [3.1. 创建项目](#31-创建项目)
    - [3.2. 构建](#32-构建)
    - [3.3. 构建 并 运行](#33-构建-并-运行)
  - [4. 进阶](#4-进阶)
    - [4.1 cargo expand 查看 宏 或 derive 生成的代码](#41-cargo-expand-查看-宏-或-derive-生成的代码)
    - [4.2. 条件编译：cfg & feature](#42-条件编译cfg--feature)
    - [4.3. 环境变量](#43-环境变量)
    - [4.4. .cargo/config 配置](#44-cargoconfig-配置)

# Rust 构建

## 1. 索引

+ [Cargo.toml](./01_cargo_toml.md)
+ [build.rs](./02_build_script.md)

## 2. 参考 [Cargo之书](https://doc.rust-lang.org/cargo/)

## 3. 入门

* [裁剪crate包的体积](https://www.aloxaf.com/2018/09/reduce_rust_size/)

### 3.1. 创建项目

+ exe：cargo new project_name
+ lib：cargo new project_name --lib

### 3.2. 构建

+ debug：cargo build
+ release：cargo build --release

### 3.3. 构建 并 运行

+ cargo run
+ 运行examples下的某个程序：cargo run --example 程序名
+ 运行单元测试：cargo test 可指定函数名（默认运行所有测试）
+ 运行性能测试：cargo bench 可指定函数名（默认运行所有测试）

## 4. 进阶

### 4.1 cargo expand 查看 宏 或 derive 生成的代码

[cargo expand](https://github.com/dtolnay/cargo-expand) 

安装：cargo install cargo-expand

### 4.2. 条件编译：cfg & feature

[条件编译：cfg & feature](https://doc.rust-lang.org/cargo/reference/config.html)

项目代码 可用：`cfg!()` 查询相关的选项

### 4.3. 环境变量

[环境变量](https://doc.rust-lang.org/cargo/reference/environment-variables.html)

Cargo 约定的环境变量，三种用途：

* cargo 内部使用
* build.rs 使用
* crates 使用

项目代码 可用：`env!()` 查询相关的选项

### 4.4. .cargo/config 配置

config --> toml格式文件

.cargo/ 的搜索路径：项目根目录 > 一直递归的父目录 > 用户目录