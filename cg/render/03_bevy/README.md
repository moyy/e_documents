# Bevy 0.9 

[Bevy 0.9](https://bevyengine.org/news/bevy-0-9/)

## 主要 crate 结构

+ 左右分层：右 依赖 左
+ 每层：上 （有可能）依赖 下
+ 下面的crate，以：`bevy_` 开头

## 主要 外部依赖

+ 下面的 crate，以：`bevy_` 开头

|crate|作用|依赖的外部crate|
|--|--|--|
|_math|数学库|[glam](https://crates.io/crates/glam)|
|_tasks|任务调度|[async-executor](https://crates.io/crates/async-executor) / [async-channel](https://crates.io/crates/async-channel)|
|_log|日志|[tracing-log](https://crates.io/crates/tracing-log)|
|_render|渲染|[wgpu](https://crates.io/crates/wgpu) & [naga](https://crates.io/crates/naga)|
|_audio|音频|[radio](https://crates.io/crates/radio)|
|_winit|窗口|[winit](https://crates.io/crates/winit)|
|_gilrs|输入|[gilrs](https://crates.io/crates/gilrs)|
|_gltf|gltf|[gltf](https://crates.io/crates/gltf)|
|_ui|GUI|flex布局：[stretch](https://crates.io/crates/stretch)|

## 参考

+ [Bevy 0.9 官方文档](https://bevyengine.org/news/bevy-0-9/)
+ [Bevy 0.9 API文档](https://docs.rs/bevy/0.9.0/bevy/)