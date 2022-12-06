# bevy_core_pipeline

## Resource

+ DrawFunctions < Transparent2d >
+ DrawFunctions < Opaque3d >
+ DrawFunctions < AlphaMask3d >
+ DrawFunctions < Transparent3d >

## RenderStage::Extract

+ extract_clear_color
+ extract_core_pipeline_camera_phases

## RenderStage::Prepare

+ prepare_core_views_system

## RenderStage::PhaseSort

+ sort_phase_system:: < Transparent2d >
+ sort_phase_system:: < Opaque3d >
+ sort_phase_system:: < AlphaMask3d >
+ sort_phase_system:: < Transparent3d >

## RenderPhase --> Resource

+ Res < ActiveCameras >
	- CAMERA_2D --> Entity
		* Component: RenderPhase::< Transparent2d >
	- CAMERA_3D --> Entity
		* RenderPhase:: < Opaque3d >
        * RenderPhase:: < AlphaMask3d >
        * RenderPhase:: < Transparent3d >

## 渲染图

定义了 clear | 2D | 3D 渲染 管线

#### render_app::World::Resource<RenderGraph>

+ SubGraph
    - clear_graph
    - draw_2d_graph
    - draw_3d_graph
    - draw_ui_graph
+ Node
    - MAIN_PASS_DEPENDENCIES: EmptyNode
    - MAIN_PASS_DRIVER: MainPassDriverNode
		* g.run_sub_graph(draw_2d_graph, ...)
		* g.run_sub_graph(draw_3d_graph, ...)
	- CLEAR_PASS_DRIVER: ClearPassDriverNode
		* g.run_sub_graph(clear_graph, ...)
+ Node-Edge
    - MAIN_PASS_DEPENDENCIES --> MAIN_PASS_DRIVER
    - CLEAR_PASS_DRIVER --> MAIN_PASS_DRIVER

#### clear_graph

+ Node
    - CLEAR_PASS: ClearPassNode

#### draw_3d_graph

+ Node
    - MAIN_PASS: MainPass3dNode
    - input_node_id = set_input([VIEW_ENTITY : SlotType::Entity])
+ Slot-Edge
    - (input_node_id, VIEW_ENTITY) --> (MAIN_PASS, IN_VIEW)

#### draw_2d_graph

+ Node
    - MAIN_PASS: MainPass2dNode
    - input_node_id = set_input([VIEW_ENTITY : SlotType::Entity])
+ Slot-Edge
    - (input_node_id, VIEW_ENTITY) --> (MAIN_PASS, IN_VIEW)

## MainPass3dNode

+ 迭代：world.Resource< DrawFunctions < Opaque3d > >
+ 迭代：world.Resource< DrawFunctions < AlphaMask3d > >
+ 迭代：world.Resource< DrawFunctions < Transparent3d > >

## MainPass2dNode

+ 迭代：world.Resource< DrawFunctions < Transparent2d > >