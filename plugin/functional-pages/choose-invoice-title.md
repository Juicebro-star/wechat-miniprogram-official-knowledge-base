---
title: "发票抬头功能页"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages/choose-invoice-title.html"
source_route: "/miniprogram/dev/framework/plugin/functional-pages/choose-invoice-title.html"
raw_html: "raw-html/plugin/functional-pages/choose-invoice-title.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages/choose-invoice-title.html](https://developers.weixin.qq.com/miniprogram/dev/framework/plugin/functional-pages/choose-invoice-title.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/plugin/functional-pages/choose-invoice-title.html`

# 发票抬头功能页

从基础库版本 [2.16.1](../../compatibility) 起，该功能页已经废弃，可以直接使用 [`wx.chooseInvoiceTitle`](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/invoice/wx.chooseInvoiceTitle.html) 实现对应的功能；点击 `functional-page-navigator` 也将不再进入功能页，直接进入发票抬头选择页。

文档

发票抬头功能页用于展示用户的发票抬头列表，用户可以选择其中的发票抬头。自基础库版本 [2.14.1](../../compatibility) 开始支持。

## 调用参数

发票抬头功能页使用 [functional-page-navigator](https://developers.weixin.qq.com/miniprogram/dev/component/functional-page-navigator.html) 进行跳转时，对应的参数 name 应为固定值 `chooseInvoiceTitle`，返回参数与 [wx.chooseInvoiceTitle](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/invoice/wx.chooseInvoiceTitle.html) 相同。

**bindsuccess 返回参数说明：**

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| type | string | 抬头类型 |
| title | string | 抬头名称 |
| taxNumber | string | 抬头税号 |
| companyAddress | string | 单位地址 |
| telephone | string | 手机号码 |
| bankName | string | 银行名称 |
| bankAccount | string | 银行账号 |
| errMsg | string | 错误信息 |

**res.type 的合法值**

| 值 | 说明 | 最低版本 |
| --- | --- | --- |
| 0 | 单位 |  |
| 1 | 个人 |  |

## 示例代码

```
<!--plugin/components/hello-component.wxml-->
  <functional-page-navigator
    name="chooseInvoiceTitle"
    version="develop"
    bind:success="onSuccess"
    bind:fail="onFail"
  >
    <button>选择发票抬头</button>
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

