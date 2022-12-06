[TOC]

# RenderBundle

## 1、用途：指令录制 缓冲器

如果 某些物件 的录制指令 总是不变，就 可以 加速

+ 先 录制到 `RenderBuldle` 中；
+ 每帧录制指令时，只要 向 CommandEncoder 执行 RernderBundle 就好了；
+ 如果 某帧 指令有改动，就：创建 新的 RenderBundle 重新录；

例子：场景中的 不透明的 静态物件

+ 初始化：用 Bundle 录制好 每个物件，不用排序
+ 每帧:
	+ setBindGroup(相机的 Uniform)
	+ executeBundles(bundle)

## 2、性能：[参考这里](https://www.dounaite.com/article/6254bfa23351efabace62866.html)

![](/uploads/render_tech/images/m_46b80d116c305036d9977811b032a8dd_r.png)

## 3、代码：参考 wgpu/examples/msaa-line

+ RenderBundleEncoder = Device.create_render_bundle_encoder(...);
+ RenderBundle = RenderBundleEncoder.finish(...);
+ RenderPass.executeBundles(&RenderBundle);

### 3.1、初始化

``` rs
let mut bundle_encoder: RenderBundleEncoder = device.create_render_bundle_encoder(
    &wgpu::RenderBundleEncoderDescriptor {
        label: None,
        color_formats: &[config.format],
        depth_stencil: None,
        sample_count,
        multiview: None,
    }
);

bundle_encoder.set_pipeline(&pipeline);
bundle_encoder.set_vertex_buffer(0, vertex_buffer.slice(..));
bundle_encoder.draw(0..vertex_count, 0..1);

let render_bundle = bundle_encoder.finish(&wgpu::RenderBundleDescriptor {
    label: Some("main"),
});
```

### 3.2、录制渲染指令

``` rs
let command_encoder: CommandEncoder = ...;
let mut rp = command_encoder.begin_render_pass(...);
rp.execute_bundles(iter::once(&render_bundle));
```

### 3.3、指令有变化，就要重新创建新的 `RenderBundle`