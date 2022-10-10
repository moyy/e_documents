- [2D渲染](#2d渲染)
  - [1、参考框架](#1参考框架)
  - [2、批量 处理](#2批量-处理)
    - [2.1、WebRender](#21webrender)
    - [2.2、Arm Mali GPU 教程系列 第2辑](#22arm-mali-gpu-教程系列-第2辑)
    - [2.3、GPU Performance for Game Artists](#23gpu-performance-for-game-artists)

# 2D渲染

## 1、参考框架

|框架链接||
|--|--|
|[WebRender](https://github.com/servo/webrender)||
|[PathFinder](https://github.com/servo/pathfinder)||
|[piet-gpu](https://github.com/linebender/piet-gpu)||
|[Skia](https://github.com/google/skia)||

## 2、批量 处理

### 2.1、[WebRender](https://mozillagfx.wordpress.com/2019/01/03/webrender-primitive-segmentation/)

这篇文章，说明，可以用 CPU 将 透明 和 不透明 分开，先集中渲染 不透明 的 部分；

![](../img/m_8f41caa6b9494d9ae891f5c3f23dd2ed_r.png)

### 2.2、[Arm Mali GPU 教程系列 第2辑](https://zhuanlan.zhihu.com/p/402312343)

这篇文章进一步说明，即使是图片，还是可以这么做。

![](../img/m_40f12667faf0fa8c65772b3ab2c41ce9_r.png)

![](../img/m_3a8c175bd1fcb1f24e40e8c3933d03d5_r.png)

![](../img/m_f2e39bde58373adc8b889251f65e5ccf_r.png)

### 2.3、[GPU Performance for Game Artists](http://fragmentbuffer.com/gpu-performance-for-game-artists/)

对粒子系统的图片，可以用 一系列 三角形 批分；减少无效像素的运行

![](../img/m_d8d0e20f96c8b235ba48c8af8e0b5fcf_r.png)

![](../img/m_64cd24dbf1d713664b7abf3797bbbb95_r.png)