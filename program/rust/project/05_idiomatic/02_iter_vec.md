- [迭代Vec: 代码片段](#迭代vec-代码片段)
  - [1. 仅要值](#1-仅要值)
  - [2. 修 改 值](#2-修-改-值)
  - [3. 索引 和 值 一起要 ：](#3-索引-和-值-一起要-)
  - [4. 两个vec，每次迭代 各取一个值：](#4-两个vec每次迭代-各取一个值)
  - [5. 两个vec，连在一起挨个迭代](#5-两个vec连在一起挨个迭代)
  - [6. 嵌套 Vec 拍平](#6-嵌套-vec-拍平)

# 迭代Vec: 代码片段

注：和其他语言一样，不要胡乱的在迭代容器的过程中 增、删 容器的元素；

## 1. 仅要值

``` rs
#[test]
fn iter_item() {
	let v: Vec<i32> = vec![100, 200, 300, 400];

	// 100, 200, 300, 400
	// item: &i32
	for item in &v {
		print!("{}, ", *item);
	}
	println!("");
}
```

## 2. 修 改 值

``` rs
#[test]
fn iter_mut_item() {
	let mut v: Vec<i32> = vec![100, 200, 300, 400];

	// item: &mut i32
	for item in v.iter_mut() {
		*item -= 100
	}

	// 0, 100, 200, 300
	for item in &v {
		print!("{}, ", item);
	}
	println!("");
}
```

## 3. 索引 和 值 一起要 ：

``` rs
#[test]
fn iter_index_and_item() {
	let v: Vec<i32> = vec![100, 200, 300, 400];

	// index: usize, item: &i32
	// (0, 100), (1, 200), (2, 300), (3, 400)
	for (index, item) in v.iter().enumerate() {
		print!("({}, {}), ", index, item);
	}
	println!("");
}
```

## 4. 两个vec，每次迭代 各取一个值：

``` rs
#[test]
fn iter_zip() {
	// 注意：v1 和 v2 不一定 等长
	let v1: Vec<i32> = vec![10, 20, 30, 40, 50, 60];
	let v2: Vec<i32> = vec![100, 200, 300, 400];

	// item1: &i32, item2: &i32;
	// (10, 100), (20, 200), (30, 300), (40, 400)
	for (item1, item2) in v1.iter().zip(&v2) {
		print!("({}, {}), ", item1, item2);
	}
	println!("");
}
```

## 5. 两个vec，连在一起挨个迭代

``` rs
#[test]
fn iter_chain() {
	// 注意：v1 和 v2 不一定 等长
	let v1: Vec<i32> = vec![10, 20, 30];

	let v2: Vec<i32> = vec![100, 200, 300];

	// item: &i32
	// 10, 20, 30, 100, 200, 300
	for item in v1.iter().chain(&v2) {
		print!("{}, ", item);
	}
	println!("");
}
```

## 6. 嵌套 Vec 拍平

``` rs
#[test]
fn iter_flat() {
	// 注意：v1 和 v2 不一定 等长
	let v: Vec<Vec<i32>> = vec![vec![10, 20, 30], vec![100, 200]];

	// item: &i32
	// 10, 20, 30, 100, 200
	for item in v.iter().flatten() {
		print!("{}, ", item);
	}
	println!("");
}
```