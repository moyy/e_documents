# build.rs

[build.rs](https://doc.rust-lang.org/cargo/reference/build-scripts.html)

在cargo构建真正运行之前，可以运行构建脚本，如果在Cargo.toml配置了`build`字段的话

## 1. 例子

* [rusty_v8](https://github.com/denoland/rusty_v8/blob/master/build.rs)
* 老莫写的：[freetype_sys](http://ser.yinengyun.com:10080/tech/freetype_sys/-/blob/master/build.rs)

## 2. 依赖库

通过 Cargo.toml 中 [build-dependencies] 指定构建时候的依赖库进行构建逻辑开发

## 3. 输入

* 通过 std::env::var_os("环境变量名"); 获取和OS环境变量的输入
* Cargo的环境变量用：std::env::var("环境变量名") 查询

## 4. 输出：通过 println!("cargo:key=value") 给编译器

希望看到构建的println!打印，要在cargo build后面 加 -vv

下面每一条都可以多次打印，因为有可能有多个 库名 或者 多个搜索路径

| key=value                     | 功能                        | 参数值                                                              | 附加说明                                          |
| ----------------------------- | --------------------------- | ------------------------------------------------------------------- | ------------------------------------------------- |
| rustc-flags=FLAGS             | 传递标志给 rustc            | 仅 支持 -l 和 -L 标志                                               |                                                   |
| rustc-link-lib=[KIND=]NAME    | 相当于gcc的-lNAME           | KIND：`static`，`dylib`(默认值)，`framework`                        | NAME是去掉 库文件名的 lib前缀 和 文件后缀 的 单词 |
| rustc-link-search=[KIND=]PATH | 相当于gcc的-LPATH           | KIND：`dependency`，`crate`，`native`，`framework` 或 `all`(默认值) |                                                   |
| rustc-cfg=FEATURE             | 通过 --cfg 标志传递给编译器 |                                                                     | 可以在 项目代码 中 通过 `cfg!` 检查               |
| rustc-env=VAR=VALUE           | 给编译器添加环境变量        |                                                                     | 可以在 项目代码 中 通过 `env!` 检查               |