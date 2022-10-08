- [crates：版本与补丁](#crates版本与补丁)
  - [1. 问题概述](#1-问题概述)
  - [2. 例子：用户 和 库](#2-例子用户-和-库)
    - [2.1. 现状](#21-现状)
    - [2.2. 两边代码都不稳定，如何处理？](#22-两边代码都不稳定如何处理)
      - [2.2.1. 用户：Cargo添加补丁](#221-用户cargo添加补丁)
      - [2.2.2. 库：稳定-升级-发布](#222-库稳定-升级-发布)
      - [2.2.3. 用户：升级依赖，删除补丁](#223-用户升级依赖删除补丁)
  - [3. 参考](#3-参考)

# crates：版本与补丁

## 1. 问题概述

如果发布crates.io后，一段时间内，需要 和别的库一起联调不稳定功能，这时候不能推到 crates.io，需要临时使用本地路径，如何处理？

## 2. 例子：用户 和 库

以 pi_render 使用 pi_ecs 为例

+ 简称 pi_render 是 用户
+ 简称 pi_ecs 是 库

### 2.1. 现状

pi_ecs 在 crates.io 的版本是 0.1.0

而 pi_render 的 Cargo.toml 如下

``` toml

[package]
name = "pi_render"
...

[dependencies]
pi_ecs = "0.1.0"

```

### 2.2. 两边代码都不稳定，如何处理？

#### 2.2.1. 用户：Cargo添加补丁

pi_render 的 Cargo.toml 添加 `[patch.crates-io]` 段，内容如下：

``` toml
[package]
name = "pi_render"
...

[dependencies]
pi_ecs = "0.1.0"

## 注意：这里 依赖 优先于 上面的 版本
[patch.crates-io]
pi_ecs = { path = "../pi_ecs" }
```

#### 2.2.2. 库：稳定-升级-发布

等 pi_ecs 调整稳定后，升级版本，比如 0.2.0，发布到 crates.io

#### 2.2.3. 用户：升级依赖，删除补丁

然后将 pi_render 的 Cargo.toml的 pi_ecs依赖改为 0.2.0，并删掉 `[patch.crates-io]` 段的内容，如下：

``` toml

[package]
name = "pi_render"
...

[dependencies]
pi_ecs = "0.2.0"

```

## 3. 参考

+ [Cargo：依赖覆盖](https://zhuanlan.zhihu.com/p/472170018)