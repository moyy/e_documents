[TOC]

# ComputePipeline

## 1、GPUComputePipelineDescriptor

|名字|类型|
|--|--|
|layout|GPUPipelineLayout or "auto"|"auto"|
|(必须) compute|GPUProgrammableStage|

## 1.1、GPUProgrammableStage

|名字|类型|
|--|--|
|(必须) module|GPUShaderModule|
|(必须) entryPoint|string|
|constants|{string: bool, f32, i32, u32, f16}|