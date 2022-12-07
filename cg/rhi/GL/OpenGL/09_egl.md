# EGL 事件

[https://github.com/android/ndk-samples/tree/main/endless-tunnel](https://github.com/android/ndk-samples/blob/main/endless-tunnel/app/src/main/cpp/native_engine.cpp)

## 渲染循环

``` rs
loop {
    // 处理 窗口 事件
    match event {
        APP_CMD_START => mIsVisible = true,
        APP_CMD_STOP => mIsVisible = false,
        APP_CMD_LOST_FOCUS => mHasFocus = false,
        APP_CMD_GAINED_FOCUS => mHasFocus = true,
        APP_CMD_INIT_WINDOW => mHasWindow = true;
        APP_CMD_TERM_WINDOW => {
            KillSurface();
            mHasWindow = false;
        }
    }

    let isRunning = mHasFocus && mIsVisible && mHasWindow;
    if !isRunning {
        continue;
    }
    
    // InitDisplay()
    if (mEglDisplay == EGL_NO_DISPLAY) {
        mEglDisplay = eglGetDisplay(EGL_DEFAULT_DISPLAY);
        if (EGL_FALSE == eglInitialize(mEglDisplay, 0, 0)) {
            continue;
        }
    }

    // InitSurface()
    if (mEglSurface == EGL_NO_SURFACE) {
        EGLint numConfigs;
        const EGLint attribs[] = {EGL_RENDERABLE_TYPE,
            EGL_OPENGL_ES2_BIT,  // request OpenGL ES 2.0
            EGL_SURFACE_TYPE,
            EGL_WINDOW_BIT,
            EGL_BLUE_SIZE, 8,
            EGL_GREEN_SIZE, 8,
            EGL_RED_SIZE, 8,
            EGL_DEPTH_SIZE, 16,
            EGL_NONE
        };

        eglChooseConfig(mEglDisplay, attribs, &mEglConfig, 1, &numConfigs);
        mEglSurface = eglCreateWindowSurface(mEglDisplay, mEglConfig, mApp->window, NULL);
        if (mEglSurface == EGL_NO_SURFACE) {
            continue;
        }
    }

    // InitContext()
    if (mEglContext == EGL_NO_CONTEXT) {
        EGLint attribList[] = {EGL_CONTEXT_CLIENT_VERSION, 2, EGL_NONE};
        mEglContext = eglCreateContext(mEglDisplay, mEglConfig, NULL, attribList);
        if (mEglContext == EGL_NO_CONTEXT) {
            continue;
        }
    }

    // makeCurrent
    if (EGL_FALSE == eglMakeCurrent(mEglDisplay, mEglSurface, mEglSurface, mEglContext)) {
        // 销毁代码在这里
        HandleEglError(eglGetError());
    }
    
    // 执行 OpenGL 代码
    // ...

    // 交换 缓冲区
    if (EGL_FALSE == eglSwapBuffers(mEglDisplay, mEglSurface)) {
        // 销毁代码在这里
        HandleEglError(eglGetError());
    }
}
```

### 销毁 时机

``` rs
bool NativeEngine::HandleEglError(EGLint error) {
    match error {
        EGL_BAD_SURFACE => KillSurface();
        EGL_CONTEXT_LOST | EGL_BAD_CONTEXT => KillContext();
        EGL_BAD_DISPLAY => {
            KillContext();
            KillSurface();
            if (mEglDisplay != EGL_NO_DISPLAY) {
                eglTerminate(mEglDisplay);
                mEglDisplay = EGL_NO_DISPLAY;
            }
        }
    }
}
```

``` rs
void NativeEngine::KillContext() {
    eglMakeCurrent(mEglDisplay, EGL_NO_SURFACE, EGL_NO_SURFACE, EGL_NO_CONTEXT);
    
    if (mEglContext != EGL_NO_CONTEXT) {
        eglDestroyContext(mEglDisplay, mEglContext);
        mEglContext = EGL_NO_CONTEXT;
    }
}
```

``` rs
void NativeEngine::KillSurface() {
    eglMakeCurrent(mEglDisplay, EGL_NO_SURFACE, EGL_NO_SURFACE, EGL_NO_CONTEXT);
    
    if (mEglSurface != EGL_NO_SURFACE) {
        eglDestroySurface(mEglDisplay, mEglSurface);
        mEglSurface = EGL_NO_SURFACE;
    }
}
```

## 事件

### Activty 和 NativeActivity 的 [事件对应](https://docs.nvidia.com/tegra/Content/AN_LC_Basics_Practice.html)

|Java Activity 成员|NativeActivity 方法|native_app_glue 事件|
|--|--|--|
|onCreate|ANativeActivity_onCreate|android_main|
|onStart|::onStart|APP_CMD_START|
|onPause|::onPause|APP_CMD_PAUSE|
|onResume|::onResume|APP_CMD_RESUME|
|onStop|::onStop|APP_CMD_STOP|
|onDestroy|::onDestroy|APP_CMD_DESTROY|
|onWindowFocusChanged|::onWindowFocusChanged|APP_CMD_GAINED/LOST_FOCUS|
|surfaceCreated|::onNativeWindowCreated|APP_CMD_INIT_WINDOW|
|surfaceDestroyed|::onNativeWindowDestroyed|APP_CMD_TERM_WINDOW|
|surfaceChanged|::onNativeWindowResized|APP_CMD_WINDOW_RESIZED|
|onConfigurationChanged|::onConfigurationChanged|APP_CMD_CONFIG_CHANGED|

### winit 0.27.5

用了 第三方库 android-activity

|winit 事件|android-activity 事件|NativeActivity 方法|
|--|--|--|
|Resumed|MainEvent::InitWindow|::onNativeWindowCreated|
|Suspended|MainEvent::TerminateWindow|::onNativeWindowDestroyed|
|Focused(bool)|MainEvent::LostFocus/GainedFocus|::onWindowFocusChanged|
|ScaleFactorChanged|MainEvent::ConfigChanged|{new_inner_size, scale_factor}|onConfigurationChanged|
|resized = true|MainEvent::WindowResized|::onNativeWindowResized|
|redraw = true|MainEvent::RedrawNeeded|::onNativeWindowRedrawNeeded|
|running = false|MainEvent::Pause|::onPause|
|running = true|MainEvent::Resume|::onResume|
|RedrawRequested||if running && redraw|
|Resized||if running && resized|

### Pxiel4 / Android 13

#### app 前台 切 后台

+ APP_CMD_LOST_FOCUS
+ APP_CMD_PAUSE
+ APP_CMD_TERM_WINDOW
+ APP_CMD_STOP
+ APP_CMD_SAVE_STATE

#### app 后台 切 前台

+ APP_CMD_START
+ APP_CMD_RESUME
+ APP_CMD_INIT_WINDOW
+ APP_CMD_WINDOW_RESIZED
+ APP_CMD_GAINED_FOCUS