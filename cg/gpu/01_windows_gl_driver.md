- [判断 Windows OpenGL硬件加速](#判断-windows-opengl硬件加速)
	- [1. GL驱动](#1-gl驱动)
	- [2. 驱动：安装 & 使用](#2-驱动安装--使用)
	- [3. 条件：硬件加速](#3-条件硬件加速)
	- [4. 判断 代码](#4-判断-代码)

# 判断 Windows OpenGL硬件加速

+ [Windows OpenGL硬件加速](https://codeantenna.com/a/SAMIAuNHh4)

## 1. GL驱动

+ 纯软件: 使用Windows自带的显卡驱动程序，软件模式。
+ MCD：现代一般不支持；GL 变换、光照 软件；光栅化 硬件厂商；
+ ICD：硬件加速，GPU 厂商自带硬件驱动

## 2. 驱动：安装 & 使用

+ Windows System32/OpenGL32.dll 是 微软 OpenGL1.1 纯软件
+ nVIDIA 驱动 在安装时，会在同目录 放 nvoglnt.dll
+ 注册表 登记 nvoglnt.dll，让 Windows 知道 硬件加速 GL驱动
+ 这样，运行GL程序，OpenGL32.dll 就会把GL调用 转到 nvoglnt.dll

## 3. 条件：硬件加速

+ 3D加速卡；
+ 安装 显卡厂商 提供的 最新驱动
+ 指定 的 像素格式 是否被 显卡硬件 所支持。

## 4. 判断 代码

**注：** 游戏开发，只要判断两种类型，ICD 和 其他，ICD 则继续玩；否则就直接 弹框提示，发送日志服务器；

用 `DescribePixelFormat` 取得该像素格式的数据，然后看结构体 `PIXELFORMATDESCRIPTOR` 中 `dwFlags` 字段

``` C++

if( !(pfd.dwFlags & PFD_GENERIC_FORMAT) && !(pfd.dwFlags & PFD_GENERIC_ACCELERATED) ) {
	// 全硬件加速，ICD模式
} else ( (pfd.dwFlags & PFD_GENERIC_FORMAT) && !(pfd.dwFlags & PFD_GENERIC_ACCELERATED) ) {
	// 纯 软件 GL；
	// 弹框提示 更新显卡驱动，不让进入游戏；并 发送日志服务器；
} else if ( (pfd.dwFlags & PFD_GENERIC_FORMAT) && (pfd.dwFlags & PFD_GENERIC_ACCELERATED) ) {
	// MCD 混合模式
	// 弹框提示 更新显卡驱动，不让进入游戏；并 发送日志服务器
} else {
	// 其他情况
	// 弹框提示 更新显卡驱动，不让进入游戏；并 发送日志服务器
}

```