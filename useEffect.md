# 副作用
副作用指的是在组件渲染期间发生的任何操作，这些操作可以改变组件以外的状态，例如：  
* 发送网络请求：在组件渲染期间，可能需要发送网络请求来获取数据。  
* 访问本地存储：在组件渲染期间，可能需要访问本地存储来读取或写入数据。  
* 改变DOM：在组件渲染期间，可能需要改变DOM，例如添加或删除元素、更改元素的属性等。  
* 订阅事件：在组件渲染期间，可能需要订阅事件，例如窗口大小改变、鼠标移动等。  
副作用可能会导致应用程序的状态变得不可预测，因此需要在组件渲染完成后进行清理。React提供了一些钩子函数，例如useEffect和useLayoutEffect，用于处理副作用。
# useEffect
Effect Hook 可以让你在函数组件中执行副作用操作。可以把它看做 componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合。
```c
useEffect(() => {
  // 副作用操作
  return () => {
    // 清理操作
  }
}, [/* 依赖项数组 */]);
```
* 默认情况下，它在第一次渲染之后和每次更新之后都会执行（不传递第二个参数）。  
* 如果想执行只运行一次的 effect（仅在组件挂载和卸载时执行），可以传递一个空依赖项数组（[]）作为第二个参数。  
* 在依赖项数组中，我们可以传入需要监控的状态或变量，当依赖项发生变化时，useEffect函数会重新执行。  
* 如果你的 effect 返回一个函数，React 将会在组件卸载的时候执行清除操作调用它。  
# useLayoutEffect
其函数签名与 useEffect 相同，但它会在所有的 DOM 变更之后同步调用 effect。  

与useEffect的区别在于，useLayoutEffect会在组件渲染前同步执行，而useEffect是在组件渲染后异步执行。这意味着，如果我们需要进行一些DOM操作，例如计算元素尺寸、调整布局等，使用useLayoutEffect可以确保操作在组件渲染前完成，避免用户看到闪烁或者布局异常的情况。

需要注意的是，由于useLayoutEffect会在组件渲染前同步执行，因此如果useLayoutEffect的操作过于耗时，会导致页面卡顿或者出现其他问题。在使用useLayoutEffect时，应该尽量避免进行复杂的操作，以确保页面性能和用户体验。如果我们只需要在组件渲染后进行一些副作用操作，可以使用useEffect来替代useLayoutEffect。

详细请移步[官网](https://react.zcopy.site/docs/hooks-effect.html)