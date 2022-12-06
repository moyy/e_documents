# Android GLES-2 扩展

## 扩展前缀

#### 公用扩展

|前缀|大概意思|说明|
|--|--|--|
|KHR_*|标准||
|ARB_*|标准||
|OES_*|EGL扩展||
|EXT_*|几家厂商联名扩展||

#### 厂商 特定 扩展（一般不关心，除非是：性能提高明显 && 该厂商占大头）

|前缀|厂商|说明|
|--|--|--|
|ANDROID_*|Google--Android||
|ARM_*|arm||
|AMD_*|amd||
|NV_|英伟达||
|QCOM_*|高通||

## 以：Android 红米-Note5（2018.03 大陆上市） 为例子

### 目前 已封装到 WebGL 的

+ GL_OES_depth24                   RenderBuffer 使用 24位深度
+ GL_OES_vertex_array_object       VAO 扩展
+ GL_OES_texture_compression_astc  ASTC压缩纹理
+ GL_EXT_texture_compression_s3tc  DDS压缩纹理（仅 Windows 支持）
	- `红米-Note5 不支持`
+ GL_ARB_instanced_arrays          几何实例化
	- `红米-Note5 不支持`

### 之后 封装到 WebGL 的

+ GL_OES_texture_float         浮点纹理
+ GL_OES_element_index_uint    u32索引
+ GL_OES_standard_derivatives  像素着色器 可计算 两个相邻像素的差 dfdx，dfdy

### 暂时没计划

#### srgb （背景 见：颜色空间 ）

+ GL_EXT_sRGB  支持sRGB采样；
+ GL_EXT_sRGB_write_control  支持sRGB写入

#### 纹理 & 顶点 格式

+ GL_OES_rgb8_rgba8
+ GL_OES_vertex_half_float  顶点Buffer，使用16位浮点精度
+ GL_EXT_texture_format_BGRA8888
+ GL_EXT_texture_type_2_10_10_10_REV
+ GL_EXT_texture_format_sRGB_override
+ GL_EXT_texture_norm16
+ GL_EXT_texture_sRGB_R8
+ GL_EXT_texture_buffer
+ GL_EXT_texture_border_clamp
+ GL_EXT_texture_sRGB_decode

#### 纹理采样

+ GL_OES_texture_npot             非2的冥尺寸纹理的支持
+ GL_OES_depth_texture             深度纹理采样
+ GL_OES_texture_stencil8          模板纹理采样
+ GL_OES_texture_3D                3D纹理
+ GL_OES_texture_float_linear      线性过滤的浮点纹理
+ GL_OES_texture_half_float        半浮点纹理（16位精度）
+ GL_OES_texture_half_float_linear 线性过滤的 16位-浮点纹理
+ GL_OES_sample_shading
+ GL_OES_texture_storage_multisample_2d_array
+ GL_OES_depth_texture_cube_map
+ GL_EXT_texture_filter_anisotropic 各向异性 的 纹理过滤
+ GL_EXT_texture_cube_map_array
+ GL_OES_sample_variables
+ GL_OES_shader_multisample_interpolation

#### 压缩纹理

+ GL_KHR_texture_compression_astc_ldr
+ GL_KHR_texture_compression_astc_hdr
+ GL_OES_compressed_ETC1_RGB8_texture

#### FBO

+ GL_OES_packed_depth_stencil
+ GL_OES_framebuffer_object
+ GL_EXT_color_buffer_float          浮点的renderbuffer，当color用
+ GL_EXT_color_buffer_half_float     16位-浮点的renderbuffer，当color用
+ GL_EXT_multisampled_render_to_texture  多重采样？
+ GL_EXT_multisampled_render_to_texture2 多重采样？

#### 混合

+ GL_KHR_blend_equation_advanced
+ GL_KHR_blend_equation_advanced_coherent
+ GL_EXT_blend_func_extended

#### Image：可以理解为 （和APP交互的）离屏渲染的缓冲区

+ GL_OES_EGL_image
+ GL_EXT_copy_image
+ GL_OES_EGL_image_external
+ GL_OES_EGL_image_external_essl3
+ GL_EXT_EGL_image_array
+ GL_EXT_EGL_image_storage
+ GL_EXT_EGL_image_external_wrap_modes

#### Shader

+ GL_OES_EGL_sync            同步机制 支持
+ GL_OES_get_program_binary  二进制Shader支持
+ GL_EXT_gpu_shader5         glsl 5.0 支持
+ GL_OES_shader_image_atomic
+ GL_EXT_geometry_shader        支持 几何着色器
+ GL_EXT_tessellation_shader    支持 镶嵌着色器（曲面细分）
+ GL_EXT_shader_io_blocks
+ GL_EXT_shader_framebuffer_fetch
+ GL_EXT_shader_non_constant_global_initializers

#### 调试

+ GL_KHR_debug
+ GL_KHR_no_error
+ GL_EXT_debug_marker
+ GL_EXT_debug_label

#### 其他

+ GL_OES_surfaceless_context 从 非表面 创建 GL上下文
+ GL_EXT_buffer_storage
+ GL_EXT_draw_buffers_indexed
+ GL_EXT_clip_control
+ GL_EXT_primitive_bounding_box
+ GL_EXT_disjoint_timer_query
+ GL_EXT_robustness
+ GL_EXT_discard_framebuffer
+ GL_EXT_external_buffer
+ GL_EXT_blit_framebuffer_params
+ GL_EXT_clip_cull_distance
+ GL_EXT_protected_textures
+ GL_EXT_memory_object
+ GL_EXT_memory_object_fd
+ GL_EXT_YUV_target
+ GL_KHR_robust_buffer_access_behavior

#### 特定厂商：特化扩展，不管

+ GL_ANDROID_extension_pack_es31a
+ GL_NV_shader_noperspective_interpolation
+ GL_AMD_compressed_ATC_texture
+ GL_ARM_shader_framebuffer_fetch_depth_stencil
+ GL_QCOM_texture_foveated
+ GL_QCOM_shader_framebuffer_fetch_noncoherent
+ GL_QCOM_alpha_test
+ GL_QCOM_tiled_rendering
+ GL_OVR_multiview_multisampled_render_to_texture
+ GL_OVR_multiview    多视图渲染
+ GL_OVR_multiview2   多视图渲染