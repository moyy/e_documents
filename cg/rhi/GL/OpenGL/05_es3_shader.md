# [GLES 3.0 着色器](https://www.khronos.org/files/webgl20-reference-guide.pdf)

相对于 GLES 2.0，添加了如下东西

## 版本

Shader 第一行 声明：#version 300 es

## 内置变量

FS：**gl_FragDepth** 访问 片段 深度

## 修饰符

+ 通用
	- const/uniform/
+ GLES 2
	attribute/varying
+ GLES 3
	- in / centroid in
	- out / centroid out
	- 针对 centroid 是否插值：**flat**、**smooth**

#### layout & uniform block

## 数据 类型

+ unit, uvec(2/3/4)
+ mat(2/3/4)x(2/3/4)
+ sampler2D[Array][Shadow]
+ samplerCube[Shadow]
+ sampler3D
+ (i/u)Sampler(2D/3D/Cube/2DArray)

## 运算符

+ 位运算：&, |, ^, >>, <<
+ 取余：%
+ 取反：~

## 数学函数

+ 四舍五入：round(x) == floor(x + 0.5)
+ isnan / isinf
+ 浮点 <--> 整数
+ pack / unpack：作用 2个float <--> 整数

## matrix 函数

+ 转置 transpose
+ 行列式 determinant
+ 求逆 inverse
+ outerProduct
	- n维列向量 和 m维行向量 相乘 = n*m rank 为 1的 矩阵

## FS 导数

+ dFdx, dFdy
+ fwidth = abs(dFdx) + abs(dFdy)

## 纹理 采样 （GLES 2 + GLES 3）

+ textureSize 取 纹理 大小
+ **texture[Proj][Lod][Offset]** (没有：textureProjOffset）
+ **texture[Proj]Grad[Offset]**
+ **texelFetch[Offset]**：取 Buffer-Texture 的 值，参数为 像素 索引

### 说明：Proj 采样

类型为：sampler2DShadow

textureProj 将 4维 齐次坐标 --> 笛卡尔坐标

textureProj 函数的采样器类型为，函数的调用结果为一个比较值。ShadowCoord的前两个分量被用来访问纹理中的深度值，第三个分量与这个深度值进行比较，在GL_NEAREST插值模式下，比较的结果为1.0或0.0。因为我们设置的比较函数为GL_LESS，如果第三个分量小于深度值，返回1.0，否则，返回0.0。

### 说明：Lod 采样

采样的时候，直接采样 对应的 level

### 说明：Grad 采样