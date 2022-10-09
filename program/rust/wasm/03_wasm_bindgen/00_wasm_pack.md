- [wasm-pack && wasm-bindgen.exe](#wasm-pack--wasm-bindgenexe)
	- [1. 用rust工具链生成 wasm](#1-用rust工具链生成-wasm)
	- [2. 生成 对应的 js](#2-生成-对应的-js)
	- [3. 如果是release版本，还做了下面两个事情：](#3-如果是release版本还做了下面两个事情)
	- [4. wasm-bindgen提供的命令行工具：wasm-bindgen.exe](#4-wasm-bindgen提供的命令行工具wasm-bindgenexe)
	- [5. 选项](#5-选项)

# wasm-pack && wasm-bindgen.exe

## 1. 用rust工具链生成 wasm

``` rs

cargo build --target=wasm32-unknown-unknown --release

```
## 2. 生成 对应的 js

+ 下载 `同版本` 的wasm-bingen源码，构建里面的： crates/cli 项目
	- 会生成一个：wasm-bindgen.exe
+ 利用wasm的导入导出符号，根据 这些符号 生成对应的js；

``` rs

wasm-bindgen.exe --out-dir pkg --out-name astc_decoder_wasmbindgen ../target/wasm32-unknown-unknown/release/astc_decoder_wasmbindgen.wasm

```

## 3. 如果是release版本，还做了下面两个事情：

+ 优化：binaryen工具
	- wasm-opt -O4 pkg/wasm_rust.wasm -o pkg/wasm_rust.wasm
+ （类似工具）裁剪代码：wabt工具
	- wasm-strip pkg/wasm_rust.wasm

## 4. wasm-bindgen提供的命令行工具：wasm-bindgen.exe

+ 用于根据 wasm 的导入导出符号，生成 js；
+ 必须连着 wasm-bindgen 一起使用
+ 解析 wasm 的时候，用到了 walrus 和 wasm-parser这两个rust库；

## 5. 选项

+ -v 版本
+ -h 使用帮助提示
+ --out-dir 输出目录
+ --out-name 指定输出文件（必须要后缀）
+ --target web, bundler, nodejs, no-modules, deno
+ --no-modules-global 初始化的变量
+ --browser JS需要和浏览器兼容
+ --typescript 输出 .d.ts
+ --no-typescript 不需要 .d.ts
+ --omit-imports               Don't emit imports in generated JavaScript
+ --debug 包含调试信息
+ --no-demangle 不混淆rust的符号名字
+ --keep-debug  保持调试信息
+ --remove-name-section 移除某些section
+ --remove-producers-section 移除某些section
+ --encode-into MODE ?
+ --reference-types ?