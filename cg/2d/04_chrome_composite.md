- [渲染 && 调度](#渲染--调度)
	- [1. 提交](#1-提交)
	- [2. CrRendererMain](#2-crrenderermain)
		- [2.1. 和 Move事件](#21-和-move事件)
		- [2.2. 和WebGL相关](#22-和webgl相关)
	- [3. CrRendererMain](#3-crrenderermain)
	- [4. commit 会引起 PendingTree的wait](#4-commit-会引起-pendingtree的wait)
	- [5. Compositor](#5-compositor)
	- [6. GPU 进程](#6-gpu-进程)
		- [6.1. CrGpuMain](#61-crgpumain)
		- [6.2. GpuVSyncThread](#62-gpuvsyncthread)
		- [6.3. Graphics Pipeline.DrawAndSwap](#63-graphics-pipelinedrawandswap)
		- [6.4. Scheduler::Running (VizCompositorThread)](#64-schedulerrunning-vizcompositorthread)

# 渲染 && 调度

## 1. 提交

## 2. CrRendererMain

### 2.1. 和 Move事件

+ ThreadControllerImpl::RunTask
    - SequenceManager RunTask
        * WidgetBaseInputHandler::OnHandleInputEvent
            + LatencyInfo.Flow
                - EventHandler::handleMouseMoveEvent
                    * HitTest
                    * SelectionController::handleMouseDraggedEvent

### 2.2. 和WebGL相关

+ CommandBufferHelper::WaitForAvailableEntries1
	- CommandBufferProxyImpl::WaitForGetOffset


+ CommandBufferHelper::Flush
    - CommandBufferProxyImpl::Flush


+ ThreadControllerImpl::RunTask
	- TimerBase::Run
		* AsyncTask Run


+ RunHighestPriorityTask
	- compositor_tq
	- ThreadControllerImpl::RunTask
	- ThreadProxy::BeginMainFrame
		* LayerTreeHost::ApplyCompositorChanges
		* WidgetBase::WillBeginMainFrame
		* WebFrameWidgetImpl::BeginMainFrame
			+ PageAnimator::serviceScriptedAnimations
				- AnimationTimeline::serviceAnimations
				- FrameRequestCallbackCollection::ExecuteFrameCallbacks
				- AsyncTask Run
                	* AsyncTask 生成
			+ AnimationHost::TickAnimations
        	+ WebFrameWidgetImpl::UpdateLifecycle
				- LocalFrameView::RunCompositingInputsLifecyclePhase
				- PaintLayerCompositor::UpdateInputsIfNeededRecursive
				- CompositingInputsUpdater::update
			+ LayerTreeHost::DoUpdateLayers
				- DrawingBuffer::prepareMailbox
				- CommandBufferProxyImpl::OrderingBarrier
			+ ProxyMain::BeginMainFrame::commit
			+ LocalFrameView::UpdateViewportIntersectionsForSubtree


+ RunHighestPriorityTask
	- compositor_tq
		* ThreadControllerImpl::RunTask
        	+ WidgetBase::DidCommitAndDrawCompositorFrame


+ RealTimeDomain::DelayTillNextTask

+ SequenceManager::DoIdleWork

## 3. CrRendererMain

+ ThreadProxy::BeginMainFrame
	- LayerTreeHost::ApplyCompositorChanges
	- WidgetBase::UpdateSelectionBounds
	- WidgetBase::UpdateTextInputStateInternal
	- ProxyMain::SetNeedsAnimate
	- AnimationHost::TickAnimations
	- ScrollTree::SetScrollOffset
	- LayerTreeHost::DoUpdateLayers
	- ProxyMain::BeginMainFrame::commit

## 4. commit 会引起 PendingTree的wait

+ PendingTree::waiting
	- PipelineReporter
	- ScheduledTasks

## 5. Compositor

+ Scheduler::BeginFrame
	- Scheduler::BeginImplFrame
		* AnimationHost::TickAnimations
		* ThreadProxy::ScheduledActionSendBeginMainFrame
		* Scheduler::ScheduleBeginImplFrameDeadline


+ Scheduler::OnBeginImplFrameDeadline
	- ProxyImpl::ScheduledActionDraw
		* LayerTreeHostImpl::PrepareToDraw
			+ DrawLayers.FrameViewerTracing
			+ LayerTreeHostImpl::BuildHitTestData
	- ProxyImpl::ScheduledActionPrepareTiles


+ ProxyImpl::NotifyReadyToCommitOnImpl
	- Scheduler::NotifyBeginMainFrameStarted
	- Scheduler::NotifyReadyToCommit
		* ProxyImpl::ScheduledActionCommit
			+ LayerTreeHostImpl::BeginCommit
				- ProxyImpl::OnCanDrawStateChanged
			+ LayerTreeHost::FinishCommitOnImplThread
				- LayerTreeHost::PushProperties
			+ LayerTreeHostImpl::CommitComplete
				- AnimationHost::TickAnimations
				- LayerTreeImpl::UpdateDrawProperties
				- ImageAnimationController::AnimateImagesForSyncTree
				- LayerTreeImpl::InvalidateRegionForImages
				- ProxyImpl::NotifyReadyToActivate
		* ProxyImpl::ScheduledActionActivateSyncTree
			+ SchedulerStateMachine::SetNeedsPrepareTiles


+ TileManager::DidFinishRunningTileTasksRequiredForActivation

+ TileManager::DidFinishRunningTileTasksRequiredForDraw

+ TileManager::DidFinishRunningAllTileTasks

+ ProxyImpl::DidReceiveCompositorFrameAckOnImplThread


+ TileManager::FlushAndIssueSignals
	- TileTaskManagerImpl::CheckForCompletedTasks
	- TileManager::CheckPendingGpuWorkAndIssueSignals
		* ProxyImpl::NotifyReadyToActivate
		* TileManager::IsReadyToDraw
		* ProxyImpl::NotifyReadyToDraw

## 6. GPU 进程

### 6.1. CrGpuMain

+ ThreadControllerImpl::RunTask
    - SequenceManager RunTask
        * Scheduler::RunNextTask
            + CommandBufferStub::OnAsyncFlush
                - CommandBufferService:PutChanged

+ ThreadControllerImpl::RunTask
    - SequenceManager RunTask
        * Scheduler::RunNextTask
            + GLContextEGL::MakeCurrent
            + SkiaOutputSurfaceImplOnGpu::FinishPaintCurrentFrame
                - DirectCompositionChildSurfaceWin::GetBuffer
                - SwapChain11::reset
                    * SwapChain11::resetOffscreenTexture
                - GLContextEGL::MakeCurrent
                - SkiaOutputSurfaceImplOnGpu::BeginAccessImages
            + SkiaOutputSurfaceImplOnGpu::SwapBuffers
            	- DirectCompositionSurfaceWin::SwapBuffers
                    * DirectCompositionChildSurfaceWin::SwapBuffers
                        - GLContextEGL::MakeCurrent
                        - DirectCompositionChildSurfaceWin::PresentSwapChain
                    * DCLayerTree::CommitAndClearPendingOverlays

### 6.2. GpuVSyncThread

+ ThreadControllerImpl::RunTask
    - SequenceManager RunTask

### 6.3. Graphics Pipeline.DrawAndSwap

+ Graphics.Pipeline.DrawAndSwap
    - WaitForSwap
    - WaitForPresentation

### 6.4. Scheduler::Running (VizCompositorThread)

+ ThreadControllerImpl::RunTask
    - SequenceManager RunTask
        * ExternalBeginFrameSource::OnBeginFrame
            + DisplayScheduler::BeginFrame
                - DisplayDamageTracker::HasPendingSurfaces
                    * DisplayScheduler::ScheduleBeginFrameDeadline
                        + Using new deadline
                - ShouldNotSendBeginFrame
        * DisplayDamageTracker::SurfaceDamageExpected
        * Graphics.Pipeline