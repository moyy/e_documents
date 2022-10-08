# 注释 && 文档

- [注释 && 文档](#注释--文档)
  - [1. 项目 README.md](#1-项目-readmemd)
  - [2. 非 文档注释](#2-非-文档注释)
  - [3. 文档注释](#3-文档注释)
    - [3.1. 内部 文档注释](#31-内部-文档注释)
    - [3.2. 模块 文档注释](#32-模块-文档注释)
  - [4. 例子](#4-例子)

## 1. 项目 README.md

每个项目 新建一个 README.md，写上库的动机，目的，好处，主要概念，Examples

在 lib.rs / main.rs 中，第一行加上

``` rs
#![doc = include_str!("README.md")]
```

## 2. 非 文档注释

遵循 C++ 风格的注释，编译器 当 空白字符 处理

+ 行 // ...
+ 块 /* ... */

## 3. 文档注释

+ Markdown 语法格式
+ 发布时 会生成 doc文档

### 3.1. 内部 文档注释

+ 写到 API 或 公用类型的 前面
+ 说明 注释**之后**的 公用概念 的 用法；
+ 格式
	- 行 ///...
	- 块 /** ... */

### 3.2. 模块 文档注释

+ 用于 解释 注释本身所在的 super 对象
+ 一般 写到 模块 开头
+ 用于 说明 整个模块的主要概念
+ 格式：
	- //! ...
	- /*! ... */

## 4. 例子

``` rs
//! 文档注释 A doc comment that applies to the implicit anonymous module of this crate

pub mod outer_module {

    //!  - Inner line doc 内部文档注释
    //!! - Still an inner line doc (but with a bang at the beginning) 内部文档注释

    /*!  - Inner block doc 内部文档注释 */
    /*!! - Still an inner block doc (but with a bang at the beginning) 内部文档注释 */

    //   - Only a comment 仅仅是 注释
    ///  - Outer line doc (exactly 3 slashes)
    //// - Only a comment 仅仅是 注释

    /*   - Only a comment 仅仅是 注释 */
    /**  - Outer block doc (exactly) 2 asterisks */
    /*** - Only a comment 仅仅是 注释 */

    pub mod inner_module {}

    pub mod nested_comments {
        /* In Rust /* we can /* nest comments */ */ 仅仅是 注释，嵌套 块注释 */

        // All three types of block comments can contain or be nested inside 仅仅是 注释
        // any other type:

        /*   /* */  /** */  /*! */  */
        /*!  /* */  /** */  /*! */  */
        /**  /* */  /** */  /*! */  */
        pub mod dummy_item {}
    }

    pub mod degenerate_cases {
        // empty inner line doc
        //!

        // empty inner block doc
        /*!*/

        // empty line comment
        //

        // empty outer line doc
        ///

        // empty block comment
        /**/

        pub mod dummy_item {}

        // empty 2-asterisk block isn't a doc block, it is a block comment
        /***/

    }

    /* The next one isn't allowed because outer doc comments
       require an item that will receive the doc */

    /// Where is my item?
}
```