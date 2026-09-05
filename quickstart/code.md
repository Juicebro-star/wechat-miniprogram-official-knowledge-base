---
title: "小程序代码构成"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/code.html"
source_route: "/miniprogram/dev/framework/quickstart/code.html"
raw_html: "raw-html/quickstart/code.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/code.html](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/code.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/quickstart/code.html`

# 小程序代码构成

在上一章中，通过开发者工具快速创建了一个 QuickStart 项目。你可以留意到这个项目里边生成了不同类型的文件：

1. 1.`.json` 后缀的 `JSON` 配置文件
2. 2.`.wxml` 后缀的 `WXML` 模板文件
3. 3.`.wxss` 后缀的 `WXSS` 样式文件
4. 4.`.js` 后缀的 `JS` 脚本逻辑文件

接下来我们分别看看这 4 种文件的作用。

## JSON 配置

JSON 是一种数据格式，并不是编程语言，在小程序中，JSON 扮演的静态配置的角色。

在项目的根目录有一个 `app.json` 和 `project.config.json`，此外在 `pages/logs` 目录下还有一个 `logs.json`，依次来说明一下它们的用途。

### 小程序配置 app.json

`app.json` 是当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。

QuickStart 项目里边的 `app.json` 配置内容比较多，其中有 `pages`、`window` 等字段。以下是一个简化版：

```
{
  "pages": [
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window": {
    "navigationBarTextStyle": "black",
    "navigationBarTitleText": "Weixin",
    "navigationBarBackgroundColor": "#ffffff"
  }
}
```

1. 1.`pages` 字段 —— 用于描述当前小程序所有页面路径，这是为了让微信客户端知道当前你的小程序页面定义在哪个目录，其中的第一个页面就是小程序的首页（打开小程序看到的第一个页面）。
2. 2.`window` 字段 —— 定义小程序所有页面的顶部背景颜色，文字颜色定义等。

其他配置项细节可以参考文档 [小程序的配置 app.json](../config) 。

### 工具配置 project.config.json

通常大家在使用一个工具的时候，都会针对各自喜好做一些个性化配置，例如界面颜色、编译配置等等，当你换了另外一台电脑重新安装工具的时候，你还要重新配置。

考虑到这点，小程序开发者工具在每个项目的根目录都会生成一个 `project.config.json`，你在工具上做的任何配置都会写入到这个文件，当你重新安装工具或者换电脑工作时，你只要载入同一个项目的代码包，开发者工具就自动会帮你恢复到当时你开发项目时的个性化配置，其中会包括编辑器的颜色、代码上传时自动压缩等等一系列选项。

其他配置项细节可以参考文档 [开发者工具的配置](https://developers.weixin.qq.com/miniprogram/dev/devtools/projectconfig) 。

### 页面配置 page.json

这里的 `page.json` 其实用来表示 pages/logs 目录下的 `logs.json` 这类和小程序页面相关的配置。

如果你整个小程序的风格是蓝色调，那么你可以在 `app.json` 里边声明顶部颜色是蓝色即可。实际情况可能不是这样，可能你小程序里边的每个页面都有不一样的色调来区分不同功能模块，因此我们提供了 `page.json`，让开发者可以独立定义每个页面的一些属性，例如刚刚说的顶部颜色、是否允许下拉刷新等等。

其他配置项细节可以参考文档 [页面配置](../config#页面配置) 。

需要注意的是， `page.json` 还需要搭配 `WXML`、`WXSS`、`JS` 文件，才能构成一个能正常运行的页面。

### JSON 语法

这里说一下小程序里 JSON 配置的一些注意事项。

JSON 文件都是被包裹在一个大括号中 {}，通过 key-value 的方式来表达数据。JSON 的 Key 必须包裹在一个双引号中，在实践中，编写 JSON 的时候，忘了给 Key 值加双引号或者是把双引号写成单引号是常见错误。

JSON 的值只能是以下几种数据格式，其他任何格式都会触发报错，例如 JavaScript 中的 undefined。

1. 1.数字，包含浮点数和整数
2. 2.字符串，需要包裹在双引号中
3. 3.Bool 值，true 或者 false
4. 4.数组，需要包裹在方括号中 []
5. 5.对象，需要包裹在大括号中 {}
6. 6.Null

还需要注意的是 JSON 文件中无法使用注释，试图添加注释将会引发报错。

## WXML 模板

从事过网页编程的人知道，网页编程采用的是 HTML + CSS + JS 这样的组合，其中 `HTML` 是用来描述当前这个页面的结构，`CSS` 用来描述页面的样子，`JS` 通常是用来处理这个页面和用户的交互。

同样道理，在小程序中也有同样的角色，其中 `WXML` 充当的就是类似 `HTML` 的角色。打开 `pages/index/index.wxml`，可以看到一段页面模版，其简化版本如下：

```
<scroll-view class="scrollarea" scroll-y type="list">
  <view class="container">
    <view class="userinfo">
      <!-- ... -->
    </view>
    <view class="usermotto">
      <text class="user-motto">{{motto}}</text>
    </view>
  </view>
</scroll-view>
```

和 `HTML` 非常相似，`WXML` 由标签、属性等等构成。但是也有很多不一样的地方，下面一一阐述一下：

1. 1.标签名字有点不一样

往往写 HTML 的时候，经常会用到的标签是 `div`、`p`、`span`，在写一个页面的时候可以根据这些基础的标签组合出不一样的组件，例如日历、弹窗等等。换个思路，既然大家都需要这些组件，为什么不能把这些常用的组件包装起来，大大提高开发效率。

从上边的例子可以看到，小程序的 `WXML` 用的标签是 `view`、`button`、`text` 等等，这些标签就是小程序给开发者包装好的基本能力，还提供了地图、视频、音频等等组件能力。

更多的组件可以参考 [小程序的组件](../view/component) 。

1. 2.多了一些 `wx:if` 这样的属性以及 {{ }} 这样的表达式

在网页的一般开发流程中，通常会通过 `JS` 操作 `DOM`（对应 `HTML` 的描述产生的树），以引起界面的一些变化响应用户的行为。例如，用户点击某个按钮的时候，`JS` 会记录一些状态到 `JS` 变量里边，同时通过 `DOM` API 操控 `DOM` 的属性或者行为，进而引起界面一些变化。当项目越来越大的时候，你的代码会充斥着非常多的界面交互逻辑和程序的各种状态变量，显然这不是一个很好的开发模式，因此就有了 MVVM 的开发模式（例如 React、Vue），提倡把渲染和逻辑分离。简单来说就是不要再让 `JS` 直接操控 `DOM`，`JS` 只需要管理状态即可，然后再通过一种模板语法来描述状态和界面结构的关系即可。

小程序的框架也是用到了这个思路，如果你需要把一个 `Hello World` 的字符串显示在界面上。

WXML 是这么写：

```
<text>{{msg}}</text>
```

JS 只需要管理状态即可：

```
this.setData({ msg: "Hello World" })
```

通过 {{ }} 的语法把一个变量绑定到界面上，称为数据绑定。仅仅通过数据绑定还不够完整的描述状态和界面的关系，还需要 `if`/`else`、`for` 等控制能力，在小程序里边，这些控制能力都用 `wx:` 开头的属性来表达。

更详细的文档可以参考 [WXML](../view/wxml/index) 。

## WXSS 样式

`WXSS` 具有 `CSS` 大部分的特性，小程序在 `WXSS` 也做了一些扩充和修改。

1. 1.新增了尺寸单位。在写 `CSS` 样式时，需要考虑到手机设备的屏幕会有不同的宽度和设备像素比，采用一些技巧来换算一些像素单位。`WXSS` 在底层支持新的尺寸单位 `rpx`，开发者可以免去换算的烦恼，只要交给小程序底层来换算即可，由于换算采用的浮点数运算，所以运算结果会和预期结果有一点点偏差。

1. 2.提供了全局的样式和局部样式。和前边 `app.json`、`page.json` 的概念相同，你可以写一个 `app.wxss` 作为全局样式，会作用于当前小程序的所有页面，局部页面样式 `page.wxss` 仅对当前页面生效。

1. 3.此外 `WXSS` 仅支持部分 `CSS` 选择器。

更详细的文档可以参考 [WXSS](../view/wxss) 。

## JS 逻辑交互

一个服务仅仅只有界面展示是不够的，还需要和用户做交互：响应用户的点击、获取用户的位置等等。在小程序里边，通过编写 `JS` 脚本文件来处理用户的操作。

```
<view>{{ msg }}</view>
<button bind:tap="clickMe">点击我</button>
```

点击 `button` 按钮的时候，希望把界面上 `msg` 显示成 `"Hello World"`，于是在 `button` 上声明一个属性：`bind:tap`，在 JS 文件里边声明了 `clickMe` 方法来响应这次点击操作：

```
Page({
  clickMe: function() {
    this.setData({ msg: "Hello World" })
  }
})
```

响应用户的操作就是这么简单，更详细的事件可以参考文档 [WXML - 事件](../view/wxml/event) 。

JS 中，必须包含一个 `Page` 页面构造器。有关于 `Page` 页面构造器更多详细的文档参考 [注册页面 Page](../app-service/page) 。

此外你还可以在 JS 中调用小程序提供的丰富的 API，利用这些 API 可以很方便的调起微信提供的能力，例如获取用户信息、本地存储、微信支付等。在前边的 QuickStart 例子中，在 `pages/index/index.js` 就调用了 [wx.getUserProfile](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserProfile.html) 获取微信用户的头像和昵称，最后通过 `setData` 把获取到的信息显示到界面上。更多 API 可以参考文档 [小程序的 API](../app-service/api) 。

## 程序

有一个特殊的 `JS` 文件 `app.js`：

```
App()
```

整个小程序只有一个 App 实例，是全部页面共享的，更多的事件回调参考文档 [注册程序 App](../app-service/app) 。

通过这个章节，你了解了小程序涉及到的文件类型以及对应的角色，在 [下个章节](release) 中，将介绍小程序协同工作与发布的相关内容。

