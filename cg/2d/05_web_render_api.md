- [WebRender](#webrender)
  - [1. 初始化](#1-初始化)
    - [1.1. 选项](#11-选项)
    - [1.2. 创建 RenderApi](#12-创建-renderapi)
    - [1.3. 创建其他](#13-创建其他)
  - [2. 渲染循环](#2-渲染循环)
  - [3. 退出 的 清理工作](#3-退出-的-清理工作)
  - [4. 渲染逻辑](#4-渲染逻辑)
    - [4.1. 原型](#41-原型)
  - [5. RenderApi](#5-renderapi)
  - [6. Transaction](#6-transaction)
    - [6.1. enum SceneMsg](#61-enum-scenemsg)
      - [6.2. enum FrameMsg](#62-enum-framemsg)
    - [6.3. enum ResourceUpdate](#63-enum-resourceupdate)
  - [7. DisplayListBuilder](#7-displaylistbuilder)
    - [7.1. enum DisplayItem](#71-enum-displayitem)

# WebRender

## 1. 初始化

### 1.1. 选项

``` rs
let opts = webrender::RendererOptions {
    resource_override_path: res_path,
    precache_flags: E::PRECACHE_SHADER_FLAGS,
    clear_color: ColorF::new(0.3, 0.0, 0.0, 1.0)
    DebugFlags::ECHO_DRIVER_MESSAGES | DebugFlags::TEXTURE_CACHE_DBG,
    ..options.unwrap_or(webrender::RendererOptions::default())
};
```

### 1.2. 创建 RenderApi

+ winit
+ gleam
+ glutin

``` rs

// Renderer, RenderApiSender
let (mut renderer, sender) = webrender::Renderer::new(gl, notifier, opts, None).unwrap();

// 创建 RenderApi
let mut api = sender.create_api();

// 创建 Document --> DocumentId
let document_id = api.add_document(device_size);

```

### 1.3. 创建其他

``` rs

// Epoch 回合
let epoch = Epoch(0);

// Pipeline 管线
let pipeline_id = PipelineId(0, 0);

```

## 2. 渲染循环

``` rs
    // 事务
    let mut txn = Transaction::new();

    // DisplayListBuilder 指令
    let mut builder = DisplayListBuilder::new(pipeline_id);
    builder.begin();

    // 渲染 逻辑
    render_frame(
        &mut api,
        &mut builder,
        &mut txn,
        device_size,
        pipeline_id,
        document_id,
    );

    let layout_size = device_size.to_f32() / euclid::Scale::new(device_pixel_ratio);
    // 设置 渲染列表
    txn.set_display_list(
        epoch,
        Some(ColorF::new(0.3, 0.0, 0.0, 1.0))
        layout_size,
        builder.end()
    );

    // 设置 pipeline
    txn.set_root_pipeline(pipeline_id);

    // 创建 一帧
    txn.generate_frame(0, RenderReasons::empty());

    // 发送 指令
    api.send_transaction(document_id, txn);

    // 更新
    renderer.update();

    // 渲染
    renderer.render(device_size, 0).unwrap();

    // 刷新重置
    let _ = renderer.flush_pipeline_info();

    // 调用 glutin 交换 缓冲区
    windowed_context.swap_buffers().ok();
```

## 3. 退出 的 清理工作

``` rs
    renderer.deinit();
```

## 4. 渲染逻辑

### 4.1. 原型

``` rs

pub fn render_frame(
    api: &mut RenderApi,
    builder: &mut DisplayListBuilder,
    txn: &mut Transaction,
    device_size: DeviceIntSize,
    pipeline_id: PipelineId,
    document_id: DocumentId,
) {
    // 内存填充大小
    let content_bounds = LayoutRect::from_size(LayoutSize::new(800.0, 600.0));

    // 滚动区域
    let root_space_and_clip = SpaceAndClipInfo::root_scroll(pipeline_id);

    let spatial_id = root_space_and_clip.spatial_id;

    // 压入上下文
    builder.push_simple_stacking_context(
        content_bounds.min,
        spatial_id,
        PrimitiveFlags::IS_BACKFACE_VISIBLE,
    );

    // 渲染 指令 在这里 填充

    // TODO 渲染四边形
    draw_rect()

    builder.pop_stacking_context();
}

```

## 5. RenderApi

WebRender 交互的主入口

+ send_transaction
+ hit_test
+ add_document
+ delete_document
+ generate_font_key & 字体 glyph 信息
+ generate_image_key
+ flush_scene_builder
+ set_parameter
+ 调试
    - send_debug_cmd
    - save_capture
    - load_capture
    - start_capture_sequence
    - stop_capture_sequence

## 6. Transaction

一组应用在 document 的 指令

+ 成员
    - scene_ops: Vec< SceneMsg >
    - frame_ops: Vec< FrameMsg >
    - resource_updates: Vec< ResourceUpdate >
    - notifications: Vec< NotificationRequest >
+ 方法
    - txn.set_root_pipeline(pipeline_id)
    - txn.set_display_list()
    - txn.update_resources()
    - txn.generate_frame()

### 6.1. enum SceneMsg

+ UpdateEpoch
+ SetRootPipeline
+ RemovePipeline
+ SetDisplayList
+ SetDocumentView
+ SetQualitySettings

#### 6.2. enum FrameMsg

+ UpdateEpoch
+ HitTest
+ RequestHitTester
+ SetScrollOffsets
+ ResetDynamicProperties,
+ AppendDynamicProperties
+ AppendDynamicTransformProperties
+ SetIsTransformAsyncZooming

### 6.3. enum ResourceUpdate

+ AddImage
+ UpdateImage
+ DeleteImage
+ AddBlobImage
+ UpdateBlobImage
+ DeleteBlobImage
+ SetBlobImageVisibleArea
+ AddFont
+ DeleteFont
+ AddFontInstance
+ DeleteFontInstance

## 7. DisplayListBuilder

+ new(pipeline_id: PipelineId) -> Self;
+ begin()
+ end()
+ reset()
+ save()
+ restore()
+ clear_save()
+ emit_display_list()
+ push_item_to_section()
+ push_item(item: &di::DisplayItem)
+ push_spatial_tree_item(item: &di::SpatialTreeItem)
+ push_iter()
+ create_gradient()
+ create_radial_gradient()
+ create_conic_gradient()
+ push_stacking_context()
+ pop_stacking_context()
+ push_stops()
+ push_backdrop_filter()
+ push_filters()
+ define_scroll_frame()
+ define_clip_chain()
+ define_clip_image_mask()
+ define_clip_rect()
+ define_clip_rounded_rect()
+ define_sticky_frame()
+ push_iframe()
+ start_item_group()
+ finish_item_group(key: di::ItemKey) -> bool
+ cancel_item_group(discard: bool)
+ push_reuse_items(key: di::ItemKey)

### 7.1. enum DisplayItem

+ Rectangle(RectangleDisplayItem)
+ ClearRectangle(ClearRectangleDisplayItem)
+ HitTest(HitTestDisplayItem)
+ Text(TextDisplayItem)
+ Line(LineDisplayItem)
+ Border(BorderDisplayItem)
+ BoxShadow(BoxShadowDisplayItem)
+ PushShadow(PushShadowDisplayItem)
+ Gradient(GradientDisplayItem)
+ RadialGradient(RadialGradientDisplayItem)
+ ConicGradient(ConicGradientDisplayItem)
+ Image(ImageDisplayItem)
+ RepeatingImage(RepeatingImageDisplayItem)
+ YuvImage(YuvImageDisplayItem)
+ BackdropFilter(BackdropFilterDisplayItem)
+ Clips
    - RectClip(RectClipDisplayItem)
    - RoundedRectClip(RoundedRectClipDisplayItem)
    - ImageMaskClip(ImageMaskClipDisplayItem)
    - ClipChain(ClipChainItem)
+ Spaces and Frames that content can be scoped under
    - Iframe(IframeDisplayItem)
    - PushReferenceFrame(ReferenceFrameDisplayListItem)
    - PushStackingContext(PushStackingContextDisplayItem)
+ These marker items indicate an array of data follows, to be used for the next non-marker item.
    - SetGradientStops
    - SetFilterOps
    - SetFilterData
    - SetFilterPrimitives
    - SetPoints
+ These marker items terminate a scope introduced by a previous item.
    - PopReferenceFrame
    - PopStackingContext
    - PopAllShadows
+ 其他
    - ReuseItems(ItemKey)
    - RetainedItems(ItemKey)