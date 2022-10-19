- [RHI：渲染硬件接口](#rhi渲染硬件接口)
  - [1. Shader](#1-shader)
  - [2. 图形 API](#2-图形-api)
  - [3. API 规范](#3-api-规范)
  - [4. 名称](#4-名称)
  - [5. 分类](#5-分类)
    - [5.1. 初始化](#51-初始化)
    - [5.2. 交换链](#52-交换链)
    - [5.3. 资源：Command](#53-资源command)
    - [5.4. 资源：Buffer, Texture, Sampler](#54-资源buffer-texture-sampler)
    - [5.5. 资源：Shader](#55-资源shader)
    - [5.6. 资源：状态](#56-资源状态)
    - [5.7. 资源：同步](#57-资源同步)
    - [5.8. 资源：查询](#58-资源查询)
  - [6. 参考](#6-参考)

# RHI：渲染硬件接口

+ [DX](./DX/README.md)
+ [GL](./GL/README.md)
+ [Metal](./Metal/README.md)
+ [Vulkan](./Vulkan/README.md)
+ [WebGPU](./WebGPU/README.md)
+ [Rafx](./rafx-api/README.md)

## 1. Shader

**注1：** WebGL = GLES-2 = GL-2.1

**注2：** WebGL2 = GLES-3.0 = GL-3.3/4.0

|阶段|名称|Vulkan|Metal|WebGPU|DX|GLES|
|--|--|--|--|--|--|--|
|`VS`|顶点着色器|Y|Y|Y|9+|2+|
|`PS` / `FS`|像素着色器|Y|Y|Y|9+|2+|
|`CS`|计算着色器|Y|Y|Y|11+|3.1+|
|`GS`|几何着色器|Y|`n/a`|`n/a`|11+|3.2|
|`HS`|曲面细分|Y|Y|`n/a`|11+|3.2|

## 2. 图形 API

|图形API|适用系统|着色语言|
|--|--|--|
|DX12|Windows 10|HLSL/DXIL|
|Vulkan|Windows / Android|SPIR-V|
|Metal|iOS、MacOS|MSL|
|WebGPU|浏览器|WGSL|
|GL/GLES|跨平台|GLSL|
|DX11|Windows 7|HLSL|

## 3. API 规范

[API 规范](https://github.com/gpuweb/gpuweb/issues/416)

NDC(标准化设备坐标)，XY范围：[-1, 1]，Z范围：[近裁剪面, 1.0]

||Vulkan|Metal|[WebGPU](https://github.com/gpuweb/gpuweb/issues/416)|DX12|DX11|GL4.6|[Unity](https://docs.unity3d.com/cn/2020.3/Manual/SL-PlatformDifferences.html)|
|--|--|--|--|--|--|--|--|
|世界坐标系|右手||左手|左手; y-up|左手; y-up|右手; y-up|左手; y-up|
|NDC坐标系|右手|左手|左手|左手|左手|左手|左手|
|NDC: XY (-1,-1)|左上|左下|左下|左下|左下|左下||
|NDC: 近裁剪面Z值|0|0|0|0|0|-1|UNITY_NEAR_CLIP_VALUE: OpenGL -1, Else: 0|
|Texture (0,0)|左上|左上|左上|左上|左上|左下|左下|
|Vieport/Fragment (0,0)|左上|左上|左上|左上|左上|左下|左下|
|Shader 格式|spirv|msl|wgsl|HLSL SM-6.x|HLSL SM-5.x|GLSL 450|HLSL|
|矩阵行列|default `row` major||only `column` major|default `row` major|default `row` major|default `column` major|default `row` major|

## 4. 名称

+ RTV: Render Target View
+ SRV: Shader Resource View
+ UAV: Unordered Access View
+ ROV: Rasterizer Order View
+ CBV: Constant Buffer View

|名称|DX12|Vulkan|GL|Metal|
|--|--|--|--|--|
||adapter|physical device||device|
||device|logical device|context|device|
||window|surface|HDC, GLXDrawable, EGLSurface|layer|
||swapchain|swapchain|Pairt of HDC, GLXDrawable, EGLSurface|layer
|||swapchain image|default framebuffer|drawable texture|
||CBV|Uniform Buffer|Uniform Buffer|buffer in constant address space|
||ROV|fragment shader interlock|GL_ARB_fragment_shader_interlock|raster order group|
||UAV -- Raw/Structured Buffer|storage buffer|storage buffer|buffer in device address space|
||typed buffer SRV/UAV|buffer view/texel buffer|texture buffer|texture buffer|
||RTV, SRV, UAV, depth/stencil view|image view|texture view|texture view|
||descriptor table|descriptor set||argument buffer|
||Stream Out|transform feedback|transform feedback|`n/a`|
||root signature|pipeline layout|||
||root parameter|descriptor set layout binding, push descriptor||argument in shader parameter list|
||command list|command buffer|part of context, display list, NV_command_list|command buffer|
||command list|secondary command buffer||parallel command encoder|
||command list bundle||light-weight display list|indirect command buffer|
||pixel shader|fragment shader|fragment shader|fragment shader|
||hull shader|tessellation control shader|tessellation control shader|tessellation compute kernel|
||domain shader|tessellation evaluation shader|tessellation evaluation shader|post-tessellation vertex shader|

## 5. 分类

### 5.1. 初始化

|概念|DX12|DX11|Vulkan|Metal|GL 4.6|WebGPU|UE4.27|说明|
|--|--|--|--|--|--|--|--|--|
|`Instance`: 入口|IDXGIFactory4|IDXGIFactory|vk::Instance|CAMetalLayer|EGL/WGL|GPUInstance|FDynamicRHI||
|物理设备|IDXGIAdapter1|IDXGIAdapter|vk::PhysicalDevice|MTLDevice|glGetString(GL_VENDOR)|GPUAdapter|`n/a`||
|`Device` 逻辑设备|ID3D12Device|ID3D11Device|vk::Device|MTLDevice|`n/a`|GPUDevice|`n/a`||

### 5.2. 交换链

|概念|DX12|DX11|Vulkan|Metal|GL 4.6|WebGPU|UE4.27|说明|
|--|--|--|--|--|--|--|--|--|
|`Surface`: 表面|ID3D12Resource|ID3D11Texture2D|vk::Surface|CAMetalLayer|EGL/WGL||FRHIRenderTargetView||
|`Swapchain`: 交换链|IDXGISwapChain3|IDXGISwapChain|vk::Swapchain|CAMetalDrawable|EGL/WGL||`n/a`||
|`Frame Buffer`: 帧缓冲区|ID3D12Resource|ID3D11RenderTargetView|vk::Framebuffer|MTLRenderPassDescriptor|FBO||FRHIRenderTargetView||

### 5.3. 资源：Command

|概念|DX12|DX11|Vulkan|Metal|GL 4.6|WebGPU|UE4.27|说明|
|--|--|--|--|--|--|--|--|--|
|`Queue`: 队列|ID3D12CommandQueue|ID3D11DeviceContext|vk::Queue|MTLCommandQueue|`n/a`||||
|`Pool`: 分配器|ID3D12CommandAllocator|ID3D11DeviceContext|vk::CommandPool|MTLCommandQueue|`n/a`||||
|`Command Buffer`|ID3D12GraphicsCommandList|ID3D11DeviceContext|vk::CommandBuffer|MTLRenderCommandEncoder|`n/a`|GPUCommandEncoder|FRHICommandList||
|`Command List`|ID3D12CommandList[]|ID3D11CommandList|vk::SubmitInfo|MTLCommandBuffer|`n/a`|GPUCommandBuffer|FRHICommandList||

|概念|DX12|DX11|Vulkan|Metal|GL 4.6|WebGPU|UE4.27|说明|
|--|--|--|--|--|--|--|--|--|
|`Render Pass`|Begin/End RenderPass||VkRenderPass|MTLRenderPassDescriptor|`n/a`|GPUComputePassEncoder / GPURenderPassEncoder|FRHIRenderPassInfo||
|`SubPass`|`n/a`|`n/a`|VkSubpassDescription|Programmable Blending|Pixel Local Storage||FRHIRenderPassInfo||

### 5.4. 资源：Buffer, Texture, Sampler

|概念|DX12|DX11|Vulkan|Metal|GL 4.6|WebGPU|UE4.27|说明|
|--|--|--|--|--|--|--|--|--|
|`Heap`: 堆|ID3D12Resource, ID3D12Heap|`n/a`|Vk::MemoryHeap|MTLBuffer|`n/a`|`n/a`|FRHIResource||
|`Buffer`: 缓冲区|ID3D12Resource|ID3D11Buffer|vk::Buffer & vk::BufferView|MTLBuffer|GLuint|GPUBuffer|FRHIIndexBuffer, FRHIVertexBuffer||
|`Texture`: 纹理|ID3D12Resource|ID3D11Texture2D|vk::ImageView|MTLTexture|GLuint|GPUTexture|FRHITexture||
|`TextureView`: 纹理视图|||vk::ImageView|||GPUTextureView|FRHITexture||
|`Sampler`: 采样器||||||GPUSampler|||

### 5.5. 资源：Shader

|概念|DX12|DX11|Vulkan|Metal|GL 4.6|WebGPU|UE4.27|说明|
|--|--|--|--|--|--|--|--|--|
|`Shader`: 着色器|ID3DBlob|ID3D11VertexShader, ID3D11PixelShader|vk::ShaderModule|MTLLibrary|GLuint|GPUShaderModule|FRHIShader||
|`Shader Binding`: 参数，Uniform Buffer|ID3D12RootSignature|ID3D11DeviceContext::VSSetConstantBuffers|vk::PipelineLayout & vk::DescriptorSet|[MTLRenderCommandEncoder setVertexBuffer: uniformBuffer]|Uniform Buffer Object|GPUBuffer|FRHIUniformBuffer||
|`Descriptor Table`|D3D12_ROOT_DESCRIPTOR_TABLE||VkDescriptorSetLayoutCreateInfo|argument buffer||GPUBindGroup||
|`Descriptor`|D3D12_ROOT_DESCRIPTOR||VkDescriptorBufferInfo, VkDescriptorImageInfo|argument||||
|`Descriptor Heap`||ID3D12DescriptorHeap|VkDescriptorPoolCreateInfo|heap||||
|`Root Parameter`|D3D12_ROOT_PARAMETER||VkDescriptorSetLayoutBinding|argument in shader parameter list||GPUBindGroupLayout||

### 5.6. 资源：状态

|概念|DX12|DX11|Vulkan|Metal|GL 4.6|WebGPU|UE4.27|说明|
|--|--|--|--|--|--|--|--|--|
|`Root Signature`: 管线布局|ID3D12RootSignature||VkPipelineLayoutCreateInfo|||GPUPipelineLayout|||
|`Pipeline`: 状态|ID3D12PipelineState|大量的函数调用|vk::Pipeline|MTLRenderPipelineState|大量的函数调用|GPUComputePipeline/GPURenderPipeline|FGraphicsPipelineStateInitializer||

### 5.7. 资源：同步

|概念|DX12|DX11|Vulkan|Metal|GL 4.6|WebGPU|UE4.27|说明|
|--|--|--|--|--|--|--|--|--|
|`Fence`: 栅栏|ID3D12Fence|ID3D11Fence|vk::Fence|MTLFence|glFenceSync||FRHIGPUFence||
|`Semaphore`: 信号量|HANDLE|HANDLE|vk::Semaphore|dispatch_semaphore_t|EGL/WGL|`n/a`|`n/a`||
|`Barrier`: 屏障|D3D12_RESOURCE_BARRIER|`n/a`|vkCmdPipelineBarrier|MTLFence|glMemoryBarrier|`n/a`|FRDGBarrierBatch||
|`Event`: 事件|`n/a`|`n/a`|Vk::Event|MTLEvent, MTLSharedEvent|EGL/WGL|`n/a`|FEvent||

### 5.8. 资源：查询

|概念|DX12|DX11|Vulkan|Metal|GL 4.6|WebGPU|UE4.27|说明|
|--|--|--|--|--|--|--|--|--|
|`QuerySet`||||||GPUQuerySet|||

## 6. 参考

+ [Shader语言 概述](https://alain.xyz/blog/a-review-of-shader-languages)
+ [现代图形 API](https://alain.xyz/blog/comparison-of-modern-graphics-apis)
+ [剖析虚幻渲染体系（13）- RHI 详解](https://www.cnblogs.com/timlly/p/15680064.html)