# 0、FAQ

## 1、怎么 局部清屏

+ 答案：做不到，因为 metal 不支持
+ 接口限制，清屏是在 createRederPass做的，而 viewport 和 scissor 都是 拿到 rp 之后

## 2、[GroupLayout，set 有 空槽位，draw 时候 失败](https://github.com/gpuweb/gpuweb/discussions/3043)

+ 原因：接口 限制，set 必须 连续
+ 解决
	- 全局 创建一个 空的 BindGroup，就是 什么都不绑定
		* let emptyGroup = device.createBindGroupLayout({entries: []})
	- 如果 某个shader被宏开关影响 有 空的 set，用 emptyGroup 绑上去；

## 3、[Uniform Struct 大小计算](/docs/render_tech/render_tech-1e0mubev2ghrl)

## 4、[切换 pipeline 后，原来的 setBindGroup 还有没有效果](https://github.com/gpuweb/gpuweb/issues/171)

## 比 GL 多的 概念

### 1、Storage Buffer

OpenGL 年代，这个叫 `SSBO`

#### 1.1、和 UBO 的 区别

+ 容量 比 UBO 大，最小也有 16MB； UBO 一般是 16KB
+ 对 GLSL `可读写`，支持 `原子操作`；而 UBO 对 GLSL 是 只读
+ 可以 在 运行期 决定 大小，支持 任意长度数组
+ 访问速度 更慢；放在 GPU 的 全局内存（Global Memory）中
	- UBO 存放在 GPU 常量内存（constant memory）中
	- GPU 对 常量内存 访问 比 全局内存 快得多
	- UBO 最差的情况下 也不会比SSBO慢

#### 1.2、GLSL 关键字

+ `buffer`
+ `writeonly`
+ `readonly`

``` glsl
#version 450

layout(set = 0, binding = 0) buffer testBufferBlock {
    uint[] data;
} testBuffer;

layout(set = 0, binding = 1) writeonly buffer testBufferWriteOnlyBlock {
    uint[] data;
} testBufferWriteOnly;

layout(set = 0, binding = 2) readonly buffer testBufferReadOnlyBlock {
    uint[] data;
} testBufferReadOnly;

void main() {
    uint a = testBuffer.data[0];
    testBuffer.data[1] = 2;

    testBufferWriteOnly.data[1] = 2;

    uint b = testBufferReadOnly.data[0];
}

```

### 2、Storage Texture

#### 2.1、和 Sampled Texture 的 区别

+ 功能区别
	- 用 整数索引 访问 内容，不用 uv 坐标转换；
	- 没有采样
+ 容量 比 Texture 大
+ 对 GLSL `可读写`，支持 `原子操作`
+ 访问速度 更慢

#### 2.2、GLSL 关键字

+ `uniform` `image`1D/2D/3D/Cube/1DArray/2DArray`
+ `readonly`
+ `writeonly`

``` glsl

layout(rgba8, binding = 1) uniform image2D img2D;

void testImg2D(in ivec2 coord) {
    vec2 size = imageSize(img2D);

	vec4 c = imageLoad(img2D, coord);

    imageStore(img2D, coord, vec4(2));
}

```

### 3、CS: 计算着色器

### 4、查询：`QuerySet`

+ 遮挡 查询
+ 运行时间 查询

### 5、 指令缓存: `RenderBundle`

### 6、 间接渲染: Indirect Draw