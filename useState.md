# 思考：setState为什么是异步的？
只有在react控制下才会存在批处理，setState才会有“异步”效果。React的setState本身并不是异步的，是因为其批处理机制给人一种异步的假象。setTimeout和原生事件会脱离react的控制。
## React的更新机制
生命周期函数和合成事件中：  
1. 无论调用多少次setState，都不会立即执行更新。而是将要更新的state存入'_pendingStateQuene',将要更新的组件存入'dirtyComponent';  
2. 当根组件didMount后，批处理机制更新为false。此时再取出'_pendingStateQuene'和'dirtyComponent'中的state和组件进行合并更新；  

原生事件和异步代码中：  
1. 原生事件不会触发react的批处理机制，因而调用setState会直接更新；  
2. 异步代码中调用setState，由于js的异步处理机制，异步代码会暂存，等待同步代码执行完毕再执行，此时react的批处理机制已经结束，因而直接更新。  

## 总结：
* react会表现出同步和异步的现象，但本质上是同步的，是其批处理机制造成了一种异步的假象。（其实完全可以在开发过程中，在合成事件和生命周期函数里，完全可以将其视为异步）  
* 在React中， 如果是由React引发的事件处理（比如通过onClick引发的事件处理），调用setState不会同步更新this.state，除此之外的setState调用会同步执行this.state 。所谓“除此之外”，指的是绕过React通过addEventListener直接添加的事件处理函数，还有通过setTimeout/setInterval产生的异步调用。  
* setState的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形式了所谓的“异步”，当然可以通过第二个参数 setState(partialState, callback) 中的callback拿到更新后的结果。

参考[setState异步](https://github.com/sisterAn/blog/issues/26)  
参考[官网useState](https://react.zcopy.site/docs/hooks-state.html)
