# GPU 三种模式

## IMR：立即渲染 模式

* PC 上 GPU 架构；
* 每个DrawCall都会立即触发驱动开始工作，完成这个DrawCall后，GPU才会执行下一个DrawCall

![](../img/m_f7d49d26c166bc8d1bb6513e67deebc2_r.png)

#### 优点

* 简单
* 无须像TBR，需要片上缓存，保存中间结果
* 无须像TBR，切换FBO时，不需要清空渲染指令，节省性能；
* 无须像TBR，缓存TriangleList，含大量VS计算时，节省性能；

#### 缺点：耗 带宽

* 有遮挡关系的两次drawcall，耗fs计算；不过目前所有的GPU都会带类似Early-Z的机制做事先判断；
* 在VS执行时，需要 大量随机读写 FrameBuffer，DepthBuffer；带来大量的内存带宽消耗；

## TBR模式：Tile Based Rendering

![](../img/m_a9cdfe8ea1cb65d69ace8384dc1af880_r.png)

+ `注1：` 移动端的GPU：不管是高通的Adreno，ARM公版架构Mali，还是苹果的PowerVR，GPU架构都是TBR模式
+ `注2：` 上面的PrimitiveList会等到该渲染目标所有的DrawCall完毕后，需要使用或者需要切换RenderTarget的时候才会执行后面的Rater；
+ `注3：` On-Chip 是GPU内部的一个高速缓存，类似于CPU的L2-Cache；

#### 特点

* 计算所有DrawCall的VS，得到的结果，根据拓扑结构组装成一堆三角形，这些三角形缓存在TriangleCache中；
* GPU将Viewport分好块，一般是 32 * 32像素，每个Tile有个triangle字段，用于存在该块的Triangle的索引；
* `TODO`：TriangleCache应该会进一步根据Tile做裁剪；
* 等这个RenderTarget结束，需要切换时，GPU就会遍历所有的tile，对里面的TriangleList进行 光栅化 和 FS；

#### 优点：节省带宽

一个tile的color，depth都很少，可以为GPU做一个告诉缓存，先在那里迭代，然后统一放到主机内存（显存）；

### 缺点

* 如果DrawCall的图元或者顶点很多，那么同样会消耗很多带宽；
* 如果FBO用到很多，每切换一次FBO，都会导致上面的过程重新来一遍；
* 一旦后面的DrawCall用到前面渲染生成的结果，强制GPU把缓存的所有draw命令都执行完毕，然后放弃当前缓存内容；

## TBDR：Tile Based Deferred Rendering

![](../img/m_8d435d2a369e7352f0e42ddf1b721039_r.png)

* PowerVR-GPU架构申请了专利；目前该架构 授权 给 苹果公司
* 通过 HSR（隐藏面消除）在FS之前，减少了不必要的fragment，降低了fs的时间和带宽；
* 比起TBR，缓解了 过度绘制（Overdraw） 的问题

#### 参考GPU架构

![](../img/m_95502fe5ab0a83b49a6e6cbb2e8b578e_r.png)

## 参考

* [移动GPU架构浅析](https://zentia.github.io/2019/09/04/Mobile-GPU-Architecture/)
* [移动设备GPU架构知识汇总](https://zhuanlan.zhihu.com/p/112120206)