- [svg 标签 & 属性](#svg-标签--属性)
  - [1、标签：svg 根](#1标签svg-根)
  - [2、渲染](#2渲染)
    - [2.1、属性：style](#21属性style)
    - [2.2、属性：transform: 变换](#22属性transform-变换)
    - [2.3、标签：渐变](#23标签渐变)
    - [2.4、标签：filter 滤镜](#24标签filter-滤镜)
  - [3、形状](#3形状)
    - [3.1、基本形状](#31基本形状)
      - [3.1.1、标签：rect 矩形 / 圆角矩形](#311标签rect-矩形--圆角矩形)
      - [3.1.2、标签：circle 圆](#312标签circle-圆)
      - [3.1.3、标签：ellipse 椭圆](#313标签ellipse-椭圆)
      - [3.1.4、标签：line 线段](#314标签line-线段)
      - [3.1.5、标签：polyline 折线](#315标签polyline-折线)
      - [3.1.6、标签：polygon 多边形](#316标签polygon-多边形)
    - [3.2、标签：path 路径](#32标签path-路径)
      - [3.2.1、`M`: MoveTo](#321m-moveto)
      - [3.2.2、`L`：LineTo](#322llineto)
      - [3.2.3、`H`：水平-LineTo](#323h水平-lineto)
      - [3.2.4、`V`：垂直-LineTo](#324v垂直-lineto)
      - [3.2.5、`Q`：2次贝塞尔](#325q2次贝塞尔)
      - [3.2.5、`T`：2次光滑贝塞尔](#325t2次光滑贝塞尔)
      - [3.2.6、`C`：3次 贝塞尔](#326c3次-贝塞尔)
      - [3.2.7、`S`：3次 光滑 贝塞尔](#327s3次-光滑-贝塞尔)
      - [3.2.8、`A`：圆弧](#328a圆弧)
      - [3.2.9、`Z`：关闭路径 ClosePath](#329z关闭路径-closepath)
    - [3.3、标签：image 图片](#33标签image-图片)
    - [3.4、标签：text 文字](#34标签text-文字)
  - [4、模式](#4模式)
    - [4.1、标签：g 分组](#41标签g-分组)
    - [4.2、标签：defs 定义](#42标签defs-定义)
    - [4.3、标签：use 使用](#43标签use-使用)
    - [4.4、标签：clipPath 裁剪路径](#44标签clippath-裁剪路径)
    - [4.5、标签：mask 遮罩](#45标签mask-遮罩)
    - [4.6、标签：pattern 平铺模式](#46标签pattern-平铺模式)
  
# [svg 标签 & 属性](https://segmentfault.com/a/1190000012071386)

## 1、标签：[svg 根](https://www.cainiaojc.com/svg/svg-element.html)

覆盖原则：

+ svg 不支持 z-index；
+ 后面元素 会覆盖 前面元素

## 2、渲染

### 2.1、属性：[style](https://www.cainiaojc.com/svg/svg-svg-and-css.html)

一些常用的：

+ fill: 填充色;
+ fill-opacity: 填充 透明度;
+ stroke: 边框色;
+ stroke-opacity: 边框 透明度;
+ stroke-width: 边框 宽度;

### 2.2、属性：[transform: 变换](https://www.cainiaojc.com/svg/svg-transformation.html)

### 2.3、标签：[渐变](https://www.cainiaojc.com/svg/svg-gradients.html)

+ 线性 `linearGradient`
+ 径向 `radialGradient`

### 2.4、标签：[filter 滤镜](https://www.cainiaojc.com/svg/svg-filters.html)

+ filter 标签 放到 defs 定义
	- 定义 id属性，以便引用
+ 引用：style属性，"filter:url(#滤镜id);"

例子：

+ 混合 feBlend
+ 高斯模糊 feGaussianBlur

``` svg
<defs>
	<filter id="blur">
		<feGaussianBlur in="SourceGraphic" stdDeviation="5" />
	</filter>
</defs>
```

## 3、形状

### 3.1、基本形状

#### 3.1.1、标签：[rect 矩形 / 圆角矩形](https://www.cainiaojc.com/svg/svg-rect-element.html)

+ x, y
+ rx, ry
+ width, height

#### 3.1.2、标签：[circle 圆](https://www.cainiaojc.com/svg/svg-circle-element.html)

+ r
+ cx, cy

#### 3.1.3、标签：[ellipse 椭圆](https://www.cainiaojc.com/svg/svg-ellipse-element.html)

+ cx, cy
+ rx, ry

#### 3.1.4、标签：[line 线段](https://www.cainiaojc.com/svg/svg-line-element.html)

+ x1, x2
+ y1, y2

#### 3.1.5、标签：[polyline 折线](https://www.cainiaojc.com/svg/svg-polyline-element.html)

+ points "0 0, 1 1, 2 2"

#### 3.1.6、标签：[polygon 多边形](https://www.cainiaojc.com/svg/svg-polygon-element.html)

+ points "0 0, 1 1, 2 2"

### 3.2、标签：[path 路径](https://www.cainiaojc.com/svg/svg-path-element.html)

d 属性 解析，起点和终点会连接 起来，形成闭合图形

+ 下面字母，大写表示 绝对位置，小写表示 相对位置
+ 例子：M 3,5 移到 (3, 5) 处
+ 例子：m 3,5 右移 3, 下移 5

#### 3.2.1、`M`: MoveTo

"M x,y"

#### 3.2.2、`L`：LineTo

"L x,y"

#### 3.2.3、`H`：水平-LineTo

"H x"

#### 3.2.4、`V`：垂直-LineTo

"V y"

#### 3.2.5、`Q`：2次贝塞尔

"Q cx, cy x, y"

#### 3.2.5、`T`：2次光滑贝塞尔

"T x, y"

跟在 Q，T 后面 补充 后面的 1个点

#### 3.2.6、`C`：3次 贝塞尔

"C c1x,c1y c2x,c2y x,y"

#### 3.2.7、`S`：3次 光滑 贝塞尔

"S c2x, c2y x, y"

跟在 C，S 后面 补充 后面的 两个点

#### 3.2.8、`A`：圆弧 

"A rx ry x-deg large-arc sweep-flag x y"

+ rx ry 半径
+ x-deg x轴旋转角度
+ large-arc：0表示大于180度，1表示小于180度
+ sweep-flag：0为沿逆时针，1为沿顺时针
+ x y：最终坐标

#### 3.2.9、`Z`：关闭路径 ClosePath

### 3.3、标签：[image 图片](https://www.cainiaojc.com/svg/svg-image.html)

``` svg
  <image x="90" y="-65" width="128" height="146" transform="rotate(45)"
     xlink:href="https://developer.mozilla.org/static/img/favicon144.png"/>
```

### 3.4、标签：[text 文字](https://www.cainiaojc.com/svg/svg-text-element.html)

``` svg
<text x="40" y="23" >Text: </text>
```

## 4、模式

定义 可 重用 的东西，一般放到 defs 标签内

### 4.1、标签：[g 分组](https://www.cainiaojc.com/svg/svg-g-element.html)

+ 可以不放到 defs
+ 每组的图形 默认 共享一个 style，本身的 style 优先级 更高

``` svg
<g style="stroke: #0000ff; stroke-width: 4px; fill: #ff0000">
	<rect x="10" y="10" width="100" height="50" />
	<circle cx="150" cy="35" r="25" />
	<circle cx="250" cy="35" r="25" style="stroke: #009900; fill: #00ff00;"/>
</g>
```

### 4.2、标签：[defs 定义](https://www.cainiaojc.com/svg/svg-defs-element.html)

``` svg
<defs>
</defs>
```

### 4.3、标签：[use 使用](https://www.cainiaojc.com/svg/svg-use-element.html)

``` svg

<defs>
	<g id="shape">
		<rect x="0" y="0" width="50" height="50" ></rect>
		<circle cx="0" cy="0" r="50" ></circle>
	</g>
</defs>

<use xlink:href="#shape" x="50" y="50" ></use>
<use xlink:href="#shape" x="200" y="50" ></use>
```

### 4.4、标签：[clipPath 裁剪路径](https://www.cainiaojc.com/svg/svg-clippath.html)

``` svg
<defs>
	<clipPath id="clip">
		<rect x="15" y="15" width="40" height="40" />
	</clipPath>
  </defs>
```

### 4.5、标签：[mask 遮罩](https://www.cainiaojc.com/svg/svg-masks.html)

clipPath 的 高级版本

``` svg
  <mask id="mask1" x="0" y="0" width="100" height="100" >
    <rect x="0" y="0" width="100" height="100"
        style="stroke:none; fill: #ffffff"/>
  </mask>
```

### 4.6、标签：[pattern 平铺模式](https://www.cainiaojc.com/svg/svg-pattern.html)

``` svg
<defs>
	<pattern id="Triangle" width="10" height="10" patternUnits="userSpaceOnUse">
		<polygon points="5,0 10,10 0,10"/>
	</pattern>
</defs>
```