1. React 将新的状态对象合并到组件的状态中。  
2. React 会调用 shouldComponentUpdate 方法来检查组件是否需要重新渲染。如果该方法返回 false，则表示组件不需要重新渲染，React 将跳过下面的步骤。  
3. React 会生成新的虚拟 DOM 树，并通过比较新旧虚拟 DOM 树的差异，计算出需要更新的部分。  
4. React 会调用 render 方法将需要更新的部分更新到真实的 DOM 树上。  
5. React 会调用 componentDidUpdate 方法来通知组件更新完成。在该方法中，可以进行一些后续操作，例如更新组件的一些外部状态。  
