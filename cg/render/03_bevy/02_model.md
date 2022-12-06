# Bevy 模型

## 参考

+ [官网](https://bevyengine.org/)
+ [表单](https://bevy-cheatbook.github.io/pitfalls/performance.html)

## 大纲

+ 功能
    - Fixed Timestep
    - Transform
    - Window & Input
    - Asset
    - Camera & Pbr
    - UI & Sprite
+ 概念
    - 基础
        * AppBuilder
        * Entity & Component
        * Resource
        * System
        * Query
        * Commands
        * Event
    - 进阶
        * Local Resource
        * Plugin & PluginGroup
        * SystemLabel
        * SystemSet
        * Hierachical Entity
        * Change Detection
        * System Chainning
        * QuerySet
        * State
        * RunCriteria
    - 高级
        * Stage
        * Non-Send Resource
        * 实现

## Entity & Components

+ Entity：一个id，标志一组Component实例；
+ Component：任何struct或enum，但是对每个Entity而言，每个Component类型必须唯一；
    - 惯用法：用 空struct 作为某个实体的“Label Component”，用于Query某类Entity
+ Bundle
    - 一个Bundle就是若干Component的捆绑
    - 用 #[derive(Bundle)对 struct/enum 进行注解
    - Component的Tuple也是Bundle，比如：(CA, CB， CC)
    - Query查询只适用于Component；
    - `TODO` 底层实现是不是：Archetype？

#### Bevy 两种 存储 方式

* 如果您要存储的数据是具有 Bundle 特征的结构，则所有具有相同组件的实体都将存储为相同的原型
    + #[derive(Bundle)] 的结构体
    + 组件元组
* 如果数据存储在基本结构中，则数据将仅与相同结构类型的其他数据一起存储。

## Resource

+ 放到World上的全局的Component，和Entity无关；
+ 每种Resource的类型必须唯一；
+ System查询资源：Res< ResType >, ResMut< ResType >
+ 初始化：实现Default，或 FromWorld
+ 创建
    - AppBuilder: insert_resource, init_resource
    - System内，通过Commands: insert_resource, init_resource

## System

+ 注：通过 State 或 RunCriteria，可以让System 偶尔运行
+ SystemParameter: 顶层参数 或 元组 数量最多16个；多出可以嵌套元组
+ 类型：有Bevy内置定死
+ 基础使用：下面类型 及其 元组
    - Commands
    - Entity
    - Component Query
    - QuerySet
    - Res, ResMut
    - EventReader EventWriter
+ 组装：在AppBuilder
    - add_setup_system(fn.system())
    - add_system(fn.system())

## Query

+ 只能Query单个Component，不能Query Bundle
+ 如果查询的结果只有单个实体（比如就只有玩家），那就直接 q.single() 好了
+ 过滤器：With，Without
    - 组合：元组表示与逻辑，Or<(...)>表示或逻辑

## Commands

`TODO:` Command不会立即生效，会缓存到Stage的末尾？如果没有用 Stage，会在下一帧更新时候看到命令的结果；

+ 资源
    - commands.insert_resource(MyData::default())
+ Entity
    - 查找：entity = commands.entity(id)
    - 生成：id = commands.spawn().insert_bundle(MyBundle::default()).insert(MyComponent::default()).id();
    - 销毁：command.entity(id).despawn()
+ 组件/Bundle
    - 加入：commands.entity(id).insert_bundle(MyBundle::default())
    - 移除：command.entity(id).remove::< MyComponent >.remove_bundle::< MyBundle >()

## Event

+ 注册：AppBuidelr().add_event::< Type >()
+ 发送：在System中用 w: EventWriter< Type >：w.send(数据)
+ 接收：在System中用 r: EventReader< Type >：for ev in r.iter() { ... }
+ 注意：
    - Event 会被存储到下一帧结束，然后丢弃；
    - 1-frame-lag: 有可能会延迟到下一帧才会收到Event

## AppBuilder

组装器：Plugin, System, Event, Resource, Stage

## Local< SystemState > 每System的资源

* 如果System函数的参数是：local: Local< SystemState >，那么就会在system的闭包维护一个local
* 每个System的Local都是不同的实例
* 同一个System可以有多个类型相同的Local< Type >，大概是 有参数顺序决定的实例，不会混淆；

## Plugin & PluginGroup

模块化 组装，有了Plugin，就可以拆分成多个crate发布

AppBuilder 可以用 add_plugins_with 添加 PluginGroup的时候，自行决定 禁用某些Plugin

``` rs

trait Plugin {
    fn build(&self, app: &mut AppBuilder);
}

```

## SystemLabel 系统执行排序

通过：SystemLabel 显示执行串行顺序

* SystemLabel，可以是 str；如果是枚举，用 #[derive(SystemLabel)]
* .label/before/after，可以指定多个before和after，会根据指定进行 拓扑排序
* AppBuilder().add_system(f.system().label("f")).add_system(p.system().label("p").after("f"));
* SystemSet，用于方便指定多个相同label的System

## SystemSet

指定一组System的共同属性：SystemLabel, State

``` rs

app_builder.add_system_set(
    SystemSet::new()
        .label("input")
        .with_system(key.system())
        .with_system(mouse.system())
        .before("update")
)

```

## Entity的层次结构 Hierachical Entity

`TODO` https://maz.digital/in-depth-analyze-entity-composition-bevy-22

* Entity push_children(&[e1, e2, ...])
    + commands.spawn().insert(Player).push_children(&[helmet,bananas]);
* 连着子Entity 递归 释放：
    + commands.entity(entity).despawn_recursive();

## Change Detection

会检查上次该System运行以来的任何变化；

+ Query-过滤器中的类型可以是
    - Added< ComponentType >
    - Changed< ComponentType > 包含了Added的情况

``` rs

query: Query<
        // components
        (&Health, &PlayerXp),
        // filters
        (Without<Enemy>, Or< (Changed<Health>, Changed< PlayerXp >) >), 
```

## System Chainning

* 串行，f.system().chain(e.system())
* 组成更大的System
* 每个函数可以分开处理一部分数据，比如有个函数只处理错误的情况

## QuerySet

* 可以进行多个有冲突的查询，每个QuerySet最多支持4个Query
* 查询结果用：q0(), q1(), q2(), q3() 区分

``` rs

fn reset_health(
    // access the health of enemies and the health of players
    // (note: some entities could be both!)
    mut q: QuerySet<(
        Query<&mut Health, With<Enemy>>,
        Query<&mut Health, With<Player>>
    )>,
) {
    // set health of enemies
    for mut health in q.q0_mut().iter_mut() {
        health.hp = 50.0;
    }

    // set health of players
    for mut health in q.q1_mut().iter_mut() {
        health.hp = 100.0;
    }
}

```

## State

* SystemSet::on_enter(状态枚举).with_system(f1.system())
* SystemSet::on_exit(状态枚举).with_system(f1.system())
* SystemSet::on_update(状态枚举).with_system(f1.system())

## Run Criteria

Bevy 的FixedStep也是一个 Run Criteria System

* 想控制某个时候才运行系统，用这个
* SystemSet::new().with_run_criteria(run_if_connected.system()).wtih_system(...)
* run_if_connected 函数返回：ShouldRun::Yes/No 决定后面系统的运行

## Stage

* 阶段之间的边界实际上是硬同步点
* 确保在下一阶段的任何系统开始之前，上一阶段的所有系统都已经完成
* add_stage_before/after(插到哪个阶段, 本阶段的Label, single || paralle || exclude)
* add_system_to_stage(Label, System)

## `TODO` Non-Send Resource

## `TODO` RemovedComponents

## 实现

#### `TODO` World

#### `TODO` Storage

#### `TODO` Archetypes

#### `TODO` Schedule