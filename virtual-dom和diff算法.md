# Virtual DOM
虚拟DOM是一个抽象的DOM表示，它是一个JavaScript对象，用来描述真实DOM的结构和内容。当React中的数据发生变化时，React会使用虚拟DOM来生成新的DOM树，并使用diff算法来比较新旧DOM树之间的差异，从而只对需要更新的部分进行操作，减少了对DOM的操作次数，避免对整个DOM树进行操作，提高了性能。
# diff
### 对比不同的元素
当根节点为不同类型的元素时，React 会拆卸原有的树并且建立起新的树。
### 对比同一类型的DOM元素
当对比两个相同类型的 React 元素时，React 会保留 DOM 节点，仅比对及更新有改变的属性。
### 对比同类型的组件元素
当一个组件更新时，组件实例会保持不变，因此可以在不同的渲染时保持 state 一致。React 将更新该组件实例的 props 以保证与最新的元素保持一致，并且调用该实例的相关生命周期钩子函数。
### 对子节点进行递归
默认情况下，当递归 DOM 节点的子元素时，React 会同时遍历两个子元素的列表。

查阅[React官网](https://react.zcopy.site/docs/reconciliation.html)
