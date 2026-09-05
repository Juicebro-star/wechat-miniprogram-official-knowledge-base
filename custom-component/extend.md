---
title: "自定义组件扩展"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/extend.html"
source_route: "/miniprogram/dev/framework/custom-component/extend.html"
raw_html: "raw-html/custom-component/extend.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/extend.html](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/extend.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/custom-component/extend.html`

# 自定义组件扩展

> 基础库 2.2.3 开始支持，低版本需做[兼容处理](../compatibility)。

`Component` 构造器扩展接口可以用来扩展 `Component` 构造器的接口功能。

可以参考 [computed](https://github.com/wechat-miniprogram/computed) 项目对相关接口的具体使用。

## Definition 形式扩展

使用 `Component` 构造器的 Definition 形式时，可以通过 `definitionFilter` 定义段来扩展构造器的接口功能。

`definitionFilter` 需要写在 `Behavior` 构造器中。`definitionFilter` 是一个函数，在被调用时会注入两个参数：

- 第一个参数是使用这个 behavior 的 component/behavior 的定义对象；
- 第二个参数是这个 behavior 所使用的 behavior 的 `definitionFilter` 函数列表。

第一个参数是其中的关键：可以访问到使用者传给 `Component` 构造器的 Definition 形式定义对象，而且可以修改它。这样，就可以为 `Component` 构造器添加额外的定义段支持。

```
const behavior3 = Behavior({
    definitionFilter(defFields, definitionFilterArr) {},
})

const behavior2 = Behavior({
  behaviors: [behavior3],
  definitionFilter(defFields, definitionFilterArr) {
    // definitionFilterArr<a href="defFields" target="_blank">0</a>
  },
})

const behavior1 = Behavior({
  behaviors: [behavior2],
  definitionFilter(defFields, definitionFilterArr) {},
})

export default Component({
  behaviors: [behavior1],
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Zv857vmU8Ubg)

上述代码中声明了 1 个自定义组件和 3 个 behavior，每个 behavior 都使用了 `definitionFilter` 定义段。那么按照声明的顺序会有如下事情发生：

1. 当进行 `behavior2` 的声明时就会调用 `behavior3` 的 `definitionFilter` 函数，其中 `defFields` 参数是 `behavior2` 的定义段，`definitionFilterArr` 参数即为空数组，因为 `behavior3` 没有使用其他的 behavior。
2. 当进行 `behavior1` 的声明时就会调用 `behavior2` 的 `definitionFilter` 函数，其中 `defFields` 参数是 `behavior1` 的定义段，`definitionFilterArr` 参数是一个长度为 1 的数组，`definitionFilterArr[0]` 即为 `behavior3` 的 `definitionFilter` 函数，因为 behavior2 使用了 behavior3。用户在此处可以自行决定在进行 behavior1 的声明时要不要调用 behavior3 的 `definitionFilter` 函数，如果需要调用，在此处补充代码 `definitionFilterArr<a href="defFields" target="_blank">0</a>` 即可，`definitionFilterArr` 参数会由基础库补充传入。
3. 同理，在进行 `component` 的声明时就会调用 `behavior1` 的 `definitionFilter` 函数。

简单概括，`definitionFilter` 函数可以理解为当 A 使用了 B 时，A 声明就会调用 B 的 `definitionFilter` 函数并传入 A 的定义对象让 B 去过滤。此时如果 B 还使用了 C 和 D ，那么 B 可以自行决定要不要调用 C 和 D 的 `definitionFilter` 函数去过滤 A 的定义对象。

## Chaining 形式扩展

使用 `Component` 构造器的 Chaining 形式时，可以通过 `chainingFilter` 方法来扩展构造器的接口功能。

`chainingFilter` 允许更改 Chaining 调用链自身的方法。例如，做一个这样的 behavior：

```
const foo = Behavior()
  .chainingFilter((chain) => {
    const property = chain.property
    // 定义一个新的 Chaining 调用链方法
    chain.stringProperty = function (propName) {
      return property.call(this, propName, String)
    }
    return chain
  })
  .register()
```

这个 behavior 向调用链上添加了 `stringProperty` 方法。这样，引入这个 behavior 之后就可以调用它添加的调用链方法了。

```
export default Component()
  // 引入含有 chainingFilter 的 behavior
  .behavior(foo)
  // 可以调用 behavior 中的调用链方法
  .stringProperty('strProp')
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/cK8f5vm98Ubz)

