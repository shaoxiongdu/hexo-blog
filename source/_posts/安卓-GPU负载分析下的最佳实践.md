---
title: GPU负载分析下的最佳实践cover:  https://api.ixiaowai.cn/gqapi/gqapi.php
tags:

- 安卓
- GPU负载
- 性能优化
  categories:
- 技术
  date:  2022-08-07 15:04:04

---

- 子控件布局设计中尽量减少不必要的背景设置如父控件布局已有背景，子控件除非UI设计要求，无需单独设置背景色，继承父控件的即可。

- 移除被遮挡的背景如子控件会完全遮挡父控件，父控件子控件同时设置了背景，会导致一次过度绘制。因为父控件在当前布局下是不可见的，所以可以直接移除背景。

- 减少布局嵌套，使视图层次扁平化，降低view重叠数量，降低overdraw可能性

- Canvas.clipRect减少重叠区域过度绘制

- 使用ClipOutlineProvider绘制圆角减少过度绘制

- 慎用半透明效果

- 应用程序可以通过setContextPriority()设置当前渲染上下文的优先级，优先级高先执行，避免其因抢不到GPU时间片而卡顿。

- 避免使用GPU合成
  - 通过 SurfaceControl.setCornerRadius 设置窗口圆角
  - Surface的PixelFormat使用RGBA_1010102
  - 通过SurfaceControl. setShadowRadius设置阴影半径
  - Cursor鼠标箭头类型的layer

- 瞬时任务较重的场景使用perflock进行CPU/GPU boost，如冷启、滑动等

- 多屏场景下，动态调整屏幕合成优先级 如主屏launcher、副驾feed同时动会出现feed流卡顿问题，考虑将用户体验/帧率要求比较高的图层合成提前。

- 降低渲染目标的复杂度，如顶点数量，光照、动态阴影、蒙层等效果，最终目的还是避免overdraw，减少GPU的计算复杂度

- 进行必要的纹理压缩，减少内存带宽压力

- 如有可能，尽量减少渲染频率，即帧率
