### 看 redux 官方文档后记录的笔记

应用中所有的 state 都以一个对象树的形式储存在一个单一的 store 中  
惟一改变 state 的办法是触发 action「一个描述发生什么的对象」  
为了描述 action 如何改变 state 树，你需要编写 reducers

你应该把要做的修改变成一个普通对象，这个对象被叫做 action，而不是直接修改 state  
然后编写专门的函数来决定每个 action 如何改变应用的 state，这个函数被叫做 reducer

随着 JavaScript 单页应用开发日趋复杂，JavaScript 需要管理比任何时候都要多的 state （状态）  
这些 state 可能包括「服务器响应、缓存数据、本地生成尚未持久化到服务器的数据」  
也包括 「UI 状态」，如激活的路由，被选中的标签，是否显示加载动效或者分页器…

强制使用 action 来描述所有变化带来的好处是可以清晰地知道应用中到底发生了什么，如果一些东西改变了，就知道为什么改变

我们约定，action 内必须使用一个字符串类型的 type 字段来表示将要执行的动作  
多数情况下，type 会被定义成字符串常量，当应用规模越来越大时，建议使用单独的模块或文件来存放 action

强调一下 Redux 应用只有一个单一的 store，当需要拆分数据处理逻辑时，应该使用 reducer 组合 而不是创建多个 store

createStore() 的第二个参数是可选的, 用于设置 state 初始状态

可以在任何地方调用 store.dispatch(action)，包括组件中、XHR 回调中、甚至定时器中

强调一下：Redux 和 React 之间没有关系。Redux 支持 React、Angular、Ember、jQuery 甚至纯 JavaScript

容器组件就是使用 store.subscribe() 从 Redux state 树中读取部分数据，并通过 props 来把这些数据提供给要渲染的组件  
可以手工来开发容器组件，但建议使用 React Redux 库的 connect() 方法来生成，这个方法做了性能优化来避免很多不必要的重复渲染