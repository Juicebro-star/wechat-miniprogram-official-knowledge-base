---
title: "数据监听器"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/observer.html"
source_route: "/miniprogram/dev/framework/observer.html"
raw_html: "raw-html/observer.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/observer.html](https://developers.weixin.qq.com/miniprogram/dev/framework/observer.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/observer.html`

# 数据监听器

数据监听器可以用于监听和响应任何数据字段的变化。从小程序基础库版本 [2.6.1](compatibility) 开始支持。

数据监听器和 [属性的 observer](component-framework/properties#属性值变化监听器) 相比，数据监听器更强大且通常具有更好的性能。

## 使用数据监听器

有时，在一些数据字段被 setData 设置时，需要执行一些操作。

例如，`this.data.sum` 永远是 `this.data.numberA` 与 `this.data.numberB` 的和。在 Definition 形式可以这样使用：

```
export default Component({
  data: {
    numberA: 0,
    numberB: 0,
  },
  lifetimes: {
    attached: function() {
      this.setData({
        numberA: 1,
        numberB: 2,
      })
    },
  },
  observers: {
    'numberA, numberB': function(numberA, numberB) {
      // 在 numberA 或者 numberB 被设置时，执行这个函数
      // 在 exparser 组件框架中
      this.setData({
        sum: numberA + numberB
      })
    }
  }
})
```

注意：在数据监听器中调用 `setData` 后，数据变更并不会同步应用到 WXML 模板上！换而言之，这里调用 `setData` 的效果相当于 [updateData](component-framework/data-updates#使用-updatedata-更新)；若使用 glass-easel 组件框架，出于表意明确的考虑，在这里推荐写成 `updateData`。

在 Chaining 形式可以这样写：

```
export default Component()
  .data(() => ({
    numberA: 0,
    numberB: 0,
    sum: 0,
  }))
  .init(function ({ setData, lifetime, observer }) {
    lifetime('attached', () => {
      setData({
        numberA: 1,
        numberB: 2,
      })
    })

    // 在 numberA 或者 numberB 被设置时，执行这个函数
    observer(['numberA', 'numberB'], (numberA, numberB) => {
      this.updateData({
        sum: numberA + numberB,
      })
    })
  })
  .register()
```

## 监听字段语法

数据监听器支持监听属性或内部数据的变化，可以同时监听多个。一次 setData 最多触发每个监听器一次。

同时，监听器可以监听子数据字段，如下例所示。

```
export default Component({
  observers: {
    'some.subfield': function(subfield) {
      // 使用 setData 设置 this.data.some.subfield 时触发
      // （除此以外，使用 setData 设置 this.data.some 也会触发）
      subfield === this.data.some.subfield
    },
    'arr[12]': function(arr12) {
      // 使用 setData 设置 this.data.arr[12] 时触发
      // （除此以外，使用 setData 设置 this.data.arr 也会触发）
      arr12 === this.data.arr[12]
    },
  }
})
```

```
export default Component()
  .init(function ({ observer }) {
    observer('some.subfield', (subfield) => {
      // 使用 setData 设置 this.data.some.subfield 时触发
      // （除此以外，使用 setData 设置 this.data.some 也会触发）
      subfield === this.data.some.subfield
    })
    observer('arr[12]', (arr12) => {
      // 使用 setData 设置 this.data.arr[12] 时触发
      // （除此以外，使用 setData 设置 this.data.arr 也会触发）
      arr12 === this.data.arr[12]
    })
  })
  .register()
```

如果需要监听所有子数据字段的变化，可以使用通配符 `**` 。

```
export default Component({
  observers: {
    'some.field.**': function(field) {
      // 使用 setData 设置 this.data.some.field 本身或其下任何子数据字段时触发
      // （除此以外，使用 setData 设置 this.data.some 也会触发）
      field === this.data.some.field
    },
  },
  lifetimes: {
    attached: function() {
      // 这样会触发上面的 observer
      this.setData({
        'some.field': { /* ... */ }
      })
      // 这样也会触发上面的 observer
      this.setData({
        'some.field.xxx': { /* ... */ }
      })
      // 这样还是会触发上面的 observer
      this.setData({
        'some': { /* ... */ }
      })
    },
  },
})
```

特别地，仅使用通配符 `**` 可以监听全部 setData 。

```
export default Component()
  .init(function ({ observer }) {
    observer('**', () => {
      // 每次 setData 都触发
    })
  })
  .register()
```

## 注意事项

请特别留意：数据监听器监听的是 setData **涉及到的数据字段** 而非变化的数据字段，即使这些数据字段的值没有发生变化，数据监听器依然会被触发。想要在此基础上，过滤未变化的数据字段、有更简便的语法，可以使用 [computed](https://github.com/wechat-miniprogram/computed) 扩展模块。

另外，在编写逻辑时，如果在数据监听器函数中使用 setData 设置本身监听的数据字段，可能会导致死循环，需要特别留意。

