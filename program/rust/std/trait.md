- [常见 Trait](#常见-trait)
  - [1. 参考](#1-参考)
  - [2. 可 `derive` 的 Trait](#2-可-derive-的-trait)
    - [2.1. 相等 和 比较](#21-相等-和-比较)
    - [2.2. 格式化 与 打印](#22-格式化-与-打印)
    - [2.3. 克隆 与 拷贝](#23-克隆-与-拷贝)
    - [2.4. 默认初始化：Default](#24-默认初始化default)
    - [2.5. Hash：实现hash的时候，尽量缓存hash值，避免重复计算](#25-hash实现hash的时候尽量缓存hash值避免重复计算)
  - [3. 析构: Drop](#3-析构-drop)
  - [4. 引用 相关](#4-引用-相关)
  - [5. 转换 相关](#5-转换-相关)
  - [6. 容器 与 迭代器](#6-容器-与-迭代器)
  - [7. 运算符 重载](#7-运算符-重载)
  - [8. 对外数据 Serde序列化 (Serialize & Deserialize)](#8-对外数据-serde序列化-serialize--deserialize)
  - [9. IO相关 Read & Write](#9-io相关-read--write)
  - [10. Auto Trait，给编译器看的](#10-auto-trait给编译器看的)
    - [10.1 并发环境](#101-并发环境)
    - [10.2 Sized 用于Trait约束，不需要实现](#102-sized-用于trait约束不需要实现)
      - [10.2.1 背景：DST：动态大小类型](#1021-背景dst动态大小类型)
  - [11. 'static Type](#11-static-type)

# 常见 Trait

Rust的类型系统 --> 泛型 & Triat --> Trait绑定 --> 面向Trait的编程

|trait|概述|说明|
|--|--|--|
|[运算符相关](./op.md))]|||
|PartialEq|||
|Eq|||
|PartialOrd|||
|Ord|||
|Display|||
|Debug|||
|Clone|||
|Debug|||
|Default|||
|Hash|||
|Drop|||
|Deref & DerefMut|||
|AsRef & AsMut|||
|Borrow & BorrowMut|||
|ToOwned|||
|From & Into|||
|TryFrom & TryInto|||
|Iterator|||
|IntoIterator|||
|FromIterator & Extend|||
|Serialize & Deserialize|||
|Read & Write|||
|Send|||
|Sync|||
|Sized & ?Sized|||
|'static Type|||

## 1. 参考

[Rust标准规范：特殊 类型 和 Trait](https://doc.rust-lang.org/reference/special-types-and-traits.html)

## 2. 可 `derive` 的 Trait

可以用 `#[derive(Trait名)]` 属性 自动 实现的Trait；会有一些约束

### 2.1. 相等 和 比较

* [PartialEq, Eq](./op.md)
	+ Eq：语义上要求 实现 [等价关系](https://zh.wikipedia.org/wiki/%E7%AD%89%E4%BB%B7%E5%85%B3%E7%B3%BB)
* [PartialOrd, Ord](./op.md)
	+ PartialOrd：语义上要求 实现 [偏序关系](https://zh.wikipedia.org/wiki/%E5%81%8F%E5%BA%8F%E5%85%B3%E7%B3%BB)
	+ Ord：语义上要求 实现 [全序关系](https://zh.wikipedia.org/wiki/%E5%85%A8%E5%BA%8F%E5%85%B3%E7%B3%BB)

背景，集合论：

* 全序集 是 任意两个元素 都 可以比较 的 偏序集。
	+ 这里Ord只要求全序；
* 良序集 是 任意非空子集 都有 最小元 的 全序集，强调的是一组集合内比较的一致性；

### 2.2. 格式化 与 打印

* [Display](https://doc.rust-lang.org/std/fmt/trait.Display.html)：可用 format!("{}", 变量名) 格式化 字符串
* [Debug](https://doc.rust-lang.org/std/fmt/trait.Debug.html)：可用 format!("{:?}", 变量名) 格式化 字符串

### 2.3. 克隆 与 拷贝

* [Clone](https://doc.rust-lang.org/std/clone/trait.Clone.html)
* [Copy: Copy是个marker，没有方法可言](https://doc.rust-lang.org/std/marker/trait.Copy.html)
	+ pub trait Copy: Clone { }

### 2.4. 默认初始化：[Default](https://doc.rust-lang.org/std/default/trait.Default.html)

### 2.5. [Hash](https://doc.rust-lang.org/std/hash/trait.Hash.html)：实现hash的时候，尽量缓存hash值，避免重复计算

## 3. 析构: [Drop](https://doc.rust-lang.org/std/ops/trait.Drop.html)

## 4. 引用 相关

[Deref & DerefMut](https://kaisery.gitbooks.io/trpl-zh-cn/content/ch15-02-deref.html)：用*符号解引用，

注意：Rust 自动解引用 规则

[AsRef & AsMut](https://doc.rust-lang.org/std/convert/index.html) 借引用

* x.as_ref();
* x.as_mut();

[Borrow & BorrowMut](https://doc.rust-lang.org/std/borrow/trait.Borrow.html) 借引用

+ 是 as_ref的更严格的版本；
+ 如果x，y实现了eq，那么 从语义上需要保证：x.borrow() == y.borrow() 当且仅当：x == y

方法：

* x.bowrrow();
* x.bowrrow_mut();

[ToOwned](https://doc.rust-lang.org/std/borrow/trait.ToOwned.html) 取所有权

是 Clone 的普适版本，提供 .to_owned() 方法，用于类型转换；

如T实现了Clone，那么可以通过&T调用.clone()；但其他形式的引用无效；

要推广到其他形式的引用，要为T实现ToOwnded，作用是 为任意引用类型 生成 对应的所有权类型；

## 5. [转换 相关](https://doc.rust-lang.org/std/convert/index.html)

* From & Into：只要实现了From，std会自动为对应类型实现Into
* TryFrom & TryInto

## 6. [容器 与 迭代器](./iterator.md)

迭代器有很多方法可以用，只要实现了next接口即可；

* 迭代器：[Iterator](https://doc.rust-lang.org/std/iter/trait.Iterator.html)
* 容器：[IntoIterator](https://doc.rust-lang.org/std/iter/trait.IntoIterator.html) & for循环
	+ rust还有常用的非trait的接口约定：container.iter(), containter.iter_mut();
* 从迭代器变集合：[FromIterator](https://doc.rust-lang.org/std/iter/trait.FromIterator.html) & [Extend](https://doc.rust-lang.org/std/iter/trait.Extend.html)

## 7. [运算符 重载](./op.md)

## 8. 对外数据 Serde序列化 (Serialize & Deserialize)

[Serialize](https://docs.serde.rs/serde/trait.Serialize.html) & [Deserialize](https://docs.serde.rs/serde/trait.Deserialize.html)

## 9. IO相关 Read & Write

[Read](https://doc.rust-lang.org/std/io/trait.Read.html) & [Write](https://doc.rust-lang.org/std/io/trait.Write.html)

## 10. [Auto Trait](https://doc.rust-lang.org/reference/special-types-and-traits.html)，给编译器看的

没有什么函数需要实现，仅仅告诉编译器，这个类型可以做到某些功能；

### 10.1 并发环境

* [Send](https://doc.rust-lang.org/std/marker/trait.Send.html)：跨线程 传递；
* [Sync](https://doc.rust-lang.org/std/marker/trait.Sync.html)：跨线程 共享；

### 10.2 [Sized](https://doc.rust-lang.org/std/marker/trait.Sized.html) 用于Trait约束，不需要实现

* Sized：编译器可确定大小的类型约束，默认都会加上
* ?Sized: 移除Sized这个约束

#### 10.2.1 背景：[DST：动态大小类型](https://runrust.miraheze.org/wiki/Dynamically_Sized_Type)

DST 目前 有四种：

* dyn trait：用于 运行时 多态
* 切片slice ( [T] )
* 字符串切片：( str )
* 高级技巧（没用过）：包含DST类型的 Struct 或 Tuple（注意：不是引用）

所有 DST的引用，都是 胖指针（带长度的指针），注意区分 DST 和 引用

切片[T]是DST，编译期不知道大小；但是&[T]是胖指针，编译器是知道大小的，就是 2 * mem::size_of(usize)

## 11. 'static Type

当定义一个函数，不想传递引用类型，怎么定义；

``` rs

#[derive(Ord, PartialOrd, Eq, PartialEq)]
struct A{ 
    i: i32
}

fn test<T: 'static + Ord>(_a: T) {
}

fn test1<T: Ord>(_a: T) {
}

fn main() {
	let a = A {i: 3};
	test1(&a);

	// 这里是通不过的，因为 &A不是'static类型；
	// test(&a);
	}

	test(a);
}

```