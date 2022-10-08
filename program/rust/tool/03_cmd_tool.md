- [Rust 命令行工具](#rust-命令行工具)
	- [1. xshell：Rust 编写 命令行工具 的 crates](#1-xshellrust-编写-命令行工具-的-crates)
	- [2. 安装](#2-安装)
	- [3. 终端](#3-终端)
	- [4. 目录 & 文件](#4-目录--文件)
	- [5. 系统信息](#5-系统信息)
	- [6. 参考](#6-参考)

# Rust 命令行工具

## 1. xshell：Rust 编写 命令行工具 的 crates

[xshell：Rust 编写 命令行工具 的 crates](https://rustcc.cn/article?id=5caec6c7-0804-47df-8dcd-9b7428b61bed)

## 2. 安装

+ 从 cargo 安装，具体命令，见下面列表
+ 从 包管理器安装 （[Windows 用 **choco**](/docs/process/process-1dm63k6a1pp77)）
	- 例子：choco install bat
	- 注：不是所有的 工具，都放到 choco 服务器 的，比如 `tokei`
	- 注: 安装之前，先运行下面命令 安装 vc环境 choco install vcredist140

## 3. 终端

| Rust工具                                         | 作用        | 安装                            |
| ------------------------------------------------ | ----------- | ------------------------------- |
| [nushell](https://github.com/nushell/nushell)    | 现代化shell | cargo install nu                |
| [starship](https://github.com/starship/starship) | 命令行提示  | cargo install starship --locked |

## 4. 目录 & 文件

| Rust工具                                            | 作用                                        | 安装                          |
| --------------------------------------------------- | ------------------------------------------- | ----------------------------- |
| [tokei](https://github.com/XAMPPRocky/tokei)        | 代码行数统计                                | cargo install tokei           |
| [fd](https://github.com/sharkdp/fd)                 | 代替 `find`，目录，文件名 搜索              | cargo install fd-find         |
| [bat](https://github.com/sharkdp/bat#how-to-use)    | 代替 `cat`，展示文件内容，语法高亮，Git集成 | cargo install --locked bat    |
| [ripgrep](https://github.com/BurntSushi/ripgrep)    | 代替 `grep`，文件内容搜索                   | cargo install ripgrep         |
| [sd](https://github.com/chmln/sd)                   | 代替 `sed`，按行 处理 文件内容              | cargo install sd              |
| [fselect](https://github.com/jhspetersson/fselect)  | 代替 `where`，使用SQL查询文件               | cargo install fselect         |
| [lsd](https://github.com/Peltoche/lsd)              | 代替 `ls`，列出目录文件信息                 | cargo install lsd             |
| [zoxide](https://github.com/ajeetdsouza/zoxide)     | 代替 `cd`，进入 目录                        | cargo install zoxide --locked |
| [watchexec](https://github.com/watchexec/watchexec) | 监视目录文件变动                            | cargo install watchexec-cli   |
| [broot](https://github.com/Canop/broot)             | 可视化访问目录树                            | cargo install broot           |

## 5. 系统信息

| Rust工具                                         | 作用                        | 安装                         |
| ------------------------------------------------ | --------------------------- | ---------------------------- |
| [procs](https://github.com/dalance/procs)        | 代替 `ps`，获取进程信息     | cargo install procs          |
| [dust](https://github.com/bootandy/dust)         | 代替 `du`，统计硬盘使用情况 | cargo install du-dust        |
| [bottom](https://github.com/ClementTsang/bottom) | 代替 `top`，进程信息        | cargo install bottom         |
| [pueue](https://github.com/nukesor/pueue)        | 任务调度                    | cargo install --locked pueue |

## 6. 参考

+ [Fancy Rust：命令行工具](https://github.com/sunface/fancy-rust/blob/main/docs/%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%B7%A5%E5%85%B7.md)