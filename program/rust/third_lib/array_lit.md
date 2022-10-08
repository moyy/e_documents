- [array_lit: 数组部分初始化](#array_lit-数组部分初始化)
  - [1. 用法](#1-用法)
  - [2. 展开](#2-展开)

# array_lit: 数组部分初始化

[array_lit: 数组部分初始化](https://crates.io/crates/array-lit)

类似C/C++，有个元素为0的数组，需要一些元素显示的赋值为非0的时候

## 1. 用法

``` rs

#[macro_use]
extern crate array_lit;

fn main() {
	// 5个元素，默认为100，但a[1] = 20, a[2] = 30
    let a: [u8; 5] = arr![100; 5; { [1]: [
        20, 30,
    ] }];

	// 打印：[100, 20, 30, 100, 100]
    // println!("{:?}", a);
}

```

## 2. 展开

上面例子，用 cargo expand 展开后的代码如下：

``` rs

fn main() {
    let a: [u8; 5] = {
        #[allow(unused_mut, unused_assignments)]
        {
            let mut arr = [100; 5];
            let mut i = 1;
            let start = i;
            let arr_inner = [20, 30];
            let end = i + arr_inner.len();
            while i < end {
                arr[i] = arr_inner[i - start];
                i += 1;
            }
            arr
        }
    };
}

```