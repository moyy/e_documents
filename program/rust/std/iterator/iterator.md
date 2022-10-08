- [迭代器](#迭代器)
  - [1. trait Iterator：核心](#1-trait-iterator核心)
  - [2. 消费者 & 适配器](#2-消费者--适配器)
  - [3. 惰性 & 无限数据结构](#3-惰性--无限数据结构)
  - [4. 适配器 函数](#4-适配器-函数)
    - [4.1 所有权 & 拷贝](#41-所有权--拷贝)
    - [4.2 源头](#42-源头)
    - [4.3 迭代](#43-迭代)
    - [4.4 过滤](#44-过滤)
    - [4.5 变换](#45-变换)
  - [5. 消费者 函数](#5-消费者-函数)
    - [5.1 遍历](#51-遍历)
    - [5.2 比较](#52-比较)
    - [5.3 找元素](#53-找元素)
    - [5.4 找位置](#54-找位置)
    - [5.5 归约：Reduce | Fold](#55-归约reduce--fold)
    - [5.6 其他](#56-其他)

# [迭代器](https://doc.rust-lang.org/std/iter/index.html)

相当于C++的 `前向迭代器`

先阅读：[使用迭代器处理元素序列](https://kaisery.gitbooks.io/trpl-zh-cn/content/ch13-02-iterators.html)

## 1. [trait Iterator](https://doc.rust-lang.org/std/iter/trait.Iterator.html)：核心

``` rs
trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;
}
```

## 2. 消费者 & 适配器

* 消费者(函数)：参数是迭代器，返回非迭代器；
* 适配器：参数是迭代器，返回另一个迭代器；利用适配器就组成 流式结构
	+ 迭代器 本身 就是 （恒等）适配器；

## 3. 惰性 & 无限数据结构

* 惰性：适配器 都是 惰性的，除非配置了消费者（这时候，会执行 `拉`操作），否则迭代器算法不会运行；
* `无限 数据结构`：基于 惰性特性 天然就支持；

数据流：容器 --> 迭代器 --> （0个或多个）适配器 --> 消费者

## 4. 适配器 函数

### 4.1 所有权 & 拷贝

|函数|作用|例子|说明|
|--|--|--|--|
|[by_ref](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.by_ref)|返回引用||如果想重用同一个迭代器的话|
|[cloned](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.cloned)||||
|[copied](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.copied)||||

### 4.2 源头

|函数|作用|例子|说明|
|--|--|--|--|
|[cycle](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.cycle)||||
|[enumerate](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.enumerate)||||
|[zip](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.zip)||||
|[chain](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.chain)|将两个迭代器串起来，one-by-one执行|||
|[take](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.take)||||
|[take_while](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.take_while)||||
|[skip](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.skip)||||
|[skip_while](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.skip_while)||||

### 4.3 迭代

|函数|作用|例子|说明|
|--|--|--|--|
|[step_by](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.step_by)||||
|[rev](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.rev)||||
|[advance_by](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.advance_by)|迭代器往前跳过n个元素|||
|[fuse](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.fuse)|||after the first None.|

### 4.4 过滤

|函数|作用|例子|说明|
|--|--|--|--|
|[filter](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.filter)||||
|[filter_map](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.filter_map)||||
|[find](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.find)||||
|[find_map](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.find_map)||||

### 4.5 变换

|函数|作用|例子|说明|
|--|--|--|--|
|[map](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.map)||||
|[map_while](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.map_while)||||
|[scan](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.scan)||||
|[flat_map](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.flat_map)||||
|[flatten](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.flatten)||||
|[inspect](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.inspect)||||
|[peekable](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.peekable)||||

## 5. 消费者 函数

### 5.1 遍历

|函数|作用|例子|说明|
|--|--|--|--|
|[for_each](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.for_each)|挨个遍历|||
|[all](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.all)|是否所有的元素都满足谓词|||
|[any](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.any)|返回：是否有满足谓词的元素|||
|[unzip](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.unzip)||||

### 5.2 比较

|函数|作用|例子|说明|
|--|--|--|--|
|[cmp](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.cmp)|逐元素比较|||
|[cmp_by](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.cmp_by)|给定谓词，逐元素比较|||
|[partial_cmp](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.partial_cmp)||||
|[partial_cmp_by](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.partial_cmp_by)||||
|[eq](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.eq)||||
|[eq_by](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.eq_by)||||
|[ge](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.ge)||||
|[gt](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.gt)||||
|[le](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.le)||||
|[lt](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.lt)||||
|[ne](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.ne)||||

### 5.3 找元素

|函数|作用|例子|说明|
|--|--|--|--|
|[nth](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.nth)||||
|[last](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.last)||||
|[max](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.max)||||
|[max_by](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.max_by)||||
|[max_by_key](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.max_by_key)||||
|[min](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.min)||||
|[min_by](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.min_by)||||
|[min_by_key](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.min_by_key)||||

### 5.4 找位置

|函数|作用|例子|说明|
|--|--|--|--|
|[count](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.count)||||
|[position](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.position)||||
|[rposition](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.rposition)||||
|[partition](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.partition)||||
|[partition_in_place](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.partition_in_place)||||

### 5.5 归约：Reduce | Fold

|函数|作用|例子|说明|
|--|--|--|--|
|[collect](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.collect)||||
|[sum](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.sum)||||
|[product](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.product)||||
|[fold](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.fold)||||
|[fold_first](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.fold_first)||||

### 5.6 其他

|函数|作用|例子|说明|
|--|--|--|--|
|[size_hint](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.size_hint)||||
|[is_partitioned](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.is_partitioned)||||
|[is_sorted](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.is_sorted)||||
|[is_sorted_by](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.is_sorted_by)||||
|[is_sorted_by_key](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.is_sorted_by_key)||||
|[try_find](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.try_find)||||
|[try_fold](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.try_fold)||||
|[try_for_each](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.try_for_each)||||