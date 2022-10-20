- [D3D12](#d3d12)
  - [1. 入口](#1-入口)
  - [2. 同步原语：`Fence` & `Barrier`](#2-同步原语fence--barrier)
  - [3. 渲染循环](#3-渲染循环)

# D3D12

## 1. 入口

|类|Vulkan 等价物|作用|说明|
|--|--|--|--|
|IDXGIFactory4|VkInstance||
|IDXGIAdapter1|VkPhysicalDevice|
|ID3D12Device|VkDevice|
|IDXGISwapChain3|VkSwapchain||Vulkan的 WSI层 为交换链 提供图像；DX12需要创建资源来表示图像|

## 2. 同步原语：`Fence` & `Barrier`

+ D3D12 没有 信号量，VkSemaphore
+ D3D12 没有 事件，VkEvent

## 3. 渲染循环

+ 等待上次Signal
+ 更新 `CBV`
+ 取到 这帧 渲染 用的 CommandList
	- 切换 `PSO`
	- 设置 Viewport
	- 准备需要的 Texture, 转换格式
	- 设置 `RTV`, `DSV`, `CBV` 等
	- 设置 `RootSignature`
	- 设置 `VertexBuffer` 和 `IndexBuffer`，调用 Draw
+ 提交 `CommandList`
+ `Present` `SwapChain`
+ Signale 信号