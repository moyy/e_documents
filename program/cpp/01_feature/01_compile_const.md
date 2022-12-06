# 编译期 & 常量

+ `nullptr` 代替：NULL，类型中就区分了 指针 和 数字
+ `constexpr` 常量表达式（编译时求值）
+ `static_assert` 编译期 断言

## `nullptr` 代替：NULL，类型中就区分了 指针 和 数字




## `constexpr` 常量表达式（编译时求值）

## static_assert 编译期 断言

断言失败，编译报错

``` c++
static_assert(sizeof(void *) == 4, "pointer's size must be 4 Bytes");
```