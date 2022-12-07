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
    EGL_CONTEXT_LOST => KillContext(); mEglContext = EGL_NO_CONTEXT
    EGL_BAD_CONTEXT => KillContext(); mEglContext = EGL_NO_CONTEXT
    EGL_BAD_DISPLAY => KillDisplay()  mEglDisplay = EGL_NO_DISPLAY
    EGL_BAD_SURFACE => KillSurface(); mEglSurface = EGL_NO_SURFACE
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