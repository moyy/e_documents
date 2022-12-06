[TOC]

# QuerySet

## 1、GPUQuerySetDescriptor

|名字|类型|默认值|
|--|--|--|
|(必须) type|"occlusion","timestamp"||
|(必须) count|u32||

## 2、遮挡查询："occlusion"

仅用于 RenderPipeline

流程：

+ beginRenderPass 时，GPURenderPassDescriptor.occlusionQuerySet = 创建的 QuerySet
+ 调用 beginOcclusionQuery()，endOcclusionQuery()，不能 嵌套