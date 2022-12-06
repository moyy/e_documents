[TOC]

# [vcpkg](https://github.com/microsoft/vcpkg/blob/master/README_zh_CN.md#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B-windows)：Windows C++ 包管理工具

微软出品：Windows C++ 包管理 工具

## 1、安装 [vcpkg](https://github.com/microsoft/vcpkg/blob/master/README_zh_CN.md#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B-windows)

+ git clone https://github.com/Microsoft/vcpkg
+ cd vcpkg
+ powershell -exec bypass scripts\bootstrap.ps1

### 1.1、设置 环境变量

+ VCPKG_ROOT 安装路径
+ VCPKG_DEFAULT_TRIPLET x64-windows

## 2、使用

### 2.1、命令 & 选项

+ vcpkg integrate install
	- 将 安装的库 和 VS 联系起来
+ x86/x64-windows
	- VCPKG_LIBRARY_LINKAGE = dynamic
	- VCPKG_CRT_LINKAGE = dynamic
+ x86/x64-windows-static
	- VCPKG_LIBRARY_LINKAGE = static
	- VCPKG_CRT_LINKAGE = static
+ x86/x64-windows-static-md
	- VCPKG_LIBRARY_LINKAGE = static
	- VCPKG_CRT_LINKAGE = dynamic

### 2.2、例1：安装 freetype x64版本

+ vcpkg install freetype:x64-windows
+ vcpkg integrate install

### 2.3、例2：安装 glew glut x86 静态库 版本

+ vcpkg install glew:x86-windows-static freeglut:x86-windows-static
+ vcpkg integrate install