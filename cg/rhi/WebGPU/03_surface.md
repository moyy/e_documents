# WebGPU Context

类似：wgpu-rs 的 `Surface`，交换链

## 1、Context

``` js
// canvas 是 HTMLCanvasElement 标签，DOM 对象
const context = canvas.getContext('webgpu');
```

### 1.1、属性

|名字|意义|
|--|--|
|canvas||
|`configure`()||
|unconfigure()||
|`getCurrentTexture`()||
|~~getPreferredFormat()~~|用 navigation.gpu.getPreferredCanvasFormat() 代替|

### 1.2、[getContext 的 第二个 参数](https://www.w3.org/TR/webgpu/#canvas-configuration)

可以 不填

``` js

// GPUCanvasConfiguration
const config = {
    device: GPUDevice,        // 默认 已经 创建的 设备
    format: GPUTextureFormat, // 默认 Adapter 的 预设 格式

	// 下面 可选
	usage: GPUTextureUsage.RENDER_ATTACHMENT, // TextureUsageFlags
    viewFormats：[],      // GPUTextureFormat
    colorSpace: "srgb",   // PredefinedColorSpace
    alphaMode = "opaque", // "premultiplied" 表示 预乘
};
```

## 2、设置 Surface

+ 当 改变 canvas 宽高时，要 调用一次

``` js

context.configure({
	device,
	format: navigation.gpu.getPreferredCanvasFormat(),
});
```