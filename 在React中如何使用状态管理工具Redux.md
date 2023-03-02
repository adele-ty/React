Redux 是一种状态管理工具，它可以帮助 React 应用程序更好地管理和共享状态。使用 Redux，您可以将应用程序的状态集中存储在单个存储库（Store）中，并通过派发（Dispatch）操作来更新该存储库中的状态。  

以下是在 React 应用程序中使用 Redux 的基本步骤：
### 创建Store
作用：Store 存储了整个应用程序的状态。应用程序中的所有组件都可以访问 Store 中的数据，以便获取和修改应用程序状态。Store 中的状态数据是只读的，应用程序中的所有状态修改都必须通过派发（Dispatch）一个 action 来触发。每当状态发生变化时，Store 都会通知订阅它的组件进行更新。  

Redux store 是使用 Redux Toolkit 中的 configureStore 函数创建的。configureStore 要求我们传入一个 reducer 参数。应用程序可能由许多不同的特性组成，每个特性都可能有自己的 reducer 函数。当调用configureStore 时，我们可以传入一个对象中的所有不同的 reducer。对象中的键名 key 将定义最终状态树中的键名 key。  

示例代码如下：  
```
// store.js

import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export default configureStore({
  reducer: {
    counter: counterReducer
  }
})
```
当我们传入一个像 {counter: counterReducer} 这样的对象时，它表示我们希望在 Redux 状态对象中有一个 state.counter 部分，并且我们希望每当 dispatch action 时 counterReducer 函数负责决定是否以及如何更新 state.counter 部分。  
### 创建Reducer  
作用：根据操作类型更新状态  
Reducer必须符合以下规则：  
* 仅使用 state 和 action 参数计算新的状态值
* 禁止直接修改 state。必须通过复制现有的 state 并对复制的值进行更改的方式来做不可变更新（immutable updates）。这是因为 Redux 要求状态是不可变的。如果 Reducer 修改了原始状态对象，那么应用程序的状态将变得不可预测。
* 禁止任何异步逻辑、依赖随机值或导致其他“副作用”的代码  
### 创建Action  
作用：描述了应用程序中发生的事件。当应用程序中的某个组件需要更新状态时，它会派发一个 action。当一个 action 被派发时，Redux 会将它传递给 Store 中的 reducer 函数。Reducer 根据操作类型更新状态，并返回一个新的状态对象。然后，Redux 将新的状态存储到 Store 中，并通知所有订阅它的组件进行更新。  
示例代码如下：  
```
// features/counter/counterSlice.js

import { createSlice } from '@reduxjs/toolkit'

export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    increment: state => {
      state.value += 1
    },
    decrement: state => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})

export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```
createSlice 函数负责生成 action 类型字符串、action creator 函数和 action 对象的工作。name 选项的字符串用作每个 action 类型的第一部分，每个 reducer 函数的键名用作第二部分。因此，"counter" 名称 + "increment" reducer 函数生成了一个 action 类型 {type: "counter/increment"}。
### 用Thunk编写异步逻辑
Thunk 是一种特定类型的 Redux 函数，可以包含异步逻辑。Thunk 是使用两个函数编写的：
* 一个内部 thunk 函数，它以 dispatch 和 getState 作为参数。
* 外部创建者函数，它创建并返回 thunk 函数。
### 在组件中订阅Store
```
import App from './App'
import store from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```
用 Provider 包裹整个根组件 App，并传入 store。任何调用 useSelector 或 useDispatch 的 React 组件都可以访问 Provider 中的 store。
### 在组件中使用State
* 使用 useSelector 提取数据
useSelector 这个 hooks 让我们的组件从 Redux 的 store 状态树中提取它需要的任何数据。我们可以编写“selector” 函数，它以 state 作为参数并返回状态树的一部分。每当一个 action 被 dispatch 并且 Redux store 被更新时，useSelector 将重新运行我们的选择器函数。如果选择器返回的值与上次不同，useSelector 将确保我们的组件使用新值重新渲染。
```
// 返回counter值
export const selectCount = state => state.counter.value

// 取得counter值
const count = useSelector(selectCount)
```
* 使用 useDispatch 来 dispatch action
```
const dispatch = useDispatch()

<button
  className={styles.button}
  aria-label="Increment value"
  onClick={() => dispatch(increment())}
>
  +
</button>
```
详细请查看[Redux官方文档](https://cn.redux.js.org/tutorials/essentials/part-2-app-structure#%E5%BA%94%E7%94%A8%E7%9B%AE%E5%BD%95)