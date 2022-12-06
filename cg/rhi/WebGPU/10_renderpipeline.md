[TOC]

# RenderPipeline

## 1、GPURenderPipelineDescriptor

|名字|类型|默认值|
|--|--|--|
|layout|GPUPipelineLayout or "auto"|"auto"|
|（必须）vertex|GPUVertexState|
|primitive|GPUPrimitiveState|
|depthStencil|GPUDepthStencilState|
|multisample|GPUMultisampleState|
|fragment|GPUFragmentState|

### 1.1、顶点 GPUVertexState

|名字|类型|
|--|--|
|buffers|GPUVertexBufferLayout[]|

#### 1.1.1、GPUVertexBufferLayout

|名字|类型|默认值|
|--|--|--|
|(必须) arrayStride|u64|
|(必须) attributes|GPUVertexAttribute[]|
|stepMode|"vertex", "instance"|"vertex"|

#### 1.1.2、GPUVertexAttribute

|名字|类型|默认值|
|--|--|--|
|(必须) format|[GPUVertexFormat](https://www.w3.org/TR/webgpu/#enumdef-gpuvertexformat)|
|(必须) offset|u64|
|(必须) shaderLocation|u32|

### 1.2、图元 GPUPrimitiveState

* unclippedDepth 需要开启 "depth-clip-control" feature

|名字|类型|默认值|
|--|--|--|
|topology|"point-list","line-list","line-strip","triangle-list","triangle-strip"|"triangle-list"|
|stripIndexFormat|"uint16", "uint32"||
|frontFace|"cw", "ccw"|"ccw", 逆时针|
|cullMode|"none", "front", "back"|"none"|
|unclippedDepth|boolean|false|

### 1.3、深度模板 GPUDepthStencilState

|名字|类型|默认值|
|--|--|--|
|(必须) format|[GPUTextureFormat](https://www.w3.org/TR/webgpu/#enumdef-gputextureformat)||
|depthWriteEnabled|boolean|false|
|depthCompare|[GPUCompareFunction](https://www.w3.org/TR/webgpu/#enumdef-gpucomparefunction)|"always"|
|stencilFront|GPUStencilFaceState||
|stencilBack|GPUStencilFaceState||
|stencilReadMask|u32|0xFFFFFFFF|
|stencilWriteMask|u32|0xFFFFFFFF|
|depthBias|u32|0|
|depthBiasSlopeScale|float|0|
|depthBiasClamp|float|0|

#### 1.3.1、GPUStencilFaceState

|名字|类型|默认值|
|--|--|--|
|compare|[GPUCompareFunction](https://www.w3.org/TR/webgpu/#enumdef-gpucomparefunction)|"always"|
|failOp|[GPUStencilOperation](https://www.w3.org/TR/webgpu/#enumdef-gpustenciloperation)|"keep"|
|depthFailOp|[GPUStencilOperation](https://www.w3.org/TR/webgpu/#enumdef-gpustenciloperation)|"keep"|
|passOp|[GPUStencilOperation](https://www.w3.org/TR/webgpu/#enumdef-gpustenciloperation)|"keep"|

### 1.4、多重采样 GPUMultisampleState

|名字|类型|默认值|
|--|--|--|
|count|u32|1|
|mask|u32|0xFFFFFFFF|
|alphaToCoverageEnabled|boolean|false|

### 1.5、片段 GPUFragmentState

|名字|类型|
|--|--|
|(必须) targets|GPUColorTargetState[]|

#### 1.5.1、GPUColorTargetState

|名字|类型|默认值|
|--|--|--|
|(必须) format|[GPUTextureFormat](https://www.w3.org/TR/webgpu/#enumdef-gputextureformat)|
|blend|GPUBlendState|
|writeMask|RED = 1, GREEN = 2, BLUE = 4, ALPHA = 8, ALL = 15|ALL|

##### 1.5.2 GPUBlendState

|名字|类型|默认值|
|--|--|--|
|operation|[GPUBlendOperation](https://www.w3.org/TR/webgpu/#enumdef-gpublendoperation)|"add"|
|srcFactor|[GPUBlendFactor](https://www.w3.org/TR/webgpu/#enumdef-gpublendfactor)|"one"|
|dstFactor|[GPUBlendFactor](https://www.w3.org/TR/webgpu/#enumdef-gpublendfactor)|"zero"|