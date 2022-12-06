[TOC]

# DownlevelCapabilities

## 1、作用

底层兼容性，WebGL/GLES 不支持的 WebGPU 特性 的 枚举

### 1.1、ShaderModel

|枚举值|作用|
|--|--|
|SM2|DX 9.0c|
|SM4|DX 10.1|
|**SM5**|DX 11.0|

### 1.2、[DownlevelFlags](https://docs.rs/wgpu/latest/wgpu/struct.DownlevelFlags.html)

|值|作用|
|--|--|
|**COMPUTE_SHADERS**|支持 CS，WebGL2/GLES3 不支持|
|**FRAGMENT_WRITABLE_STORAGE**|在 fs 上 使用 storage|
|INDIRECT_EXECUTION|间接渲染，WebGL2/GLES3 不支持|
|BASE_VERTEX|draw 的 base_vertex 传 非0值|
|READ_ONLY_DEPTH_STENCIL|shader中 读 ds 数据，WebGL2/GLES3 不支持|
|**NON_POWER_OF_TWO_MIPMAPPED_TEXTURES**|纹理 支持 非2的幂 的 mipmap|
|CUBE_ARRAY_TEXTURES|是否 支持 cube-array 纹理|
|COMPARISON_SAMPLERS|用于 比较的采样器|
|INDEPENDENT_BLEND|每color 分开指定 混合操作|
|**VERTEX_STORAGE**|vs 支持 storage buffer|
|**ANISOTROPIC_FILTERING**|各向异性过滤|
|**FRAGMENT_STORAGE**|fs 支持 storage buffer|
|MULTISAMPLED_SHADING|sample-rate shading|
|DEPTH_TEXTURE_AND_BUFFER_COPIES|深度 纹理 和 buffer间 拷贝，WebGL2/GLES3 不支持|
|**WEBGPU_TEXTURE_FORMAT_SUPPORT**|能用 webgpu 规定的 所有 纹理 usage|
|BUFFER_BINDINGS_NOT_16_BYTE_ALIGNED|可绑 非 16倍数 大小的 buffer，WebGL2 不支持|

## 2、测试数据

+ 笔记本 Win 11, "NVIDIA GeForce RTX 3050 Ti Laptop GPU"
+ vivo iQOO U3X, "Adreno (TM) 619"

|字段|笔记本 Win 11/**Vulkan**|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|--|
|**COMPUTE_SHADERS**|✔️|✔️|✔️|
|**FRAGMENT_WRITABLE_STORAGE**|✔️|✔️|✔️|
|**INDIRECT_EXECUTION**|✔️|✔️|✔️|
|BASE_VERTEX|✔️|✔️|✔️|
|READ_ONLY_DEPTH_STENCIL|✔️|✔️||
|NON_POWER_OF_TWO_MIPMAPPED_TEXTURES|✔️|✔️|✔️|
|CUBE_ARRAY_TEXTURES|✔️|✔️|✔️|
|COMPARISON_SAMPLERS|✔️|✔️|✔️|
|INDEPENDENT_BLEND|✔️|✔️|✔️|
|**VERTEX_STORAGE**|✔️|✔️|✔️|
|ANISOTROPIC_FILTERING|✔️|✔️||
|**FRAGMENT_STORAGE**|✔️|✔️|✔️|
|MULTISAMPLED_SHADING|✔️|✔️||
|DEPTH_TEXTURE_AND_BUFFER_COPIES|✔️|✔️||
|**WEBGPU_TEXTURE_FORMAT_SUPPORT**|✔️|✔️||
|BUFFER_BINDINGS_NOT_16_BYTE_ALIGNED|✔️|✔️|✔️|