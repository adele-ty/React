# Virtual DOM
虚拟DOM是一个抽象的DOM表示，它是一个JavaScript对象，用来描述真实DOM的结构和内容。当React中的数据发生变化时，React会使用虚拟DOM来生成新的DOM树，并使用diff算法来比较新旧DOM树之间的差异，从而只对需要更新的部分进行操作，减少了对DOM的操作次数。

# Virtual DOM 真的比操作原生 DOM 快吗？
## 1. 原生 DOM 操作 vs 通过框架封装操作。
这是一个性能 vs 可维护性的取舍。框架的意义在于为你掩盖底层的 DOM 操作，让你用更声明式的方式来描述你的目的，从而让你的代码更容易维护。没有任何框架可以比纯手动的优化 DOM 操作更快，因为框架的 DOM 操作层需要应对任何上层 API 可能产生的操作，它的实现必须是普适的。针对任何一个 benchmark，我都可以写出比任何框架更快的手动优化，但是那有什么意义呢？在构建一个实际应用的时候，你难道为每一个地方都去做手动优化吗？出于可维护性的考虑，这显然不可能。框架给你的保证是，你在不需要手动优化的情况下，我依然可以给你提供过得去的性能。

## 2. 对 React 的 Virtual DOM 的误解。
React 从来没有说过 “React 比原生操作 DOM 快”。React 的基本思维模式是每次有变动就整个重新渲染整个应用。如果没有 Virtual DOM，简单来想就是直接重置 innerHTML。很多人都没有意识到，在一个大型列表所有数据都变了的情况下，重置 innerHTML 其实是一个还算合理的操作... 真正的问题是在 “全部重新渲染” 的思维模式下，即使只有一行数据变了，它也需要重置整个 innerHTML，这时候显然就有大量的浪费。

我们可以比较一下 innerHTML vs. Virtual DOM 的重绘性能消耗：  
innerHTML: render html string O(template size) + 重新创建所有 DOM 元素 O(DOM size)  
Virtual DOM: render Virtual DOM + diff O(template size) + 必要的 DOM 更新 O(DOM change)  

Virtual DOM render + diff 显然比渲染 html 字符串要慢，但是！它依然是纯 js 层面的计算，比起后面的 DOM 操作来说，依然便宜了太多。可以看到，innerHTML 的总计算量不管是 js 计算还是 DOM 操作都是和整个界面的大小相关，但 Virtual DOM 的计算量里面，只有 js 计算和界面大小相关，DOM 操作是和数据的变动量相关的。前面说了，和 DOM 操作比起来，js 计算是极其便宜的。这才是为什么要有 Virtual DOM：它保证了 1）不管你的数据变化多少，每次重绘的性能都可以接受；2) 你依然可以用类似 innerHTML 的思路去写你的应用。

参考[深入理解React虚拟DOM](https://www.cnblogs.com/yumingxing/p/9438457.html)  
参考[从零开始，手写一个简易的Virtual DOM](https://zhuanlan.zhihu.com/p/68491595)
