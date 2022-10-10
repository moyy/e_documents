# Astc 压缩纹理

## 1. 索引

+ [效果](./01_effor.md)
+ [解码性能：Rust & C++](./02_performance.md)
+ [Astc 头部格式](./03_header.md)

## 背景知识

* [移动平台压缩纹理总结](https://zhuanlan.zhihu.com/p/59518368)
* [Unity2019.4总结的压缩纹理格式](https://docs.unity3d.com/cn/2019.4/Manual/class-TextureImporterOverride.html)
* [几种主流压缩纹理的实现原理](https://blog.csdn.net/BugRunner/article/details/50538770)

## 简介

2012年提出 ASTC，自适应可伸缩纹理压缩格式；（Adaptive Scalable Texture Compression）

* 移动显卡上：支持硬件解压；
	+ iOS：苹果从 A8 处理器开始支持（即从iphone6，iPad mini 4 开始）
	+ Android
		- 全部支持：运行 Vulkan 或 OpenGL ES 3.1 的所有设备；
		- 部分支持：运行 OpenGL ES 3.0 的部分设备；
* 基于块的有损纹理压缩格式
* 有着较高的压缩率和较好的压缩质量。
* 有多种压缩率可选
  
![](../img/m_ff0a62261bcdd01684f6c2cf6c8dd173_r.png)

## 参考

* [2020年 压缩纹理](https://aras-p.info/blog/2020/12/08/Texture-Compression-in-2020/)
* [astc简介](https://github.com/ARM-software/astc-encoder/blob/master/Docs/FormatOverview.md)
* [khronos 压缩格式规范 1.2版本](https://www.khronos.org/registry/DataFormat/specs/1.2/dataformat.1.2.html)
* [C++库: astc-encoder](https://github.com/ARM-software/astc-encoder/)