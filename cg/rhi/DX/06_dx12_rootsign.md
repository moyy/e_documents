- [Root Signature](#root-signature)
	- [1. 三种 Root Parameter](#1-三种-root-parameter)
		- [1.1. Constants](#11-constants)
		- [1.2. Views](#12-views)
		- [1.3. Descriptor Table](#13-descriptor-table)
	- [3. Root Parameter 建议](#3-root-parameter-建议)

# Root Signature

+ 类似 WebGPU 的 BindGroup
+ 创建 Root Signature 需要 Root Paramater 数组

## 1. 三种 Root Parameter

+ Root Paramater 数组 占用总空间 不超过 128B
+ 因此一般很少用到 Constants

### 1.1. Constants

+ 访问速度 最快
+ 只能 表示 Constant Buffer, 占用 存储空间 和 Constant buffer 的 大小 相同

### 1.2. Views

+ 每项 4B, 访问速度 中等
+ 表示 `CSV`. `SRV`. `UAV`
	- SRV 不能是 Texture

### 1.3. Descriptor Table

+ 访问速度 最慢；
+ 由一组 Descriptor Range 初始化
	- 每个 Range 表示 一组 连续地址的 `SRV`/`UAV`/`CBV`/`Sampler`
	- 每项 占用 2B

## 3. Root Parameter 建议

+ 尽可能小;
+ 经常变化的，在数组前面
+ PSO 共享 Root Signature，减少切换