# 移动端 GPU

## 1. 索引

+ [功耗 和 带宽](./01_bandwidth.md)
+ [TBR](./02_tbr.md)
+ [Arm Mali](./03_arm_mali.md)

## 2. CPU/GPU

* ARM 指令集架构
	+ Arm-V7a：32位
	+ Arm-V8a：64位
* ARM-Core
	+ ARM公司 出卖的是 CPU/GPU 设计 框架；
* `SoC`：System on Chip，片上系统；
	+ 集成了 CPU，GPU，内存，USB，射频，显示控制器，音视频解码，感应器 等功能和接口；

## 3. `SoC` 厂商

* GPU架构
	+ `Arm Mali`：ARM 公版 GPU 架构；
	+ `Adreno`：高通
	+ `ProerVR` GPU架构：Apple M1 前；
	+ Apple 自研：M1 架构 以后

|厂商|CPU 品牌|GPU|
|--|--|--|
|Apple 苹果|A14|powerVR架构：TBDR|
|Qualcomm 高通|Snapdrgen(骁龙)|Adreno|
|MediaTek 联发科|天玑|Mali|
|HiSillcon 华为海思|麒麟 Kirin|Mali|
|Sansung 三星|Exynos|Mali|