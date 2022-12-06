# bevy_pbr

## 获取

+ graph = render_app.world.get_resource_mut::< RenderGraph >();
+ draw_3d_graph = graph.get_sub_graph_mut("draw_3d");

## Node

+ SHADOW_PASS, ShadowPassNode

## Node-Edge

SHADOW_PASS --> MAIN_PASS

## Slot-Edge

(input_node, VIEW_ENTITY) --> (SHADOW_PASS, ShadowPassNode::IN_VIEW)

## add_render_command

+ add_render_command::< Shadow, DrawShadowMesh>();
+ material.rs
	+ add_render_command:: < Transparent3d, DrawMaterial < M > >()
	+ add_render_command:: < Opaque3d, DrawMaterial < M > >()
	+ add_render_command:: < AlphaMask3d, DrawMaterial < M > >()
+ wireframe.rs
	.add_render_command::<Opaque3d, DrawWireframes>()

#### type DrawShadowMesh

``` rs

type DrawShadowMesh = (
	SetItemPipeline,
	SetShadowViewBindGroup < 0 >,
	SetMeshBindGroup < 1 >,
	DrawMesh,
)
```

#### type DrawMaterial

M: SpecializedMaterial

``` rs
type DrawMaterial < M > = (
    SetItemPipeline,
    SetMeshViewBindGroup<0>,
    SetMaterialBindGroup<M, 1>,
    SetMeshBindGroup<2>,
    DrawMesh,
);
```

#### type DrawWireframes

``` rs
type DrawWireframes = (
    SetItemPipeline,
    SetMeshViewBindGroup<0>,
    SetMeshBindGroup<1>,
    DrawMesh,
);
```