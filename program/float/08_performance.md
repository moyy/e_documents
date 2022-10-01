- [浮点数：舍入 & 性能](#浮点数舍入--性能)
  - [相关函数回顾](#相关函数回顾)
  - [注：（在 astc解码库中得到验证）上述 函数，除to_int_unchecked外，性能 妥 妥 的 `慢`](#注在-astc解码库中得到验证上述-函数除to_int_unchecked外性能-妥-妥-的-慢)
  - [注：如果要用 as i32 变整数，注意，是`朝0`截断；](#注如果要用-as-i32-变整数注意是朝0截断)
  - [正数 的 快速 四舍五入](#正数-的-快速-四舍五入)
  - [所有 的 中速 四舍五入（如果实在想要四舍五入，大部分可以用这个）](#所有-的-中速-四舍五入如果实在想要四舍五入大部分可以用这个)
  - [子午线在pi_lib/util 的 四舍五入](#子午线在pi_libutil-的-四舍五入)

# 浮点数：舍入 & 性能

## 相关函数回顾

|函数|作用|
|--|--|
|[f32::round](https://doc.rust-lang.org/nightly/std/primitive.f32.html#method.round)|四舍五入|
|[f32::floor](https://doc.rust-lang.org/nightly/std/primitive.f32.html#method.floor)|下取整|
|[f32::ceil](https://doc.rust-lang.org/nightly/std/primitive.f32.html#method.ceil)|上去整|
|[f32::trunc](https://doc.rust-lang.org/nightly/std/primitive.f32.html#method.trunc)|取整数，同 as i32 等行为，朝0截断|
|[f32::fract](https://doc.rust-lang.org/nightly/std/primitive.f32.html#method.fract)|取小数|
|[f32::to_int_unchecked](https://doc.rust-lang.org/nightly/std/primitive.f32.html#method.to_int_unchecked)||

## 注：（在 astc解码库中得到验证）上述 函数，除to_int_unchecked外，性能 妥 妥 的 `慢`

unsafe to_int_unchecked的 效果 和 性能同 as i32

## 注：如果要用 as i32 变整数，注意，是`朝0`截断；

* 5.8f32 as i32 == 5i32
* -5.8f32 as i32 == -5i32

## 正数 的 快速 四舍五入

``` rs
(f + 0.5f32) as i32
```

## 所有 的 中速 四舍五入（如果实在想要四舍五入，大部分可以用这个）

性能损耗介于 正数 和 floor之间，比没有判断慢一点，比floor要少很多；

``` rs

fn round(f: f32) -> i32 {
    if f > 0.0 {
        (f + 0.5f32 as i32
    } else {
		// 为了解决 -3.5也能四舍五入
        (f - 0.5f32 + 0.0001) as i32
    }
}

#[test]
fn test_round() {
	assert_eq!(0, round(0.0));

    assert_eq!(3, round(3.2));
	assert_eq!(4, round(3.7));
	assert_eq!(4, round(3.5));

	assert_eq!(-3, round(-3.2));
	assert_eq!(-4, round(-3.7));
	assert_eq!(-3, round(-3.5));
}

```

## 子午线在pi_lib/util 的 四舍五入

参考：[四舍五入在go语言中为何如此困难](https://www.cnblogs.com/thinkeridea/p/14222806.html)

* 性能已测试，如同 floor，比 round快一点点，但比上面方法慢；

``` rs
// precision = 0 四舍五入 到 整数
// precision = -2 四舍五入到 百分位
fn round(f: f32, precision: i32) -> f32 {

	let p = 10.0f32.powi(precision);

	(f * p + 0.5).floor() / p
}
```