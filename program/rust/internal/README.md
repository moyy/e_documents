# Rust 内幕

## 1. 索引

+ [类型系统 型变](./01_covariance.md)
+ [运算符的构建](./02_op.md)

## 2. 参考

+ [Why no-std](https://justjjy.com/Rust-no-std)
+ [测量内存](https://rust-analyzer.github.io/blog/2020/12/04/measuring-memory-usage-in-rust.html)

## 3. 内存管理

+ Allocator = Global
+ box 关键字 src/liballoc/heap.rs
	- box_free
    - exchange_malloc