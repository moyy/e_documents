# bitflags 类似C++的类枚举

[bitflags: 类似C++的类枚举](https://crates.io/crates/bitflags)

当你要实现像C++类似的 位 枚举的时候，比如：

``` c++

// 使用中可以通过 几个值 的 并交叉表示 标记集合的 时候
enum ColorMode {
	R = 1,
	G = 2,
	B = 4,
	A = 8,
}

```

对应的Rust使用：

``` rs

#[macro_use]
extern crate bitflags;

bitflags! {
    struct Flags: u32 {
        const A = 0b00000001;
        const B = 0b00000010;
        const C = 0b00000100;
        const ABC = Self::A.bits | Self::B.bits | Self::C.bits;
    }
}

fn main() {
    let e1 = Flags::A | Flags::C;
    let e2 = Flags::B | Flags::C;
    assert_eq!((e1 | e2), Flags::ABC);   // union
    assert_eq!((e1 & e2), Flags::C);     // intersection
    assert_eq!((e1 - e2), Flags::A);     // set difference
    assert_eq!(!e2, Flags::A);           // set complement
}

```