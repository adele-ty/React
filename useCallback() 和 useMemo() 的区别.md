useCallback 和 useMemo 都是 React 中用于优化性能的 Hook，它们的作用类似，都是用来缓存计算结果以避免不必要的重复计算。不过，它们的应用场景有所不同。

# useCallback
useCallback 主要用于缓存函数引用，可以避免因为函数引用的变化导致子组件不必要的重新渲染。它接受两个参数：一个是回调函数，另一个是依赖项数组。当依赖项数组中的值发生变化时，useCallback 会重新创建一个新的回调函数，否则会返回之前缓存的回调函数。因此，useCallback 的主要作用是避免因为依赖项的变化而导致的重复创建回调函数的问题。当需要传递函数引用给子组件时，可以使用 useCallback 缓存函数引用，避免子组件因为函数引用的变化而不必要的重新渲染。
```jsx
import React, { useState, useCallback } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increment</button>
    </div>
  );
}
```

# useMemo
useMemo 主要用于缓存计算结果，可以避免因为计算的重复执行而导致的性能问题。它接受两个参数：一个是计算函数，另一个是依赖项数组。当依赖项数组中的值发生变化时，useMemo 会重新计算计算函数的返回值，否则会返回之前缓存的结果。因此，useMemo 的主要作用是避免因为重复计算而导致的性能问题。当需要进行复杂的计算时，可以使用 useMemo 缓存计算结果，避免重复计算的性能问题。

```jsx
import React, { useMemo } from 'react';

function MyComponent({ a, b }) {
  const result = useMemo(() => {
    console.log('Calculating result...');
    return a + b;
  }, [a, b]);

  return (
    <div>
      <p>Result: {result}</p>
    </div>
  );
}
```
