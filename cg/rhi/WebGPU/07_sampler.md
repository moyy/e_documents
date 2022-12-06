[TOC]

# Sampler 采样器

|名字|意义|
|--|--|
|Sampler = Device.createSampler(GPUSamplerDescriptor)|

## 描述：GPUSamplerDescriptor

|名字|类型|默认值|
|--|--|--|
|u/v/w|"repeat"，"mirror-repeat"，"clamp-to-edge"|"clamp-to-edge"|
|mag/min/mipmap|"linear"，"nearest"|"nearest"|
|lodMinClamp|f32|0|
|lodMaxClamp|f32|32|
|[Clamp] maxAnisotropy|u16|1|
|compare|[GPUCompareFunction](https://www.w3.org/TR/webgpu/#enumdef-gpucomparefunction)|