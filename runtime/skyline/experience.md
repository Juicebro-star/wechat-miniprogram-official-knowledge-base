---
title: "快速体验"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/skyline/experience.html"
source_route: "/miniprogram/dev/framework/runtime/skyline/experience.html"
raw_html: "raw-html/runtime/skyline/experience.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/skyline/experience.html](https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/skyline/experience.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/runtime/skyline/experience.html`

# 快速体验

## 环境要求

目前，[安卓微信 8.0.33](https://weixin.qq.com/cgi-bin/readtemplate?lang=zh_CN&t=weixin_faq_list&head=true)、[iOS 微信 8.0.34](https://weixin.qq.com/cgi-bin/readtemplate?lang=zh_CN&t=weixin_faq_list&head=true) 起内置了 Skyline 渲染引擎，可先更新到该版本，预览时通过[强切开关](migration/index#快捷切换入口)打开，方可体验 Skyline。

## 快速体验

以下小程序已适配 Skyline，可直接扫码打开体验。

扫码打开小程序助手，其中首页、切换小程序、版本查看、成员管理和成员申请均已适配。

![](../../assets/fe8b3f717270db4f.jpeg)

扫码小程序示例，可进入 `交互动画` tab 页体验 Skyline 的新特性。

![](../../assets/74c809728aeeb46b.png)

## 演示案例

以下是用 Skyline 实现的各种常见交互动画的示例，可通过视频直接预览效果，也可直接扫码在移动端上体验。

通讯录

使用 scroll-view 自定义模式，配合 sticky 吸顶布局容器，实现通讯录字母交错吸顶的效果。

[点击查看源代码](https://github.com/wechat-miniprogram/awesome-skyline/tree/main/examples/address-book)

![](../../assets/dabf59b9ad520786.bin)

[](https://res.wx.qq.com/op_res/1TdiJdN3xciFb1tvKi0sWcl0RuH-UExi7jQ2CtYmU5zb-gXGaw6vXjaRHmQGC1jnilqmr1O2EzTHDmUTBJKbdQ)

半屏

基于 worklet 动画，通过手势协商机制，实现在半屏内列表往下拉到顶之后，无缝切换到半屏下拉的效果。

[点击查看源代码](https://github.com/wechat-miniprogram/awesome-skyline/tree/main/examples/half-screen)

![](../../assets/a83d6eaf5663e5f2.bin)

[](https://res.wx.qq.com/op_res/1TdiJdN3xciFb1tvKi0sWWTnHp-vub75b40Ne_aomJD4STifjtuoXbc1DutV-8H28R7vuSzsCf3DFtqLwH2atg)

分段半屏

通过 worklet 动画、手势协商，实现分段半屏，处于不同位置时联动半屏后的地图改变缩放比例。

[点击查看源代码](https://github.com/wechat-miniprogram/awesome-skyline/tree/main/examples/segmented-half-screen)

![](../../assets/5593f8d8d53a1667.bin)

[](https://res.wx.qq.com/op_res/8IhRLhb7LpCYzAnQUC__R6gZPf2T1cLJmeh67jXs2c2AsQcXnYqr21NGDU-KsOr5z2xdEZvy61llzfYz6Kaqlg)

相册

使用自定义路由、共享元素动画、手势系统等实现列表中图片共享放大过渡到图片预览页效果，并实现预览图片的手势交互。

[点击查看源代码](https://github.com/wechat-miniprogram/awesome-skyline/tree/main/examples/album)

![](../../assets/27fb9b62526c7cc5.bin)

[](https://res.wx.qq.com/op_res/jGOCATNiP1cfu0sKmlQMk1krH51OftEGjnjXsEftFY3DVWaeP8slZB-qQriinVdTUYS2G3DD7Lt9TsuTNA_1TQ)

Tab 指示条

利用 swiper 切换时的逐帧回调，配合 worklet 动画实现 tab 指示条顺滑切换的效果。

[点击查看源代码](https://github.com/wechat-miniprogram/awesome-skyline/tree/main/examples/tab-indicator)

![](../../assets/7bf600ce4dbc01f5.bin)

[](https://res.wx.qq.com/op_res/7ArvIfANT4eL8Ia2Z_Oq2kNz85k_8Rqum-WzGVF-QsBXg21KdZQBp_6EvpBaw43FONgJpwduibDlaVTuPxahhA)

卡片转场

scroll-view 瀑布流模式配合共享元素动画实现卡片柔性转场效果。

[点击查看源代码](https://github.com/wechat-miniprogram/awesome-skyline/tree/main/examples/card_transition)

![](../../assets/484bef0437d66f48.bin)

[](https://res.wx.qq.com/op_res/jGOCATNiP1cfu0sKmlQMk5R9TDNo2hCDYiZs6MQsNmY_fTLlk-qHLJWwVhpGxdk8Qekq140v8nioENabjJ6W2w)

搜索栏吸附

scroll-view 吸顶布局结合 worklet 布局轻松实现搜索栏吸附效果。

[点击查看源代码](https://github.com/wechat-miniprogram/awesome-skyline/tree/main/examples/product-list)

![](../../assets/623a43fc8f638b0e.bin)

[](https://res.wx.qq.com/op_res/eki_c6cJ1PZ0aWNRUv3qb20dUMHtj_N2LvI4rTMko_bDc46CWxyR8WMihzCQV50NduHwiMTCguiclrda7HF_yg)

沉浸式商品浏览

小程序手势 + worklet 在页面中实现广告、商品无缝切换。

[点击查看源代码](https://github.com/wechat-miniprogram/awesome-skyline/tree/main/examples/expanded-scroll-view)

![](../../assets/d026fcbb0e61e6ca.bin)

[](https://res.wx.qq.com/op_res/eki_c6cJ1PZ0aWNRUv3qb786yBYAod7YODmzQNe1Xp3SJpC-GG9T1B82wOT0OYl41Ej4Lvt1YFMnvobB74AoRg)

