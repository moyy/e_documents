- [内存 & 指针 & 引用](#内存--指针--引用)
  - [1. 引用](#1-引用)
  - [2. 转换: 引用 / 指针 / 数字](#2-转换-引用--指针--数字)
  - [3. Drop Flag](#3-drop-flag)
  - [4. Layout (struct & enum)](#4-layout-struct--enum)
  - [5. 直接 堆分配，避免 栈分配内存](#5-直接-堆分配避免-栈分配内存)

# 内存 & 指针 & 引用

## 1. 引用

* **注：** 引用 就是 安全的 非空指针：
* **内存布局：** 和 指针 一样

## 2. 转换: 引用 / 指针 / 数字

+ 指针 变 数字：用 `as`
+ 指针 变引用 （同类型）
	``` rs
	let ref: &mut u32 = unsafe { &mut *ptr };
	```
+ 指针 变 引用 （不同类型）
	``` rs
	let val_casts = unsafe { &mut *(ptr as *mut i32 as *mut u32) };
	```

## 3. Drop Flag

[Drop Flag](https://doc.rust-lang.org/nomicon/drop-flags.html)

Rust：Drop函数的调用时机；

大部分时候，都是像C++一样，编译器会在变量作用域离开的代码，插入对象drop函数的调用指令；

但是Rust的析构比C++复杂的地方，在于Rust的对象有延迟初始化 和 默认move语义，所以有时候需要编译器在栈上设置drop flag的布尔值，去判断当作用域到了之后，要不要调用drop函数；

比如：

``` rs
fn test() {
	let x;
	if condition {
		x = Box::new(0);    // x未初始化，无需调用析构，直接覆盖
		println!("{}", x);
	}

	// 函数结束，x离开作用域，但x可能是未初始化的；
	// 编译器会在栈上分配一个bool值，用来跟踪x是否被初始化的；
	// 到这里，会判断drop-flag，如果为真，就调用x的drop函数；
}
```

## 4. Layout (struct & enum)

**结构体，默认是Rust布局**

**注：** 不能像C一样用一个结构体直接解码二进制数据，如果非得用，要加上：`#[repr(packed)]` 声明紧凑布局；

+ Rust和C的结构体布局，最大的不同在于：
    * Rust布局的布局 被调整 成：从大到小，因此Rust代码不能依赖声明的顺序；
+ 其他都相同：
    * 最大成员长度 SIZE
    * 起始地址被 SIZE 整除
    * 每个成员起始地址偏移，能被自身长度整除；不够前面补0
    * 总大小能被 SIZE 整除，不够后面 补0

``` rs

// Rust先调整成：b, d, a, c, e
// 最大长度b的大小4B
// A大小：4 + 2 + 1 + 1 + 1 = 9，补3B，让长度被4整除
// size = 12 字节
// #[repr(C)] // 如果打开这个注释，没有调整布局，长度就是：1+补位3+4+1+补位1+2+1+补位3 = 16 B
struct A {
    a: u8,
	b: i32,
	c: u8,
	d: u16,
	e: u8,
}

// size 值是 12
let size = std::mem::`size_of`::<A>
```

如果是C布局：

``` rs
// 没有调整布局，长度就是：1+补位3+4+1+补位1+2+1+补位3 = 16 B
#[repr(C)]
struct AA {
    a: u8,
	b: i32,
	c: u8,
	d: u16,
	e: u8,
}

// size1 值是 16
let size1 = std::mem::`size_of`::<AA>

```


枚举：

``` rs

enum EE {
    A = 1,
    B = 2,
    C = 4,
    D = 8,
    E = 16,
}

// TODO: 为什么 size 值是 1 ？
let size = std::mem::`size_of`::<EE>

```

## 5. 直接 堆分配，避免 栈分配内存

Rust可以直接 从堆上分配 数据结构，只是要自己写代码。。。

``` rs

use std::alloc::{alloc_zeroed, Layout};

#[repr(C)]
struct BigStruct([u128; 1 << 20]);

impl Drop for BigStruct {
    fn drop(&mut self) {
        println!("Goodbye, cruel world!");
    }
}

fn main() {
    let boxed = unsafe {
        let layout = Layout::new::<BigStruct>();
		// 如果是 复杂结构体，要用 ptr::write 一个个字段 拷贝代码，慢；
		// 也可以直接用 Vec --> into_boxed() 变成 Box，堆到堆 ！
        let raw_allocation = alloc_zeroed(layout) as *mut BigStruct;
        Box::from_raw(raw_allocation)
    };

    println!("{}", boxed.0[(1 << 20) - 1]);
}


```