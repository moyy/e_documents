- [双向迭代器 DoubleEndedIterator](#双向迭代器-doubleendediterator)
  - [1. 找到 前一个 元素](#1-找到-前一个-元素)
  - [2. 提供的方法](#2-提供的方法)

# 双向迭代器 DoubleEndedIterator

[std::iter::DoubleEndedIterator](https://doc.rust-lang.org/std/iter/trait.DoubleEndedIterator.html)

从两边遍历的迭代器；

## 1. 找到 前一个 元素

``` rs
pub trait DoubleEndedIterator: Iterator {
	// 必须实现的方法：从后找前一个；
    fn next_back(&mut self) -> Option<Self::Item>;
}
```

## 2. 提供的方法

|函数|作用|例子|说明|
|--|--|--|--|
|[advance_back_by](https://doc.rust-lang.org/std/iter/trait.DoubleEndedIterator.html#method.advance_back_by)|
|[nth_back](https://doc.rust-lang.org/std/iter/trait.DoubleEndedIterator.html#method.nth_back)|
|[rfind](https://doc.rust-lang.org/std/iter/trait.DoubleEndedIterator.html#method.rfind)|
|[rfold](https://doc.rust-lang.org/std/iter/trait.DoubleEndedIterator.html#method.rfold)|
|[try_rfold](https://doc.rust-lang.org/std/iter/trait.DoubleEndedIterator.html#method.try_rfold)|