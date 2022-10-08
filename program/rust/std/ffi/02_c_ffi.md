- [Rust-C：指针传递 & 堆内存管理](#rust-c指针传递--堆内存管理)
	- [1. C指针传递到rust](#1-c指针传递到rust)
		- [1.1. C风格字符串的问题](#11-c风格字符串的问题)
		- [1.2. C指针的释放问题](#12-c指针的释放问题)
	- [2. Rust指针 传到 C](#2-rust指针-传到-c)

# Rust-C：指针传递 & 堆内存管理

## 1. C指针传递到rust

### 1.1. C风格字符串的问题

+ 注意 CString & CStr <--> str & String

### 1.2. C指针的释放问题
up
注意 C指针内存的 **释放**，和库使用的内存分配器有关。

最稳妥的方案是："**谁分配谁释放**"，每个 库/类 都要曝露自己的释放函数；

要 严格保证 从哪个 库/类 分配的内存，要回到那个 库/类 实现去释放；因为你没办法改第三方库的构建；

+ auto a = malloc(120 * sizeof(A)) <--> free(a)
+ auto a = new A(); <--> delete a;
+ auto a = new A[10]; <--> delete[] a;

原因：

特别是Windows的CRT有静态、动态之分；（背景：注意 [区分 库的静动态 和 CRT库的静动态](https://docs.microsoft.com/zh-cn/cpp/c-runtime-library/crt-library-features?view=msvc-160)）

如果是用了静态CRT的库(lib/dll)，会内联libc的malloc和free实现，包括堆内存的数据结构。

## 2. Rust指针 传到 C

如果内存是函数立即用，函数调用完成后，不握住任何Rust内存，随便怎么传都可以；

但是如果传到C的内存：C层模块需要 跨函数 握住，又不想拷贝的话：

+ 第一：必须分配到Rust的堆上（用Box，Rc，Arc等指针指针）；
+ 第二：想办法让Rust不调用 智能指针 的 析构函数（drop）
	- Box::into_raw(box)； 这个函数会返回box在堆上数据的指针；并转移出来，box变量超过作用域后，不再释放析构函数；
	- Rc 和 Arc 同理
+ 第三：在需要用的时候，可以直接用：std::mem::transmute 方法，强转为对应类型引用来使用，没必要调用 Box::from_raw(ptr)，因为那样会消耗较多的时间；
+ 第四：等C层不要的时候，要掉Rust层的一个方法将指针回到Rust的生命周期管理上回收掉，否则会有内存泄漏问题；
	- Box::from_part(ptr)