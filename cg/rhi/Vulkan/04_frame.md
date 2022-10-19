- [渲染一帧：从 提交 到呈现](#渲染一帧从-提交-到呈现)
  - [1. Fence 围栏，用于：CPU-GPU同步；](#1-fence-围栏用于cpu-gpu同步)
    - [1.1. 场景](#11-场景)
    - [1.2. 状态](#12-状态)
  - [2. Semaphore 信号量，用于 GPU-GPU 同步](#2-semaphore-信号量用于-gpu-gpu-同步)
    - [2.1. 场景](#21-场景)
    - [2.2. 状态](#22-状态)
  - [3. 流程图](#3-流程图)
    - [3.1. 渲染循环](#31-渲染循环)
    - [3.2. 第1部分：等待 cmd_buffer 可用](#32-第1部分等待-cmd_buffer-可用)
    - [3.3. 第2部分：提交 到 渲染队列，发送呈现指令](#33-第2部分提交-到-渲染队列发送呈现指令)
  - [4. 大概代码流程](#4-大概代码流程)
    - [4.1. 数据结构：伪码](#41-数据结构伪码)
    - [4.2. 渲染：伪代码](#42-渲染伪代码)
  - [5. 参考](#5-参考)

# 渲染一帧：从 提交 到呈现

交换链具有 3缓冲区 的模式下，渲染一帧的流程如下：

* 获取可用的 命令缓冲区；
* 向该 命令缓冲区 录制 这一帧的所有命令；
* 提交该缓冲区到图形队列执行渲染
* 渲染完成后，将 该image 提交到交换链后端，这种操作叫：呈现 Present；

这过程 涉及 到CPU-GPU，GPU-GPU 的 同步 问题，需要用：Fence 和 Semaphore 解决；

## 1. Fence 围栏，用于：CPU-GPU同步；

### 1.1. 场景

* 作用：当GPU渲染过慢时，用于堵塞CPU
	+ Fence 只能从 GPU 单向通知 CPU；
* 场景：同一个CommandBuffer 还没有被执行完，是不能够再次被开始执行的；
	+ 注：可以在submit时，判断一个Queue到底忙不忙；方法是：提交一个有效的fence，将info填空

### 1.2. 状态

* 两种状态：唤醒 signaled、未唤醒 unsignaled
* 初态由Vulkan通过API设置，设置为：唤醒态，否则wait-for-fence会堵死；
	+ 而且每次 提交渲染队列时，都需要调用reset将fence设置为 未唤醒态；
* 一般由 GPU驱动 唤醒fence；

## 2. Semaphore 信号量，用于 GPU-GPU 同步

用于 两个不同队列之间，或者同一个队列的两次submit之间的同步；

### 2.1. 场景

* finish_sp: 控制 render 和 present 的 同步；
	+ present的前提是：该Image已经render完毕；
* available_sp: 控制 同一个Image 释放到可用队列 和 当渲染目标 之间 的 同步；
	+ Image当渲染目标的前提是：该Image已经 还到 “可用队列” 中；（见：参考--三缓冲）

### 2.2. 状态

* 两种状态：唤醒 signaled，未唤醒 unsignaled
	+ 状态切换和初始状态设置，完全 由 GPU处理。
* 3缓冲下，每帧 各有一个 { ImageAvaliableSemaphore, RenderFinishSemaphore }
    + ImageAvaliableSemaphore：渲染：要将color 写入 颜色缓冲区 前，要 等 该 颜色缓冲区 之前的内容 present（呈现）到 屏幕（或者离屏目标）结束 之后；
    + RenderFinishSemaphore：present（呈现）到屏幕之前，要 等 该帧的 渲染命令全部执行结束之后；

## 3. 流程图

### 3.1. 渲染循环

![](http://ser.yinengyun.com:10080/tech/img/uploads/b3f73e3e07a43cede3a0d3781db41984/从提交到呈现-第_1_页.png)

### 3.2. 第1部分：等待 cmd_buffer 可用

![](http://ser.yinengyun.com:10080/tech/img/uploads/3c1c683aa82b862b1d477370213a24c8/从提交到呈现-第_2_页.png)


### 3.3. 第2部分：提交 到 渲染队列，发送呈现指令

![](http://ser.yinengyun.com:10080/tech/img/uploads/c26b748e871af88c95adaa87f1724ecd/从提交到呈现-第_3_页.png)

## 4. 大概代码流程

### 4.1. 数据结构：伪码

``` rs

// 交换链，3缓冲

// SwapChain的 ImageCount = min{3, 最小支持数 + 1, 最大支持数};
// Image数 = ImageView数 = FrameBuffer数 = CommandBuffer数

// fence 保证 CPU层面 cmdBuffer 可用；
// avalible_sp 保证 GPU image上次交换完毕，可用于这一次写输出；
// finish_sp 保证 GPU present时 必须 渲染完毕；

struct Renderer {
    fences: [Fence; ImageCount],

    available_sp: [Semaphore; ImageCount],
    finish_sp: [Semaphore; ImageCount],

    cmd_buffers: CmdBuffer[ImageCount],

    // 其他，比如swapchain，device 等；
};

```

### 4.2. 渲染：伪代码

``` rs

fn draw_frame(&self) {

    let frame = self.current_frame;
    self.current_frame = (self.current_frame + 1) % 3;

    let fence = self.fence[frame];

    // 等待对应的fence被唤醒；说明cmd-buffer可用；
    self.device.wait_for_fence(fence, 超时无穷);

    // 拿cmdbuffer取录制指令
    let cmd_buffer = self.cmd_buffers[frame];
    draw_to_buffer(cmd_buffer);

    let available_sp = self.available_sp[frame];
    let finish_sp = self.finish_sp[frame];

    // 等待 image可用，唤醒 available_sp
    // 因为fence为null，所以设置超时时间无效；
    let image_index = self.swapchain.acquire_next_image(available_sp, fence: null);

    // 将 fence 重置为 未唤醒状态；然后提交到graphic队列中
    self.device.reset_fence(fence);

    self.device.queue_submit(
        self.graphics_queue, 

        fence, // 当全部执行完毕后，唤醒 fence，这样该cmd_buffer才能用于下一个指令录制；

        submit_info: {
            cmd_buffer,

            // 下面两个设置的意思是说，当执行到写颜色的阶段
            //  发现available_sp还没有唤醒，GPU 需要等；
            wait_mask: COLOR_ATTACHMENT_OUTPUT,
            wait_sp: available_sp,

            // 全部完成命令后，唤醒 finish_sp，这样下面的 呈现操作 才能进行；
            sinal_sp: finish_sp,
        }
    );

    // 等待 finish_sp 被唤醒后，将image呈现到显示屏
    self.swapchain.present(self.present_queue, wait: finish_sp, image_index);

```

## 5. 参考

* [背景知识：三缓冲 -- 呈现模式，可用图像](./pi_app-1cclfvmmkptdo)
* [Vulkan：编程模型 -- 多线程的渲染的套路](./pi_app-1cc58gk1285rb)
* [Vulkan 同步和缓存控制之: Fence 和 Semaphore](https://zhuanlan.zhihu.com/p/24817959)
* [Vulkan 同步和缓存控制之：Barrier和 Event](https://zhuanlan.zhihu.com/p/80692115)