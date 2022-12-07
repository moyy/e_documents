# EGL 事件

[https://github.com/android/ndk-samples/tree/main/endless-tunnel](https://github.com/android/ndk-samples/blob/main/endless-tunnel/app/src/main/cpp/native_engine.cpp)

## 渲染循环

``` rs
loop {
    // 释放 窗口，前后台切换时候 会 调用一次
    如果 窗口事件为 APP_CMD_TERM_WINDOW => KillSurface();

    if(!IsAnimatinng()) continue; 
    
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
## 帧推 条件

``` rs
bool NativeEngine::IsAnimating() {
    return mHasFocus && mIsVisible && mHasWindow;
}

mIsVisible:
    APP_CMD_START 时 为 true
    APP_CMD_STOP 时 为 false

mHasFocus:
    APP_CMD_LOST_FOCUS 时 为 false
    APP_CMD_GAINED_FOCUS 时 为 true

mHasWindow
    APP_CMD_INIT_WINDOW 为 true
    APP_CMD_TERM_WINDOW 为 false

```

## 切到 后台再 切回来 时 的 事件顺序：

### Pxiel4 / Android 13

+ APP_CMD_LOST_FOCUS
+ APP_CMD_PAUSE
+ APP_CMD_TERM_WINDOW
+ APP_CMD_STOP
+ APP_CMD_SAVE_STATE
+ APP_CMD_START
+ APP_CMD_RESUME
+ APP_CMD_INIT_WINDOW
+ APP_CMD_WINDOW_RESIZED
+ APP_CMD_GAINED_FOCUS

### 小米9 / Android 10

+ APP_CMD_PAUSE
+ APP_CMD_LOST_FOCUS
+ APP_CMD_TERM_WINDOW
+ APP_CMD_STOP
+ APP_CMD_SAVE_STATE
+ APP_CMD_START
+ APP_CMD_RESUME
+ APP_CMD_INIT_WINDOW
+ APP_CMD_WINDOW_RESIZED
+ APP_CMD_GAINED_FOCUS


