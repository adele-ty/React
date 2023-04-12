# useRef
当我们需要在函数组件中保存一个可变的值，并且希望该值在组件重新渲染时保持不变，就可以使用React中的useRef Hook。useRef返回一个引用对象，可以用来存储任意可变值，并且在组件重新渲染时保持不变。下面是一个使用useRef的例子：
```c
import React, { useRef } from 'react';

function Example() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <div>
      <input type="text" ref={inputRef} />
      <button onClick={handleClick}>Focus</button>
    </div>
  );
}
```
在上面的代码中，我们使用useRef创建了一个引用对象inputRef，并将其传递给input元素的ref属性。当按钮被点击时，我们调用inputRef.current.focus()来将输入框聚焦。由于inputRef.current在整个组件的生命周期内保持不变，所以在组件重新渲染时，输入框的焦点不会丢失。
# forwardRef
forwardRef可以将父组件中的ref对象传递给子组件，从而让子组件可以访问父组件中的DOM元素或组件实例。下面是一个使用forwardRef的例子：
```c
import React, { forwardRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  return <input type="text" ref={ref} />;
});

function Example() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <div>
      <MyInput ref={inputRef} />
      <button onClick={handleClick}>Focus</button>
    </div>
  );
}
```
在上面的代码中，我们创建了一个名为MyInput的子组件，并使用forwardRef将其包装。在Example组件中，我们创建了一个引用对象inputRef，并将其传递给MyInput组件的ref属性。当按钮被点击时，我们调用inputRef.current.focus()来将输入框聚焦。由于MyInput组件使用了forwardRef包装，并将父组件传递进来的ref对象传递给了输入框，所以我们可以在父组件中通过inputRef.current来访问输入框的DOM元素。

参考[useRef/forwardRef](https://juejin.cn/post/6889824743889829902)