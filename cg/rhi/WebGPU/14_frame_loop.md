## CommandEncoder

``` js

// 每帧 都 新建 Encoder
const commandEncoder = device.createCommandEncoder();

// 录制 命令，RenderPass 或者 ComputePass
// ...

// 结束 编码器 得到 命令缓冲区
const commandBuffer = commandEncoder.finish();

// 提交 到 队列
device.queue.submit([commandBuffer]);

```

### 1、常规

|名字|意义|
|--|--|
|label|string, 名字|
|finish()|返回 `CommandBuffer`|
|clearBuffer()||

### 2、计算 & 渲染

|名字|意义|
|--|--|
|beginRenderPass()|`RenderPass`|
|beginComputePass()|`ComputePass`|

### 3、拷贝

|名字|意义|
|--|--|
|copyBufferToBuffer()||
|copyTextureToTexture()||
|copyBufferToTexture()|Buffer的行 必须 256字节 对齐|
|copyTextureToBuffer()|Buffer的行 必须 256字节 对齐|

#### 3.1、[Buufer <--> Texture中，Buffer 对齐](https://www.w3.org/TR/webgpu/#gpu-image-copy-buffer)

![](/uploads/render_tech/images/m_6dd57ba168c2a88340c14bdd438a54ff_r.png)

### 4、查询

|名字|意义|
|--|--|
|resolveQuerySet()||
|writeTimestamp()||

### 5、调试

|名字|意义|
|--|--|
|insertDebugMarker()||
|popDebugGroup()||
|pushDebugGroup()||

## RenderPass

``` js

const textureView = context.getCurrentTexture().createView();

// GPURenderPassDescriptor
const rpDesc = {
	colorAttachments: [{
		view: textureView,
		clearValue: { r: 0.0, g: 0.0, b: 0.0, a: 1.0 },
		loadOp: 'clear',
		storeOp: 'store',
	}],
};
const rp = commandEncoder.beginRenderPass(rpDesc);

// 录制 RenderPass 命令
rp.setPipeline(pipeline);
rp.draw(3, 1, 0, 0);

rp.end();

```

### 1、创建 GPURenderPassDescriptor

|名字|类型|默认值|
|--|--|--|
|(必须) colorAttachments|GPURenderPassColorAttachment[]|
|depthStencilAttachment|GPURenderPassDepthStencilAttachment|
|occlusionQuerySet|GPUQuerySet|
|timestampWrites|GPURenderPassTimestampWrites|
|maxDrawCount|u64|50000000|

#### 1.1、GPURenderPassColorAttachment

|名字|类型|作用|
|--|--|--|
|(必须) view|GPUTextureView||
|(必须) loadOp|"load", "clear"||
|(必须) storeOp|"store", "discard"||
|resolveTarget|GPUTextureView|多重采样|
|clearValue|{r: f64, g: f64, b: f64, a: f64 }||

### 2、常规，结束

|名字|意义|
|--|--|
|label|string, 名字-标签|
|end()|
|endPass()|

### 3、设置 状态

|名字|意义|
|--|--|
|executeBundles()|
|setViewport()|
|setScissorRect()|
|setPipeline()|
|setVertexBuffer()|
|setIndexBuffer()|
|setBindGroup()|
|setBlendConstant()|
|setStencilReference()|

### 4、DrawCall

|名字|意义|
|--|--|
|draw()|
|drawIndexed()|
|drawIndirect()|
|drawIndexedIndirect()|

### 5、查询：遮挡 & 时间

|名字|意义|
|--|--|
|beginOcclusionQuery()|
|endOcclusionQuery()|
|writeTimestamp()|

### 6、调试

|名字|意义|
|--|--|
|insertDebugMarker()|
|popDebugGroup()|
|pushDebugGroup()|

## ComputePass

``` js
```

### 1、创建 GPUComputePassDescriptor

|名字|类型|
|--|--|
|timestampWrites|GPUComputePassTimestampWrite[]|

#### 1.1、GPUComputePassTimestampWrite

|名字|类型|
|--|--|
|(必须) querySet|GPUQuerySet|
|(必须) queryIndex|u32|
|(必须) location|"beginning", "end"|

### 2、常规，结束

|名字|意义|
|--|--|
|label||
|end()||

### 3、设置

|名字|意义|
|--|--|
|setPipeline()||
|setBindGroup()||

### 4、执行

|名字|意义|
|--|--|
|dispatchWorkgroups()||
|dispatchWorkgroupsIndirect()||

### 5、调试

|名字|意义|
|--|--|
|pushDebugGroup()|
|popDebugGroup()|
|insertDebugMarker()|