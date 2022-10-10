- [GPU 监控 工具](#gpu-监控-工具)
  - [1. PC](#1-pc)
  - [2. 移动](#2-移动)
  - [3. 以 AGI 为例](#3-以-agi-为例)

# [GPU 监控 工具](https://github.com/bombomby/optick-rs)

## 1. PC

+ [CapFrameX 帧率监听](https://www.capframex.com/features)
+ [RenderDoc](https://renderdoc.org/)
	- PC 和 Mail 的 GPU 比较稳定
	- 对于 高通的 GPU相对来说 不 稳 定
+ [Windows：PIX on Windows](https://devblogs.microsoft.com/pix/download/)
+ [nvidia: nsight](https://developer.nvidia.com/nsight-visual-studio-edition-2022_2)
+ [Intel: GPA](https://www.intel.com/content/www/us/en/developer/tools/graphics-performance-analyzers/overview.html)
+ [AMD GPUOpen: Radeon GPU Profiler](https://gpuopen.com/rgp/)
	- Radeon Developer Panel
	- Radeon GPU Analyzer
	- Radeon GPU Profiler
	- Radeon Memory Visualizer

## 2. 移动

+ [华为, huawei-graphics-profiler](https://developer.huawei.com/consumer/cn/huawei-graphics-profiler)
+ [agi: Android GPU Inspector](https://gpuinspector.dev/)
+ [gapid: Google Graphics API Debugger](https://github.com/google/gapid)
+ [高通 Adreno：Adreno Profiler](https://developer.qualcomm.com/software/snapdragon-profiler)
+ [ARM Mali: Arm Mobile Studio](https://developer.arm.com/Tools%20and%20Software/Arm%20Mobile%20Studio)
	- [Mali Offline Compiler](https://developer.arm.com/Tools%20and%20Software/Mali%20Offline%20Compiler)
+ Apple, Metal: XCode

## 3. 以 AGI 为例

+ 对 GPU 来说，最具挑战性的任务之一 就是 在着色器中 获取和过滤纹理数据。
+ 通过采集 带宽 、缓存行为、滤镜渲染 三个方面的数据，使用 AGI 监视与纹理相关的 GPU 工作负载。