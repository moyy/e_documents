# Android-EGL：离屏 渲染

离屏 渲染：EGL Surface、EGL Image

## 相同点

* 都是作为 渲染目标、纹理

## EGL Surface

* WindowSurface：和 窗口 绑定；
* （推荐使用？）PixmapSurface：离屏，来自 原生 平台 系统；
* PbufferSurface：EGL 创建；
	+ 可用 eglCreatePbufferFromClientBuffer，让 PBuffer也来自 原生 平台 系统；

#### 用法

* RenderTarget：eglMakeCurrent，切换上下文；
* 纹理：eglBindTexImage；

## EGL Image

* EGL 扩展：EGL_KHR_image，
	+ EGL_KHR_image_base：定义 EGLImage、eglCreateImageKHR；
	+ EGL_KHR_image_pixmap：原生平台的pixmap可以被创建为EGL Image
* Android 平台：EGL_Android_image_native_buffer：使得 ANativeWindowBuffer 被创建为EGL Image；
* EGL_KHR_gl_texture_2D_image，EGL_KHR_gl_texture_3D_image，EGL_KHR_gl_renderbuffer_image和EGL_KHR_gl_texture_cubemap_image 使得 GLES 资源 可以 被创建为EGL Image

#### 使用

* EGL Image 的 结构体：GLeglImageOES
* 扩展 GL_OES_EGL_image 当做2D Texture或者Renderbuffer使用，可以作为 源 和 目标
* 扩展 GL_OES_EGL_image_external 定义新的Texture类型GL_TEXTURE_EXTERNAL_OES，使得诸如YUV等以前不被OpenGL ES支持的格式的EGL Image，可以被直接作为一个Texture从而被OpenGL ES所支持

## 参考

* [EGL 资源 数据共享](http://blog.sciencenet.cn/blog-1420268-912560.html)