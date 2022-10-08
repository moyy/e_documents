
# FFI & C库交互

## 索引

+ [C 导出](./01_c_export.md)
+ [C-Rust 交互](./02_c_ffi.md)
+ [C 构建](./03_c_build.md)
+ [C++ 变 Rust](./04_bindgen.md)

## 参考

+ [Rust 死灵书](https://doc.rust-lang.org/nomicon/index.html)
+ [std::FFI](https://doc.rust-lang.org/std/ffi/index.html)
+ [libc：对各平台的系统库的C-API绑定](https://crates.io/crates/libc)
	+ unix-like平台：libc, libm, librt, libdl, libutil 和 libpthread
	+ OSX: libsystem_c, libsystem_m, libsystem_pthread, libsystem_malloc 和 libdyld
	+ Windows 平台：VS CRT中的符号
		- 在 Windows 平台上，建议使用 winapi 这个 crate 进行开发。

##### 自动将C++库变成Rust的库

* [C/C++库 变 Rust](https://github.com/rust-lang/rust-bindgen)

## FFI 类型

+ [C风格字符串--以0结尾](https://doc.rust-lang.org/std/ffi/index.html)：[CStr](https://doc.rust-lang.org/std/ffi/struct.CStr.html) & [CString](https://doc.rust-lang.org/std/ffi/struct.CString.html)
	* 注意和Rust的str，String 的 内存布局 和 转换

## [Rust-C：指针传递 & 堆内存管理](/docs/rust_tech/rust_tech-1cl3kkbi5gboo)

## [Rust-C：结构体 和 枚举 的 传递](https://doc.rust-lang.org/nightly/reference/type-layout.html)

结构体，枚举，元组 这些复合类型，默认用Rust的布局方式；一般是不用关心的；

但是下面两种场景除外：

* 拷贝C的结构体到Rust，或者将Rust的结构体拷贝到C
* 将一段二进制数据，用std::mem::transmute 硬解释为某种Rust结构体

需要了解 C结构体 和 Rust结构体 的内存布局 是 不一样的，需要显示指定

#### 第一种情况：C和Rust的结构体 互相拷贝

所有通过 FFI 交互的 类型 都应该 有 repr(C)

如果不声明，所有的类型默认是：#[repr(Rust)]

``` rs

#[repr(C)]
struct ThreeInts {
    first: i16,
    second: i8,
    third: i32
}

```

#### 第二种情况：将一段紧凑的二进制数据 强行 解释为 某个Rust结构体

比如来自文件或者网络的二进制数据，懒得写反序列化代码了，就可以这样：

要用紧凑布局 packed

``` rs

#[repr(C, packed)]
struct ThreeInts {
    first: i16,
    second: i8,
    third: i32
}

```

## [FFI](https://doc.rust-lang.org/nightly/book/ffi.html#linking) && [与C库链接](https://doc.rust-lang.org/reference/linkage.html)

#### 困惑，也许不用管它 [#[link(name = 库名 kind = 链接方式)]](https://internals.rust-lang.org/t/meaning-of-link-kinds/2686/4)

* kind = "dylib" (the default)
* kind = "static"
* kind = "framework" (OS X specific)

``` rs

extern crate libc;

use libc::{
    c_char, c_int, c_long, c_short, c_uchar, c_uint, c_ulong, c_ushort, c_void, ptrdiff_t, size_t,
};

// C风格 枚举
#[repr(C)]
#[derive(Debug, Copy, Clone, PartialEq, Eq, Hash)]
pub enum YGWrap {
    YGWrapNoWrap = 0,
    YGWrapWrap = 1,
    YGWrapWrapReverse = 2,
}

// C风格的结构体
#[repr(C)]
#[derive(Debug, Copy, Clone, Hash, PartialEq, Eq, Default)]
pub struct FT_Vector {
    pub x: FT_Pos,
    pub y: FT_Pos,
}

// link 告诉rustc，这里需要链接freetype
// link可以在Cargo.toml中通过 links = "freetype" 来通知
// kind暂时弄不清楚，也许不用关心；具体看上面的链接
#[link(name = "freetype", kind = "static")]
extern "C" {
    pub fn FT_Init_FreeType(alibrary: *mut FT_Library) -> FT_Error;
    pub fn FT_Library_Version(
        library: FT_Library,
        amajor: *mut c_int,
        aminor: *mut c_int,
        apatch: *mut c_int,
    );
}

```

## C库构建

惯例：C/C++库 在 rust-binding 一般叫：**-sys

常见的-构建 依赖库：

+ pkg-config 在系统（特别是linux系统）找 需要的库是否已经安装；
+ cc：给定OS平台，使用对应的 c编译器构建（gcc，linux）；
+ cmake，cmake命令行的rust封装；
	* 见：下面build.rs链接的例子：freetype_sys/build.rs

## [build.rs: 构建脚本](/docs/rust_tech/rust_tech-1cl1qgjetqkcs)