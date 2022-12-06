[TOC]

# [Features](https://docs.rs/wgpu/latest/wgpu/struct.Features.html)

+ `Feature`: 特性
+ 使用 某个 Feature 之前，必须 查询 是否可用；

## 1、作用

### 1.1、Web & Native

|字段|作用|
|--|--|
|`DEPTH_CLIP_CONTROL`|指定 深度 裁剪范围|
|`DEPTH24UNORM_STENCIL8`|u24-深度，8bit-模板|
|`DEPTH32FLOAT_STENCIL8`|f32-深度，8bit-模板|
|`TEXTURE_COMPRESSION_BC`|DDS 压缩|
|TEXTURE_COMPRESSION_ETC2|ec2 压缩纹理|
|`TEXTURE_COMPRESSION_ASTC_LDR`|astc 压缩纹理|
|INDIRECT_FIRST_INSTANCE|间接渲染，第一个isntance索引 可以 不是 0|
|TIMESTAMP_QUERY|查询 时间戳|
|PIPELINE_STATISTICS_QUERY|查询 pipeline 统计情况|
|`SHADER_FLOAT16`|shader 支持 f16|

### 1.2、Native

仅 wgpu-rs 支持，webgpu 不支持的特性；

|字段|作用|
|--|--|
|MAPPABLE_PRIMARY_BUFFERS|去掉 webgpu限制：map 必须和 copy 的 usage 匹配|
|TEXTURE_BINDING_ARRAY|`uniform texture2D textures[10]`|
|BUFFER_BINDING_ARRAY|`uniform myBuffer { .... } buffer_array[10]`|
|STORAGE_RESOURCE_BINDING_ARRAY|shader 可用 storage buffer 或 texture 数组|
|SAMPLED_TEXTURE_AND_STORAGE_BUFFER_ARRAY_NON_UNIFORM_INDEXING|用变量 索引纹理 `texture_array[vertex_data]`|
|UNIFORM_BUFFER_AND_STORAGE_TEXTURE_ARRAY_NON_UNIFORM_INDEXING||
|PARTIALLY_BOUND_BINDING_ARRAY|绑定的 bindgroup 的 数组长度 小于 布局指定的|
|`MULTI_DRAW_INDIRECT`|间接渲染 -- 多次|
|MULTI_DRAW_INDIRECT_COUNT|间接渲染 -- 多次|
|`PUSH_CONSTANTS`|push constants|
|ADDRESS_MODE_CLAMP_TO_BORDER|sampler 寻址模式 clamp-to-border|
|`POLYGON_MODE_LINE`|边框 模式 渲染|
|`POLYGON_MODE_POINT`|点 模式 渲染|
|TEXTURE_ADAPTER_SPECIFIC_FORMAT_FEATURES||
|SHADER_FLOAT64|shader 支持 f64|
|VERTEX_ATTRIBUTE_64BIT|vs 输入 支持 u64|
|CONSERVATIVE_RASTERIZATION||
|VERTEX_WRITABLE_STORAGE|vs 支持 吸入 storage buffer|
|`CLEAR_TEXTURE`|纹理 清颜色|
|SPIRV_SHADER_PASSTHROUGH||
|SHADER_PRIMITIVE_INDEX||
|`MULTIVIEW`|多目标 渲染|
|TEXTURE_FORMAT_16BIT_NORM|纹理格式 u16|
|ADDRESS_MODE_CLAMP_TO_ZERO|sampler 寻址：clamp-to-0|
|TEXTURE_COMPRESSION_ASTC_HDR|astc hdr 纹理压缩|
|WRITE_TIMESTAMP_INSIDE_PASSES||

## 2、测试数据

### 2.1、Web & Native

+ 笔记本 Win 11, "NVIDIA GeForce RTX 3050 Ti Laptop GPU"
+ vivo iQOO U3X, "Adreno (TM) 619"

|字段|笔记本 Win 11/**Vulkan**|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|--|
|DEPTH_CLIP_CONTROL|✔️|||
|DEPTH24UNORM_STENCIL8|✔️|✔️||
|DEPTH32FLOAT_STENCIL8|✔️|✔️||
|TEXTURE_COMPRESSION_BC|✔️|||
|TEXTURE_COMPRESSION_ETC2||✔️|✔️|
|TEXTURE_COMPRESSION_ASTC_LDR||✔️|✔️|
|INDIRECT_FIRST_INSTANCE|✔️|✔️||
|TIMESTAMP_QUERY|✔️|✔️||
|PIPELINE_STATISTICS_QUERY|✔️|||
|SHADER_FLOAT16|✔️|||

### 2.2、Native

+ 笔记本 Win 11, "NVIDIA GeForce RTX 3050 Ti Laptop GPU"
+ vivo iQOO U3X, "Adreno (TM) 619"

|字段|笔记本 Win 11/**Vulkan**|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|--|
|MAPPABLE_PRIMARY_BUFFERS|✔️|✔️|
|TEXTURE_BINDING_ARRAY|✔️|✔️|
|BUFFER_BINDING_ARRAY|✔️|✔️|
|STORAGE_RESOURCE_BINDING_ARRAY|✔️|✔️|
|SAMPLED_TEXTURE_AND_STORAGE_BUFFER_ARRAY_NON_UNIFORM_INDEXING|✔️||
|UNIFORM_BUFFER_AND_STORAGE_TEXTURE_ARRAY_NON_UNIFORM_INDEXING|✔️||
|PARTIALLY_BOUND_BINDING_ARRAY|✔️||
|MULTI_DRAW_INDIRECT|✔️|✔️|
|MULTI_DRAW_INDIRECT_COUNT|✔️|✔️|
|**PUSH_CONSTANTS**|✔️|✔️|✔️|
|**ADDRESS_MODE_CLAMP_TO_BORDER**|✔️|✔️|✔️|
|POLYGON_MODE_LINE|✔️|✔️|
|POLYGON_MODE_POINT|✔️|✔️|
|**TEXTURE_ADAPTER_SPECIFIC_FORMAT_FEATURES**|✔️|✔️|✔️|
|SHADER_FLOAT64|✔️||
|VERTEX_ATTRIBUTE_64BIT|||
|CONSERVATIVE_RASTERIZATION|✔️||
|**VERTEX_WRITABLE_STORAGE**|✔️|✔️|✔️|
|**CLEAR_TEXTURE**|✔️|✔️|✔️|
|SPIRV_SHADER_PASSTHROUGH|✔️|✔️|
|SHADER_PRIMITIVE_INDEX|✔️|✔️|
|MULTIVIEW|✔️|✔️|
|TEXTURE_FORMAT_16BIT_NORM|✔️|✔️|
|**ADDRESS_MODE_CLAMP_TO_ZERO**|✔️|✔️|✔️|
|TEXTURE_COMPRESSION_ASTC_HDR||✔️|
|WRITE_TIMESTAMP_INSIDE_PASSES|✔️|✔️|

## [扩展特性](https://github.com/gfx-rs/wgpu/wiki/Extensions)

### 1、push constant

类似 WebGL 时代的 Uniform，只是经过 加速的；

放到 GPU 的 常量-缓冲区，shader读取速度极快，适用于 容量极小（一般不超过 256 字节），更新极其频繁 的 场合；

+ `WebGPU` 目前 不 支 持；
+ 需要判断 API 兼容性，不一定支持；
+ 可以 每帧 通过 CommandEncoder 更新；

#### 1.1、glsl 代码

``` glsl
layout( push_constant ) uniform constants
{
	vec4 data;
	mat4 render_matrix;
} PushConstants;
```