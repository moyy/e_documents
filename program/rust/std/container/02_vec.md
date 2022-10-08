- [std::vec](#stdvec)
  - [1. 内存布局](#1-内存布局)
  - [2. slice & Iterator](#2-slice--iterator)
  - [3. 初始化](#3-初始化)
  - [4. 当`Stack`栈用](#4-当stack栈用)
  - [5. 类型转换](#5-类型转换)
  - [6. 大小 & 容量](#6-大小--容量)
  - [7. 数据操作](#7-数据操作)

# std::vec

[std::vec](https://doc.rust-lang.org/std/vec/struct.Vec.html)

## 1. 内存布局

![](/uploads/rust_tech/images/m_3d67fad127c3d42372d1f9dcd1467e29_r.png)

## 2. slice & Iterator

[slice方法](/docs/rust_tech/rust_tech-1cm0q70scl2bh) 和 [Iterator方法](/docs/rust_tech/rust_tech-1ckg9bg8eiqf9)

## 3. 初始化

|名称|作用|例子|说明|
|--|--|--|--|
|宏：vec![元素1, ...]||||
|[Vec::new](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.new)||||
|[Vec::with_capacity](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.with_capacity)||||

## 4. 当`Stack`栈用

|名称|作用|例子|说明|
|--|--|--|--|
|[Vec::is_empty](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.is_empty)|判空|||
|[Vec::pop](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.pop)|弹：末尾删除元素 并返回|||
|[Vec::push](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.push)|推：末尾插入元素|||

## 5. 类型转换

|名称|作用|例子|说明|
|--|--|--|--|
|[Vec::as_ptr](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.as_ptr)|返回数据的指针|||
|[Vec::as_mut_ptr](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.as_mut_ptr)|可变指针|||
|[Vec::as_slice](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.as_slice)|返回数据slice|||
|[Vec::as_mut_slice](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.as_mut_slice)|可变slice|||
|[Vec::into_boxed_slice](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.into_boxed_slice)||||
|[Vec::into_raw_parts](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.into_raw_parts)||||
|[Vec::from_raw_parts](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.from_raw_parts)||||

## 6. 大小 & 容量

|名称|作用|例子|说明|
|--|--|--|--|
|取：[Vec::len](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.len)|取长度|||
|取：[Vec::capacity](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.capacity)|取容量|||
|[Vec::shrink_to](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.shrink_to)|收缩cap|||
|[Vec::reserve](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.reserve)|保留cap|||
|[Vec::reserve_exact](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.reserve_exact)|保留cap|||
|[Vec::try_reserve](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.try_reserve)|保留cap|||
|[Vec::try_reserve_exact](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.try_reserve_exact)|保留cap|||
|[Vec::shrink_to_fit](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.shrink_to_fit)|收缩cap|||
|[Vec::clear](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.clear)|将len设为0|||
|[Vec::resize](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.resize)|改变len|||
|[Vec::resize_with](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.resize_with)|改变len|||
|[Vec::truncate](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.truncate)|截断len|||
|`unsafe` [Vec::set_len](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.set_len)|非安全，设置len，需要保证有足够容量，还有需要保证扩容的地方已经被初始化，通常和spare_capacity_mut一起用|||
|[Vec::spare_capacity_mut](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.spare_capacity_mut)|返回剩余备用的容量，用于执行初始化|||

## 7. 数据操作

|名称|作用|例子|说明|
|--|--|--|--|
|[Vec::leak](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.leak)|让编译器忘掉释放内存，返回可变引用|||
|[Vec::append](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.append)|将另一个vec内容的所有权加到self后面||调用后，参数的vec的len=0|
|[Vec::extend_from_slice](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.extend_from_slice)|将slice的内容通过clone的方式扩展到self的尾部||slice的元素必须实现Clone|
|[Vec::insert](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.insert)|将参数插入到指定的位置|||
|[Vec::swap_remove](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.swap_remove)|快速删除，交换最后一个元素过来，并删除最后一个元素|||
|[Vec::remove](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.remove)|删除指定位置的元素|||
|[Vec::splice](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.splice)|返回拼接`迭代器`，既有删除又有替换和添加可以考虑||跟js类似|
|[Vec::split_off](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.split_off)||||
|[Vec::drain](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.drain)|将指定range的内容从self中移除并返回|||
|[Vec::drain_filter](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.drain_filter)|将符合谓词的内容从self移除并返回|||
|[Vec::retain](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.retain)|仅保留符合谓词的元素|||
|[Vec::dedup](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.dedup)|删掉连续重复的内容|||
|[Vec::dedup_by](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.dedup_by)||||
|[Vec::dedup_by_key](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.dedup_by_key)||||