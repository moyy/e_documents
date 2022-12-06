# ShaderModule

## 着色器

+ `VS` 顶点着色器
+ `FS` 像素/片段 着色器
+ `CS` 计算 着色器

### 语法

+ 必须是 wgsl
+ wgpu-rs 有 naga 可以用 glsl，hlsl

### 注：不支持 的 功能

其功能 通过 `CS` 模拟

+ `GS` 几何 着色器
+ `HS`、`DS` 曲面细分 着色器
+ `Stream Output` / `Transfrom Feedback` 变换反馈

## 创建 GPUShaderModuleDescriptor

|||
|--|--|
|(必须) code|string|
|sourceMap|不管|
|hints|不管，GPUShaderModuleCompilationHint|

## Uniform Struct 大小 和 对齐

### 1、规则

类似于C语言的 结构体 对齐；

+ 结构体 每个成员的 起始地址 都有 对齐
+ 最后 整个结构体的大小 能整除 最大对齐的那个成员；
+ 基础类型 对齐 必须 是 2 的 幂，具体见表

![](/uploads/render_tech/images/m_92855d07dc080bef6fb4259a6cc8ffbf_r.png)

### 2、建议

+ 尽量 不用 vec3
	- 只有 vec3 的 大小 12 不能 乘除 对齐 16
+ 如果用 vec3，那么 最好是 vec3 + float 组合

### 3、例子

下面结构体的大小 是 80 字节

``` glsl

layout(set = 0, binding = 0) uniform struct ColorEffect {
	vec3 hsb;
	vec3 color_balance;
	float scale_shadow_in;
	float scale_shadow_out;
	float scale_mid;
	float scale_highlight_in;
	float scale_highlight_out;
	vec3 vignette;
	vec3 vignette_color;
}

```

#### 3.1、查表

+ float 对齐 是 4
+ vec3 对齐是 16

#### 3.2、计算

|字段|类型|对齐地址|大小|下个紧凑地址|
|--|--|--|--|--|
|hsb|vec3|0|12|12|
|color_balance|vec3|16|12|28|
|scale_shadow_in|float|28|4|32|
|scale_shadow_out|float|32|4|36|
|scale_mid|float|36|4|40|
|scale_highlight_in|float|40|4|44|
|scale_highlight_out|float|44|4|48|
|vignette|vec3|48|12|60|
|vignette_color|vec3|64|12|76|

+ ColorEffect 对齐 = 各成员对齐的最大值 = vec3 的 对齐值 = 16
+ ColorEffect 大小 = floor(76 / 16) * 16 = 80
	- 紧凑大小 76 补多少字节 能被 16 整除；

### 4、参考

+ [规范：4.4.7.1 对齐和大小](https://www.w3.org/TR/WGSL/#alignment-and-size)