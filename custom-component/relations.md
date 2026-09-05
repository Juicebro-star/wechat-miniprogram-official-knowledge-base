---
title: "组件间关系"
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
official_url: "https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/relations.html"
source_route: "/miniprogram/dev/framework/custom-component/relations.html"
raw_html: "raw-html/custom-component/relations.html"
---

> 来源：微信开放文档（官方页面）
> 原始链接：[https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/relations.html](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/relations.html)
> 抓取日期：2026-09-05
> 原始 HTML：`raw-html/custom-component/relations.html`

# 组件间关系

有时需要实现这样的一对组件：

```
<custom-ul>
  <custom-li> item 1 </custom-li>
  <custom-li> item 2 </custom-li>
</custom-ul>
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/Tu8Qzvmy8UbL)

这个例子中，`<custom-ul>` 和 `<custom-li>` 都是自定义组件，它们有相互间的关系，相互间的通信往往比较复杂。此时，可以使用 `relations` 让它们之间能相互通信。

使用 relations 时，相关联的两个组件要将对方加入 relations 中。（两个组件都需要将对方加入 relations 中，否则不会生效。）

## Definition 形式的 relations

以上述关系为例。对于 `<custom-ul>` 组件：

```
export default Component({
  relations: {
    './custom-li': { // 关联到的组件的相对路径
      type: 'child', // 关联的目标节点应为子节点
      linked: function(target) {
        // 每次有 custom-li 被插入时执行
        // target 是该 custom-li 节点实例对象，触发在该节点 attached 生命周期之后
      },
      linkChanged: function(target) {
        // 每次有 custom-li 被移动后执行
        // target 是该 custom-li 节点实例对象，触发在该节点 moved 生命周期之后
      },
      unlinked: function(target) {
        // 每次有 custom-li 被移除时执行
        // target 是该 custom-li 节点实例对象，触发在该节点 detached 生命周期之后
      },
    },
  },
  lifetimes: {
    attached() {
      // 使用 getRelationNodes 可以获得 nodes 数组，包含所有已关联的 custom-li（按节点树顺序排列）
      var nodes = this.getRelationNodes('./custom-li')
    },
  },
})
```

对于 custom-li 组件：

```
export default Component({
  relations: {
    './custom-ul': { // 关联到的组件的相对路径
      type: 'parent', // 关联的目标节点应为父节点
      linked: function(target) {
        // 每次被插入到 custom-ul 时执行
        // target 是 custom-ul 节点实例对象，触发在 attached 生命周期之后
      },
      linkChanged: function(target) {
        // 每次被移动后执行
        // target 是 custom-ul 节点实例对象，触发在 moved 生命周期之后
      },
      unlinked: function(target) {
        // 每次被移除时执行
        // target 是 custom-ul 节点实例对象，触发在 detached 生命周期之后
      },
    },
  },
})
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/FR8eDvmJ8Ubm)

## Chaining 形式的 relations 和 trait behaviors

在 Chaining 形式使用时，relations 定义在 `init` 中。虽然也可以直接指定 relations 的目标组件，但更推荐使用 [trait behaviors](../component-framework/trait-behaviors) 来进行逻辑解耦。

> trait behaviors 的组件框架支持情况请参考其文档。

首先，在其中一个组件上实现一个 trait behavior。例如，`<custom-ul>` 和 `<custom-li>` 两个组件，可以在 `<custom-li>` 组件中导出一些接口给 `<custom-ul>` 使用，那么，在 `<custom-li>` 上：

```
// common.ts

// 定义一些导出的接口
interface IListItem {
  getId(): string
}

// 包装成 trait behavior
export const listItem = Behavior.trait<IListItem>()
```

```
// custom-li.ts
import { listItem } from './common'

export default Component()
  .data(() => ({
    id: '',
  }))
  .init(function ({ data, implement, relation }) {
    // 定义关联关系
    relation({
      type: 'parent',
      target: './custom-ul',
      linked: function(parent) {
        // 每次被插入到 custom-ul 时执行
      },
      linkChanged: function(parent) {
        // 每次被移动后执行
      },
      unlinked: function(parent) {
        // 每次被移除时执行
      },
    })

    // 实现接口
    implement(listItem, {
      getId() {
        return data.id
      },
    })
  })
  .register()
```

在 `<custom-ul>` 中，定义关系时不要直接指定 target 为目标组件，而是用 `listItem` 代替：

```
import { listItem } from './common'

export default Component()
  .init(function ({ relation, lifetime }) {
    // 定义关联关系
    // relation 应在 init 函数返回前调用
    // 不应该写在生命周期函数、事件回调函数、其他方法内部
    const liRelation = relation({
      type: 'child',
      target: listItem, // 使用 trait behavior
      linked(child) {
        // 每次有 custom-li 被插入时执行
        // 此时可以访问 trait behavior 中的接口
        const li = child.traitBehavior(listItem)!
        li.getId()
      },
      linkChanged: function(child) {
        // 每次有 custom-li 被移动后执行
      },
      unlinked: function(child) {
        // 每次有 custom-li 被移除时执行
      },
    })

    lifetime('attached', () => {
      wx.nextTick(() => {
        // listAsTrait 方法可以列出来所有子节点所实现的 trait behavior
        for (let li of liRelation.listAsTrait()) {
          li.getId()
        }
      })
    })
  })
  .register()
```

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/awuhfwmX8Obu)

## relations 定义选项

每个关联定义包含目标组件路径及其对应选项，可包含的选项见下表。

| 选项 | 类型 | 是否必填 | 描述 |
| --- | --- | --- | --- |
| type | String | 是 | 目标组件的相对关系，对于父子关系应分别为 `parent`、`child`，对于祖先-后代关系，应分别为 `ancestor`、`descendant` |
| linked | Function | 否 | 关系生命周期函数，当关系被建立时触发，触发时机在组件 attached 生命周期之后 |
| linkChanged | Function | 否 | 关系生命周期函数，当关系发生改变时触发，触发时机在组件 moved 生命周期之后 |
| unlinked | Function | 否 | 关系生命周期函数，当关系被销毁时触发，触发时机在组件 detached 生命周期之后 |
| target | String | 否 | 如果这一项被设置，则它表示关联的目标节点所应具有的 trait behavior 或 behavior，所有满足要求的组件节点都会被关联 |

