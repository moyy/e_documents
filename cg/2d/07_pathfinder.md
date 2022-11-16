- [Servo:Pathfinder](#servopathfinder)
  - [1. 概述](#1-概述)
    - [1.1. 性能：笔记本上](#11-性能笔记本上)
    - [1.2. enum GLVersion](#12-enum-glversion)
    - [1.3. enum RendererLevel](#13-enum-rendererlevel)
  - [2. 设置深度值](#2-设置深度值)
  - [3. Program](#3-program)
    - [3.1. "d3d9/fill" FillProgramD3D9](#31-d3d9fill-fillprogramd3d9)
    - [3.2. "d3d9/tile" TileProgramD3D9](#32-d3d9tile-tileprogramd3d9)
    - [3.3. "d3d9/tile_clip_copy" ClipTileCopyProgramD3D9](#33-d3d9tile_clip_copy-cliptilecopyprogramd3d9)
    - [3.4. "d3d9/tile_clip_combine" ClipTileCombineProgramD3D9](#34-d3d9tile_clip_combine-cliptilecombineprogramd3d9)
    - [3.5. "d3d9/tile_copy" CopyTileProgram](#35-d3d9tile_copy-copytileprogram)
  - [4. Renderer::render_command()](#4-rendererrender_command)
  - [5. 数据结构](#5-数据结构)
    - [5.1. struct Fill](#51-struct-fill)
    - [5.2. DrawTileBatchD3D9](#52-drawtilebatchd3d9)
    - [5.3. struct TileObjectPrimitive](#53-struct-tileobjectprimitive)
    - [5.4. struct Clip](#54-struct-clip)
    - [5.5. struct TileBatchTexture](#55-struct-tilebatchtexture)

# Servo:Pathfinder

+ 官方: https://github.com/servo/pathfinder
+ fork-维护: https://github.com/GaiaWorld/pathfinder

## 1. 概述

+ GPU 渲染方案
+ 纯 Rust 实现的 矢量图、Canvas2D API 子集、Path 渲染器；

### 1.1. 性能：笔记本上

tiger.svg，1920*1080

+ GL4-D3D9 方案: 300 fps；cpu 40%，gpu 40%
+ GL4-D3D11 方案: 460 fps；cpu 14%，gpu 43%

### 1.2. enum GLVersion

目前 内部-实现 的 渲染后端 API

+ GL3, GL 3.3+
+ GLES3, GLES 3.0+
+ GL4, GL 4.3+

### 1.3. enum RendererLevel

计算 和 渲染 流程

+ D3D9: D3D-9 / GL 3.0 / WebGL 2.0 
    - CPU: Bin 用 rayon-并行计算 16*16 Tile
    - GPU: fill / composite
+ D3D11: D3D-11 / GL 4.3 / Metal / Vulkan / WebGPU
    - GPU: Bin 用 计算着色器 计算 16*16 Tile
    - GPU: fill / composite

## 2. 设置深度值

D3D9 方案中

+ 文件: renderer\src\builder.rs
+ 方法: build_tile_batches_for_draw_path_display_item(
+ 调用点: z_buffer_data: DenseTileMap::from_builder(|_| 0, tile_bounds)

## 3. Program

### 3.1. "d3d9/fill" FillProgramD3D9 

+ attribute
    - uvec2 aTessCoord;
    - uvec4 aLineSegment;
    - int aTileIndex;
+ uniform
    - sampler2D uAreaLUT;
    - vec2 uFramebufferSize;
    - vec2 uTileSize;

### 3.2. "d3d9/tile" TileProgramD3D9 

+ attribute
    - ivec2 aTileOffset;
    - ivec2 aTileOrigin;
    - uvec4 aMaskTexCoord0;
    - ivec2 aCtrlBackdrop;
    - int aPathIndex;
    - int aColor;
+ uniform
    - mat4 uTransform;
    - vec2 uTileSize;
    - sampler2D uTextureMetadata;
    - ivec2 uTextureMetadataSize;
    - sampler2D uZBuffer;
    - ivec2 uZBufferSize;
    - sampler2D uColorTexture0;
    - sampler2D uMaskTexture0;
    - sampler2D uDestTexture;
    - sampler2D uGammaLUT;
    - vec2 uColorTextureSize0;
    - vec2 uMaskTextureSize0;
    - vec2 uFramebufferSize;

### 3.3. "d3d9/tile_clip_copy" ClipTileCopyProgramD3D9

+ attribute
    - aTileOffset;
    - aTileIndex;
+ uniform
    - sampler2D uSrc;
    - vec2 uFramebufferSize;

### 3.4. "d3d9/tile_clip_combine" ClipTileCombineProgramD3D9

+ attribute
    - ivec2 aTileOffset;
    - int aDestTileIndex;
    - int aDestBackdrop;
    - int aSrcTileIndex;
    - int aSrcBackdrop;
+ uniform
    - sampler2D uSrc;
    - vec2 uFramebufferSize;

### 3.5. "d3d9/tile_copy" CopyTileProgram

+ attribute
    - ivec2 aTilePosition;
+ uniform
    - vec2 uTileSize
    - sampler2D uSrc;
    - mat4 uTransform;
    - vec2 uFramebufferSize;

## 4. Renderer::render_command()

+ RenderCommand::Start
+ RenderCommand::UploadTextureMetadata(Vec<TextureMetadataEntry>)
+ RenderCommand::AddFillsD3D9(Vec<Fill>)
  - RendererD3D9::add_fills()
  - 积累到 65536 后，就 RendererD3D9::draw_buffered_fills
+ RenderCommand::FlushFillsD3D9
    - RendererD3D9::draw_buffered_fills();
      * RendererD3D9::draw_fills() --> draw_elements_instanced()
+ RenderCommand::DrawTilesD3D9(DrawTileBatchD3D9)
    - RendererD3D9::upload_and_draw_tiles();
      * RendererD3D9::clip_tiles() --> draw_elements_instanced()
      * upload_tiles()
      * upload_z_buffer()
      * draw_tiles() --> draw_elements_instanced()
+ RenderCommand::Finish

## 5. 数据结构

### 5.1. struct Fill

+ line_segment: LineSegmentU16(from_x, from_y, to_x, to_y)
+ link: u32

### 5.2. DrawTileBatchD3D9

+ tiles: Vec<TileObjectPrimitive>,
+ clips: Vec<Clip>,
+ z_buffer_data: DenseTileMap<i32>,
+ color_texture: Option<TileBatchTexture>,
+ filter: Filter,
+ blend_mode: BlendMode,

### 5.3. struct TileObjectPrimitive

+ tile_x: i16,
+ tile_y: i16,
+ alpha_tile_id: AlphaTileId,
+ path_id: PathId,
+ color: u16,
+ ctrl: u8,
+ backdrop: i8,

### 5.4. struct Clip

+ dest_tile_id: AlphaTileId,
+ dest_backdrop: i32,
+ src_tile_id: AlphaTileId,
+ src_backdrop: i32,

### 5.5. struct TileBatchTexture

+ page: TexturePageId,
+ sampling_flags: TextureSamplingFlags,
+ composite_op: PaintCompositeOp,