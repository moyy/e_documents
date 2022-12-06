# bevy_render

## 结构

+ render_app: App
	- render_app::world::Resource<RenderGraph>
+ renderer
	- Device, Queue, CommandEncoder
+ render_resource: wgpu 的 封装
+ render_graph
+ render_phase: 5个 RenderStage
	- Extract：将 AppWorld 的 数据 拷贝到 RenderWorld 来
	- Prepare: 准备 GPU 资源，上传 Buffer & Texture
	- Queue：归类 渲染列表，半透明，不透明，UI，粒子...
	- PhaseSort：渲染列表 排序
	- Render

## render_phase 渲染列表

### trait PhaseItem 渲染项

+ type SortKey: Ord
+ fn sort_key(&self) -> Self::SortKey
+ fn draw_function(&self) -> DrawFunctionId

#### P: PhaseItem

+ trait EntityPhaseItem: PhaseItem
+ trait CachedPipelinePhaseItem: PhaseItem
+ trait BatchedPhaseItem: EntityPhaseItem

### trait Draw < P >

``` rs
self.draw (
	world: &World,
	pass: &mut TrackedRenderPass>,
	view: Entity,
	item: &P
);
```

### struct RenderPhase < P > 渲染列表

+ items: Vec < P >,

### struct DrawFunctions < P >

+ internal: RwLock < DrawFunctionsInternal < P > >,
	- draw_functions: Vec < Box < dyn Draw < P > > >,
	- indices: HashMap < TypeId, DrawFunctionId >,

### trait RenderCommand < P >

+ type Param: SystemParam;

``` rs
RenderCommand::render<'w>(
	view: Entity,
	item: &P,
	param: SystemParamItem<'w, '_, Self::Param>,
	pass: &mut TrackedRenderPass<'w>,
) -> RenderCommandResult;
```

##### 子类

+ struct SetItemPipeline

### trait AddRenderCommond::add_render_command

``` rs

impl AddRenderCommand for App {
	fn add_render_command < P, C >(&mut self) -> &mut Self
    where
		P: PhaseItem
		C: RenderCommand<P> + Send + Sync + 'static
        <C::Param as SystemParam>::Fetch: ReadOnlySystemParamFetch,
}
```

## RenderStage::Render

``` rs

fn render_system(world: &mut World) {
	graph = Resource<RenderGraph>;

	graph.update();

	render_device = Resource<RenderDevice>;
	render_queue = Resource<RenderQueue>;

	RenderGraphRunner::run(
        graph,
        render_device,
        render_queue,
        world,
    );

	Present();

}
```

## RenderGraphRunner::run

+ 创建 CommandEncoder
+ 收集 没有 input_slots 的 Node

+ 提交 CommandEncoder

## struct RenderGraph

+ nodes: HashMap<NodeId, NodeState>,
+ node_names: HashMap<Cow<'static, str>, NodeId>,
+ sub_graphs: HashMap<Cow<'static, str>, RenderGraph>,
+ input_node: Option<NodeId>, // "GraphInputNode"

|方法|作用|返回|
|--|--|--|
|update(&mut World)|更新 node & subgraph||
|set_input(inputs: Vec<SlotInfo>)|GraphInputNode|NodeId|
|add_node(name, n: Node)||NodeId|
|add_slot_edge|||
|add_node_edge|||
|add_sub_graph|||
|input_node()||Option<&NodeState>|
|get_node_state(label)||Result<&NodeState>|
|get_node_state_mut(label)||Result<&NodeState>|
|get_node_id(label)||Result<&NodeId>|
|get_node(label)||&T:Node|
|get_node_mut||&mut T:Node|
|validate_edge|||
|has_edge|||
|iter_nodes|||
|iter_nodes_mut|||
|iter_sub_graphs|||
|iter_sub_graphs_mut|||
|iter_node_inputs|||
|iter_node_outputs|||
|get_sub_graph|||
|get_sub_graph_mut|||

## struct NodeState

+ id: NodeId
+ name: Option<Cow<'static, str>>
+ type_name: &'static str
+ node: Box<dyn Node>
+ input_slots: SlotInfos
+ output_slots: SlotInfos
+ edges: Edges

## struct SlotInfos

+ slots: Vec<SlotInfo>

## struct SlotInfo

+ name: str
+ slot_type: SlotType

## enum SlotType

+ Buffer
+ TextureView
+ Sampler
+ Entity

## enum SlotValue

+ Buffer(Buffer)
+ TextureView(TextureView)
+ Sampler(Sampler)
+ Entity(Entity)

## trait Node

``` rs
    fn update(
        &self,
        &mut world
    ) {

    }

    fn run(
        &self,
        graph: &mut RenderGraphContext,
        _render_context: &mut RenderContext,
        _world: &World,
    );
```

## enum Edge

+ SlotEdge: output 在 input 之前 执行
    - input_node: NodeId
    - input_index: usize
    - output_node: NodeId
    - output_index: usize
+ NodeEdge：output 在 input 之前 执行
    - input_node: NodeId
    - output_node: NodeId

|||
|--|--|
|get_input_node(&self)|NodeId|
|get_output_node(&self)|NodeId|

## struct Edges

+ id: NodeId,
+ input_edges: Vec<Edge>,
+ output_edges: Vec<Edge>,

|||
|--|--|
|add_input_edge(edge: Edge)||
|add_output_edge(edge: Edge)||
|has_input_edge(edge: &Edge)|bool|
|has_output_edge(edge: &Edge)|bool|
|get_input_slot_edge(index: usize)|Result<&Edge>|
|get_output_slot_edge(index: usize)|Result<&Edge>|

## struct RenderContext

+ render_device: RenderDevice
+ command_encoder: CommandEncoder

## struct RenderGraphContext

某个 Node 的 执行环境

+ graph: &'a RenderGraph,
+ node: &'a NodeState,
+ inputs: &'a [SlotValue],
+ outputs: &'a mut [Option< SlotValue >],
+ run_sub_graphs: Vec< RunSubGraph >,

|方法|作用||
|--|--|--|
|inputs|||
|input_info|||
|output_info|||
|get_input|||
|get_input_texture|||
|get_input_sampler|||
|get_input_buffer|||
|get_input_entity|||
|set_output|||
|run_sub_graph|||
|finish|||