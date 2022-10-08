- [ffmpeg-rs](#ffmpeg-rs)
  - [1. Windows MSVC 环境](#1-windows-msvc-环境)
  - [2. 项目](#2-项目)
  - [3. 运行报错：找不到 DLL](#3-运行报错找不到-dll)

# [ffmpeg-rs](https://github.com/zmwangx/rust-ffmpeg/wiki/Notes-on-building)

## 1. Windows MSVC 环境

+ 安装 [LLVM](https://releases.llvm.org/download.html)，配置 环境变量 LIBCLANG_PATH="D:\Program Files\LLVM\bin"
+ 安装 [vcpkg](https://github.com/microsoft/vcpkg/blob/master/README_zh_CN.md#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B-windows)
+ vcpkg install ffmpeg:x64-windows
+ 设置环境变量 FFMPEG_DIR="vcpkg安装目录\installed\x64-windows"

## 2. 项目

+ git clone https://github.com/zmwangx/rust-ffmpeg.git
+ 上网找个 mp4 视频文件，比如 test.mp4
+ cargo run --example dump-frames test.mp4

## 3. 运行报错：找不到 DLL

"vcpkg安装目录\installed\x64-windows\bin\所有的dll" 拷贝到 target/debug/deps/