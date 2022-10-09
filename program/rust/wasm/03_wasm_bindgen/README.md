- [wasm-bindgen llvm-wasm 后端目标](#wasm-bindgen-llvm-wasm-后端目标)
	- [1. 索引](#1-索引)
	- [2. 简介](#2-简介)
	- [3. 参考](#3-参考)

# wasm-bindgen llvm-wasm 后端目标

## 1. 索引

+ [wasm-pack](./00_wasm_pack.md)
+ [宏展开](./01_macro.md)
+ [多库](./02_multi_lib.md)

## 2. 简介

+ target=wasm32-unknown-unknown
+ 构建流程
	- rs --`rustc`--> LLVM-IR --`llc && wasmld`--> wasm
+ rustc拥有 wasm内存分配库wee_alloc，可以直接生成相关wasm内存指令，不需要通过胶水代码。
+ wasm-bindgen，主要工作：
	- 提供 运行时库的支持和转换；
	- 提供 非数字参数转换，比如：u8array
	- 提供 和js层的交互

## 3. 参考

+ [Emscripten，构建为 Wasm](https://emscripten.org/docs/compiling/WebAssembly.html)
+ [Wasm 调研，2018年10月](https://d1nn3r.github.io/2018/10/09/web-assembly-Research/)
+ [Emscripten](https://emscripten.org/index.html)
+ [Binaryen](https://github.com/WebAssembly/binaryen)