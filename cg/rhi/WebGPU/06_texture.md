[TOC]

# Texture & Sampler

## 1、Texture

### 1、属性

|名字|类型|意义|
|--|--|--|
|label|string|
|width|GPUIntegerCoordinate|
|height|GPUIntegerCoordinate|
|depthOrArrayLayers|GPUIntegerCoordinate|
|mipLevelCount|GPUIntegerCoordinate|
|sampleCount|GPUSize32|
|dimension|GPUTextureDimension|
|format|GPUTextureFormat|
|usage|GPUTextureUsageFlags|

### 1.1、创建

|名字|意义|
|--|--|
|Texture = Device.createTexture(GPUTextureDescriptor)|
|Texture = Context.getCurrentTexture()|

### 1.2 描述 GPUTextureDescriptor

|名字|类型|默认值|
|--|--|--|
|(必须) size|GPUExtent3D||
|(必须) format|[GPUTextureFormat](https://www.w3.org/TR/webgpu/#enumdef-gputextureformat)||
|(必须) usage|GPUTextureUsageFlags||
|dimension|"1d", "2d", "3d"|"2d"|
|mipLevelCount|u32|1|
|sampleCount|u32|1|
|viewFormats|[GPUTextureFormat](https://www.w3.org/TR/webgpu/#enumdef-gputextureformat)[]|[]|

#### 1.2.1、GPUTextureUsageFlags

|名字|类型|默认值|
|--|--|--|
|(必须) width|u32||
|height|u32|1|
|depthOrArrayLayers|u32|1|

#### 1.2.2、GPUTextureUsageFlags

|值||
|--|--|
|COPY_SRC = 1||
|COPY_DST = 2||
|TEXTURE_BINDING = 4||
|`STORAGE_BINDING` = 8||
|RENDER_ATTACHMENT = 16||

### 1.2、销毁

|名字|意义|
|--|--|
|destroy()|

### 1.3、GPUExternalTexture

## 2、TextureView

### 2.1、创建

|名字|意义|
|--|--|
|TextureView = Texture.createView(GPUTextureViewDescriptor)|

### 2.2 描述 GPUTextureViewDescriptor

|名字|类型|默认值|
|--|--|--|
|format|[GPUTextureFormat](https://www.w3.org/TR/webgpu/#enumdef-gputextureformat)||
|dimension|"1d","2d","2d-array","cube","cube-array","3d"||
|aspect|"all", "stencil-only", "depth-only"|"all"|
|baseMipLevel|i32|0|
|mipLevelCount|i32||
|baseArrayLayer|i32|0|
|arrayLayerCount|i32||