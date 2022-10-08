- [B-Tree](#b-tree)
  - [1. 初始化](#1-初始化)
  - [2. cap & len](#2-cap--len)
  - [3. k-v](#3-k-v)
  - [4. 迭代 & 元素](#4-迭代--元素)
  - [5. 修改](#5-修改)

# B-Tree

[BTreeMap](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html)

## 1. 初始化

| 名称                                                                                       | 作用 | 例子 | 说明 |
| ------------------------------------------------------------------------------------------ | ---- | ---- | ---- |
| [new](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.new) |      |      |      |

## 2. cap & len

| 名称                                                                                                 | 作用 | 例子 | 说明 |
| ---------------------------------------------------------------------------------------------------- | ---- | ---- | ---- |
| [clear](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.clear)       |      |      |      |
| [is_empty](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.is_empty) |      |      |      |
| [len](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.len)           |      |      |      |

## 3. k-v

| 名称                                                                                                              | 作用 | 例子 | 说明 |
| ----------------------------------------------------------------------------------------------------------------- | ---- | ---- | ---- |
| [contains_key](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.contains_key)      |      |      |      |
| [get](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.get)                        |      |      |      |
| [get_mut](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.get_mut)                |      |      |      |
| [get_key_value](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.get_key_value)    |      |      |      |
| [first_key_value](https://doc.rustlang.org/std/collections/btree_map/struct.BTreeMap.html#method.first_key_value) |      |      |      |
| [last_key_value](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.last_key_value)  |      |      |      |
| [keys](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.keys)                      |      |      |      |
| [values](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.values)                  |      |      |      |
| [values_mut](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.values_mut)          |      |      |      |
| [into_keys](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.into_keys)            |      |      |      |
| [into_values](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.into_values)        |      |      |      |

## 4. 迭代 & 元素

| 名称                                                                                                       | 作用 | 例子 | 说明 |
| ---------------------------------------------------------------------------------------------------------- | ---- | ---- | ---- |
| [iter](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.iter)               |      |      |      |
| [iter_mut](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.iter_mut)       |      |      |      |
| [entry](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.entry)             |      |      |      |
| [first_entry](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.first_entry) |      |      |      |
| [last_entry](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.last_entry)   |      |      |      |
| [range](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.range)             |      |      |      |
| [range_mut](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.range_mut)     |      |      |      |

## 5. 修改

| 名称                                                                                                         | 作用 | 例子 | 说明 |
| ------------------------------------------------------------------------------------------------------------ | ---- | ---- | ---- |
| [append](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.append)             |      |      |      |
| [drain_filter](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.drain_filter) |      |      |      |
| [insert](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.insert)             |      |      |      |
| [pop_first](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.pop_first)       |      |      |      |
| [pop_last](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.pop_last)         |      |      |      |
| [remove](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.remove)             |      |      |      |
| [remove_entry](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.remove_entry) |      |      |      |
| [split_off](https://doc.rust-lang.org/std/collections/btree_map/struct.BTreeMap.html#method.split_off)       |      |      |      |