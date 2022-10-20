- [资源 与 视图](#资源-与-视图)
	- [1. ID3D12Resource](#1-id3d12resource)
		- [1.1. 创建描述：CD3DX12_RESOURCE_DESC](#11-创建描述cd3dx12_resource_desc)
		- [1.2. 创建](#12-创建)
		- [1.3. 资源种类：`TODO`](#13-资源种类todo)
			- [1.3.1. 例子：静态 Buffer](#131-例子静态-buffer)
		- [1.4. 更改内容](#14-更改内容)
	- [2. 视图](#2-视图)
		- [2.1. `SRV`: Shader Resource View](#21-srv-shader-resource-view)
		- [2.2. `UAV`: Unordered Access View](#22-uav-unordered-access-view)
		- [2.3. `CBV`: Constant Buffer View](#23-cbv-constant-buffer-view)
		- [2.4. `DSV`: Depth Stencil View 深度模板目标](#24-dsv-depth-stencil-view-深度模板目标)
		- [2.5. `RTV`: Render Target View 颜色目标](#25-rtv-render-target-view-颜色目标)

# 资源 与 视图

## 1. ID3D12Resource

Resource 代表 显存 资源

### 1.1. 创建描述：CD3DX12_RESOURCE_DESC

|描述种类||
|--|--|
|CD3DX12_RESOURCE_DESC::`Buffer`||
|CD3DX12_RESOURCE_DESC::`Tex1D`||
|CD3DX12_RESOURCE_DESC::`Tex2D`||
|CD3DX12_RESOURCE_DESC::`Tex3D`||

### 1.2. 创建

``` c++
ID3D12Resource *resource;
m_device->CreateCommittedResource(&resource);
```

### 1.3. 资源种类：`TODO`

+ `ReservedResource`: 没有分配实际显存
+ `CommittedResource`: both a resource and an implicit heap
+ `PlacedResource`

#### 1.3.1. 例子：静态 Buffer

+ 静态Buffer: `VB`, `IB`, `Texture Buffer`
+ 创建 静态资源 时，Heap 类型 为 D3D12_HEAP_TYPE_DEFAULT，无法直接从内存上传；
+ 内存 --> `staging buffer` --> 静态Buffer
+ staging buffer，Heap 类型 为 D3D12_HEAP_TYPE_UPLOAD

### 1.4. 更改内容

+ resource->Map(...);
+ resource->Unmap(...);

## 2. 视图

|简写|create内容||
|--|--|--|
|`CBV`|ConstantBufferView||类似 Vulkan 的 `Push Const`|
|`SRV`|ShaderResourceView|类似 Vulkan 的 `UBO` 或 `Sampled Texture`，Shader 只读|
|`UAV`|UnorderedAccessView|类似 Vulkan 的 `StorageBuffer`、`Storage Texture` Shader 可读写|
|`DSV`|DepthStencilView|深度模板 目标|
|`RTV`|RenderTargetView|颜色 目标|

### 2.1. `SRV`: Shader Resource View

+ Shader 只读
+ 类似 Vulkan 的 `UBO` 或 `Sampled Texture`
+ struct D3D12_SHADER_RESOURCE_VIEW_DESC

|种类|作用|
|--|--|
|Buffer||
|Texture1D||
|Texture1DArray||
|Texture2D||
|Texture2DArray||
|Texture2DMS||
|Texture2DMSArray||
|Texture3D||
|TextureCube||
|TextureCubeArray||
|RaytracingAccelerationStructure||

### 2.2. `UAV`: Unordered Access View

+ Shader 可读写
+ 类似 Vulkan 的 `StorageBuffer`、`Storage Texture`
+ struct D3D12_UNORDERED_ACCESS_VIEW_DESC

|种类|作用|
|--|--|
|Buffer||
|Texture1D||
|Texture1DArray||
|Texture2D||
|Texture2DArray||
|Texture3D||

### 2.3. `CBV`: Constant Buffer View

+ 常量缓冲区，容量极小，速度很快
+ 类似 Vulkan 的 `Push Const`
+ struct D3D12_CONSTANT_BUFFER_VIEW_DESC
	- D3D12_GPU_VIRTUAL_ADDRESS BufferLocation;
	- UINT SizeInBytes;

### 2.4. `DSV`: Depth Stencil View 深度模板目标

### 2.5. `RTV`: Render Target View 颜色目标