- [创建 ID3D12Device::Create***](#创建-id3d12devicecreate)
  - [1.1. View](#11-view)
  - [1.2. Command](#12-command)
  - [1.3. `PSO` Pipeline State Objects](#13-pso-pipeline-state-objects)
    - [1.3.1. 构建 `PSO` 需要](#131-构建-pso-需要)
  - [1.4. 堆](#14-堆)
  - [1.5. 显存](#15-显存)
  - [1.6. 其他](#16-其他)

# 创建 ID3D12Device::Create***

## 1.1. View

|简写|create内容||
|--|--|--|
|`CBV`|ConstantBufferView||类似 Vulkan 的 `Push Const`|
|`SRV`|ShaderResourceView|类似 Vulkan 的 `UBO` 或 `Sampled Texture`，Shader 只读|
|`UAV`|UnorderedAccessView|类似 Vulkan 的 `StorageBuffer`、`Storage Texture` Shader 可读写|
|`DSV`|DepthStencilView|深度模板 目标|
|`RTV`|RenderTargetView|颜色 目标|

## 1.2. Command

|create内容|作用|说明|
|--|--|--|
|CommandAllocator||
|CommandList||
|CommandQueue||
|CommandSignature|||

## 1.3. `PSO` Pipeline State Objects

|create内容|作用|说明|
|--|--|--|
|ComputePipelineState|||
|GraphicsPipelineState|||

### 1.3.1. 构建 `PSO` 需要

+ `InputLayout`: VB 布局
+ `RootSignature`: Uniform
+ `Shader`
+ `PrimitiveTopologyType`: 图元类型
+ `RasterizerState`
+ `BlendState`
+ `DepthStencilState`
+ `SampleDesc`: MSAA
+ `RTV` 格式 个数
+ `DSV` 格式

## 1.4. 堆

|create内容|作用|说明|
|--|--|--|
|Heap|placed resources and reserved resources|
|QueryHeap||
|DescriptorHeap|||

## 1.5. 显存

|create内容|作用|说明|
|--|--|--|
|CommittedResource|both a resource and an implicit heap|
|PlacedResource||
|ReservedResource||

## 1.6. 其他

|create内容|作用|说明|
|--|--|--|
|Sampler|||
||Fence|||
|RootSignature|||
|SharedHandle|||