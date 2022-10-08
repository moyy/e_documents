- [区间 Range](#区间-range)
	- [1. 左闭-右开 区间](#1-左闭-右开-区间)
	- [2. 全-闭 区间：注意 `=`](#2-全-闭-区间注意-)
	- [3. 反向迭代](#3-反向迭代)
	- [4. 跳两步迭代](#4-跳两步迭代)
	- [5. 反向跳两步迭代](#5-反向跳两步迭代)

# 区间 Range

[std::ops::Range](https://doc.rust-lang.org/std/ops/struct.Range.html)，实现了 双向迭代器：[trait DoubleEndedIterator](https://doc.rust-lang.org/std/iter/trait.DoubleEndedIterator.html)

经过测试：不支持 f32 / f64

## 1. 左闭-右开 区间

```rs
#[test]
fn range_base() {
	// 3, 4, 5
	for i in 3..6 {
		print!("{}, ", i);
	}
	println!("");
}
```

## 2. 全-闭 区间：注意 `=`

``` rs
#[test]
fn range_close() {
	// 3, 4, 5, 6
	for i in 3..=6 {
		print!("{}, ", i);
	}
	println!("");
}
```

## 3. 反向迭代

``` rs
#[test]
fn range_rev() {
	// 6, 5, 4, 3
	for i in (3..=6).rev() {
		print!("{}, ", i);
	}
	println!("");
}
```

## 4. 跳两步迭代

``` rs
#[test]
fn range_step() {
	// 3, 5, 7, 9
	for i in (3..10).step_by(2) {
		print!("{}, ", i);
	}
	println!("");
}
```

## 5. 反向跳两步迭代

``` rs
#[test]
fn range_step_rev() {
	// 9, 7, 5, 3
	for i in (3..10).step_by(2).rev() {
		print!("{}, ", i);
	}
	println!("");
}
```