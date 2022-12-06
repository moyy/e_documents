[TOC]

# [Limits](https://docs.rs/wgpu/latest/wgpu/struct.Limits.html)

## 1、作用

|字段|作用|
|--|--|
|max_texture_dimension_1d|创建 1D 纹理，size 最大值|
|max_texture_dimension_2d|2D 纹理 宽高 像素 的 最大值|
|max_texture_dimension_3d|3D 纹理 宽高深 像素 的 最大值|
|max_texture_array_layers|2D 纹理 数组长度 的 最大值|
|max_bind_groups|同时附加到管道的绑定组的最大数|
|max_dynamic_uniform_buffers_per_pipeline_layout|单个Pipeline中 Dynamic UBO 绑定 的 最大数|
|max_dynamic_storage_buffers_per_pipeline_layout|单个Pipeline中 Dynamic Storage Buffer 绑定最大数|
|max_sampled_textures_per_shader_stage|一个Shader中 采样Texture 最大个数|
|max_samplers_per_shader_stage|一个Shader中 Sampler 最大个数|
|max_storage_buffers_per_shader_stage|一个Shader中 Storage Buffer 最大个数|
|max_storage_textures_per_shader_stage|一个Shader中 Storage Texture 最大个数|
|max_uniform_buffers_per_shader_stage|一个Shader中 Uniform Buffer 最大个数|
|max_uniform_buffer_binding_size|BindGroup 每个Bind 到 Uniform Buffer 的 最大字节数|
|max_storage_buffer_binding_size|BindGroup 每个Bind 到 Storage Buffer 的 最大字节数|
|max_vertex_buffers|创建 RenderPipeline 时，VertexState::buffers 的最大长度|
|max_vertex_attributes|创建 RenderPipeline 时，VertexBufferLayout::attributes 的最大长度，对所有 VertexState::buffers 求和|
|max_vertex_buffer_array_stride|创建 RenderPipeline 时 VertexBufferLayout::array_stride 的最大值|
|max_push_constant_size|可用于推送常量的存储量（以字节为单位）；需要启用 Features::PUSH_CONSTANTS|
|min_uniform_buffer_offset_alignment|创建或时所需BufferBindingType::Uniform的对齐方式|
|min_storage_buffer_offset_alignment|创建或时所需BufferBindingType::Storage的对齐方式|
|max_inter_stage_shader_components|用于级间通信（顶点输出到片段输入）的输入或输出位置的最大允许组件数|
|max_compute_workgroup_storage_size|计算入口点中用于工作组内存的最大字节数|
|max_compute_invocations_per_workgroup|workgroup_size计算入口点的维度乘积的最大值|
|max_compute_workgroup_size_x|ShaderModule计算阶段入口点的 workgroup_size X 维度的最大值|
|max_compute_workgroup_size_y|ShaderModule计算阶段入口点的 workgroup_size Y 维度的最大值|
|max_compute_workgroup_size_z|ShaderModule计算阶段入口点的 workgroup_size Z 维度的最大值|
|max_compute_workgroups_per_dimension|操作的每个维度的最大值ComputePass::dispatch(x, y, z)|
|max_buffer_size|保证缓冲区分配失败的限制；根据可用内存、碎片和其他因素，低于最大缓冲区大小的缓冲区分配可能不会成功|

## 2、测试数据

+ 笔记本 Win 11, "NVIDIA GeForce RTX 3050 Ti Laptop GPU"
+ vivo iQOO U3X, "Adreno (TM) 619"

|字段|笔记本 Win 11/**Vulkan**|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|--|
|max_texture_dimension_1d|32768|16384|16384|
|max_texture_dimension_2d|32768|16384|16384|
|max_texture_dimension_3d|16384|2048|2048|
|max_texture_array_layers|2048|2048|2048|
|**max_bind_groups**|8|**4**|8|
|**max_dynamic_uniform_buffers_per_pipeline_layout**|15|32|14|
|**max_dynamic_storage_buffers_per_pipeline_layout**|16|16|**4**|
|max_sampled_textures_per_shader_stage|1048576|524288|16|
|max_samplers_per_shader_stage|1048576|524288|16|
|**max_storage_buffers_per_shader_stage**|1048576|524288|**4**|
|**max_storage_textures_per_shader_stage**|1048576|524288|**4**|
|max_uniform_buffers_per_shader_stage|1048576|524288|**14**|
|max_uniform_buffer_binding_size|65536|65536|65536|
|max_storage_buffer_binding_size|2147483648|536870912|536870912|
|max_vertex_buffers|16|16|32|
|max_vertex_attributes|32|32|16|
|max_vertex_buffer_array_stride|2048|2048|2147483647|
|**max_push_constant_size**|256|128|**64**|
|min_uniform_buffer_offset_alignment|64|64|32|
|min_storage_buffer_offset_alignment|16|64|64|
|max_inter_stage_shader_components|128|112|124|
|max_compute_workgroup_storage_size|49152|32768|32768|
|max_compute_invocations_per_workgroup|1024|1024|1024|
|max_compute_workgroup_size_x|1024|1024|1024|
|max_compute_workgroup_size_y|1024|1024|1024|
|max_compute_workgroup_size_z|64|64|64|
|max_compute_workgroups_per_dimension|65535|65535|65535|
|max_buffer_size|18446744073709551615|2147483647|2147483647|