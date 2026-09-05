---
title: "页面路由"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route.html"
source_route: "/miniprogram/dev/framework/app-service/route.html"
raw_html: "raw-html/app-service/route.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route.html](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/app-service/route.html`

# 页面路由

在小程序中，所有页面的创建、销毁及状态转换都由页面路由来表达和进行控制。

## 路由的时机

路由会以事件形式表示。路由事件的发起可以大致分为以下几类：

- 通过用户的操作（如按下返回按钮）发起；
- 通过 API（如 `this.router.navigateTo`）发起，如果路由被确定执行，API 的 `success` 回调函数将被触发，否则将触发 `fail`；
- 通过组件（如 `<navigator>`）发起，如果路由被确定执行，组件的 `success` 事件将被触发，否则将触发 `fail`。

当一次路由被确定执行（API 或组件通知 `success`）时，没有操作可以取消这一次路由。

当多次路由被连续发起时，如果当前的路由事件还未处理完毕，后续的路由事件将等待当前路由处理，并排队依次执行，直到所有待处理的路由都被执行完毕。

> 一个简单的例子：用户点击返回按钮触发了 `navigateBack`，小程序在页面栈当前栈顶页的 `onUnload` 中调用 `wx.redirectTo`，**并不能** 将当前正在被销毁的页面重定向为一个新页面，而是会先完成页面返回，再将页面返回后的新栈顶页重定向到新的页面。

当一次路由被确定执行（API 或组件通知 `success`）时，实际的页面创建、跳转、销毁等可能并未马上发生，但也不能被取消。

## 页面栈

目前，小程序的页面会被组织为一个页面栈加若干不在栈中的悬垂页面的组合形式。其中，页面栈按顺序存放了通过跳转依次打开的页面，而当前已经创建但非活跃的 tabBar 页面及处于画中画模式（如 [`video`](https://developers.weixin.qq.com/miniprogram/dev/api/media/video/VideoContext.html#小窗特性说明)、[`live-player`](https://developers.weixin.qq.com/miniprogram/dev/component/live-player.html#小窗特性说明) 等）中的页面将以悬垂页面的形式存在。

全局接口 [`getCurrentPages`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/getCurrentPages) 可以用来获取当前页面栈。

小程序冷启动完成后，在整个小程序存活过程中（除去某次路由执行到一半的中间状态外），页面栈中都将存在至少一个页面。

页面栈的具体行为可以参见下面具体路由行为中的详细描述。

## 页面生命周期函数

每个小程序页面都有若干生命周期函数。它们可以在页面注册时定义，并会在相应的时机触发。它们包括：

- [组件生命周期](../component-framework/lifetimes) ；
- [组件所在页面的生命周期](../component-framework/lifetimes#组件所在页面的生命周期) ；
- [Page 接口](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page#生命周期回调函数) 中的 `onLoad`, `onShow`, `onRouteDone`, `onHide`, `onUnload` 等。

### 页面路由监听

从基础库版本 [3.5.5](../compatibility) 开始，基础库提供了一组针对路由事件的监听函数。相比页面生命周期函数，它们能更好地针对某次路由进行响应。详见 [页面路由监听](route-event-listener)。

部分生命周期函数会按固定顺序成组触发。

### 页面创建生命周期

当新的页面被创建时，以下生命周期函数会被依次触发：

1. 组件 `lifetimes` 中的 `created`（组件树自底向上）
2. 组件 `lifetimes` 中的 `attached`（组件树自顶向下）
3. 组件 `pageLifetimes` 中的 `show`（组件树自顶向下）
4. `Page` 中的 `onLoad`
5. `Page` 中的 `onShow`

注意：`show` `onShow` 这样的生命周期也会在已隐藏页面重新可见时被触发，所以不应该用于页面初始化时的一次性操作，而应该用于类似“刷新页面数据“这样需要在页面重新可见时执行的操作。

### 页面销毁生命周期

当页面被销毁时，以下生命周期函数会被依次触发：

1. 组件 `lifetimes` 中的 `beforeDetach`（组件树自顶向下）
2. 组件 `lifetimes` 中的 `detached`（组件树自底向上）
3. `Page` 中的 `onUnload`

### 页面隐藏生命周期

当正在展示的页面被隐藏时，以下生命周期函数会被依次触发：

1. 组件 `pageLifetimes` 中的 `hide`（组件树自顶向下）
2. `Page` 中的 `onHide`

### 页面展示生命周期

当已隐藏页面重新可见时，以下生命周期函数会被依次触发：

1. 组件 `pageLifetimes` 中的 `show`（组件树自顶向下）
2. `Page` 中的 `onShow`

## 路由类型

下面的内容将详细说明每个路由将如何触发页面的生命周期函数。

小程序目前的路由类型可以大致分为：小程序启动、打开新页面、页面重定向、页面返回、Tab 切换、重加载、关闭小窗页面。

### 小程序启动

- openType: `appLaunch`

小程序启动路由 `appLaunch` 表示一次新的小程序冷启动，并加载第一个页面。`appLaunch` 在小程序生命周期中会且仅会出现一次，且必定是第一个路由事件。

**触发方式**

`appLaunch` 仅能由小程序冷启动被动触发。

**页面栈及生命周期处理**

由于 `appLaunch` 必定是启动时的第一个路由，而路由前没有任何页面存在，此时页面栈必定为空。`appLaunch` 会创建路由事件指定的页面，并将其推入页面栈作为栈中唯一的页面。这个页面的 [页面创建生命周期](#页面创建生命周期) 会被触发。

### 打开新页面

- openType: `navigateTo`

打开新页面路由 `navigateTo` 表示打开一个新的页面，并将其推入页面栈。

**触发方式**

- 调用 API [`Router.navigateTo`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Router)、[`wx.navigateTo`](https://developers.weixin.qq.com/miniprogram/dev/api/navigate/wx.navigateToMiniProgram.html),
- 使用组件 [`<navigator open-type="navigateTo"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/functional-page-navigator.html)
- 用户点击一个视频小窗（如 [`video`](https://developers.weixin.qq.com/miniprogram/dev/api/media/video/VideoContext.html#小窗特性说明)）

`navigateTo` 的目标必须为非 tabBar 页面。

**页面栈及生命周期处理**

`navigateTo` 事件发生时，页面栈当前的栈顶页面将首先被隐藏，它的 [页面隐藏生命周期](#页面隐藏生命周期) 会被触发。

之后框架将创建路由事件指定的页面，并将其推入页面栈作为新的栈顶，它的 [页面创建生命周期](#页面创建生命周期) 会被触发。

作为一种特殊情况，如果 `navigateTo` 事件发生时，页面栈当前的栈顶页面满足小窗模式逻辑，或事件由用户点击视频小窗发起，那么页面栈及生命周期的的处理会有所不同。

### 页面重定向

- openType: `redirectTo`

页面重定向路由 `redirectTo` 表示将页面栈当前的栈顶页面替换为一个新的页面。

**触发方式**

1. 调用 API [`Router.redirectTo`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Router)，[`wx.redirectTo`](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.redirectTo.html)
2. 使用组件 [`<navigator open-type="redirectTo"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/functional-page-navigator.html)

`redirectTo` 的目标必须为非 tabBar 页面。

**页面栈及生命周期处理**

`redirectTo` 事件发生时，页面栈当前的栈顶页面将首先被弹出并销毁，它的 [页面销毁生命周期](#页面销毁生命周期) 会被触发。

之后框架将创建路由事件指定的页面，并将其推入页面栈作为新的栈顶，它的 [页面创建生命周期](#页面创建生命周期) 会被触发。

### 页面返回

- openType: `navigateBack`

页面返回路由 `navigateBack` 表示将页面栈当前的栈顶的若干个页面依次弹出并销毁。

**触发方式**

1. 调用 API [`Router.navigateBack`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Router)，[`wx.navigateBack`](https://developers.weixin.qq.com/miniprogram/dev/api/navigate/wx.navigateBackMiniProgram.html)
2. 使用组件 [`<navigator open-type="navigateBack"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/functional-page-navigator.html)
3. 用户按左上角返回按钮，或触发操作系统返回的动作（如按下系统返回键、屏幕边缘向内滑动等）
4. 用户点击一个视频小窗（如 [`video`](https://developers.weixin.qq.com/miniprogram/dev/api/media/video/VideoContext.html#小窗特性说明)）

如果页面栈中当前只有一个页面，`navigateBack` 调用请求将失败（无论指定的 `delta` 是多少）；

如果页面栈中当前的页面数量少于调用时指定的 `delta` + 1（即调用后页面数量将少于一个），`navigateBack` 将弹出到只剩页面栈当前的页面栈底的页面为止（即至少保留一个页面）。如果页面栈中当前只有一个页面，`navigateBack` 调用请求将失败（无论指定的 `delta` 是多少）。

**页面栈及生命周期处理**

`navigateBack` 事件发生时，页面栈当前的栈顶页面将被弹出并销毁，它的 [页面销毁生命周期](#页面销毁生命周期) 会被触发。

以上操作将重复 `delta` 次（或直至页面栈中只剩下一个页面）。

一种特殊情况是，如果 `navigateBack` 发生时，页面栈当前的栈顶页面满足小窗模式逻辑，或事件由用户点击视频小窗发起，那么页面栈及生命周期的的处理会有所不同。

### Tab 切换

- openType: `switchTab`

Tab 切换路由 `switchTab` 表示切换到指定的 tab 页面。

**触发方式**

1. 调用 API [`Router.switchTab`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Router)，[`wx.switchTab`](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.switchTab.html)
2. 使用组件 [`<navigator open-type="switchTab"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/functional-page-navigator.html)
3. 用户点击 Tab Bar 中的 Tab 按钮

`switchTab` 的目标必须为 tabBar 页面。

**页面栈及生命周期处理**

由于 `navigateTo` 和 `redirectTo` 不能指定 tabBar 页面作为目标，因此当一个 tabBar 页面出现在页面栈中时，它必定为页面栈的第一个页面（即栈底页面）；同时，框架会保证任一 tabBar 页面在小程序中最多同时存在一个页面实例。`switchTab` 的行为主要基于这两点进行。

作为特例，如果 `switchTab` 的目标页面刚好就是栈底页面，此时相当于以 `delta` 等于页面总量减一来执行 `navigateBack`，以与其类似的规则触发生命周期。如果 `switchTab` 的目标页面不是栈底页面，就按照下面的规则来处理。

如果当前页面栈中存在多于一个页面，页面栈当前的栈顶页面将被弹出并销毁，它的 [页面销毁生命周期](#页面销毁生命周期) 会被触发。

以上操作将被重复执行多次，直到页面栈中只剩下一个页面。

之后，将这个页面从页面栈中弹出，并成为悬垂页面，且如果路由开始时页面栈中存在多于一个页面，那么它的 [页面隐藏生命周期](#页面隐藏生命周期) 会被触发。

最后，如果目标 tabBar 页已经被创建过（现在是一个悬垂页面），就将它推入页面栈，它的 [页面展示生命周期](#页面展示生命周期) 会被触发；反之，如果目标 tabBar 未被创建过，就创建它、推入页面栈，它的 [页面创建生命周期](#页面创建生命周期) 会被触发。

### 重加载

- openType: `reLaunch`, `autoReLaunch`

重加载路由 `reLaunch` 或 `autoReLaunch` 表示销毁当前所有的页面，并载入一个新页面。

重加载路由的两种 openType 的区别主要为是由接口调用触发、还是由用户触发，这两种 openType 的路由逻辑基本一致。

**触发方式**

1. （`reLaunch`）调用 API [`Router.reLaunch`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Router)，[`wx.reLaunch`](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.reLaunch.html)
2. （`reLaunch`）使用组件 [`<navigator open-type="reLaunch"/>`](https://developers.weixin.qq.com/miniprogram/dev/component/functional-page-navigator.html)
3. （`autoReLaunch`）小程序处于后台时，用户从 [热启动的 B 类场景](../runtime/operating-mechanism#小程序启动时打开的页面) 重新进入小程序

`reLaunch` 可以指定任意页面作为目标页面，无论它是否是小程序的首页或是否 tabBar 页。

请注意：`reLaunch` 及 `autoReLaunch` 仅代表一种路由，**并不等于小程序重启，小程序会在当前的 AppService 上继续运行**，既不会重新启动 AppService 的 JavaScript 运行环境，也不会重新注入小程序代码，各种 JS 的全局变量或全局状态也不会被重置。

**页面栈及生命周期处理**

`reLaunch` 或 `autoReLaunch` 事件发生时，页面栈中的所有页面将由顶至底依次被弹出并销毁，它们的 [页面销毁生命周期](#页面销毁生命周期) 会被触发。

之后所有悬垂页面将以不确定的顺序逐个被销毁，它们的 [页面销毁生命周期](#页面销毁生命周期) 会被触发。

所有页面都被销毁后，目标页面将被创建，并推入页面栈成为栈中唯一的页面，它的 [页面创建生命周期](#页面创建生命周期) 会被触发。

### 关闭小窗页面

- openType: `dismissPip`

关闭小窗页面路由 `dismissPip` 表示关闭一个正处于小窗模式的页面。

## 附注

`switchTab` 事件的处理逻辑较为复杂，下面的表格用以展示在各种情况下进行 `switchTab` 时生命周期的触发情况，作为辅助说明。在这个表格中，我们假设：

- `tabA`, `tabB` 为 tabBar 页面
- `C` 是一个非 tabBar 页面，并且我们只会从 `tabA` 页面打开它
- `D` 是一个非 tabBar 页面，并且我们只会从 `tabB` 页面打开它

| 当前页面 | `switchTab` 目标页面 | 触发的生命周期（按顺序） |
| --- | --- | --- |
| `tabA` | `tabA` | Nothing happened |
| `tabA` | `tabB` | `tabA` [页面隐藏生命周期](#页面隐藏生命周期)，`tabB` [页面创建生命周期](#页面创建生命周期) |
| `tabA` | `tabB`（再次打开） | `tabA` [页面隐藏生命周期](#页面隐藏生命周期)，`tabB` [页面展示生命周期](#页面展示生命周期) |
| `C` | `tabA` | `C` [页面销毁生命周期](#页面销毁生命周期)，`tabA` [页面展示生命周期](#页面展示生命周期) |
| `C` | `tabB` | `C` [页面销毁生命周期](#页面销毁生命周期)，`tabB` [页面创建生命周期](#页面创建生命周期) |
| `D` | `tabB` | `D` [页面销毁生命周期](#页面销毁生命周期)，`tabB` [页面展示生命周期](#页面展示生命周期) |
| `D`（从转发进入） | `tabA` | `D` [页面销毁生命周期](#页面销毁生命周期)，`tabA` [页面创建生命周期](#页面创建生命周期) |
| `D`（从转发进入） | `tabB` | `D` [页面销毁生命周期](#页面销毁生命周期)，`tabB` [页面创建生命周期](#页面创建生命周期) |

