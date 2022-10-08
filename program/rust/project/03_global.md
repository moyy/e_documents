- [全局变量 和 动态库](#全局变量-和-动态库)
  - [1. 要点](#1-要点)
  - [2. 例子：log 库](#2-例子log-库)
  - [3. 例子：OpenGL 的 Context](#3-例子opengl-的-context)

# 全局变量 和 动态库

## 1. 要点

+ 为了方便，调用函数时，通常想隐藏 环境实例
	- 方法1：设成 `全局 static 变量`
	- 方法2：设成 `线程局部存储`
+ 全局 static 变量 会 构建到 对应 的 exe 和 dll/so 上，**不同的 dll/so 会有 不同的 static 变量实例**
	- 不管是 `static` 还是 `lazy_static`
+ 如果想 多个dll/so 共享 `全局 static 变量`
	- 方法1：将 那个库 变成 dll/so，例子：`CRT`(C 运行时库) 的 `MT` 和 `MD` 的 区别；
	- 方法2：写个 pub fn，将 `全局 static 变量` 指针 返回
+ `线程局部存储`：由 OS 提供，多个 dll/so 可以共享，但是 不能跨线程 获取；

## 2. 例子：log 库

log::info(...) 没有传递 上下文，那么 是 怎么 获取 日志环境的；

在 对应的 日志库，比如 env_logger 里

+ 实现日志环境 struct impl log::Log
+ 调用 log::set_logger，设置到 log crate 的 静态变量 Logger

那么，可以肯定，log 在每个 dll/so/exe 都 分别 初始化，对应的 库 才能 有 日志打印

## 3. 例子：OpenGL 的 Context

glClear(r, g, b, a) 也是没有传递 GL环境，那么 GL环境从哪来的；

答案是：从 对应的 `线程局部存储` 来

+ `线程局部存储` 是 每线程 的 静态变量，OS 提供的方法
+ 和 dll/so 无关；只要在 GL Context 初始化 的那个线程 调用 `makeCurrent` 后，就 能 调用 OpenGL 方法；
	-  但 跨线程调用 OpenGL 就会 直接崩溃；
	- 除非你在两个 线程都 初始化一次 GL Context；
	- 初始化 GL Context 库：Windows 是 WGL，Android 是 EGL；