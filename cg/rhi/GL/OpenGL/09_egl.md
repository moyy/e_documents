# EGL 事件

ndk-samples\endless-tunnel\app\src\main\cpp\native_engine.cpp

## 渲染循环

``` rs
loop {
    if(!IsAnimatinng()) continue; 
    
    InitDisplay(): if (mEglDisplay == EGL_NO_DISPLAY) 创建
    InitSurface(): if (mEglSurface == EGL_NO_SURFACE) 创建
    InitContext(): if (mEglContext == EGL_NO_CONTEXT) 创建

    if (EGL_FALSE == eglMakeCurrent(mEglDisplay, mEglSurface, mEglSurface, mEglContext)) {
        // 销毁代码在这里
        HandleEglError(eglGetError());
    }
    
    // 执行真正 的 渲染逻辑，raf 等

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

APP_CMD_LOST_FOCUS
APP_CMD_PAUSE
APP_CMD_TERM_WINDOW
APP_CMD_STOP
APP_CMD_SAVE_STATE
APP_CMD_START
APP_CMD_RESUME
APP_CMD_INIT_WINDOW
APP_CMD_WINDOW_RESIZED
APP_CMD_GAINED_FOCUS

### 小米9 / Android 10

APP_CMD_PAUSE
APP_CMD_LOST_FOCUS
APP_CMD_TERM_WINDOW
APP_CMD_STOP
APP_CMD_SAVE_STATE
APP_CMD_START
APP_CMD_RESUME
APP_CMD_INIT_WINDOW
APP_CMD_WINDOW_RESIZED
APP_CMD_GAINED_FOCUS


