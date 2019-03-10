[office-link](https://facebook.github.io/react/docs/installation.html)
[ryf-link](http://www.ruanyifeng.com/blog/2016/09/react-technology-stack.html)

React 应用都是构建在组件之上
props 是组件包含的两个核心概念之一，另一个是 state
props是属性传递，只读不写（试一试写props什么后果）
当组件状态 state 有更改的时候，React 会自动调用组件的 render 方法重新渲染整个组件的 UI

当然如果真的这样大面积的操作 DOM，性能会是一个很大的问题，所以 React 实现了一个Virtual DOM，组件 DOM 结构就是映射到这个 Virtual DOM 上，React 在这个 Virtual DOM 上实现了一个 diff 算法，当要重新渲染组件的时候，会通过 diff 寻找到要变更的 DOM 节点，再把这个修改更新到浏览器实际的 DOM 节点上，所以实际上不是真的渲染整个 DOM 树。这个 Virtual DOM 是一个纯粹的 JS 数据结构，所以性能会比原生 DOM 快很多。


如果在 JSX 中使用的属性不存在于 HTML 的规范中，这个属性会被忽略。如果要使用自定义属性，可以用 data- 前缀。
可访问性属性的前缀 aria- 也是支持的。

除了前面提到的 class 要写成 className，比较典型的还有:
style 属性接受由 CSS 属性构成的 JS 对象
onChange 事件表现更接近我们的直觉（不需要 onBlur 去触发）
表单的表现差异比较大，要单独再讲

组件生成的 HTML 结构只能有一个单一的根节点。

props 就是组件的属性，由外部通过 JSX 属性传入设置，一旦初始设置完成，就可以认为 this.props 是不可更改的，所以不要轻易更改设置 this.props 里面的值（虽然对于一个 JS 对象你可以做任何事）。

state 是组件的当前状态，可以把组件简单看成一个“状态机”，根据状态 state 呈现不同的 UI 展示。
一旦状态（数据）更改，组件就会自动调用 render 重新渲染 UI，这个更改的动作会通过 this.setState 方法来触发

一条原则：让组件尽可能地少状态(这样组件逻辑就越容易维护。)


你也可以用纯粹的函数来定义无状态的组件(stateless function)，这种组件没有状态，没有生命周期，只是简单的接受 props 渲染生成 DOM 结构
无状态组件非常简单，开销很低，如果可能的话尽量使用无状态组件。比如使用箭头函数定义：

```js
const HelloMessage = (props) => <div> Hello {props.name}</div>;
render(<HelloMessage name="John" />, mountNode);
```

因为无状态组件只是函数，所以它没有实例返回，这点在想用 refs 获取无状态组件的时候要注意

一般来说，一个组件类由 extends Component 创建，并且提供一个 render 方法以及其他可选的生命周期函数、组件相关的事件或方法来定义

getInitialState
  初始化 this.state 的值，只在组件装载之前调用一次
getDefaultProps
  只在组件创建时调用一次并缓存返回的对象（即在 React.createClass 之后就会调用）
render-必须
  组装生成这个组件的 HTML 结构（使用原生 HTML 标签或者子组件），也可以返回 null 或者 false，这时候 ReactDOM.findDOMNode(this) 会返回 null。

componentWillMount
  只会在装载之前调用一次，在 render 之前调用，你可以在这个方法里面调用 setState 改变状态，并且不会导致额外调用一次 render
componentDidMount

更新组件触发，不会在首次 render 组件的周期调用
componentWillReceiveProps
shouldComponentUpdate
componentWillUpdate
componentDidUpdate

componentWillUnmount

显式调用 bind(this) 将事件函数上下文绑定要组件实例上

React 实现了一个“合成事件”层（synthetic event system），这个事件模型保证了和 W3C 标准保持一致，所以不用担心有什么诡异的用法，并且这个事件层消除了 IE 与 W3C 标准实现之间的兼容问题。

“合成事件”会以事件委托（event delegation）的方式绑定到组件最上层，并且在组件卸载（unmount）的时候自动销毁绑定的事件。

e.nativeEvent.stopImmediatePropagation() ------- 原生事件阻止冒泡

”合成事件“ 的 event 对象只在当前 event loop 有效，比如你想在事件里面调用一个 promise，在 resolve 之后去拿 event 对象会拿不到（并且没有错误抛出）：
```js
handleClick(e) {
  promise.then(() => doSomethingWith(e));
}
```

[支持事件列表](https://facebook.github.io/react/docs/events.html)

所有通过 JSX 这种方式绑定的事件都是绑定到“合成事件”，除非你有特别的理由，建议总是用 React 的方式处理事件
在 componentDidMount 方法里面通过 addEventListener 绑定的事件就是浏览器原生事件
使用原生事件的时候注意在 componentWillUnmount 解除绑定 removeEventListener

大部分情况下你不需要通过查询 DOM 元素去更新组件的 UI，你只要关注设置组件的状态（setState）

ReactDOM.render 返回对组件的引用也就是组件实例（对于无状态状态组件来说返回 null）

当组件加载到页面上之后（mounted），你都可以通过 react-dom 提供的 findDOMNode() 方法拿到组件对应的 DOM 元素
另外一种方式就是通过在要引用的 DOM 元素上面设置一个 ref 属性指定一个名称，然后通过 this.refs.name 来访问对应的 DOM 元素。
如果 ref 是设置在原生 HTML 元素上，它拿到的就是 DOM 元素，如果设置在自定义组件上，它拿到的就是组件实例，这时候就需要通过 findDOMNode 来拿到组件的 DOM 元素

你可以使用 ref 到的组件定义的任何公共方法，比如 this.refs.myTypeahead.reset()
Refs 是访问到组件内部 DOM 节点唯一可靠的方法
Refs 会自动销毁对子组件的引用（当子组件删除时）

props.children 通常是一个组件对象的数组，但是当只有一个子元素的时候，props.children 将是这个唯一的子元素，而不是数组了。

Action -> Dispatcher -> Store -> View

整个流程如下：
首先要有 action，通过定义一些 action creator 方法根据需要创建 Action 提供给 dispatcher
View 层通过用户交互（比如 onClick）会触发 Action
Dispatcher 会分发触发的 Action 给所有注册的 Store 的回调函数
Store 回调函数根据接收的 Action 更新自身数据之后会触发一个 change 事件通知 View 数据更改了
View 会监听这个 change 事件，拿到对应的新数据并调用 setState 更新组件 UI

一个应用只需要一个 dispatcher 作为分发中心，管理所有数据流向，分发动作给 Store，没有太多其他的逻辑（一些 action creator 方法也可以放到这里）。

整个应用只有唯一一个可信数据源，也就是只有一个 Store
State 只能通过触发 Action 来更改
State 的更改必须写成纯函数，也就是每次更改总是返回一个新的 State，在 Redux 里这种函数称为 Reducer

在 HTML 中 `<textarea>` 的值可以由子节点（文本）赋值，但是在 React 中，要用 value 来设置
表单元素包含以上任意一种状态属性都支持 onChange 事件监听状态值的更改

一个受控的表单组件，它所有状态属性更改涉及 UI 的变更都由 React 来控制（状态属性绑定 UI）

你可以通过传递一个数组指定多个选中项：<select multiple={true} value={['B', 'C']}>
