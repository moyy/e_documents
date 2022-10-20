- [OpenGL中 混合函数](#opengl中-混合函数)
  - [1. 重点：`预乘要点`：](#1-重点预乘要点)
  - [2. 混合 说明](#2-混合-说明)
    - [2.1. 参数](#21-参数)
    - [2.2. 函数：glBlendEquation, glBlendFunc](#22-函数glblendequation-glblendfunc)
    - [2.3. 函数：glBlendEquationSeparate, glBlendFuncSeparate](#23-函数glblendequationseparate-glblendfuncseparate)
  - [3. Babylon.js 混合 模式](#3-babylonjs-混合-模式)
  - [4. 模拟 PS 图像 25种 混合模式](#4-模拟-ps-图像-25种-混合模式)

# OpenGL中 混合函数

[可视化工具](https://www.andersriggelsen.dk/glblendfunc.php)

## 1. 重点：`预乘要点`：

* FBO初值：dst.rgba = (0, 0, 0, 0)
* 物体渲染到 FBO:
	+ normal模式：
		+ rgb = `src.a` * src.rgb + `(1-src.a)` * dst.rgb
		+ a = = `1` * src.a + `(1 - src.a)` * dst.a
	+ add 模式：
		- rgb = `src.a` * src.rgb + `1` * dst.rgb
		- a = `0` * src.a + `1` * dst.a
* FBO 混合到目标，预乘模式
	+ rgb = `1` * src.rgb + `(1-src.a)` * dst.rgb
	+ a = `1` * src.a + `1` * dst.a

## 2. 混合 说明

### 2.1. 参数

+ dst：已经 在 目标的rgba；
+ src：待混合的rgba；
+ FactorDst: 目标因子；
+ FactorSrc：源因子；
+ op: 混合操作本身，有+，-，max，min

### 2.2. 函数：glBlendEquation, glBlendFunc

公式：FactorSrc * src.rgba op FactorDst * dst.rgba

这里的乘法和加法是指：对每个分量分别独立的相乘和相加；

* glBlendEquation：控制 op，有：+，-，max，min
* glBlendFunc：控制 FactorSrc 和 FactorDst的；

### 2.3. 函数：glBlendEquationSeparate, glBlendFuncSeparate

alpha 公式：FactorSrc_a * src.a op_a FactorDst_a * dst.a

rgb 公式：FactorSrc_rgb * src.rgb op_rgb FactorDst_rgb * dst.rgb

* glBlendEquationSeparate：控制 op_alpha 和 op_rgb，有：+，-，max，min
* glBlendFuncSeparate：控制:
    + FactorSrc_a 和 FactorDst_a; FactorSrc_rgb 和 FactorDst_rgb;

## 3. Babylon.js 混合 模式

[Babylon.js 混合 模式](https://github.com/BabylonJS/Babylon.js/blob/master/src/Engines/Extensions/engine.alpha.ts)

+ dst.a   =   FSrcA * src.a +     FDstA * dst.a
+ dst.rgb = FSrcRGB * src.rgb + FDstRGB * dst.rgb

** 注：** `红色`名称 是 常用 混合

|名称|FSrcRGB|FDstRGB|FSrcA|FDstA|特殊说明|
|--|--|--|--|--|--|
|`disable`|-|-|-|-|用 gl.disable(gl.BLEND)，效果：src 直接覆盖 |
|`combine`/`normal`（混合）|src.a|1 - src.a|1|1||
|`add`（加）|src.a|1|0|1||
|`premultiplied`（预乘）|1|1 - src.a|1|1||
|[premultiplied_porterduff](http://ssp.impulsetrain.com/porterduff.html)|1|1 - src.a|1|1 - src.a||
|oneone|1|1|0|1||
|subtract（减）|0|1 - src.rgb|1|1||
|multiply（乘）|dst.rgb|0|1|1||
|maximized（最大）|src.a|1 - src.rgb|1|1||
|interpolate（插值）|const.rgb|1 - const.rgb|const.a|1 - const.a||
|screen_mode（屏幕？）|1|1 - src.rgb|1|1 - src.a||
|oneone_oneone|1|1|1|1||
|alpha2color|dst.a|1|0|0||
|alpha_rev_one_minus|1 - dst.rgb|1 - src.rgb|1 - dst.a|1 - src.a||
|src_dst_1-src.a|1|1 - src.a|1|1 - src.a||
|oneone_onezero|1|1|1|0||
|exclusion（排除）|1 - dst.rgb|1 - src.rgb|0|1||

## 4. 模拟 PS 图像 25种 混合模式

[模拟 PS 图像 25种 混合模式](https://zhuanlan.zhihu.com/p/26141725)

+ [代码](https://www.shadertoy.com/view/XdS3RW)
+ PS 混合模式 分类
	- 普通类：alpha混合、溶解
	- 变亮类：亮度加强、颜色变浅、去色、滤色、线性减淡等
	- 变暗类：变暗、深色、叠加、颜色加深、线性加深
	- 对比类：叠加、强光、柔光、线性光、点光
	- 负片类：差值、排除
	- 相消：减去、划分
	- HSL类：色相、饱和度、明度