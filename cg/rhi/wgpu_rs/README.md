# [wgpu-rs](https://docs.rs/wgpu/latest/wgpu/)

wgpu-rs 是 WebGPU 的 rust 实现

包括 一些 WebGPU 没有的 功能

## 平台 实现 crate

|API|rust-crate|适用 OS-平台|配合crate|
|--|--|--|--|
|DX|[d3d12](https://docs.rs/d3d12/0.4.1/d3d12/)|Windows|`winapi`|
|Metal|[metal](https://docs.rs/crate/metal/0.23.1)|iOS / MacOS|`objc`、`core-graphics-types`|
|Vulkan|[ash](https://docs.rs/ash/0.33.3+1.2.191/ash/)|Windows / Android|`gpu-alloc`, `apu-descriptor`|
|GL|[glow](https://docs.rs/glow/0.11.0/glow/)|Windows / Android|`khronos-egl`|