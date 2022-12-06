[TOC]

# Buffer

## 1、创建

|名字|意义|
|--|--|
|Buffer = Device.createBuffer(GPUBufferDescriptor)|

### 1.1、描述 [GPUBufferDescriptor](https://www.w3.org/TR/webgpu/#GPUBufferDescriptor)

``` js
// GPUBufferDescriptor
const desc = {
    size: u64,
    usage: GPUBufferUsageFlags,
    mappedAtCreation: boolean, // 默认：false
};
```

### 1.2、用途 GPUBufferUsageFlags

|字段名|描述|
|--|--|
|MAP_READ|映射|
|MAP_WRITE|映射|
|COPY_SRC|拷贝 源|
|COPY_DST|拷贝 目标|
|INDEX|IB|
|VERTEX|VB|
|UNIFORM|UBO|
|`STORAGE`|SBO|
|`INDIRECT`|间接渲染|
|`QUERY_RESOLVE`|查询结果|

## 2、成员

|名字|意义|
|--|--|
|label|string|
|size|i64|
|usage|GPUBufferUsageFlags|

## 3、映射：显存 --> 内存

|名字|意义|
|--|--|
|async mapAsync(GPUMapModeFlags, offset, size)|
|unmap()|
|ArrayBuffer = getMappedRange(offset, size)|

## 4、销毁

|名字|意义|
|--|--|
|destroy()|