- [原生类型：slice](#原生类型slice)
  - [1. 杂项](#1-杂项)
  - [2. 类型转换](#2-类型转换)
  - [3. 判断 元素](#3-判断-元素)
  - [4. clone & copy & extend](#4-clone--copy--extend)
  - [5. string](#5-string)
  - [6. 取元素](#6-取元素)
  - [7. 元素变换](#7-元素变换)
  - [8. 查找](#8-查找)
  - [9. 排序](#9-排序)
  - [10. 分割：根据某些条件分成 多个slice](#10-分割根据某些条件分成-多个slice)
  - [11. 分组](#11-分组)
  - [12. 根据条件 返回 连续分组的slice 的迭代器](#12-根据条件-返回-连续分组的slice-的迭代器)

# 原生类型：slice

[原生类型：slice](https://doc.rust-lang.org/std/primitive.slice.html)

可以认为它就是C++的 `随机访问 迭代器`

## 1. 杂项

|名称|作用|例子|说明|
|--|--|--|--|
|[strip_prefix](https://doc.rust-lang.org/std/primitive.slice.html#method.strip_prefix)|去掉前缀切片|||
|[strip_suffix](https://doc.rust-lang.org/std/primitive.slice.html#method.strip_suffix)|去掉后缀切片|||
|[align_to](https://doc.rust-lang.org/std/primitive.slice.html#method.align_to)||||
|[align_to_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.align_to_mut)||||

## 2. 类型转换

|名称|作用|例子|说明|
|--|--|--|--|
|[to_vec](https://doc.rust-lang.org/std/primitive.slice.html#method.to_vec)||||
|[iter](https://doc.rust-lang.org/std/primitive.slice.html#method.iter)||||
|[iter_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.iter_mut)||||
|[as_mut_ptr](https://doc.rust-lang.org/std/primitive.slice.html#method.as_mut_ptr)||||
|[as_mut_ptr_range](https://doc.rust-lang.org/std/primitive.slice.html#method.as_mut_ptr_range)||||
|[as_ptr](https://doc.rust-lang.org/std/primitive.slice.html#method.as_ptr)||||
|[as_ptr_range](https://doc.rust-lang.org/std/primitive.slice.html#method.as_ptr_range)||||
|[as_chunks](https://doc.rust-lang.org/std/primitive.slice.html#method.as_chunks)||||
|[as_chunks_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.as_chunks_mut)||||

## 3. 判断 元素

|名称|作用|例子|说明|
|--|--|--|--|
|[len](https://doc.rust-lang.org/std/primitive.slice.html#method.len)||||
|[is_empty](https://doc.rust-lang.org/std/primitive.slice.html#method.is_empty)||||
|[starts_with](https://doc.rust-lang.org/std/primitive.slice.html#method.starts_with)||||
|[ends_with](https://doc.rust-lang.org/std/primitive.slice.html#method.ends_with)||||

## 4. clone & copy & extend

|名称|作用|例子|说明|
|--|--|--|--|
|[fill](https://doc.rust-lang.org/std/primitive.slice.html#method.fill)||||
|[repeat](https://doc.rust-lang.org/std/primitive.slice.html#method.repeat)||||
|[join](https://doc.rust-lang.org/std/primitive.slice.html#method.join)||||
|[clone_from_slice](https://doc.rust-lang.org/std/primitive.slice.html#method.clone_from_slice)||||
|[concat](https://doc.rust-lang.org/std/primitive.slice.html#method.concat)||||
|[connect](https://doc.rust-lang.org/std/primitive.slice.html#method.connect)||||
|[copy_from_slice](https://doc.rust-lang.org/std/primitive.slice.html#method.copy_from_slice)||||
|[copy_within](https://doc.rust-lang.org/std/primitive.slice.html#method.copy_within)||||

## 5. string

|名称|作用|例子|说明|
|--|--|--|--|
|[is_ascii](https://doc.rust-lang.org/std/primitive.slice.html#method.is_ascii)||||
|[eq_ignore_ascii_case](https://doc.rust-lang.org/std/primitive.slice.html#method.eq_ignore_ascii_case)||||
|[make_ascii_lowercase](https://doc.rust-lang.org/std/primitive.slice.html#method.make_ascii_lowercase)||||
|[make_ascii_uppercase](https://doc.rust-lang.org/std/primitive.slice.html#method.make_ascii_uppercase)||||
|[to_ascii_lowercase](https://doc.rust-lang.org/std/primitive.slice.html#method.to_ascii_lowercase)||||
|[to_ascii_uppercase](https://doc.rust-lang.org/std/primitive.slice.html#method.to_ascii_uppercase)||||

## 6. 取元素

|名称|作用|例子|说明|
|--|--|--|--|
|[first](https://doc.rust-lang.org/std/primitive.slice.html#method.first)||||
|[first_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.first_mut)||||
|[last](https://doc.rust-lang.org/std/primitive.slice.html#method.last)||||
|[last_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.last_mut)||||
|[get](https://doc.rust-lang.org/std/primitive.slice.html#method.get)||||
|[get_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.get_mut)||||
|[get_unchecked](https://doc.rust-lang.org/std/primitive.slice.html#method.get_unchecked)||||
|[get_unchecked_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.get_unchecked_mut)||||

## 7. 元素变换

|名称|作用|例子|说明|
|--|--|--|--|
|[reverse](https://doc.rust-lang.org/std/primitive.slice.html#method.reverse)||||
|[swap](https://doc.rust-lang.org/std/primitive.slice.html#method.swap)||||
|[swap_with_slice](https://doc.rust-lang.org/std/primitive.slice.html#method.swap_with_slice)||||
|[rotate_left](https://doc.rust-lang.org/std/primitive.slice.html#method.rotate_left)||||
|[rotate_right](https://doc.rust-lang.org/std/primitive.slice.html#method.rotate_right)||||


## 8. 查找

|名称|作用|例子|说明|
|--|--|--|--|
|[contains](https://doc.rust-lang.org/std/primitive.slice.html#method.contains)||||
|[select_nth_unstable](https://doc.rust-lang.org/std/primitive.slice.html#method.select_nth_unstable)||||
|[select_nth_unstable_by](https://doc.rust-lang.org/std/primitive.slice.html#method.select_nth_unstable_by)||||
|[select_nth_unstable_by_key](https://doc.rust-lang.org/std/primitive.slice.html#method.select_nth_unstable_by_key)||||
|[binary_search](https://doc.rust-lang.org/std/primitive.slice.html#method.binary_search)||||
|[binary_search_by](https://doc.rust-lang.org/std/primitive.slice.html#method.binary_search_by)||||
|[binary_search_by_key](https://doc.rust-lang.org/std/primitive.slice.html#method.binary_search_by_key)||||

## 9. 排序

|名称|作用|例子|说明|
|--|--|--|--|
|[is_sorted](https://doc.rust-lang.org/std/primitive.slice.html#method.is_sorted)||||
|[is_sorted_by](https://doc.rust-lang.org/std/primitive.slice.html#method.is_sorted_by)||||
|[is_sorted_by_key](https://doc.rust-lang.org/std/primitive.slice.html#method.is_sorted_by_key)||||
|[sort](https://doc.rust-lang.org/std/primitive.slice.html#method.sort)||||
|[sort_by](https://doc.rust-lang.org/std/primitive.slice.html#method.sort_by)||||
|[sort_by_cached_key](https://doc.rust-lang.org/std/primitive.slice.html#method.sort_by_cached_key)||||
|[sort_by_key](https://doc.rust-lang.org/std/primitive.slice.html#method.sort_by_key)||||
|[sort_unstable](https://doc.rust-lang.org/std/primitive.slice.html#method.sort_unstable)||||
|[sort_unstable_by](https://doc.rust-lang.org/std/primitive.slice.html#method.sort_unstable_by)||||
|[sort_unstable_by_key](https://doc.rust-lang.org/std/primitive.slice.html#method.sort_unstable_by_key)||||

## 10. 分割：根据某些条件分成 多个slice

|名称|作用|例子|说明|
|--|--|--|--|
|[partition_at_index](https://doc.rust-lang.org/std/primitive.slice.html#method.partition_at_index)||||
|[partition_at_index_by](https://doc.rust-lang.org/std/primitive.slice.html#method.partition_at_index_by)||||
|[partition_at_index_by_key](https://doc.rust-lang.org/std/primitive.slice.html#method.partition_at_index_by_key)||||
|[partition_dedup](https://doc.rust-lang.org/std/primitive.slice.html#method.partition_dedup)||||
|[partition_dedup_by](https://doc.rust-lang.org/std/primitive.slice.html#method.partition_dedup_by)||||
|[partition_dedup_by_key](https://doc.rust-lang.org/std/primitive.slice.html#method.partition_dedup_by_key)||||
|[partition_point](https://doc.rust-lang.org/std/primitive.slice.html#method.partition_point)||||

## 11. 分组

|名称|作用|例子|说明|
|--|--|--|--|
|[windows](https://doc.rust-lang.org/std/primitive.slice.html#method.windows)|||[1,2,3,4,5] --2--> [1,2], [2,3], [3,4], [4,5]|
|[chunks](https://doc.rust-lang.org/std/primitive.slice.html#method.chunks)|||[1,2,3,4,5] --2--> [1,2], [3,4], [5]|
|[chunks_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.chunks_mut)||||
|[chunks_exact](https://doc.rust-lang.org/std/primitive.slice.html#method.chunks_exact)||||
|[chunks_exact_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.chunks_exact_mut)||||
|[rchunks](https://doc.rust-lang.org/std/primitive.slice.html#method.rchunks)||||
|[rchunks_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.rchunks_mut)||||
|[rchunks_exact](https://doc.rust-lang.org/std/primitive.slice.html#method.rchunks_exact)||||
|[rchunks_exact_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.rchunks_exact_mut)||||
|[array_chunks](https://doc.rust-lang.org/std/primitive.slice.html#method.array_chunks)|||[1,2,3,4,5] --2--> [1,2], [3,4]，同时另一个接口取到：5|
|[array_chunks_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.array_chunks_mut)||||
|[array_windows](https://doc.rust-lang.org/std/primitive.slice.html#method.array_windows)||||

## 12. 根据条件 返回 连续分组的slice 的迭代器

|名称|作用|例子|说明|
|--|--|--|--|
|[split](https://doc.rust-lang.org/std/primitive.slice.html#method.split)||||
|[split_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.split_mut)||||
|[split_at](https://doc.rust-lang.org/std/primitive.slice.html#method.split_at)||||
|[split_at_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.split_at_mut)||||
|[split_first](https://doc.rust-lang.org/std/primitive.slice.html#method.split_first)||||
|[split_first_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.split_first_mut)||||
|[split_inclusive](https://doc.rust-lang.org/std/primitive.slice.html#method.split_inclusive)||||
|[split_inclusive_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.split_inclusive_mut)||||
|[split_last](https://doc.rust-lang.org/std/primitive.slice.html#method.split_last)||||
|[split_last_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.split_last_mut)||||
|[rsplit](https://doc.rust-lang.org/std/primitive.slice.html#method.rsplit)||||
|[rsplit_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.rsplit_mut)||||
|[splitn](https://doc.rust-lang.org/std/primitive.slice.html#method.splitn)||||
|[splitn_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.splitn_mut)||||
|[rsplitn](https://doc.rust-lang.org/std/primitive.slice.html#method.rsplitn)||||
|[rsplitn_mut](https://doc.rust-lang.org/std/primitive.slice.html#method.rsplitn_mut)||||