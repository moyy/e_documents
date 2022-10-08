- [std::mem](#stdmem)
	- [1. ManuallyDrop< T >](#1-manuallydrop-t-)
	- [2. MaybeUninit < T >](#2-maybeuninit--t-)
	- [3. 函数](#3-函数)
		- [3.1. 布局信息：对齐 & 大小](#31-布局信息对齐--大小)
		- [3.2. 枚举信息](#32-枚举信息)
		- [3.3. 初始化 & 析构](#33-初始化--析构)
		- [3.4. mem::needs_drop 返回值](#34-memneeds_drop-返回值)
	- [4. 内存操作](#4-内存操作)

# std::mem

[std::mem](https://doc.rust-lang.org/std/mem/index.html)

## 1. ManuallyDrop< T >

[ManuallyDrop< T >](https://doc.rust-lang.org/std/mem/struct.ManuallyDrop.html)

* 尽量用这个代替mem::forget函数；
* 用于防止编译器自动调用T的析构函数；

实现：

* 实现用lang_item，以前用 union 实现；
* Rust规范约定：不会为union默认实现 drop；

创建：

``` rs
let value = T {...};
let t = ManuallyDrop<T>::new(value);
```

需要手工释放：

* 可以用 t.`drop()`;
    + 内部调用：ptr::drop_in_place();
* 也可以：let t: T = t.`into_inner`();

## 2. MaybeUninit < T > 

[MaybeUninit < T >](https://doc.rust-lang.org/std/mem/union.MaybeUninit.html)

* 需要创建 可能 未初始化 的 变量（比如FFI交互的时候）
* 结构体数组，按需 部分 初始化；

``` rs

// 创建，下面3个函数选一个；
let t = MaybeUninit<T>::uninit();
let arr: [MaybeUninit<T>; 43] = MaybeUninit<T>::uninit_array::<43>;
let t: MaybeUninit<T> = MaybeUninit<T>::zeroed(); // 按二进制0初始化整个T；

// 一定要 先初始化 再 用指针
let ptr = t.as_ptr();
// 或：let ptr = t.as_mut_ptr();

// 如果确定初始化了，可以拿T出来用；
// 后面自动调用init_t的析构函数；
let init_t = t.assume_init();

```

## 3. 函数

### 3.1. 布局信息：对齐 & 大小

| 函数                                                                        | 例子                               | 说明                                                              |
| --------------------------------------------------------------------------- | ---------------------------------- | ----------------------------------------------------------------- |
| [mem::align_of](https://doc.rust-lang.org/std/mem/fn.align_of.html)         | let s = mem::align_of::<i32>();    | s是i32的最小对齐字节数，实际字节数必须是s的倍数                   |
| [mem::align_of_val](https://doc.rust-lang.org/std/mem/fn.align_of_val.html) | let s = mem::align_of_val(&5i32)； | s是5i32对应类型（i32）的最小对齐字节数，实际字节数必须是s的倍数； |
| [mem::size_of](https://doc.rust-lang.org/std/mem/fn.size_of.html)::<i32>(); | let s = mem::size_of::<i32>();     | i32的字节数                                                       |
| [mem::size_of_val](https://doc.rust-lang.org/std/mem/fn.size_of_val.html)   | let s = mem::size_of_val(&5i32)；  | i32的字节数                                                       |

### 3.2. 枚举信息

| 函数                                                                          | 作用                               |
| ----------------------------------------------------------------------------- | ---------------------------------- |
| [mem::discriminant](https://doc.rust-lang.org/std/mem/fn.discriminant.html)   | 判断两个枚举变量用同一种枚举类型； |
| [mem::variant_count](https://doc.rust-lang.org/std/mem/fn.variant_count.html) | 有几种枚举值类型                   |

``` rs
enum Foo {
	A(&'static str),
	B(i32),
	C(i32)
}

// 判断两个枚举变量用同一种枚举类型；
assert_eq!(mem::`discriminant`(&Foo::A(123)), mem::`discriminant`(&Foo::A(456)));

// Foo有三个值
assert_eq!(3, mem::`variant_count`::<Foo>());

```

### 3.3. 初始化 & 析构

| 函数                                                                    | 作用                                    | 说明                                      |
| ----------------------------------------------------------------------- | --------------------------------------- | ----------------------------------------- |
| [mem::zeroed](https://doc.rust-lang.org/std/mem/fn.zeroed.html)         | 全用0初始化的T值                        | 注：对&，Rc这些智能指针而言，行为未定义！ |
| [mem::drop](https://doc.rust-lang.org/std/mem/fn.drop.html)             | 释放掉T的值（调用析构函数，并释放内存） |                                           |
| [mem::needs_drop](https://doc.rust-lang.org/std/mem/fn.needs_drop.html) | 如果返回false，表示T的析构函数不重要    | `ptr::drop_in_place` 已经进行了这个检查； |
| [mem::forget](https://doc.rust-lang.org/std/mem/fn.forget.html)         | 不调用T的析构函数                       | 尽量使用 `ManuallyDrop`                   |

### 3.4. mem::needs_drop 返回值

* 实现了 `Drop` 的 类型，返回 true
* 实现了 `Copy` 的 类型，返回 false
* `ManuallyDrop`< T >，返回 false
* [POD类型](https://www.cnblogs.com/Braveliu/p/12237340.html)，默认 返回 false
	+ POD类型：对象 调用 memcpy() 后 还能 正常 使用的类型
* `struct`、`enum`、`tuple` 根据 字段决定；

## 4. 内存操作

| 函数                                                                            | 作用 | 例子                                                                             | 说明                                         |
| ------------------------------------------------------------------------------- | ---- | -------------------------------------------------------------------------------- | -------------------------------------------- |
| [mem::replace](https://doc.rust-lang.org/std/mem/fn.replace.html)               | 替换 | let old_v = mem::replace(&mut v, vec![3, 4]);                                    | 将v的值设置成vec![3, 4]，返回v的旧值的所有权 |
| [mem::swap](https://doc.rust-lang.org/std/mem/fn.swap.html)                     | 交换 | mem::swap(&mut x, &mut y);                                                       | 交换x, y的值                                 |
| [mem::take](https://doc.rust-lang.org/std/mem/fn.take.html)                     | 取值 | let old_v = mem::[take](https://doc.rust-lang.org/std/mem/fn.take.html)(&mut v); | 返回v的所有权，将v的值设置成默认值           |
| [mem::transmute](https://doc.rust-lang.org/std/mem/fn.transmute.html)           | 转换 | let v = unsafe { mem::transmute<T, U>(e: T) -> U };                              | 比如：用于 u32枚举--数字                     |
| [mem::transmute_copy](https://doc.rust-lang.org/std/mem/fn.transmute_copy.html) | TODO | TODO                                                                             | TODO                                         |