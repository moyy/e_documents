[TOC]

# 支持的纹理格式

WebGPU 不支持 RGB 格式的 24-bit 纹理

## 1、Surface.get_supported_formats

+ 笔记本 Win 11, "NVIDIA GeForce RTX 3050 Ti Laptop GPU"
+ vivo iQOO U3X, "Adreno (TM) 619"

|字段|笔记本 Win 11/**Vulkan**|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|--|
|Rgba8UnormSrgb||✔️|✔️|
|Bgra8UnormSrgb|✔️||✔️|
|Rgba8Unorm||✔️||
|Bgra8Unorm|✔️|||
|Rgba16Float||✔️||
|Rgb10a2Unorm|✔️|✔️||

## 2、Adapter: `不支持` 的 纹理格式

``` rs
// 以 srgb 为例
let format = wgpu::TextureFormat::Rgba8UnormSrgb;

let features = adapter.get_texture_format_features(format);

// 用途 为空，则 不被当前显卡 所 支持
let is_unsupport = features.allowed_usages.is_empty();

```


+ 笔记本 Win 11, "NVIDIA GeForce RTX 3050 Ti Laptop GPU"
+ vivo iQOO U3X, "Adreno (TM) 619"

|字段|笔记本 Win 11/**Vulkan**|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|--|
|R16[S/U]norm|||❌|
|Rg16[S/U]norm|||❌|
|Rgba16[S/U]norm|||❌|
|压缩纹理: DDS/Bc系列||❌|❌|
|压缩纹理: Astc系列|❌|||
|压缩纹理: Etc2系列|❌|||
|压缩纹理: Eac系列|❌|||

## 3、Adapter: `不支持` 作为 渲染目标 的 纹理格式

+ 一般来说，压缩纹理，不支持 作为 渲染目标

### 3.1、windows

+ 笔记本 Win 11, "NVIDIA GeForce RTX 3050 Ti Laptop GPU"

|字段|笔记本 Win 11/**Vulkan**|
|--|--|
|`Rgb9e5Ufloat`|❌|
|压缩纹理: Bc系列|❌|

### 3.2、Android

+ vivo iQOO U3X, GPU: “Adreno (TM) 619”

|字段|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|
|`R8Snorm`||❌|
|`Rg8Snorm`||❌|
|`Rgba8Snorm`||❌|
|`Rgb9e5Ufloat`|❌|❌|
|压缩纹理: Etc2系列|❌|❌|
|压缩纹理: Eac系列|❌|❌|