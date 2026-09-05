---
title: "动画曲线"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/skyline/easing-function.html"
source_route: "/miniprogram/dev/framework/runtime/skyline/easing-function.html"
raw_html: "raw-html/runtime/skyline/easing-function.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/skyline/easing-function.html](https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/skyline/easing-function.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/runtime/skyline/easing-function.html`

# 动画曲线

在 `Skyline` 下，部分滚动容器和上下文接口的动画，可通过 `easingFunction` 字段指定动画曲线，如 `ScrollViewContext.triggerTwoLevel`、`DraggableSheetContext.scrollTo` 等。该字段接收一段字符串（字符串字面量，如 `'ease'`），取值可分为三类。

## 内置曲线

`easingFunction` 可传入下表所示的取值，框架会在动画期间按对应曲线的进度驱动动画。

| 取值 | 说明 | 等价贝塞尔参数 |
| --- | --- | --- |
| `linear` | 匀速运动 | - |
| `ease` | 先加速后减速，默认取值 | `cubic-bezier(0.25, 0.1, 0.25, 1)` |
| `easeIn` | 加速进入 | `cubic-bezier(0.42, 0, 1, 1)` |
| `easeOut` | 减速离开 | `cubic-bezier(0, 0, 0.58, 1)` |
| `easeInOut` | 前半程加速、后半程减速 | `cubic-bezier(0.42, 0, 0.58, 1)` |
| `fastOutSlowIn` | 快速进入后缓慢收尾，Material 风格 | `cubic-bezier(0.4, 0, 0.2, 1)` |
| `slowMiddle` | 中部缓慢，首尾较快 | `cubic-bezier(0.15, 0.85, 0.85, 0.15)` |
| `fastLinearToSlowEaseIn` | 起始接近线性，随后缓慢减速 | `cubic-bezier(0.18, 1, 0.04, 1)` |
| `easeInToLinear` | 缓慢加速后转为线性 | `cubic-bezier(0.67, 0.03, 0.65, 0.09)` |
| `linearToEaseOut` | 起始线性后缓慢减速 | `cubic-bezier(0.35, 0.91, 0.33, 0.97)` |
| `easeInSine` | 正弦缓入，温和加速 | `cubic-bezier(0.47, 0, 0.745, 0.715)` |
| `easeOutSine` | 正弦缓出，温和减速 | `cubic-bezier(0.39, 0.575, 0.565, 1)` |
| `easeInOutSine` | 正弦缓入缓出 | `cubic-bezier(0.445, 0.05, 0.55, 0.95)` |
| `easeInQuad` | 平方缓入 | `cubic-bezier(0.55, 0.085, 0.68, 0.53)` |
| `easeOutQuad` | 平方缓出 | `cubic-bezier(0.25, 0.46, 0.45, 0.94)` |
| `easeInOutQuad` | 平方缓入缓出 | `cubic-bezier(0.455, 0.03, 0.515, 0.955)` |
| `easeInCubic` | 立方缓入 | `cubic-bezier(0.55, 0.055, 0.675, 0.19)` |
| `easeOutCubic` | 立方缓出 | `cubic-bezier(0.215, 0.61, 0.355, 1)` |
| `easeInOutCubic` | 立方缓入缓出 | `cubic-bezier(0.645, 0.045, 0.355, 1)` |
| `easeInQuart` | 四次方缓入 | `cubic-bezier(0.895, 0.03, 0.685, 0.22)` |
| `easeOutQuart` | 四次方缓出 | `cubic-bezier(0.165, 0.84, 0.44, 1)` |
| `easeInOutQuart` | 四次方缓入缓出 | `cubic-bezier(0.77, 0, 0.175, 1)` |
| `easeInQuint` | 五次方缓入 | `cubic-bezier(0.755, 0.05, 0.855, 0.06)` |
| `easeOutQuint` | 五次方缓出 | `cubic-bezier(0.23, 1, 0.32, 1)` |
| `easeInOutQuint` | 五次方缓入缓出 | `cubic-bezier(0.86, 0, 0.07, 1)` |
| `easeInExpo` | 指数缓入，起始极慢后急剧加速 | `cubic-bezier(0.95, 0.05, 0.795, 0.035)` |
| `easeOutExpo` | 指数缓出，急剧减速后趋于平缓 | `cubic-bezier(0.19, 1, 0.22, 1)` |
| `easeInOutExpo` | 指数缓入缓出 | `cubic-bezier(1, 0, 0, 1)` |
| `easeInCirc` | 圆形缓入 | `cubic-bezier(0.6, 0.04, 0.98, 0.335)` |
| `easeOutCirc` | 圆形缓出 | `cubic-bezier(0.075, 0.82, 0.165, 1)` |
| `easeInOutCirc` | 圆形缓入缓出 | `cubic-bezier(0.785, 0.135, 0.15, 0.86)` |
| `easeInBack` | 缓入并越过起点少许再回正（有过冲） | `cubic-bezier(0.6, -0.28, 0.735, 0.045)` |
| `easeOutBack` | 缓出并越过终点少许再回正（有过冲） | `cubic-bezier(0.175, 0.885, 0.32, 1.275)` |
| `easeInOutBack` | 首尾均有少量过冲 | `cubic-bezier(0.68, -0.55, 0.265, 1.55)` |

## 弹性与反弹曲线

| 取值 | 说明 |
| --- | --- |
| `elasticIn` / `easeInElastic` | 弹性缓入，起始反向摆动后冲向终点 |
| `elasticOut` / `easeOutElastic` | 弹性缓出，临近终点来回摆动并衰减 |
| `elasticInOut` / `easeInOutElastic` | 首尾均有弹性摆动 |
| `bounceIn` / `easeInBounce` | 反弹缓入，从下方多次弹跳进入 |
| `bounceOut` / `easeOutBounce` | 反弹缓出，落地式多次弹跳衰减 |
| `bounceInOut` / `easeInOutBounce` | 首尾均有反弹效果 |

弹性曲线按弹簧运动计算，反弹曲线按分段弹跳函数计算，均可在到达终点附近时产生回弹的视觉效果，适合强调类动画。

## 自定义贝塞尔曲线

`easingFunction` 支持直接传入一条三次贝塞尔曲线，写法形如 `cubic-bezier(x1, y1, x2, y2)`。

其中 `x1`、`y1`、`x2`、`y2` 为四个数值，例如 `cubic-bezier(0.25, 0.1, 0.25, 1)`。

- `x1`、`x2` 为控制点横坐标，通常取值 `[0, 1]`，决定速度变化的时机
- `y1`、`y2` 为控制点纵坐标，可超出 `[0, 1]`，超出部分会产生过冲（回弹）效果
- 数值支持小数、整数、负数，各参数间用英文逗号分隔，逗号前后可有空格，如 `cubic-bezier(0.68, -0.55, 0.265, 1.55)` 即内置 `easeInOutBack` 的曲线

可将 `easingFunction` 设为上述字符串写法后，通过调整动画时长与目标位置直观对比不同曲线的过渡效果。

