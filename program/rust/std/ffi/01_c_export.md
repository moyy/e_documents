- [C/C++库 导出](#cc库-导出)
  - [1. 头文件模板: astc.h](#1-头文件模板-astch)
  - [2. 实现文件模板: astc.cpp](#2-实现文件模板-astccpp)

# C/C++库 导出

## 1. 头文件模板: astc.h

``` c++

// 防止C符号多次被定义（因为C可以在头文件写实现）
// 保证项目中独一无二的 头文件名_H
#ifndef ASTCENC_H
#define ASTCENC_H

// MSVC会带这个宏
// 因为MSVC默认不导出任何符号到dll，所以要做这个处理
#ifdef WIN32
	// MSVC的dll导出方式：如果是生成DLL的模块，一般会带上：库名_EXPOTRS 这个宏
	#ifdef ASTCENC_EXPORTS
	// 一般这个dll或者lib的公有API，会带上：库名_API 的宏
	#define ASTCENC_API __declspec(dllexport)
	#else
	#define ASTCENC_API __declspec(dllimport)
	#endif
#else
	// 如果是非msvc环境，所有的符号默认导出到so或者a，无需指定什么选项
	#define ASTCENC_API
#endif

// C++有运算符重载，所以需要做一次name mangling，ABI在gnu和msvc不兼容
// 这里是告诉编译器，这些函数按C的命名方式生成和使用；
#ifdef __cplusplus
extern "C" {
#endif

	// 函数声明统一是：API宏 返回类型 函数名(类型1 名1, 类型2 名2, ...);
	ASTCENC_API void astc_init_context();

	ASTCENC_API void astc_uninit_context();

#ifdef __cplusplus
}
#endif
#endif

```

## 2. 实现文件模板: astc.cpp

``` cpp

// 引入头文件
#include "astc.h"

// 不要写API宏了，那个是给声明用的；
void astc_init_context() {
	// 用C或者C++代码 实现
}

void astc_uninit_context() {
	// 用C或者C++代码 实现
}

```