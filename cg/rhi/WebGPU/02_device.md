[TOC]

## Device & Queue

## 1、GPUDevice 设备

+ 从 Adapter 获取
+ 功能：创建 WebGPU 对象，线程安全
+ 注：如果 Adapter 失效，那么 它创建的所有对象 也 不可用

``` js
const device = await adapter.requestDevice();
```

### 1.1、属性

+ 注：Device 的 features 优先于 Adapter 的；
+ 注：Device 的 limits 优先于 Adapter 的；

|名|类型|默认值|意义|
|--|--|--|--|
|label|string|""||
|queue|`GPUQueue`|||
|lost|Promise<>||用于 设备丢失|
|features|GPUSupportedFeatures|||
|limits|GPUSupportedLimits|||

### 1.2、requestDevice 的 参数

可以不填；

requiredFeatures/requiredLimits 的含义是 “我的程序要这样，返回一个满足这些 要求的 设备”，而不是 “我要这么多要求，你给我返回一个这些参数的设备”

``` js

// GPUDeviceDescriptor
const options = {
	requiredFeatures: [],           // 上面 features 的 名字
    requiredLimits: {字符串: 数字},  // 上面 limits 的 名字 和 数量
    defaultQueue: {},
};
```

### 1.3、设备丢失

### 1.4、对象 = create对象(参数)

|参数-名|意义|
|--|--|
|`CommandEncoder`Descriptor||
|`RenderBundleEncoder`Descriptor||
|`Buffer`Descriptor||
|`Texture`Descriptor||
|`Sampler`Descriptor||
|`BindGroupLayout`Descriptor||
|`BindGroup`Descriptor||
|`ShaderModule`Descriptor||
|`PipelineLayout`Descriptor||
|`RenderPipeline`Descriptor|有 异步版，返回 Promise|
|`ComputePipeline`Descriptor|有 异步版，返回 Promise|
|`QuerySet`Descriptor||

### 1.5、其他

|名|意义|
|--|--|
|destroy()||
|`onuncapturederror`()||
|ExternalTexture = `importExternalTexture`(ExternalTextureDescriptor)||
|async `pushErrorScope`(ErrorFilter)||
|async `popErrorScope`()||

## 2、GPUQueue 队列

+ WebGPU 中，一个 Device 只能有一个 Queue
+ 只要硬件支持，包含：计算，渲染，拷贝 功能

### 2.1 属性

|名|类型|默认值|意义|
|--|--|--|--|
|label|string|""||
|submit()|||提交：渲染 或 计算 的 命令缓冲区|
|`onSubmittedWorkDone`()|||GPU 完成提交工作时 的 回调函数|
|writeBuffer()|||写Buffer：内存 --> 显存|
|writeTexture()|||写Texture：内存 --> 显存|
|copyExternalImageToTexture()|||拷 Image --> Texture|