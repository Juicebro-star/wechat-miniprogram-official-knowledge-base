---
title: "发票功能页"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages/choose-invoice.html"
source_route: "/miniprogram/dev/framework/plugin/functional-pages/choose-invoice.html"
raw_html: "raw-html/plugin/functional-pages/choose-invoice.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages/choose-invoice.html](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages/choose-invoice.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/plugin/functional-pages/choose-invoice.html`

# 发票功能页

从基础库版本 [2.16.1](../../compatibility) 起，该功能页已经废弃，可以直接使用 [`wx.chooseInvoice`](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/invoice/wx.chooseInvoice.html) 实现对应的功能；点击 `functional-page-navigator` 也将不再进入功能页，直接进入发票选择页。

文档

发票功能页用于展示用户的发票列表，用户可以选择其中的发票。自基础库版本 [2.14.1](../../compatibility) 开始支持。

## 调用参数

发票功能页使用 [functional-page-navigator](https://developers.weixin.qq.com/miniprogram/dev/component/functional-page-navigator.html) 进行跳转时，对应的参数 name 应为固定值 `chooseInvoice`，返回参数与 [wx.chooseInvoice](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/invoice/wx.chooseInvoice.html) 相同。

**bindsuccess 返回参数说明：**

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| invoiceInfo | String | 用户选中的发票信息，格式为一个 JSON 字符串，包含三个字段： card_id：所选发票卡券的 cardId，encrypt_code：所选发票卡券的加密 code，报销方可以通过 cardId 和 encryptCode 获得报销发票的信息，app_id： 发票方的 appId。 |

## 示例代码

```
<!--plugin/components/hello-component.wxml-->
  <functional-page-navigator
    name="chooseInvoice"
    version="develop"
    bind:success="onSuccess"
    bind:fail="onFail"
  >
    <button>选择发票</button>
  </functional-page-navigator>
```

```
// plugin/components/hello-component.js
Component({
  methods: {
    onSuccess: function (res) {
      console.log(res.detail);
    },
    onFail: function (res) {
      console.log(res);
    }
  }
});
```

