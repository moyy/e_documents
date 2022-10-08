- [双端队列 VecDeque](#双端队列-vecdeque)
  - [1. 初始化](#1-初始化)
  - [2. cap & len](#2-cap--len)
  - [3. 元素 & 迭代](#3-元素--迭代)
  - [4. 查找](#4-查找)
  - [5. 修改](#5-修改)

# 双端队列 VecDeque 

[VecDeque](https://doc.rust-lang.org/std/collections/struct.VecDeque.html)

环状Vec 实现的 双端队列

## 1. 初始化

| 名称                                                                                                 | 作用 | 例子 | 说明 |
| ---------------------------------------------------------------------------------------------------- | ---- | ---- | ---- |
| [new](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.new)                     |      |      |      |
| [with_capacity](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.with_capacity) |      |      |      |

## 2. cap & len

| 名称                                                                                                         | 作用 | 例子 | 说明 |
| ------------------------------------------------------------------------------------------------------------ | ---- | ---- | ---- |
| [len](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.len)                             |      |      |      |
| [capacity](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.capacity)                   |      |      |      |
| [clear](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.clear)                         |      |      |      |
| [resize](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.resize)                       |      |      |      |
| [resize_with](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.resize_with)             |      |      |      |
| [reserve](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.reserve)                     |      |      |      |
| [reserve_exact](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.reserve_exact)         |      |      |      |
| [try_reserve](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.try_reserve)             |      |      |      |
| [try_reserve_exact](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.try_reserve_exact) |      |      |      |
| [shrink_to](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.shrink_to)                 |      |      |      |
| [shrink_to_fit](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.shrink_to_fit)         |      |      |      |

## 3. 元素 & 迭代

| 名称                                                                                                 | 作用 | 例子 | 说明 |
| ---------------------------------------------------------------------------------------------------- | ---- | ---- | ---- |
| [back](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.back)                   |      |      |      |
| [back_mut](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.back_mut)           |      |      |      |
| [contains](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.contains)           |      |      |      |
| [front](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.front)                 |      |      |      |
| [front_mut](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.front_mut)         |      |      |      |
| [get](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.get)                     |      |      |      |
| [get_mut](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.get_mut)             |      |      |      |
| [is_empty](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.is_empty)           |      |      |      |
| [iter](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.iter)                   |      |      |      |
| [iter_mut](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.iter_mut)           |      |      |      |
| [as_slices](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.as_slices)         |      |      |      |
| [as_mut_slices](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.as_mut_slices) |      |      |      |
| [range](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.range)                 |      |      |      |
| [range_mut](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.range_mut)         |      |      |      |

## 4. 查找

| 名称                                                                                                               | 作用 | 例子 | 说明 |
| ------------------------------------------------------------------------------------------------------------------ | ---- | ---- | ---- |
| [binary_search](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.binary_search)               |      |      |      |
| [binary_search_by](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.binary_search_by)         |      |      |      |
| [binary_search_by_key](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.binary_search_by_key) |      |      |      |

## 5. 修改

| 名称                                                                                                         | 作用 | 例子 | 说明 |
| ------------------------------------------------------------------------------------------------------------ | ---- | ---- | ---- |
| [push_front](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.push_front)               |      |      |      |
| [push_back](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.push_back)                 |      |      |      |
| [pop_front](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.pop_front)                 |      |      |      |
| [pop_back](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.pop_back)                   |      |      |      |
| [append](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.append)                       |      |      |      |
| [insert](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.insert)                       |      |      |      |
| [drain](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.drain)                         |      |      |      |
| [remove](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.remove)                       |      |      |      |
| [retain](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.retain)                       |      |      |      |
| [split_off](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.split_off)                 |      |      |      |
| [swap](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.swap)                           |      |      |      |
| [swap_remove_back](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.swap_remove_back)   |      |      |      |
| [swap_remove_front](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.swap_remove_front) |      |      |      |
| [truncate](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.truncate)                   |      |      |      |
| [make_contiguous](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.make_contiguous)     |      |      |      |
| [rotate_left](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.rotate_left)             |      |      |      |
| [rotate_right](https://doc.rust-lang.org/std/collections/struct.VecDeque.html#method.rotate_right)           |      |      |      |