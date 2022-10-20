- [WebGL 扩展](#webgl-扩展)
  - [1. VAO: OES_vertex_array_object](#1-vao-oes_vertex_array_object)
  - [2. 几何实例化 ANGLE_instanced_arrays](#2-几何实例化-angle_instanced_arrays)
  - [3. EXT_blend_minmax](#3-ext_blend_minmax)
  - [4. 纹理](#4-纹理)
    - [4.1. 压缩纹理](#41-压缩纹理)
    - [4.2. EXT_color_buffer_float](#42-ext_color_buffer_float)
    - [4.3. EXT_texture_norm16](#43-ext_texture_norm16)
    - [4.4. EXT_color_buffer_half_float](#44-ext_color_buffer_half_float)
    - [4.5. WEBGL_color_buffer_float](#45-webgl_color_buffer_float)
    - [4.6. OES_texture_float](#46-oes_texture_float)
    - [4.7. OES_texture_float_linear](#47-oes_texture_float_linear)
    - [4.8. OES_texture_half_float](#48-oes_texture_half_float)
    - [4.9. OES_texture_half_float_linear](#49-oes_texture_half_float_linear)
    - [4.10. EXT_float_blend](#410-ext_float_blend)
    - [4.11. WEBGL_depth_texture](#411-webgl_depth_texture)
    - [4.12. EXT_sRGB](#412-ext_srgb)
    - [4.13 过滤](#413-过滤)
  - [5. glsl](#5-glsl)
    - [5.1. OES_standard_derivatives](#51-oes_standard_derivatives)
    - [5.2. WEBGL_multi_draw](#52-webgl_multi_draw)
    - [5.3. EXT_shader_texture_lod](#53-ext_shader_texture_lod)
    - [5.4. EXT_frag_depth](#54-ext_frag_depth)
  - [6. 其他](#6-其他)
    - [6.1. OES_element_index_uint](#61-oes_element_index_uint)
    - [6.2. WEBGL_draw_buffers](#62-webgl_draw_buffers)
    - [6.3. WEBGL_lose_context](#63-webgl_lose_context)
    - [6.4. KHR_parallel_shader_compile](#64-khr_parallel_shader_compile)
    - [6.5. WEBGL_debug_renderer_info](#65-webgl_debug_renderer_info)
    - [6.6. WEBGL_debug_shaders](#66-webgl_debug_shaders)

# WebGL 扩展

[WebGL 扩展](https://registry.khronos.org/webgl/extensions/)

## 1. VAO: OES_vertex_array_object

+ ext.VERTEX_ARRAY_BINDING_OES
+ ext.createVertexArrayOES()
+ ext.deleteVertexArrayOES()
+ ext.isVertexArrayOES()
+ ext.bindVertexArrayOES()

## 2. 几何实例化 ANGLE_instanced_arrays

+ ext.drawArraysInstancedANGLE()
+ ext.drawElementsInstancedANGLE()
+ ext.vertexAttribDivisorANGLE()

## 3. EXT_blend_minmax

gl.blendEquation / gl.blendEquationSeparate

+ gl.MIN
+ gl.MAX

## 4. 纹理

### 4.1. 压缩纹理

+ `ASTC` WEBGL_compressed_texture_astc
+ `DDS` WEBGL_compressed_texture_s3tc
+ WEBGL_compressed_texture_pvrtc
+ WEBGL_compressed_texture_etc1
+ WEBGL_compressed_texture_etc
+ WEBGL_compressed_texture_s3tc_srgb

### 4.2. EXT_color_buffer_float

(笔记本不支持) 

renderbufferStorage()

+ gl.R16F
+ gl.RG16F
+ gl.RGBA16F
+ gl.R32F
+ gl.RG32F
+ gl.RGBA32F
+ gl.R11F_G11F_B10F

### 4.3. EXT_texture_norm16

笔记本 不支持

+ ext.R16_EXT
+ ext.RG16_EXT
+ ext.RGB16_EXT
+ ext.RGBA16_EXT
+ ext.R16_SNORM_EXT
+ ext.RG16_SNORM_EXT
+ ext.RGB16_SNORM_EXT
+ ext.RGBA16_SNORM_EXT

### 4.4. EXT_color_buffer_half_float

renderbufferStorage(), internalformat 参数：

+ ext.RGBA16F_EXT
+ ext.RGB16F_EXT

### 4.5. WEBGL_color_buffer_float

renderbufferStorage()

+ ext.RGBA32F_EXT
+ ext.RGB32F_EXT (Deprecated)

### 4.6. OES_texture_float

texImage2D / texSubImage2D 的 type 参数填 float，pixel 填 Float32Array.

### 4.7. OES_texture_float_linear

用 float 纹理 进行 线性过滤

### 4.8. OES_texture_half_float

texImage2D / texSubImage2D 的 type 参数填 ext.HALF_FLOAT_OES，pixel 填 f16数组

### 4.9. OES_texture_half_float_linear

支持 f16 的 线性过滤

### 4.10. EXT_float_blend

用 float 纹理 做渲染目标 ，可以混合

### 4.11. WEBGL_depth_texture

+ ext.UNSIGNED_INT_24_8_WEBGL 24位深度
+ texImage2D
    - format / internalformat: gl.DEPTH_COMPONENT and gl.DEPTH_STENCIL
    - type: gl.UNSIGNED_SHORT, gl.UNSIGNED_INT, and ext.UNSIGNED_INT_24_8_WEBGL.
    - pixels: Uint16Array or a Uint32Array

### 4.12. EXT_sRGB

texImage2D / texSubImage2D() / renderbufferStorage

+ ext.SRGB_EXT
+ ext.SRGB_ALPHA_EXT
+ ext.SRGB8_ALPHA8_EXT

### 4.13 过滤

+ EXT_texture_filter_anisotropic

## 5. glsl

### 5.1. OES_standard_derivatives

+ glsl 用函数 dFdx, dFdy, fwidth & #extension GL_OES_standard_derivatives : enable
+ ext.FRAGMENT_SHADER_DERIVATIVE_HINT_OES

### 5.2. WEBGL_multi_draw

+ glsl: gl_DrawID && #extension GL_ANGLE_multi_draw : require
+ ext.multiDrawArraysWEBGL()
+ ext.multiDrawElementsWEBGL()
+ ext.multiDrawArraysInstancedWEBGL()
+ ext.multiDrawElementsInstancedWEBGL()

### 5.3. EXT_shader_texture_lod

### 5.4. EXT_frag_depth

fs 直接 写 深度 gl_FragDepthEXT = 0.5;

## 6. 其他

### 6.1. OES_element_index_uint

drawElements() type 参数 gl.UNSIGNED_INT

### 6.2. WEBGL_draw_buffers

gl.framebufferRenderbuffer(), gl.framebufferTexture2D(), gl.getFramebufferAttachmentParameter() ext.drawBuffersWEBGL(), and gl.getParameter() methods.

+ ext.drawBuffersWEBGL()
+ ext.COLOR_ATTACHMENT[0-15]_WEBGL 
+ ext.DRAW_BUFFER[0-15]_WEBGL 
+ ext.MAX_DRAW_BUFFERS_WEBGL

### 6.3. WEBGL_lose_context

模拟 设备丢失 和 恢复

+ ext.WEBGL_lose_context.loseContext()
+ ext.WEBGL_lose_context.restoreContext()

### 6.4. KHR_parallel_shader_compile

### 6.5. WEBGL_debug_renderer_info

### 6.6. WEBGL_debug_shaders