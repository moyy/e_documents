- [FrameGraph](#framegraph)
	- [1. 模型](#1-模型)
	- [2. 概述](#2-概述)
	- [3.ShaderGraph](#3shadergraph)
	- [4. RenderGraph/FrameGraph](#4-rendergraphframegraph)
		- [4.1. Pre-Z-Pass RenderPass](#41-pre-z-pass-renderpass)
		- [4.2. Shadow-Cast RenderPass](#42-shadow-cast-renderpass)
	- [5. System-调度](#5-system-调度)
		- [5.1. 特殊 Stage](#51-特殊-stage)
		- [5.2. 二分图：component-system](#52-二分图component-system)
	- [6. 参考](#6-参考)

# FrameGraph

## 1. 模型

有向无环图 + 拓扑排序 来 分析 依赖关系

## 2. 概述

## 3.ShaderGraph

+ 根据Shader名称描述 + 动态 宏声明，动态的 生成和组装Shader
+ 输出：一个完整的 VS，FS，GS 等
+ 参考：Babylon 的 NodeMaterial
+ `注`：不关心 参数值 的 传递流程 和 声明周期管理，不关心运行时，只关心 Shader代码 的 组装，和 编译后 有哪些 变量信息

## 4. RenderGraph/FrameGraph

+ 关心 RenderPass 和 RenderPass 的 组装关系
+ 一帧 渲染到 屏幕的 整个渲染流程
+ RenderPass：网格 用一个Shader 渲染到 一次 渲染目标 的 渲染过程；

举例：一堆物体渲染到屏幕，中间经过 如下几个 RenderPass

### 4.1. Pre-Z-Pass RenderPass

目的：将物体先渲染到深度缓冲区，后面的物体就可以利用GPU的Pre—Z技术 去 减少 FS的 执行，因为FS的 逐像素光影 计算 是 计算大头。

做什么：将 不透明物体的 深度信息 先渲染到深度缓冲区；（通过 关掉颜色写 就可以做到这点）

### 4.2. Shadow-Cast RenderPass

目的：为了 让 场景物体看到实时阴影，需要将阴影渲染到一个 FBO-Texture 中；

做什么：对一个标注需要 生成阴影的Mesh，将相机 调整到 光照方向上，然后渲染一次到 阴影图 (ShadowMap) 中

有2个灯光需要阴影，就需要 2个 Shadow Map RenderPass

## 5. System-调度

+ Scheduler 调度器
+ Stage 串行
	- 串行: System 依次执行
	- 并行：内部的System根据 DAG 依赖分析后执行
+ 结构：Map-Reduce

### 5.1. 特殊 Stage

+ Prepare-Stage 永远是第一个 Stage
+ 如果 有循环依赖，就 通过指定另一个用于同步的Component来强行拆分；

### 5.2. 二分图：component-system

二分图 连通分量

## 6. 参考

+ [使用有向无环图组织 GPU 工作](https://levelup.gitconnected.com/organizing-gpu-work-with-directed-acyclic-graphs-f3fd5f2c2af3)
+ [Render Graph与现代图形API](https://zhuanlan.zhihu.com/p/425830762)
+ [为什么要谈论渲染图](https://logins.github.io/graphics/2021/05/31/RenderGraphs.html)
+ [Early Adopters Program](https://ourmachinery.com/post/)