---
title: "页面路由监听"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route-event-listener.html"
source_route: "/miniprogram/dev/framework/app-service/route-event-listener.html"
raw_html: "raw-html/app-service/route-event-listener.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route-event-listener.html](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/route-event-listener.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/app-service/route-event-listener.html`

# 页面路由监听

> 基础库 3.5.5 开始支持，低版本需做[兼容处理](../compatibility)。

这篇指南主要说明从基础库版本 [3.5.5](../compatibility) 起可用的 *页面路由事件监听函数* 的使用方法。如果需要了解页面路由的类型及逻辑等基本信息，可以参考 [页面路由](route)。

## 何时使用页面路由监听函数

每次路由可能触发多个页面的多个页面生命周期。

在实践中，针对“页面创建“、“页面销毁“这样的页面生命周期来编程往往更为合理。例如，页面初始化时需要执行的逻辑，可以写在 [页面创建生命周期](route#页面创建生命周期) 中。

但也有个别情况，针对路由本身来编程更为合理。例如：当小程序进行重加载 `reLaunch` 路由时，小程序可能需要重设一些全局状态来保证后续逻辑正常工作，或者模拟近似于重新启动的效果。

此外，由于页面路由事件 `onBeforePageLoad` 触发时机在页面初始化之前（有时会比其他页面生命周期早很多），可以在这个路由事件中发起网络请求，以便更早获取网络数据。

## 所有监听及触发时序

| 页面路由监听 | 触发时机 | 每次路由中的触发次数 |
| --- | --- | --- |
| [`wx.onBeforeAppRoute`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onBeforeAppRoute.html) | 路由事件下发到基础库，基础库执行路由逻辑前触发 | 一次 |
| [`wx.onAppRoute`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onAppRoute.html) | 路由事件下发到基础库，基础库执行路由逻辑后触发 | 一次 |
| [`wx.onAppRouteDone`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onAppRouteDone.html) | 路由对应的动画（页面推入、推出等）完成时触发 | 一次 |
| [`wx.onBeforePageLoad`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onBeforePageLoad.html) | 路由引发的页面创建之前触发 | 不限 |
| [`wx.onAfterPageLoad`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onAfterPageLoad.html) | 路由引发的页面创建完成后触发 | 不限 |
| [`wx.onBeforePageUnload`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onBeforePageUnload.html) | 路由引发的页面销毁之前触发 | 不限 |
| [`wx.onAfterPageUnload`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onAfterPageUnload.html) | 路由引发的页面销毁完成后触发 | 不限 |

例如，在一次 `redirectTo` 中，监听和处理逻辑将按以下顺序触发：

1. [`wx.onBeforeAppRoute`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onBeforeAppRoute.html)
2. [`wx.onBeforePageUnload`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onBeforePageUnload.html)
3. 旧页面的 [页面销毁生命周期](route#页面销毁生命周期)
4. 旧页面弹出页面栈，此时开始 [`getCurrentPages`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/getCurrentPages) 接口不再能获取到旧页面
5. [`wx.onAfterPageUnload`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onAfterPageUnload.html)
6. [`wx.onBeforePageLoad`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onBeforePageLoad.html)
7. 创建新页面，此过程中触发 [页面创建生命周期](route#页面创建生命周期) 中的组件 `created` 生命周期
8. 新页面压入页面栈，此时开始 [`getCurrentPages`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/getCurrentPages) 接口可以获取到新页面
9. 挂载新页面，此过程中触发 [页面创建生命周期](route#页面创建生命周期) 中除组件 `created` 以外的生命周期
10. [`wx.onAfterPageLoad`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onAfterPageLoad.html)
11. [`wx.onAppRoute`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onAppRoute.html)
12. （新页面推入动画完成时）[`wx.onAppRouteDone`](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-route/wx.onAppRouteDone.html)

对于其他路由，可以结合 [页面路由](route) 中的具体路由逻辑进行类推。

## 路由事件 ID

为了在多次监听回调中识别同一个路由事件，框架会为每一次独立的路由事件生成一个在小程序实例中唯一的 ID，称为 *路由事件 ID*。在所有页面路由监听函数中，事件参数中都将携带一个字符串 `routeEventId`，表示这个路由事件 ID。小程序可以通过读取回调中的 `routeEventId`，来将同一个路由在不同时间节点触发的不同回调进行关联。例如：

```
const redirectToContext = {};
wx.onBeforeAppRoute(res => {
  if (res.openType === "redirectTo") {
    redirectToContext[res.routeEventId] = { startTime: new Date() };
  }
});
wx.onBeforePageUnload(res => {
  const context = redirectToContext[res.routeEventId];
  if (context !== undefined) {
    context.from = res.page.is;
    context.data = res.page.data;
  }
});
wx.onAfterPageLoad(res => {
  const context = redirectToContext[res.routeEventId];
  if (context !== undefined) {
    console.log(
      `A "redirectTo" route replaced page "${context.from}" to "${
        res.page.is
      }", which is started at ${context.startTime.toString()}`
    );
    res.page.setData(context.data);
    delete redirectToContext[res.routeEventId];
  }
});
```

这个例子中，我们通过 `routeEventId` 关联了一次 `redirectTo` 中的页面创建和页面销毁：在页面销毁时记录了旧页面的数据，并将其应用到了新页面上。

## 可能的用例

1. 进行路由上报，方便还原用户使用路径：

   ```
   wx.onAppRoute(res => {
     myReportAppRoute(res.timeStamp, res.openType, res.path, res.query);
   });
   ```
2. 小程序冷启动或热启动时，重置所有状态：

   ```
   wx.onBeforeAppRoute(res => {
     if (["appLaunch", "reLaunch", "autoReLaunch"].includes(res.openType)) {
       myGlobalState.reset();
     }
   });
   ```

   这可以解决一些常见情景，例如小程序当前在后台，用户扫码热启动，触发 `autoReLaunch` 时进行状态清理。
3. 新页面创建前先进行网络请求，使页面首屏创建和等待网络请求并行进行：

   ```
   const pageRequestData = {};
   wx.onBeforePageLoad(res => {
     pageRequestData[res.routeEventId] = new Promise((resolve, reject) => {
       wx.request({
         url: `https://mysite.wechat.qq.com/page-data?path=${res.path}&param=${res.query.param}`,
         success(res) {
           resolve(res);
         },
         fail(res) {
           reject(res);
         }
       });
     });
   });
   wx.onAfterPageLoad(res => {
     pageRequestData[res.routeEventId]
       .then(data => {
         res.page.setData(data);
       })
       .catch(err => {
         console.error("page data init error", err);
       });
   });
   ```

   当页面比较复杂时，页面创建需要一定时间。这个做法能充分利用页面的创建时间来等待网络请求返回，从而更快地将业务数据应用到页面上，展示给用户。

