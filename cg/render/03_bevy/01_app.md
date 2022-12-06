# APP 框架

+ bevy_app
+ bevy_core
+ bevy_asset

## bevy_app

+ 定义了: CoreStage 和 WorldApp
+ 定义了：App, SubApp 每个 App都有自己的 World 和 Scheduler
+ 定义了：Plugin，PluginGroup
	- Plugin::build 可以 在 初始化时 向 App 注入 ECS概念

除Startup外，每帧 按这个 顺序 执行 Stage:

+ CoreStage::First
+ CoreStage::StartUp -- 仅执行一次；
	- StartupStage::PreStartup
	- StartupStage::StartUp
	- StartupStage::PorstStartUp
+ CoreStage::PreUpdate
+ CoreStage::Update
+ CoreStage::PostUpdate
+ CoreStage::Last

## bevy_core

## bevy_asset