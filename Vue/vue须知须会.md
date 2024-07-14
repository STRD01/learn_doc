# vue须知须会
这些不是面试题，是工作中排查问题的思路，是必须要掌握的框架底层逻辑。

## 生命周期

Vue.js 是一个[渐进式 JavaScript 框架](/工程化/不同框架特性.md)，用于构建用户界面。Vue 的生命周期提供了8个钩子函数在特定阶段运行代码。

1. beforeCreate：实例初始化之后，数据观测和事件配置之前被调用。在这个阶段，还无法访问 data 和 methods。
2. created：实例创建完成后被调用。在这个阶段，实例已完成数据观测、属性和方法的初始化，但尚未挂载 DOM。
3. beforeMount：在挂载开始之前被调用。相关的 render 函数首次被调用。
4. mounted：实例被挂载后调用，el 被新创建的 vm.$el 替换。这个钩子在 DOM 挂载后调用，可以进行 DOM 操作。
5. beforeUpdate：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。可以在这个钩子中访问现有的 DOM。
6. updated：由于数据更改导致的虚拟 DOM 重新渲染和打补丁之后调用。此时 DOM 已更新。
7. beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。vue3中重新命名为beforeUnmount。
8. destroyed：实例销毁之后调用。调用后，Vue 实例的所有指令解绑，所有事件监听器移除，所有子实例被销毁。vue3中重新命名为unmounted。
9. activated：被` <keep-alive>` 缓存的组件激活时调用。
10. deactivated：被` <keep-alive>` 缓存的组件停用时调用。

- setup:  Vue 3 中 Composition API 的新钩子，用于在组件实例化之前执行代码。

### Vue 2 和 Vue 3 生命周期区别
1. 新增的 setup 函数：Vue 3 引入了 Composition API 和 setup 函数，用于在组件实例化之前执行逻辑。
2. 生命周期钩子重命名：Vue 3 将 beforeDestroy 重命名为 beforeUnmount，将 destroyed 重命名为 unmounted。
3. 新增Composition API: Vue 3 引入了更灵活的 Composition API，使得可以在 setup 函数中使用生命周期钩子，而不是仅限于 Options API。

### `<keep-alive>` 组件的生命周期

[`<keep-alive>` 是 Vue 内置的一个抽象组件](/Vue/keep-alive.md)，用于缓存状态在组件切换时保持不变。当组件被` <keep-alive>` 包裹时，它会有两个额外的生命周期钩子：

- activated：当被 `<keep-alive>` 缓存的组件激活时调用。这是组件从非活动状态变为活动状态时的钩子。适用于需要在组件显示时执行某些操作的场景。
- deactivated：当被 `<keep-alive>` 缓存的组件停用时调用。这是组件从活动状态变为非活动状态时的钩子。适用于需要在组件隐藏时执行某些操作的场景。

## 响应式系统原理

### Vue 2 的响应式系统

Vue 2 的响应式系统基于 Object.defineProperty，通过劫持对象属性的 getter 和 setter 实现数据变化的检测。

1. 数据劫持：当一个 Vue 实例被创建时，Vue 会遍历数据对象的每个属性，并使用 Object.defineProperty 将它们转换为 getter 和 setter。
2. 依赖收集：每个属性的 getter 中，会记录哪些组件依赖了这个属性（收集依赖）。
3. 派发更新：当属性的 setter 被调用时，通知所有依赖这个属性的组件进行更新。

#### 核心概念

1. Observer：递归遍历对象的属性，将它们转换为 getter 和 setter。
2. Dep（Dependency）：依赖管理器，收集依赖，并在数据变化时通知依赖进行更新。
3. Watcher：具体的依赖项，负责更新视图或执行回调函数。

#### 优缺点

- 优点：
  - 对大部分情况来说，性能表现不错，简单易懂。

- 缺点：
  - 无法检测数组索引和对象属性的添加或删除，性能和内存占用较高。

### Vue 3 的响应式系统

Vue 3 的响应式系统基于 ES6 的 [Proxy](/JavaScript/Proxy.md) 对象，提供了更强大和灵活的能力。

1. Proxy：Vue 3 使用 Proxy 来劫持对象，对对象的任何操作都会触发代理的回调函数，从而实现对属性的完全监控。
2. Reflect：配合 Proxy 使用，帮助实现默认行为。

#### 核心概念

1. Reactive：创建一个响应式对象，用 Proxy 包装原始对象。
2. Ref：用来创建一个响应式的单值，适合处理基础类型。
3. Effect：收集依赖并在响应式数据变化时重新执行。

#### 优缺点

- 优点：
  - 能够监听对对象和数组的所有操作，包括属性的添加、删除、数组索引的改变等，性能更好，内存占用更低。
  - 避免了递归遍历对象的初始化开销，对于复杂和嵌套的对象结构，性能更好。
  - 无需针对每个属性定义 getter 和 setter，代码更简洁，维护更容易。

- 缺点：
  - 在不支持 Proxy 的环境下（如 IE11），需要 polyfill 支持。

### Vue 2 和 Vue 3 的比较

- Vue 2：使用 Object.defineProperty 实现响应式系统，适用于大部分应用，但在处理复杂数据结构和大规模数据时存在性能和内存问题。
- Vue 3：使用 Proxy 实现响应式系统，解决了 Vue 2 的性能和内存问题，并提供了更强大的 API，使得处理复杂数据结构更加便捷。

- 响应式实现方式
  - Vue 2：使用 Object.defineProperty，只能劫持对象已有的属性。
  - Vue 3：使用 Proxy，能够劫持所有操作，包括属性的添加、删除和数组索引的变化。
- 性能和内存
  - Vue 2：对于大规模数据和复杂结构，性能和内存占用较高。
  - Vue 3：在性能和内存方面有显著提升，尤其是在处理嵌套对象和数组时，因为 Proxy 能够更高效地劫持操作。
- API 易用性
  - Vue 2：API 相对简单，但在处理复杂数据结构时需要更多的手动工作（如 $set、$delete）。
  - Vue 3：提供了更强大的响应式 API，如 reactive 和 ref，使得处理复杂数据结构更为方便。

## 虚拟dom


## Diff算法


## 渲染器设计原理


## 组件的实现原理


## 编译器的技术原理


## 性能优化