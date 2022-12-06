# demo 性能 数据

## 1、备注

### 1.1、硬件设备

|机器|操作系统|GPU|窗口大小|
|--|--|--|--|
|笔记本|Win 11|"NVIDIA GeForce RTX 3050 Ti Laptop GPU"|1920 * 1080|
|vivo iQOO U3X|Android 11|"Adreno (TM) 619"|1080 * 2408|

### 1.2、渲染 API

+ `GLES`：只用于 Android，present模式 仅为 fifo，受到 垂直同步影响
+ `Vulkan`：present模式 为 maibox，不受 垂直同步影响

## 2、Demo 01_clear

+ 效果：清屏
+ 注：见 目录 `Adapter` 下面的 各个章节，有各种 特性，限制 的 报告

||笔记本 Win 11/**Vulkan**|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|--|
|avg frame|0.28 ms / 3500 fps|8.3 ms / 120 fps|16.7 ms / 60 fps|

## 3、Demo 02_triangle

+ 效果：三角形，展示 attribute 的 用法

||笔记本 Win 11/**Vulkan**|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|--|
|avg frame|0.30 ms|8.3 ms / 120 fps|16.7 ms / 60 fps|
|create_vb|130 - 200 us, 96 B, map|3.47 ms, 96 B, map|120 - 780 us, 96 B, map|
|create_ib|6 - 13 us, 6 B, map|41.8 us, 6 B, map|60 - 236 us, 6 B, map|
|create_vs|220 - 300 us|201 us|230 us - 3.6 ms|
|create_fs|30 - 65 us|108.3 us|80 us - 390 us|
|create_pipeline_layout|30 - 50 µs|7.8 us|4.4 us - 177 us|
|create_pipeline|2.5 - 3.2 ms|6.8 ms|0.9 ms - 3.0 ms|

## 3、Demo 03_image

+ 效果：texture

||笔记本 Win 11/**Vulkan**|Vivo iQOO U3X/**Vulkan**|Vivo iQOO U3X/**GLES3**|
|--|--|--|--|
|avg frame||||
|create_vb||||
|create_ib||||
|create_vs||||
|create_fs||||
|create_pipeline_layout||||
|create_pipeline||||