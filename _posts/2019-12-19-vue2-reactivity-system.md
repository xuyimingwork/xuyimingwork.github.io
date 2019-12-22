---
title: 'vue 响应式原理'
date: 2019-12-19T20:39:30
categories:
  - 前端
tags:
  - vue
  - vue2
  - 源码
  - 响应式
---

## 角色与关键点

从小片段开始

```js
let price = 5
let quentity = 2
let total = price * quentity
```

在上述的代码片段中，有两种角色：变量、计算过程

`price`、`quentity`、`total` 是变量，`total = price * quentity` 是计算过程

在当前情况下，更新 `price` 或 `quentity`，并不会导致 `total`更新 —— 除非，重复执行计算过程。

即：在检测到 `price` 或 `quentity` 变化时，重新执行 `total = price * quentity`

此处有几个关键点：

- 检测 `price` 与 `quentity` 的变化
- 由于 `price` 与 `quentity` 的变化后需要重新执行计算过程，因此需要将计算过程 `total = price * quentity` 保存到某个地方，以便后续重新执行
- 将变量与计算过程关联，即程序需要明确在 `price` 变化后执行对应的 `total = price * quentity` 计算过程

## 原理与思路

调整下上面的代码，将计算过程保存到函数 `cb` 中

```js
let price = 5
let quentity = 2
let total = 0
function cb() {
  total = price * quentity
}
cb()
```

现在，在 `price` 或 `quentity` 更新后，手动执行 `cb`，即可得到更新后的 `total`。我们需要这个过程自动执行，就需要解决上述中的关键点。此外，我们还可以应用观察者模式让代码结构更加清晰。

先捋下角色以便使用观察者模式。`price` 或 `total` 这类变量变化后需要通知 `cb` 这一计算过程执行，因此，`price` 与 `total` 是观察对象，`cb` 是观察者。又有，`cb` 的执行依赖于 `price` 和 `total`，因此，可以说 `price` 与 `total` 是 `cb` 的依赖。

> 场外：观察者模式中，`cb` 是 `price` 和 `total` 的观察者或者说订阅者（subscriber）；在 `cb` 执行过程中，`price` 和 `total` 又是 `cb` 的依赖（dependency），这些角色体现在了 vue 源码的变量命名中，因此需要搞清角色间的关系

由上所述，对应每个变量，建立一个 `Dep` 类；对应每个计算过程，建立 `Watcher` 类。`Dep` 类是观察对象，实现观察对象相关接口，`Watcher` 是观察者，实现观察者相关接口。

再来看前面提到的关键点，对于检测 `price` 与 `quentity` 的变化，我们可以将 `price` 与 `quentity` 作为某个对象（比如 `data`）的属性，通过 `defineProperty` 属性描述符的 `set` 定义 `price` 与 `quentity` 属性的 `setter` 方法来实现。`setter` 方法会在每次给属性赋值时被调用，当检测到赋值发生了变化，调用属性对应的 `Dep` 实例的 `notify` 方法，通知相关 `Watcher` 进行 `update`（此处的 `notify` 与 `update` 是观察者模式相关接口）

对于将计算过程 `cb` 保存到某个地方，目前很明确，就是保存在 `Watcher` 中。

> 场外：`Dep` 作为观察对象，内部需要维护一个订阅者（观察者）列表 —— `subs`，`subs` 中的每一项都是 `Watcher`，这样，当变化发生，调用 `Dep` 的 `notify` 方法，在 `notify` 中遍历 `subs`，调用每个 `Watcher` 的 `update` 方法。而 `update` 具体要做的事就是重新执行一遍 `cb`

对于如何将变量与计算过程关联，即知晓某一计算过程依赖哪些变量。我们再来看下 `cb` 这一计算过程：

**`total = price * quentity`**

在 `cb` 这一计算过程中，它 **读取了** `price` 和 `quentity` 的值，然后将其相乘并 **赋值到** `total` 上。上面我们说 `price` 和 `quentity` 是 `cb` 的依赖，从这里我们又发现，`cb` 在执行过程中会 **读取** 它依赖的变量的值。注意这里 `total` 的重新赋值是 `cb` 产生的影响，`cb` 的计算只依赖于 `price` 和 `quentity`

回想我们要的响应式系统，在首次执行某个计算过程后，若后续该计算过程中的依赖变量发生变化，重复执行该计算过程。结合上述，如果我们在首次执行 `cb` 时，能 **代理** `cb` 对 `price` 和 `quentity` 的 **读取**，即可建立变量与计算过程的关联。

而 **代理** `cb` 对 `price` 和 `quentity` 的 **读取**，可以通过 `defineProperty` 属性描述符的 `get` 定义 `price` 与 `quentity` 属性的 `getter` 方法来实现，即在 `getter` 中建立变量与计算过程的关联。

从上面的观察者模式来说，`cb` 存储在 `Watcher` 中，`price` 和 `quentity` 各自对应一个 `Dep`，是观察对象，这里的建立联系就是将 `Watcher` 添加到 `Dep` 的订阅者列表中。

> 场外：这里还可以再绕一层，`Dep` 对应的变量是 `Watcher` 对应的计算过程的依赖，建立联系的过程就是给 `Watcher` 添加依赖。至于如何给 `Watcher` 添加依赖，依据观察者模式，仍是将 `Watcher` 添加到 `Dep` 的订阅者（观察者）列表（`subs`）中。

## 梳理总结

到这里，实际上已经完成了 vue 的响应式原理。我们梳理下过程：

- 初始化变量，即设置变量的 `getter` 与 `setter`
  - 在 `getter` 中建立变量与当前执行的计算过程的关联
  - 在 `setter` 中通知订阅者更新
- 首次执行计算过程，触发变量的 `getter` 进行依赖搜集
- 等待变量重新赋值，触发计算过程再次执行

## 代码实现

将变量包裹到 data 中

```js
const data = {
  price: 5,
  quentity: 2
}
let total = 0
function cb() {
  total = data.price * data.quentity
}
cb()
```

定义 `Dep`

```js
class Dep {
  static target

  constructor() {
    this.subs = []
  }

  addSub(watcher) {
    this.subs.push(watcher)
  }

  notify() {
    this.subs.forEach(watcher => watcher.updata())
  }
}

Dep.target = null
```

定义 `Watcher`

```js
class Watcher {
  constructor(cb) {
    this.cb = cb
    this.get()
  }

  get() {
    Dep.target = this
    this.cb()
    Dep.target = null
  }

  updata() {
    this.cb()
  }
}
```

响应式方法

```js
function walk(data) {
  Object.keys(data).forEach(function defineReactive(key) {
    const dep = new Dep()
    let value = data[key]
    Object.defineProperty(data, key, {
      get: function() {
        if (Dep.target) dep.addSub(Dep.target)
        return value
      },
      set: function(newValue) {
        if (value === newValue) return
        value = newValue
        dep.notify()
      }
    })
  })
}
```

使用

```js
const data = {
  price: 5,
  quentity: 2
}
let total = 0
function cb() {
  total = data.price * data.quentity
}

walk(data)
new Watcher(cb)
```

![](/assets/images/WX20191222-222555@2x.png)

## 再总结一波

vue 组件实际的页面是通过 `render` 函数渲染出来的，而首次执行 `render` 函数的过程，就是上面 `Watcher` 收集变量依赖的过程，之后，当变量出现变化，就会触发依赖该变量的方法重新执行，最终导致页面重新渲染。

![](/assets/images/data.png)

而在 vue 生命周期的 `beforeCreate` 和 `Created` 之间的初始化操作，就是对变量进行响应式处理，也就是前面的 `walk` 操作。

![](/assets/images/lifecycle.png)

## 彩蛋

把 `walk` 与 `new Watcher(cb)` 的动作放到一个新的类中，如 `MiniVue`：

```js
class MiniVue {
  constructor({ data, methods }) {
    this.walk(data)
    this.firstRun(methods)
  }

  firstRun(methods) {
    Object.keys(methods).forEach(key => new Watcher(methods[key]))
  }

  walk(data) {
    Object.keys(data).forEach(function defineReactive(key) {
      const dep = new Dep()
      let value = data[key]
      Object.defineProperty(data, key, {
        get: function() {
          if (Dep.target) dep.addSub(Dep.target)
          return value
        },
        set: function(newValue) {
          if (value === newValue) return
          value = newValue
          dep.notify()
        }
      })
    })
  }
}
```

然后就可以：

```js
const data = {
  price: 5,
  quentity: 2,
  total: 0
}

new MiniVue({
  data: data,
  methods: {
    calculateTotal() {
      console.log('calculate total')
      data.total = data.price * data.quentity
    },
    logTotal() {
      console.log('log total ', data.total)
    }
  }
})
```

![](/assets/images/WX20191222-223414@2x.png)

> 注意：这里对 methods 的操作只是仿了个 vue 初始化的样子，实际环境中的 vue **并不是** 这样处理 methods 的。

参见：

- [Build a Reactivity System](https://www.vuemastery.com/courses/advanced-components/build-a-reactivity-system)

> 场外：当我意识到 vue 响应式的实现使用了观察者模式，且变量为观察对象，计算过程为观察者后，我便很快理解了这部分代码，希望本文对你理解 vue 有所帮助。
