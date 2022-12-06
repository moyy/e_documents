[TOC]

# 适配器、设备

## 1、navigation.gpu

``` js
console.log("navigator.gpu = ", navigator.gpu);
```

### 1.1、属性

|名|解释|
|--|--|
|Adapter = await requestAdapter()|请求 适配器|
|[GPUTextureFormat](https://www.w3.org/TR/webgpu/#enumdef-gputextureformat) = getPreferredCanvasFormat()|获取 canvas 的 纹理格式|

## 2、GPUAdapter 适配器

适配器：显卡 抽象

``` js
const adapter = await navigator.gpu.requestAdapter();
```

### 2.2 创建：GPU.requestAdapter

参数：可以不填；

``` js

// GPURequestAdapterOptions
const options = {
    powerPreference: "high-performance", // "low-power" 代表 集显
    forceFallbackAdapter: false, // 一般不管它
};
```

### 2.1 属性

|名|类型|默认值|意义|
|--|--|--|--|
|name|string|"Default"||
|isFallbackAdapter|boolean|false||
|features|GPUSupportedFeatures，是 HashSet |||
|limits|GPUSupportedLimits|||

### 2.2、方法

|名|意义|
|--|--|
|requestDevice()|请求 对应的 设备|
|requestAdapterInfo()|请求对应的 信息|

### 2.3、AdapterInfo

|字段名|意义|类型|例子|
|--|--|--|--|
|architecture|架构|string|"ampere"|
|description|描述|string|
|device|设备型号|string|
|vendor|厂商|string|"nvidia"|

### 2.3 [features 功能](https://www.w3.org/TR/webgpu/#feature-index)

|名|意义|
|--|--|
|shader-f16|Shader 用 `f16` `half` 半精度浮点数|
|texture-compression-bc|压缩纹理：`DDS`/`DXT`|
|texture-compression-etc2|压缩纹理: `ETC2`|
|texture-compression-astc|压缩纹理: `ASTC`|
|depth32float-stencil8|深度-模板 格式的纹理|
|depth-clip-control|深度 裁剪 功能|
|timestamp-query|查询：GPU工作时间|
|indirect-first-instance||
|bgra8unorm-storage||
|rg11b10ufloat-renderable||

### 2.4 [limits 限制](https://www.w3.org/TR/webgpu/#supported-limits)

比如：最大 绑定 BindGroup 个数，最大 纹理大小