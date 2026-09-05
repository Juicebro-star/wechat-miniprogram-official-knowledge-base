---
title: "Tap 事件"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/view/tap.html"
source_route: "/miniprogram/dev/framework/view/tap.html"
raw_html: "raw-html/view/tap.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/view/tap.html](https://developers.weixin.qq.com/miniprogram/dev/framework/view/tap.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/view/tap.html`

# Tap 事件

用户的鼠标点击、在触屏设备上手指触摸是最常见的界面交互方式。在这些情况下，小程序基础库会提供一系列事件来描述用户的触摸与点击状态。

## 触摸与点击事件列表

所有触摸与点击事件都是冒泡事件。具体来说，它们在冒泡时都可以穿越组件边界，而且具有捕获阶段（相关概念请参考 [事件系统](../component-framework/event-system) 章节）。

### Touch 类事件

Touch 类事件描述单个手指在屏幕上的触摸状态。

| 类型 | 触发条件 | 最低版本 |
| --- | --- | --- |
| touchstart | 手指触摸动作开始 |  |
| touchmove | 手指触摸后移动 |  |
| touchcancel | 手指触摸动作被打断，如来电提醒，弹窗 |  |
| touchend | 手指触摸动作结束 |  |
| touchforcechange | 在支持 3D Touch 的设备上，重按时会触发 | [1.9.90](../compatibility) |

Touch 类事件在 event 对象上有 `touches` 和 `changedTouches` 两个数组字段，分别表示当前处于触摸状态的手指触摸信息、发生变化了的手指触摸信息。

每个触摸信息对象，主要包含有以下几个字段。

| 字段名 | 含义 |
| --- | --- |
| identifier | 手指标识符，用来表示它对应于哪个手指 |
| clientX | 触摸位置的水平坐标（相对于视口） |
| clientY | 触摸位置的竖直坐标（相对于视口） |
| pageX | 触摸位置的水平坐标（相对于页面，计入页面滚动位置） |
| pageY | 触摸位置的竖直坐标（相对于页面，计入页面滚动位置） |

### Tap 类事件

Tap 类事件用来表达一次“点击“。它可以是触摸引发的，也可以是鼠标点击引发的。

| 类型 | 触发条件 | 最低版本 |
| --- | --- | --- |
| tap | 手指触摸后马上离开、鼠标单击 |  |
| longpress | 手指触摸（或鼠标长按）超过 350ms 再离开，如果指定了事件回调函数并触发了这个事件，tap 事件将不被触发 | [1.5.0](../compatibility) |
| longtap | 手指触摸（或鼠标长按）超过 350ms 再离开（推荐使用longpress事件代替） |  |

Tap 类事件的 detail 包含以下几个字段。

| 字段名 | 含义 |
| --- | --- |
| x | 水平坐标（相对于视口） |
| y | 竖直坐标（相对于视口） |

### Pointer 事件

在 PC 上，pointer 事件也会被触发，可以使用 pointer 事件来实现划入、右键等定制能力（类似于 [web 标准](https://developer.mozilla.org/zh-CN/docs/Web/API/Pointer_events) 的 pointer 事件）。

示例：

```
<view
  bind:pointerdown="pointerdown"
  bind:pointerup="pointerup"
  bind:pointermove="pointermove"
>
  测试代码
</view>
```

右键响应的示例：[在开发者工具中预览效果](https://developers.weixin.qq.com/s/l1pW2qmy7RZJ "在开发者工具中预览效果")

## 点击态

为防止敏感接口被滥用，部分 API 触发时会检查并消耗点击态（如 `wx.openSetting`、`wx.requestSubscribeMessage`），当点击态失效时，则对应功能会受到影响（调用失败或降级）。

每当用户点击时，相应 tap 事件的事件响应函数会获得点击态。除此之外，以下 API 的回调内也具有点击态：

- `wx.requestPayment` 与 `wx.requestOrderPayment`，但这两个 API 获得的点击态不可用于触发 `wx.openEmbeddedMiniProgram`，详情可见[支付后打开半屏小程序能力的相关调整通知](https://developers.weixin.qq.com/community/develop/doc/000644871006f83372416ff2c66801)；
- wx.showModal 与 wx.showActionSheet，点击态仅在 success 回调或接口调用成功后的 complete 中获取。

点击态只在当前事件响应函数所在的宏任务内有效。因此，setTimeout/setInterval 所包裹的函数不具备点击态，但 Promise/async/await 内有效。

在 iOS 15 及以下，由于 iOS 的 Promise 实现存在问题，小程序使用 polyfill 的 Promise。故 iOS 下使用 Promise/async/await 会失去点击态：

```
// <button bind:tap="handleTap">tap</button>
handleTap: async () => {
  // 点击态有效
  setTimeout(() => {
    // 失去点击态
  }, 100);
  Promise.resolve().then(() => {
    // Android 上具备点击态，iOS 上失去点击态
  });
};
```

在 tap 事件处理函数中进行异步请求是一个常见的行为。当 `wx.request`、`wx.downloadFile` 或 `wx.getSetting` 被调用时具有点击态，则会延续该点击态至 success/complete/fail 回调函数：

```
// <button bind:tap="handleTap">tap</button>
handleTap: async () => {
  // 点击态有效
  wx.request({
    url: "https://www.thissitedoesnotexist.com/step1",
    complete: () => {
      // 点击态有效
      wx.request({
        url: "https://www.thissitedoesnotexist.com/step2",
        complete: () => {
          // 点击态有效
        },
      });
    },
  });
};
```

单次 tap 生成的点击态被消耗之后，无法再延续点击态：

```
// <button bind:tap="handleTap">tap</button>
handleTap: async () => {
  // 点击态有效
  wx.openSettings({}); // 成功调用
  wx.request({
    url: "https://www.thissitedoesnotexist.com",
    complete: () => {
      // 失去点击态，因为点击态被 wx.openSettings 消耗掉了
    },
  });
  wx.openSettings({}); // 成功调用
};
```

