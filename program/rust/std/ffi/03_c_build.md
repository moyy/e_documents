- [C语言：编译 & 链接 选项 （gcc & clang & emcc）](#c语言编译--链接-选项-gcc--clang--emcc)
  - [1. 编译流程](#1-编译流程)
  - [2. 预处理](#2-预处理)
  - [3. 编译](#3-编译)
  - [4. 链接](#4-链接)
  - [5. emcc & wasm](#5-emcc--wasm)
    - [`-s OPTION[=VALUE]` Emscripten 构建选项](#-s-optionvalue-emscripten-构建选项)
  - [6. 例子：emcc 命令](#6-例子emcc-命令)
  - [7. 参考](#7-参考)

# C语言：编译 & 链接 选项 （gcc & clang & emcc）

## 1. 编译流程

+ 预处理：实际上 就是 字符串 替换 展开
	- include：头文件
	- ifdef & define：宏 & 条件编译
+ 编译：每个源码独立（词法-语法-中间代码生成-优化-目标代码生成）；
+ 链接：将 每个编译目标 的 符号 对应起来；

## 2. 预处理

编译命令：

+ `-D` 定义宏（相当于 C代码 #define KEY Value）：`-DKEY=Value`
+ `-I` 指定搜索路径：`-I目录路径`

## 3. 编译

+ `-std=c++1y`
+ `-Wall` 显示警告信息
+ 优化：`-Os` `-Oz` 优先优化目标文件大小
+ 优化：`-O0` 、`-O1` 、`-O2` 、`-O3`
+ `-fPIC` 告诉编译器产生与位置无关代码(Position-Independent Code)，生成的.o全部使用相对路径，这样不需要重定位；如果不加，那么dll或so被加载的时候，会由于需要重定位而被拷贝一份；
+ `-fexceptions` 或 `-fno-exceptions` 支持异常处理，产生一些 stack unwind 信息；

## 4. 链接

+ `-shared` 生成 动态库/共享库，so/dll
+ `-fno-omit-frame-pointer` 函数少了几个换栈基址的指令，不会存储到rbp去；性能有提升，但是调试时候看不到堆栈；只能看到当前指令在哪个函数内部；

## 5. emcc & wasm

[emcc & wasm](https://emscripten.org/docs/tools_reference/emcc.html)

### `-s OPTION[=VALUE]` [Emscripten 构建选项](https://github.com/emscripten-core/emscripten/blob/master/src/settings.js)

+ `-s WASM=0` =1生成wasm，=0生成asm.js，=2都生成
+ `-s EXIT_RUNTIME=1` 编译出的wasm默认情况下不会退出运行时，这是web情况下期待的方式；主程序main虽然运行结束了，但模块没有退出，静态变量可以保持在内存中，不释放；
+ `-o 目标名.bc` wasm库的C++后缀是bc，可以作为另一个wasm的链接库；

## 6. 例子：emcc 命令

``` bat

emcc -std=c++1y -fno-omit-frame-pointer -fexceptions -Wall -I./Source -O3 -fPIC -DASTCENC_SSE=0 -s WASM=0 -s EXIT_RUNTIME=1 -o 目标名.bc 带上所有的C++文件，相对于执行批处理的目录

```

## 7. 参考

+ [Emscripten教程](https://emscripten.org/docs/getting_started/Tutorial.html#tutorial)