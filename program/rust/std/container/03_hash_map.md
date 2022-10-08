- [哈希表 HashMap](#哈希表-hashmap)
  - [1. 初始化](#1-初始化)
  - [2. 容量 & 长度 & Hash](#2-容量--长度--hash)
  - [3. 取 K-V](#3-取-k-v)
  - [4. 修改](#4-修改)
  - [5. 迭代 & 元素](#5-迭代--元素)
  - [6. enum Entry 组合子方法](#6-enum-entry-组合子方法)

# 哈希表 HashMap

[HashMap](https://doc.rust-lang.org/std/collections/struct.HashMap.html)

## 1. 初始化

|名称|作用|例子|说明|
|--|--|--|--|
|[new](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.new)||||
|[with_capacity](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.with_capacity)||||
|[with_capacity_and_hasher](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.with_capacity_and_hasher)||||
|[with_hasher](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.with_hasher)||||

## 2. 容量 & 长度 & Hash

|名称|作用|例子|说明|
|--|--|--|--|
|[hasher](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.hasher)||||
|[is_empty](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.is_empty)||||
|[capacity](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.capacity)||||
|[len](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.len)||||
|[clear](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.clear)||||
|[shrink_to](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.shrink_to)||||
|[shrink_to_fit](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.shrink_to_fit)||||
|[reserve](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.reserve)||||
|[try_reserve](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.try_reserve)||||

## 3. 取 K-V

|名称|作用|例子|说明|
|--|--|--|--|
|[into_keys](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.into_keys)||||
|[into_values](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.into_values)||||
|[contains_key](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.contains_key)||||
|[get](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.get)||||
|[get_key_value](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.get_key_value)||||
|[get_mut](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.get_mut)||||
|[keys](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.keys)||||
|[values](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.values)||||
|[values_mut](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.values_mut)||||

## 4. 修改

|名称|作用|例子|说明|
|--|--|--|--|
|[insert](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.insert)||||
|[remove](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.remove)||||
|[remove_entry](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.remove_entry)||||
|[retain](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.retain)|保留满足谓词的元素，其他元素统统删掉|||
|[drain](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.drain)|清空map，返回k-v迭代器|||
|[drain_filter](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.drain_filter)||||

## 5. 迭代 & 元素

|名称|作用|例子|说明|
|--|--|--|--|
|[iter](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.iter)||||
|[iter_mut](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.iter_mut)||||
|[entry](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.entry)||||
|[raw_entry](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.raw_entry)||||
|[raw_entry_mut](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.raw_entry_mut)||||

## 6. enum Entry 组合子方法

- 已使用：Occupied(OccupiedEntry<'a, K, V>)
- 空slot：Vacant(VacantEntry<'a, K, V>)

|名称|作用|例子|说明|
|--|--|--|--|
|[insert](https://doc.rust-lang.org/std/collections/hash_map/enum.Entry.html#method.insert)||||
|[key](https://doc.rust-lang.org/std/collections/hash_map/enum.Entry.html#method.key)||||
|[or_default](https://doc.rust-lang.org/std/collections/hash_map/enum.Entry.html#method.or_default)|为空返回默认|||
|[and_modify](https://doc.rust-lang.org/std/collections/hash_map/enum.Entry.html#method.and_modify)|有值修改|||
|[or_insert](https://doc.rust-lang.org/std/collections/hash_map/enum.Entry.html#method.or_insert)|为空插入|||
|[or_insert_with](https://doc.rust-lang.org/std/collections/hash_map/enum.Entry.html#method.or_insert_with)||||
|[or_insert_with_key](https://doc.rust-lang.org/std/collections/hash_map/enum.Entry.html#method.or_insert_with_key)||||