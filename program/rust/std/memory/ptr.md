- [std::ptr](#stdptr)
  - [1. PhantomData< T >](#1-phantomdata-t-)
  - [2. std::ptr::NonNull](#2-stdptrnonnull)
    - [2.1 方法](#21-方法)
  - [3. 空 指针](#3-空-指针)
  - [4. 取值](#4-取值)
  - [5. 内存操作](#5-内存操作)
    - [5.1. 读写](#51-读写)
    - [5.2. 拷贝 & 交换](#52-拷贝--交换)
    - [5.3. 变 Slice](#53-变-slice)

# std::ptr

[std::ptr](https://doc.rust-lang.org/std/ptr/index.html)

## 1. [PhantomData< T >](https://doc.rust-lang.org/std/marker/struct.PhantomData.html)

`Phantom`: 幻象；无实体，只能看见，无法使用。

+ 可以改变struct的[型变](/docs/rust_tech/rust_tech-1clpotdoo9vij)，以便满足编译器的检查要求。
+ 当我们需要在结构体S中加入一个不使用的类型T，只是为了告诉编译器，当前结构体S和类型T有某种潜在关系，当drop检查的时候可以作为一种参考。

## 2. [std::ptr::NonNull](https://doc.rust-lang.org/std/ptr/struct.NonNull.html)

非空 指针 *mut T [协变版本](/docs/rust_tech/rust_tech-1clpotdoo9vij)

**作用：** NonNull 空指针优化，如Option< NonNull< *mut u64 > >，因为NonNull不可能为空，所以Option不需要额外的tag来判断为空的情况，编译器在编译的时候就会优化掉，以节省内存。

没有NonNull的时候，都是使用*const T + PhantomData< T >实现`协变`结构体，在需要的时候把 *const T 转换为 *mut T。有了NonNull以后，就不需要了。

NonNull<T>内部是一个NonZero< *const T >，*const T是`协变`的，所以NonNull< T >也是`协变`

### 2.1 方法

+ let non_null = NonNull::new(*mut T);
+ 可变指针：as_ptr() -> *mut T
+ 可变借用：as_mut() -> &mut T

## 3. 空 指针

|函数|作用|例子|
|--|--|--|
|[ptr::null](https://doc.rust-lang.org/std/ptr/fn.null.html)|创建空指针返回|let p: *const i32 = ptr::null()|
|[ptr::null_mut](https://doc.rust-lang.org/std/ptr/fn.null_mut.html)|同上，可变语义|let p: *mut i32 = ptr::null_mut();|

## 4. 取值

|函数|作用|例子|说明|
|--|--|--|--|
|[ptr::hash](https://doc.rust-lang.org/std/ptr/fn.hash.html)|返回指针的hash值|let h = ptr::hash(p);||
|[ptr::eq](https://doc.rust-lang.org/std/ptr/fn.eq.html)|比较两个地址是否相等|ptr::eq(p1, p2)||
|[ptr::drop_in_place](https://doc.rust-lang.org/std/ptr/fn.drop_in_place.html)|执行值的drop函数|ptr::drop_in_place(p)|内部会做 mem::needs_drop() 检查|

## 5. 内存操作

### 5.1. 读写

|函数|作用|例子|说明|
|--|--|--|--|
|[ptr::read](https://doc.rust-lang.org/std/ptr/fn.read.html)|读指针的内容|let v: T = ptr::read< T >(src: *const T)||
|[ptr::write](https://doc.rust-lang.org/std/ptr/fn.write.html)|将src的内容按二进制拷贝到dst|ptr::write< T >(dst：* mut T, src：T)|写完后，会**忽略**掉src的drop函数|
|[ptr::replace](https://doc.rust-lang.org/std/ptr/fn.replace.html)|替换|let old_val = ptr::replace< T >(dst: *mut T, src: T);||

### 5.2. 拷贝 & 交换

|函数|作用|例子|说明|
|--|--|--|--|
|[ptr::copy](https://doc.rust-lang.org/std/ptr/fn.copy.html)|拷贝|ptr::copy< T >(src, dst, count)|拷贝 count * size_of::<T>() 字节|
|[ptr::copy_nonoverlapping](https://doc.rust-lang.org/std/ptr/fn.copy_nonoverlapping.html)|地址无重叠的拷贝|ptr::copy_nonoverlapping< T >(src, dst, count)|拷贝的区间没有重叠|
|[ptr::swap](https://doc.rust-lang.org/std/ptr/fn.swap.html)|交换|ptr::swap(x: *mut T, y: *mut T)||
|[ptr::swap_nonoverlapping](https://doc.rust-lang.org/std/ptr/fn.swap_nonoverlapping.html)|地址没有重叠的交换|ptr::swap_nonoverlapping(x: *mut T, y: *mut T)||

### 5.3. 变 Slice

|函数|作用|例子|
|--|--|--|
|[ptr::slice_from_raw_parts](https://doc.rust-lang.org/std/ptr/fn.slice_from_raw_parts.html)|给指针和长度，构造 *const [T]|let ptr_slice = ptr::slice_from_raw_parts< T >(data, len);|
|[ptr::slice_from_raw_parts_mut](https://doc.rust-lang.org/std/ptr/fn.slice_from_raw_parts_mut.html)|同上，可变语义|let ptr_slice = ptr::slice_from_raw_parts_mut< T >(data, len)|