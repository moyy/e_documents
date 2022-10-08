- [Rustup](#rustup)
  - [1. 主要概念](#1-主要概念)
  - [2. 命令](#2-命令)
  - [3. target: 编译目标项](#3-target-编译目标项)
  - [4. component: 组件](#4-component-组件)

# Rustup

[Rustup](https://doc.rust-lang.org/edition-guide/rust-2018/rustup-for-managing-rust-versions.html)

+ [**每个target 的 Component 可用状态**](https://rust-lang.github.io/rustup-components-history/)
+ [**Rust Component 可用状态**](https://rust-lang-nursery.github.io/rust-toolstate/)

## 1. 主要概念

* toochain：工具链；stable/beta/nightly
* [target：编译目标项](https://rust-lang.github.io/rustup-components-history/)
* [component：组件](https://rust-lang.github.io/rustup/concepts/components.html)

## 2. 命令

| 作用                                | bash/sh 输入                                  | 说明                                          |
| ----------------------------------- | --------------------------------------------- | --------------------------------------------- |
| 更新 rustc                          | rustup update                                 |                                               |
| 查看 已安装 toolchain               | rustup show                                   | `active`是正在使用的工具链，通过`default`切换 |
| 安装 toolchain                      | rustup toolchain install nightly-2021-12-05   |                                               |
| 卸载 toolchain                      | rustup toolchain uninstall nightly-2021-12-05 |                                               |
| 切换 默认 toolchain                 | rustup default nightly-2021-12-05             |                                               |
| 为 default toolchain 添加 target    | rustup target add 编译目标项                  | 每 toolchain 一个                             |
| 为 default toolchain 移除 target    | rustup target remove 编译目标项               | 每 toolchain 一个                             |
| 为 default toolchain 添加 component | rustup component add 组件                     | 每 toolchain 一个                             |
| 为 default toolchain 移除 component | rustup component remove 组件                  | 每 toolchain 一个                             |

## 3. target: 编译目标项

描述方式：**架构-供应商-OS-ABI**

| 名称                    | 作用           | 适用OS          | 说明    |
| ----------------------- | -------------- | --------------- | ------- |
| x86_64-pc-windows-msvc  | MSVC 工具链    | Windows         | T1 支持 |
| x86_64-pc-windows-gnu   | GNU 工具链     | Windows         | T1 支持 |
| x86_64-apple-darwin     | GNU 工具链     | Mac OS          | T2 支持 |
| wasm32-unknown-unknown  | 纯 rust 工具链 | WASM            | T2 支持 |
| aarch64-linux-android   | ARMv8-64       | Andorid         | T2 支持 |
| armv7-linux-androideabi | ARMv7a-32      | Andorid         | T2 支持 |
| aarch64-apple-ios       | iPhone 真机    | iOS 13 及其以上 | T2 支持 |
| x86_64-apple-ios        | iOS 模拟器     | iOS             | T2 支持 |

## 4. component: 组件

可以理解成 具体工具

| 名称          | 作用                 | 说明            |
| ------------- | -------------------- | --------------- |
| rustc         | 编译器               |                 |
| cargo         | 构建工具             |                 |
| rustfmt       | 格式化               |                 |
| rust-std      | 标准库               |                 |
| rust-docs     | 文档                 | rustup doc      |
| clippy        | lint工具             |                 |
| miri          | Rust解析器           | 检查 未定义行为 |
| rust-analysis | 语法和语义分析       | 给RLS使用       |
| rls           | 语言服务器           | IDE基础设施     |
| rust-src      | 标准库 & 编译器 源码 | 跳转，提示      |