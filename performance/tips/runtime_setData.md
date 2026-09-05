---
title: "合理使用 setData"
type: source_document
layer: L4
owner: external
claim_type: fact
status: archived
domain: mini_program
project: wechat_miniprogram_docs
source_type: official_web
visibility: public
retrieved: "2026-09-05"
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/performance/tips/runtime_setData.html"
source_route: "/miniprogram/dev/framework/performance/tips/runtime_setData.html"
raw_html: "raw-html/performance/tips/runtime_setData.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/performance/tips/runtime_setData.html](https://developers.weixin.qq.com/miniprogram/dev/framework/performance/tips/runtime_setData.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/performance/tips/runtime_setData.html`

# 合理使用 setData

数据更新方法（如 `setData`）是小程序开发中是比较容易引发性能问题的接口。

关于数据更新方法的一些细节，可以参考 [高级数据更新方法](../../component-framework/data-updates) 。

在 [数据更新策略](../../component-framework/data-update-strategy) 文档中，描述了数据更新采用的算法原理，供参考。

## 影响数据更新性能的因素

影响数据更新方法性能的因素主要有：

1. 调用数据更新方法的组件 Shadow 树总节点量；
2. 更新的数据量。

其中，前者的影响比后者更大。

## 使用建议

### 1. 合理划分组件，避免 Shadow 树过大

如 [数据更新算法](../../component-framework/data-update-strategy#数据更新算法) 中所述，对于主要采用的数据更新算法，其算法性能与 Shadow 树中的节点总量、节点复杂程度显著正相关。因此，最有效的策略是合理划分组件、控制 Shadow 树大小。

- ✅ 当页面或组件 Shadow 树中的节点数量过多时，适当进行组件拆分，减少当前 Shadow 树中节点数量；

### 2. 控制数据更新的频率

每次 setData 都会使组件框架触发一次数据更新算法。过于频繁（毫秒级）的调用 `setData`，会导致以下后果：

- 逻辑层 JS 线程持续繁忙，无法正常响应用户操作的事件，也无法正常完成页面切换；
- 视图层 JS 线程持续处于忙碌状态，逻辑层 -> 视图层通信耗时上升，视图层收到消息的延时较高，渲染出现明显延迟；
- 视图层无法及时响应用户操作，用户滑动页面时感到明显卡顿，操作反馈延迟，用户操作事件无法及时传递到逻辑层，逻辑层亦无法及时将操作处理结果及时传递到视图层。

因此，开发者在调用 setData 等数据更新方法时要注意：

- ✅ 仅在需要进行页面内容更新时调用数据更新方法；
- ✅ 通过 [组合更新](../../component-framework/data-updates#组合更新) 等方式，对连续的 setData 调用尽可能的进行合并；
- ❌ 避免不必要的数据更新调用；
- ❌ 避免以过高的频率持续数据更新调用；
- ❌ 避免在页面滚动回调中每次都调用数据更新方法。

### 3. 对于无法避免的频繁更新，降低每次更新开销

如果无法避免频繁数据更新（例如：秒杀倒计时），用以下方式降低更新开销：

- ✅ 封装为独立的组件，将 Shadow 树尽可能变小，在组件内进行数据更新；
- ✅ 尽量满足 [数据更新算法](../../component-framework/data-update-strategy#数据更新算法) 中绑定映射表更新的条件，降低更新开销；
- ✅ 使用 [CSS contain 属性](https://developer.mozilla.org/en-US/docs/Web/CSS/contain) 限制计算布局、样式和绘制等的范围；
- ❌ 避免大组件内高频的数据更新调用。

![](https://res.wx.qq.com/wxdoc/dist/assets/img/example1.045b130c.jpg)

### 4. data 应只包括渲染相关的数据

在使用 exparser 组件框架时，data 应只包含界面上用到的数据。用数据更新方法更新渲染无关的字段，会触发额外的渲染流程、增加逻辑层 -> 视图层的通信，影响渲染耗时。

- ✅ 页面或组件的 data 字段，应用来存放 WXML 中用到的数据字段；
- ✅ 页面或组件渲染间接相关的数据可以设置为 [纯数据字段](../../pure-data)，可以使用 observers 监听变化；
- ✅ 页面或组件渲染无关的数据，应挂在非 data 的字段下，如 `this.userData = {userId: 'xxx'}` 或作为 Chaining 形式 `init` 内的局部变量；
- ❌ 避免在 data 中包含 WXML 中用不到的数据字段；
- ❌ 避免使用 data 在页面或组件方法间进行数据共享；
- ❌ 避免滥用 [纯数据字段](../../pure-data) 来保存数据。

### 5. setData 应只传发生变化的数据

更新的数据量会影响数据拷贝和数据通讯的耗时，增加页面更新的开销，造成页面更新延迟。

- ✅ setData 应只传入发生变化的字段；
- ✅ 使用其他 [高级数据更新方法](../../component-framework/data-updates) 来代替 setData；
- ❌ 不要在 setData 中偷懒一次性传所有 data：`this.setData(this.data)`。

### 6. 控制后台态页面的数据更新

由于小程序逻辑层是单线程运行的，后台态页面进行数据更新也会抢占前台页面的运行资源，且后台态页面的的渲染用户是无法感知的，会产生浪费。在某些平台上，小程序渲染层各 WebView 也是共享同一个线程，后台页面的渲染和逻辑执行也会导致前台页面的卡顿。

- ✅ 页面切后台后的更新操作，应尽量避免，或延迟到页面展示后进行；
- ❌ 避免在切后台后仍进行高频的数据更新，例如倒计时更新。

## 性能分析

开发者可以通过组件的 [setUpdatePerformanceListener](../../update-perf-stat) 接口获取更新性能统计信息，来分析产生性能瓶颈的组件。

