```js
https://mp.weixin.qq.com/s/2DZrMMRX9kwPs6c05QZgbg

1 react如何工作

当 React 应用程序运行时,  它会在内存中创建用户界面的虚拟表示,  称为虚拟 DOM. Virtual DOM 是一个轻量级 JavaScript 对象,  包含实际 DOM 元素的所有属性. 
当 React 组件的 state 或 props 发生变化时,  React 会创建一个新的 VDOM 树. 
VDOM 与 React 的diff算法相结合,  计算新的和旧的 VDOM 表示之间的差异. 
它仅更新实际 DOM 中已更改的部分,  从而最大限度地减少整页刷新的需要并提高性能. 

2  Shadow DOM 和 Virtual DOM 有什么区别
虚拟 DOM: 它是库在内存中保存的实际 DOM(文档对象模型)的轻量级副本. 当对虚拟 DOM 进行更改时,  库会计算更新实际 DOM 的最有效方法,  并且仅进行这些特定更改,  而不是重新渲染整个 DOM. 

影子 DOM(Shadow DOM)允许你将一个 DOM 树附加到一个元素上,  并且使该树的内部对于在页面中运行的 JavaScript 和 CSS 是隐藏的. 

具体例子:  https://developer.mozilla.org/zh-CN/docs/Web/API/Web_components/Using_shadow_DOM

3 元素和组件有什么区别?
React 中的 Element 是一个普通对象,  描述组件实例或 DOM 节点及其所需的属性,  也称为 props. 元素是 React 应用程序的最小构建块,  通常使用 JSX 创建,  JSX 是 JavaScript 的语法扩展. 
const element = <h1>Hello, {name}</h1>;

const element = React.createElement("h1");
//returns an object similar to this one:
{
  type: 'h1',
  props: {}
}
另一方面,  组件是可重用的 UI 部分,  可以由一个或多个元素组成. React 中的组件可以是函数组件,  也可以是类组件. 它们封装了渲染和行为的逻辑,  并且可以接受输入数据(道具)并维护内部状态. 

const App(){
  return <div>Hello World !</div>;
}
export default App;

4 state & props 
- 状态用于管理组件的内部数据及其随时间的变化. 状态是可变的
- Props(属性的缩写)是一种将数据从父组件传递到子组件的机制. 它们是只读的(不可变的),  有助于使组件可重用和可定制. 

5 PureComponent & React.memo()
React 中最常见的问题之一是组件不必要地重新渲染. React 提供了两个方法,  在这些情况下非常有用: 
* React.memo():这可以防止不必要地重新渲染函数组件
* PureComponent:这可以防止不必要地重新渲染类组件
这两种方法都依赖于对传递给组件的props的浅比较,  如果 props 没有改变,  那么组件将不会重新渲染. 虽然这两种工具都非常有用,  但是浅比较会带来额外的性能损失,  因此如果使用不当,  这两种方法都会对性能产生负面影响. 

如果传给子组件的派生状态或函数,  每次都是新的引用,  那么 PureComponent 和 React.memo 优化就会失效. 所以需要使用 useMemo[缓存结果] 和 useCallback【缓存函数】 来生成稳定值,  并结合 PureComponent 或 React.memo 避免子组件重新 Render. 

6 合成事件 SyntheticEvent
在 React 中,  合成事件是 React 提供的一种事件处理机制,  它是对原生 DOM 事件的封装和优化. React 使用合成事件来处理用户交互,  以提高性能和跨浏览器一致性. 

合成事件的特点包括: 
1. 跨浏览器兼容性: React 封装了原生 DOM 事件,  使得开发者无需关心不同浏览器的兼容性问题. 
2. 事件委托: React 使用事件委托的方式来处理事件,  将事件绑定在最外层容器上,  通过事件冒泡机制来处理具体的事件,  减少了事件绑定的数量,  提高了性能. 
3. 事件池: React 使用事件池来管理合成事件对象,  使得事件对象可以被重用,  减少了对象的创建和垃圾回收,  提高了性能. 
4. SyntheticEvent 对象: 合成事件会传递一个 SyntheticEvent 对象作为参数,  这个对象是对原生事件对象的包装,  提供了一些额外的属性和方法,  使得事件处理更加灵活和方便. 

使用合成事件可以使得事件处理更加简洁和高效,  同时提供了一些额外的特性和优化,  方便开发者进行事件处理和交互操作. 

react17之前是绑定在document上的,17之后是绑定在#root
 React 事件机制的一些重要特点和使用方式: 

合成事件: React 使用合成事件来代替原生 DOM 事件,提供了跨浏览器一致性. 合成事件是 SyntheticEvent 的实例,它是对底层的原生事件的封装,并提供了与原生事件相同的属性和方法. 

7 组件生命周期
1. 挂载阶段 :  由ReactDOM.render()触发---初次渲染
- constructor() 这是创建组件时调用的第一个方法. 它用于初始化状态和绑定事件处理程序. 
- getDerivedStateFromProps 当接收到新的 props 或 state 时,  在渲染之前调用此方法. 根据新的 props 更新 state
- render() 此方法负责根据当前状态和属性渲染组件的 UI; 返回要渲染的元素
- componentDidMount() 该方法在组件第一次渲染后调用. 例如数据获取或设置订阅. 

2. 更新阶段 : 由组件内部this.setSate()或父组件重新render触发
- getDerivedStateFromProps  同上
- shouldComponentUpdate() 该方法在组件重新渲染之前调用. 用于优化性能,返回 false 可以阻止组件更新. 
- render()  再次调用 render 方法来根据状态或 props 的变化来更新组件的 UI. 
- getSnapshotBeforeUpdate 在 DOM 更新前调用. 通常用于在组件更新之前读取某些 DOM 属性或状态,然后在组件更新完成后使用这些信息. 一个常见的用例是处理滚动位置.[一个典型的用例是维护滚动位置. 例如,在一个聊天应用中,你可能希望在新消息到来时保持滚动位置不变,或者滚动到底部. ] 返回一个值,这个值会作为 componentDidUpdate 的第三个参数. 
- componentDidUpdate() 该方法在组件因 state 或 props 变化而重新渲染后被调用. 它用于在更新后执行操作,  例如更新 DOM 以响应状态更改. 

3. 卸载组件 : 由ReactDOM.unmountComponentAtNode()触发
- componentWillUnmount()  在组件从 DOM 中删除之前调用此方法. 它用于执行任何清理,  例如取消网络请求或清理订阅. 

错误处理: 
static getDerivedStateFromError(error): 当后代组件抛出错误时,在"渲染"阶段调用此方法. 它允许组件更新其状态以响应错误. 

componentDidCatch(error, info): 当后代组件抛出错误时,在"提交"阶段调用此方法. 它用于捕获组件树中发生的错误并执行副作用,例如记录错误. 

8 React 高阶组件
React 高阶组件(Higher Order Component,  HOC)是一种用于重用组件逻辑的高级技朮. HOC 是一个函数,  接受一个组件作为参数,  然后返回一个新的增强版组件. 通过 HOC,  可以在不改变组件原有结构的情况下,  添加额外的功能, 逻辑或属性. 
// 定义一个 HOC
const withLogger = (WrappedComponent) => {
  return class extends React.Component {
    componentDidMount() {
      console.log(`Component ${WrappedComponent.name} has mounted`);
    }

    render() {
      return <WrappedComponent {...this.props} />;
    }
  };
};

// 使用 HOC 包装组件
const EnhancedComponent = withLogger(MyComponent);

// 使用增强版组件
<EnhancedComponent />;

9. 什么是 context 和 useContext Hook?

在 React 中,  Context 提供了一种通过组件树传递数据的方法,  而无需在每个级别手动向下传递 props. 旨在共享可被视为 React 组件树的全局数据的数据

useContext() 挂钩用于使用函数组件内的上下文数据. 它将上下文对象作为参数并返回当前上下文值. 

import React, { createContext, useContext } from 'react';

// Create a context
const ThemeContext = createContext('light');

// A component that consumes the context using the useContext hook
const ThemedComponent = () => {
  const theme = useContext(ThemeContext);

  return <div>Current theme: {theme}</div>;
};

// A component that provides the context value using the Provider
const App = () => {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedComponent />
    </ThemeContext.Provider>
  );
};

10 函数组件【无状态组件】 / 类组件【状态组件】

* 函数组件和类组件各有优缺点,  但是函数组件更简洁,  开发更容易; 
* 类组件过多的使用this让整个逻辑看起来很混乱; 

随着 React hooks 的引入,  有状态组件也可以使用函数式组件来编写. 

函数组件+hooks 更优雅的逻辑复用方式
hooks(主要是useEffect)取代了生命周期的概念(减少API),  让开发者的代码更加"声明化"
Class 组件不易拆解,  相同的业务逻辑逻辑在各处; 

useEffect -> 可以看作是 componentDidMount componentDidUpdate componentWillUnmount 三个生命周期函数的组合

11.为什么我们不应该直接更新状态?
当你直接更新状态时,  React 不会检测到发生了变化,  因为它不会触发重新渲染过程. 这可能会导致您的 UI 无法反映更新后的状态,  从而导致难以调试的不一致和错误. [性能下降等]
应该使用 setState 方法来更新状态. 这是因为 React 的状态更新是异步的,  并且批量更新的

12 回调函数作为 setState() 的参数的目的是什么?
setState() 不会立即改变 this.state() ,  而是创建一个挂起的状态转换. 调用此方法后访问 this.state() 可能会返回现有值. (意味着我们在调用 setState() 时不应该依赖当前状态)

解决方案是将一个函数传递给 setState(),  并以先前的状态作为参数. 通过这样做,  我们可以避免由于 setState() 的异步特性而导致用户在访问时获取旧状态值的问题. 

// Problem : It will not update count with +3 on each click

const hanldeCountIncrement = () => {
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);
} 
// count will be 1 not 3

// Solution: always use prev to update state.

const hanldeCountIncrement = () => {
setCount((prev) => prev + 1);
setCount((prev) => prev + 1);
setCount((prev) => prev + 1);
}

// on each click, value of count will update by +3

setState()函数通常被认为是异步的,这意味着调用setState()时不会立刻改变react组件中state的值,setState通过触发一次组件的更新来引发重汇,多次setState函数调用产生的效果会合并,最终只执行一次渲染. 

14 如何在 JSX 回调中绑定方法或事件处理程序?

在 React 中,  有几种方法可以在 JSX 回调中绑定方法或事件处理程序. 以下是最常见的方法: 

在 JSX 中使用箭头函数(内联绑定): 

class MyComponent extends React.Component {
  handleClick = () => {
    // Handler logic
  };

  render() {
    return <button onClick={() => this.handleClick()}>Click me</button>;
  }
}
在使用箭头函数的函数组件中: 

import React from 'react';

const MyComponent = () => {
  const handleClick = () => {
    // Handler logic
  };

  return <button onClick={handleClick}>Click me</button>;
};

export default MyComponent;
构造函数中的绑定: 

class MyComponent
 extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // Handler logic
  }

  render() {
    return <button onClick={this.handleClick}>Click me</button>;
  }
}

15 refs 有什么用
在 React 中,  "ref"是一个对象,  它提供了一种引用或访问特定 DOM 节点或 React 元素的方法. Refs 通常用于与 DOM 命令式交互,  例如聚焦输入, 获取其尺寸或访问其方法. 

当你希望组件"记住"某些信息,  但又不想让这些信息 触发新的渲染 时,  你可以使用 ref . 

在函数组件中使用 useRef() 钩子

import React, { useRef, useEffect } from 'react';

const MyComponent = () => {
  const myRef = useRef(null);

  useEffect(() => {
    // Accessing the DOM node using the ref
    myRef.current.focus();
  }, []);

  return <input ref={myRef} />;
};

何时使用 ref

* 存储 timeout ID
* 存储和操作 DOM 元素,  
* 存储不需要被用来计算 JSX 的其他对象. 
如果你的组件需要存储一些值,  但不影响渲染逻辑,  请选择 ref. 

不要在渲染过程中读取或写入 ref.current.  如果渲染过程中需要某些信息,  请使用 state 代替. 

16 forwardRef 允许组件使用 ref 将 DOM 节点暴露给父组件. 

例子
//  子组件
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {

    const { label, ...otherProps } = props;

    return (
        <label>
            {label}
            <input {...otherProps} ref={ref} />
        </label>
    );

});

//  父组件
function Form() {

  const ref = useRef(null);

  function handleClick() {
      ref.current.focus();
  }

  return (
    <form>
        <MyInput label="Enter your name:" ref={ref} />
        <button type="button" onClick={handleClick}>
            编辑
        </button>
    </form>
  );
}

17 React Fiber
React Fiber 是 React 16 中引入的一种新的协调算法. 它旨在使 React 应用程序更快, 更流畅,  特别是对于具有大量更新的复杂应用程序. 

React Fiber 的工作原理是将协调过程分解为更小的工作单元,  称为纤维. 纤程可以按任何顺序调度和执行,  这使得 React 可以确定工作的优先级并避免阻塞主线程. 

这使得 React 应用程序即使在长时间运行的任务(例如渲染大型列表或对复杂场景进行动画处理)期间也能保持响应. 

React16 实现了新的基于 requestIdleCallback 的调度器(因为 requestIdleCallback 兼容性和稳定性问题,自己实现了 polyfill),通过任务优先级的思想,在高优先级任务进入的时候,中断 reconciler. 

为了适配这种新的调度器,推出了 FiberReconciler,将原来的树形结构(vdom)转换成 Fiber 链表的形式(child/sibling/return),整个 Fiber 的遍历是基于循环而非递归,可以随时中断. 

18. 什么是受控组件和非受控组件?

React 中有两种处理表单的主要方法, 它们在基本层面上有所不同: 数据的管理方式. 

非受控组件: 在非受控组件中, 表单数据由 DOM 本身处理, React 不通过状态控制输入值. 

输入值由 DOM 管理, 通常在需要时使用 ref 来访问输入值. 

当您想要将 React 与非 React 代码或库集成, 或者当您需要优化大型表单的性能时, 不受控制的组件非常有用. 

import React, { useRef } from 'react';

const UncontrolledComponent = () => {
  const inputRef = useRef(null);

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log('Input value:', inputRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" ref={inputRef} />
      <button type="submit">Submit</button>
    </form>
  );
};

受控组件: 表单数据由 React 组件(而不是 DOM)处理,  方法是将输入值存储在状态中,  并在输入更改时更新状态. 

输入值由 React 状态控制,  输入的更改通过事件处理程序进行处理,  从而更新状态. 

当组件管理的表单字段中的元素状态发生变化时,  我们使用 onChange 属性来跟踪它. 

import React, { useState } from 'react';

const ControlledComponent = () => {
  const [value, setValue] = useState('');

  const handleChange = (event) => {
    setValue(event.target.value);
  };

  return (
    <input
      type="text"
      value={value}
      onChange={handleChange}
    />
  );
};

19. 如何用动态键名设置状态?
  const handleChange = async (event) => {
    const { name, value } = event.target;
    setFormData(prevData => ({ ...prevData, [name]: value }));
    // Dynamic key name using computed property name
  };

20. 如何在 React 中对 props 应用验证?
在 React 中, 您可以使用 PropTypes 或 TypeScript 对 props 应用验证. 

MyComponent.propTypes = {
  name: PropTypes.string.isRequired, // Require a string prop
  age: PropTypes.number.isRequired, // Require a number prop
};

如果您使用 TypeScript, 您可以为 props 定义接口并直接指定类型. TypeScript 将在编译时检查类型, 提供静态类型检查. 

interface MyComponentProps {
  name?: string;
  age?: number;
}

const MyComponent: React.FC<MyComponentProps> = ({ name = '', age = 18}) => {}

interface & type 相同点: 
############# 
//  都可以用来描述一个对象
interface User {
  name: string
  age: number
}

type User = {
  name: string
  age: number
};

//  都允许拓展: 
interface Name { 
  name: string; 
}
interface User extends Name { 
  age: number; 
}

type Name = { 
  name: string; 
}
type User = Name & { age: number  };

###########  type 可以而 interface 不行
type 可以声明基本类型别名, 联合类型, 元组等类型
// 基本类型别名
type Name = string

// 联合类型
type Status = "active" | "inactive" | "pending";
const userStatus: Status = "active";

// 具体定义数组每个位置的类型 【元组类型】
type PetList = [Dog, Pet]


##############.  interface可以而 type不行
interface 能够声明合并

interface User {
  name: string
  age: number
}

interface User {
  sex: string
}

/*
User 接口为 {
  name: string
  age: number
  sex: string 
}
*/

同名的属性会以最后一个属性为准

interface 有只读属性
interface Person {
  readonly name: string;
  age: number;
}

21 React 中的 Children 属性是一个特殊的属性
下面的 Card 组件将接收一个被设为 <Avatar /> 的 children prop 并将其包裹在 div 中渲染: 
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}


export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}

22.什么是渲染道具?
Render props 是 React 中的一种模式, 其中组件的 render 方法返回一个函数, 并且该函数作为 prop 传递给子组件. 该函数通常称为"render prop", 负责渲染组件的内容. 

Render props 提供了一种在 React 组件之间以灵活且可重用的方式共享代码和行为的方法. render prop 以一个函数作为参数, 负责渲染组件的 UI. 

该函数可用于渲染任何类型的 UI, 包括其他 React 组件. 

const MyComponent = ({ render }) => {
  return render();
};

const MyOtherComponent = () => {
  const display = () => (
       <div>
         <h1>This is my component!</h1>
       </div>
  )
  return (
    <MyComponent render={display} />
  );
};

// This will render the following HTML:
// <h1>This is my component!</h1>

23. 如何进行 AJAX 调用
组件挂载: 首次挂载组件时可以进行AJAX调用. 这通常在类组件的 `componentDidMount` 生命周期方法中完成, 或者在函数组件的带有空依赖数组 ([]) 的 `useEffect` 挂钩中完成

组件卸载: 如果需要在组件卸载时取消 AJAX 请求或执行清理, 可以在类组件的 `componentWillUnmount` 生命周期方法中或在函数组件的 `useEffect` 钩子返回的清理函数中执行此操作. 

useEffect(() => {
  // Make AJAX call

  return () => {
    // Cancel AJAX requests or perform cleanup
  };
}, []);

24. 什么是 React Hook?有哪些重要的钩子?
React Hooks 是使函数组件能够使用 React 中的状态和生命周期功能的函数. 它们在 React 16.8 中引入, 是为了解决函数组件中的状态管理和副作用问题, 允许开发人员在不编写类的情况下使用状态和其他 React 功能. 

- useState  向组件添加状态变量 const [state, setState] = useState(initialState)
- useEffect  将组件与外部系统同步 useEffect(setup, dependencies?)
- useMemo  在每次重新渲染的时候能够缓存计算的结果. const cachedValue = useMemo(calculateValue, dependencies)
- useCallback 在多次渲染中缓存函数 const cachedFn = useCallback(fn, dependencies)
- useReducer 允许你向组件里面添加一个 reducer const [state, dispatch] = useReducer(reducer, initialArg, init?)
- useContext 读取和订阅组件中的 context  const value = useContext(SomeContext)
- useRef 帮助引用一个不需要渲染的值 const ref = useRef(initialValue)

25. React 中的错误边界是什么?

错误边界的工作方式类似于 JavaScript catch {} 块,但适用于组件. 只有类组件可以是错误边界. 

错误边界是 React 组件,它可以捕获子组件树中任何位置的 JavaScript 错误,记录这些错误,并显示后备 UI,而不是崩溃的组件树. 

错误边界会在渲染期间, 生命周期方法以及其下方的整个树的构造函数中捕获错误. 

错误边界无法捕获自身内部的错误. 
https://zh-hans.react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary

如果类组件定义了生命周期方法 static getDerivedStateFromError() 或 componentDidCatch() 中的一个(或两个),则该类组件将成为错误边界. 使用 static getDerivedStateFromError() 在引发错误后呈现后备 UI. 
使用 componentDidCatch() 来记录错误信息. 

//  例子
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      hasError: false,
      error: null,
      errorInfo: null
    };
  }

  static getDerivedStateFromError(error) {
    // 更新状态,以便下一次渲染将显示后备 UI. 
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    this.setState({
      error: error,
      errorInfo: errorInfo
    });
    // You can also log the error to an error reporting service
    console.error('Error:', error);
    console.error('Error Info:', errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // 你可以渲染任何自定义后备 UI
      return this.props.fallback;
    }

    return this.props.children;
  }
}

然后你可以用它包装组件树的一部分: 

<ErrorBoundary fallback={<p>Something went wrong</p>}>
  <Profile />
</ErrorBoundary>
如果 Profile 或其子组件抛出错误,ErrorBoundary 将"捕获"该错误,然后显示带有你提供的错误消息的后备 UI,并向你的错误报告服务发送生产错误报告. 

目前还没有办法将错误边界编写为函数式组件. 但是你不必自己编写错误边界类. 例如,你可以使用 react-error-boundary 包来代替. 

26.react-dom包有什么用?

React DOM 是一个 JavaScript 库,用于将 React 组件渲染到浏览器的(DOM). 它提供了许多与 DOM 交互的方法,例如创建元素, 更新属性和删除元素. 

React DOM 与 React 结合使用来构建用户界面. React 使用虚拟 DOM 来跟踪 UI 的状态,React DOM 负责更新真实 DOM 以匹配虚拟 DOM. 
//  example
ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>,
)

27.如何在React中使用装饰器?

在 React 中,装饰器是包装组件以提供附加功能的高阶函数. 
装饰器是 React 中的一项强大功能,它允许您向组件添加功能,而无需修改其代码. 这对于添加日志记录, 性能跟踪或要应用于多个组件的其他功能非常有用. 
要在 React 中使用装饰器,首先需要安装 babel-plugin-transform-decorators-legacy 包. 安装该软件包后,您需要将 .babelrc 文件添加到项目根目录中. .babelrc 文件应包含以下代码: 

{
  "plugins": ["babel-plugin-transform-decorators-legacy"]
}
添加 .babelrc 文件后,您需要更新 tsconfig.json 文件以启用实验性装饰器. 为此,请将以下行添加到 tsconfig.json 文件中: 

"experimentalDecorators": true

以下代码演示了如何使用装饰器在渲染 React 组件时记录该组件的名称: 
import React from "react";

function logComponent(Component) {
  return class extends React.Component {
    render() {
      console.log(Component.name);
      return <Component {...this.props} />;
    }
  };
}

@logComponent
class MyComponent extends React.Component {
  render() {
    return <div>Hello, world!</div>;
  }
}

export default MyComponent;

28 forceUpdate(callback?) 
强制组件重新渲染. 
通常来说,这是没必要的. 如果组件的 render 方法仅读取了 this.props, this.state 或 this.context 时,当你在组件或其任一父组件内调用 setState 时,它就将自动重新渲染. 但是如果组件的 render 方法是直接读取外部数据源时,则必须告诉 React 在该数据源更改时更新用户界面. 这就是 forceUpdate 的作用. 

尽量避免使用 forceUpdate 并且在 render 中尽量只读取 this.props 和 this.state. 
`forceUpdate方法是React中类组件特有的,不能在函数式组件中使用. `

29. React Portal
提供了一种将组件的内容渲染到 DOM(文档对象模型)树的不同部分(通常位于其父组件之外)的方法. 
createPortal(children, domNode, key?) 
调用 createPortal 创建 portal,并传入 JSX 与实际渲染的目标 DOM 节点
// ...
import { createPortal } from 'react-dom';

<div>
  <p>这个子节点被放置在父节点 div 中. </p>
  {createPortal(
    <p>这个子节点被放置在 document body 中. </p>,
    document.body
  )}
</div>

30. 如何在页面加载时将输入元素聚焦?

方法一: 使用自动对焦属性: 
import React from 'react';

const MyComponent = () => {
  return <input type="text" autoFocus />;
};

export default MyComponent;

方法二: 使用引用对象: 
import React, { useEffect, useRef } from 'react';

const MyComponent = () => {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return <input type="text" ref={inputRef} />;
};

export default MyComponent;

32. 优化 React App 有哪些方法?
a)  代码分割/延迟加载/动态导入
- 代码拆分涉及将 JavaScript 包分解为更小, 更易于管理的块. 您可以根据不同的路由, 组件或其他逻辑划分将其拆分为单独的文件,而不是一次性将整个应用程序代码发送到客户端. 
- 延迟加载是一种在初始页面加载时推迟非关键资源加载的策略. 
- React.lazy 和 Suspense 形成了延迟加载依赖项并仅在需要时加载的完美方式

//  例子
import React from 'react'
import { BrowserRouter, Routes, Route } from 'react-router-dom'

const TodoList = React.lazy(() => import('./routes/TodoList'))
const NewTodo = React.lazy(() => import('./routes/NewTodo'))

const App = () => (
  <BrowserRouter>
    <React.Suspense fallback={<p>Please wait</p>}>
      <Routes>
        <Route exact path="/" element={<TodoList/>} />
        <Route path="/new" element={<NewTodo/>} />
      </Routes>
    </React.Suspense>
  </BrowserRouter>
)

在 ReactJS 应用程序中,您可以使用 Webpack 等工具实现代码分割, 延迟加载和动态导入,Webpack 为这些功能提供内置支持. 

动态 import() 语句异步加载模块,Webpack 会自动拆分代码并为动态导入的模块生成单独的包. 

b) 服务器端渲染(SSR
c) 优化捆绑包大小: 密切关注捆绑包大小,并通过删除未使用的依赖项, 使用树摇动和最小化大型库的使用来优化它. 
d) React.memo() 或 PureComponent
e) 使用 React.Fragments 或 <> </> 
f) 节流和去抖动事件操作
g) useMemo() 和 useCallback(): 这两个钩子都可以通过减少组件需要重新渲染或记住组件或昂贵操作的结果的次数来帮助优化 React 组件. 
h) 使用 Web Workers 执行 CPU 大量任务: Web Workers 可以在 Web 应用程序的后台线程中运行脚本操作,与主执行线程分开. 通过在单独的线程中执行繁重的处理,主线程(通常是 UI)能够运行而不会被阻塞或减慢. 
i) 虚拟化长列表: 列表虚拟化或窗口化是一种在渲染长数据列表时提高性能的技术. 该技术在任何给定时间仅渲染一小部分行

有几种不同的方法可以在 React 中实现受保护的路由. 一种常见的方法是使用 React Router 库. React Router 允许您定义路由并指定哪些用户有权访问每个路由. 

const ProtectedRoute = ({ component: Component, isAuthenticated, ...rest }) => (
  <Route
    {...rest}
    render={(props) =>
      isAuthenticated ? <Component {...props} /> : <Redirect to="/login" />
    }
  />
);

34. React 编码最佳实践
组件组合: 将您的 UI 分解为更小的, 可重用的组件
单一职责原则 (SRP): 每个组件都应具有单一职责
使用函数组件: 只要有可能,就使用函数组件而不是类组件. 函数式组件更简单, 更简洁, 更容易推理. 使用 useState 和 useEffect 等钩子来管理功能组件中的状态和副作用. 
避免在渲染方法中使用复杂的 JSX: 将复杂的 JSX 结构分解为更小, 更易于管理的组件或辅助函数
避免直接状态变更: 更新状态时,始终使用 React 提供的函数(例如,类组件中的 setState, 功能组件中的 useState hook)以避免直接变更状态. 
优化性能: 通过最大限度地减少不必要的重新渲染
优雅地处理错误: 实施错误边界以捕获和处理组件中的错误. ErrorBoundary
使用 PropTypes 或 TypeScript 进行类型检查
测试组件: 为组件编写测试涉及使用 Jest 和 React 测试库等测试库来确保组件按预期运行. 


35. 如何进行React应用程序的组件级和端到端测试?
单元测试: 使用 Jest 等测试框架以及 Enzyme 或 React 测试库等工具为各个组件编写单元测试. 
export default Button;

// button.test.js
import React from 'react';
import { render, fireEvent } from '@testing-library/react';
import Button from './Button';

test('renders button with correct label', () => {
  const { getByText } = render(<Button label="Click me" />);
  const buttonElement = getByText('Click me');
  expect(buttonElement).toBeInTheDocument();
});

test('triggers onClick function when button is clicked', () => {
  const onClickMock = jest.fn();
  const { getByText } = render(<Button label="Click me" onClick={onClickMock} />);
  const buttonElement = getByText('Click me');
  fireEvent.click(buttonElement);
  expect(onClickMock).toHaveBeenCalledTimes(1);
});

集成测试: 通过编写集成测试来测试不同组件如何协同工作. 

端到端测试: 使用 Cypress 或 Selenium 等工具编写端到端测试,模拟用户在真实浏览器环境中与应用程序的交互. 

快照测试: 快照测试是一种捕获组件输出"快照"并将其与先前存储的快照进行比较的方法. 

37. React 18有哪些新功能?
- 自动批处理:
React 18 引入了一个新的自动批处理功能,该功能将状态更新分组在一起并一次性渲染它们. 这可以通过减少 DOM 更新次数来提高性能. 

- 并发反应: 
React 18还引入了一种新的并发模式,允许React同时处理多个任务. 这可以通过使 React 更好地响应用户输入来提高性能. 它帮助 React 根据不同任务的重要性和紧急程度确定更新和渲染的优先级,确保高优先级更新得到更快的处理. 

- Suspense:
React 18 还引入了一个新的Suspense功能,允许 React 延迟渲染组件,直到其数据可用. 这可以防止 React 在等待数据时呈现空白屏幕,从而改善用户体验. 

- New Hooks: React 18 引入了新的 Hook,例如 useId[useId 是一个 React Hook,可以生成传递给无障碍属性的唯一 ID. 

], useTransition[useTransition 是一个帮助你在不阻塞 UI 的情况下更新状态的 React Hook.  例子 - 切换不同tab时,若某tab尚未加载完,也可以点击其他tab

] 等,

- New APIs: React 18 引入了一些新的 API,例如 createRoot, hydrateRoot 等

38 react 设计模式
状态管理模式 ,  不可变数据模式 ,  错误边界模式 ,  Context API ,  高阶组件 (HOC): HOC 是接受组件作为参数并返回具有增强功能的新组件的函数. 

39 Next.js 是一个构建在 React 之上的框架,并提供服务器端渲染, 静态站点生成和自动路由等附加功能. 

40 React v19 的新特性概览
React 编译器: React 实现了一个新的编译器. 目前,Instagram 已经在利用这项技术了. 
[
编译器内核其实就是「旧的 AST 输入,新的 AST 输出」. 在后台,编译器使用自定义代码表示和转换管道来执行语义分析. 
React19之前, 「手动记忆化」. 在之前的API中,这意味着应用useMemo, useCallback和memo API来手动调整React在状态变化时重新渲染的部分. 手动记忆化只是一种「权宜之计」,它会使代码变得复杂,容易出错,并需要额外的工作来保持更新
React 将「自行决定何时以及如何改变状态并更新 UI」. 
有了这个功能,我们不再需要手动处理这个问题. 这也意味着让人诟病的 useMemo(), useCallback() 和 memo要被历史的车轮无情的碾压. 
]
服务器组件(RSC): 经过多年的开发,React 引入了服务器组件,而不是需要借助Next.js
[react server components 是一种新的 React 技术,它允许您在服务器上渲染组件. 
Next.js 是第一个在生产环境中实现它们的. 从 Next.js 13 开始,「默认情况下所有组件都是服务器组件」. 要使组件在客户端运行,我们需要使用'use client'指令. 
在 React 19 中,服务器组件将直接集成到 React 中,带来了一系列优势: 数据获取 /  缓存 / 性能 / FCP / SEO
默认情况下,React 中的所有组件都是客户端组件. 只有使用 'use server' 时,组件才是服务器组件. 
]
动作(Action): 动作也将彻底改变我们与 DOM 元素的交互方式. 
[
这将是我们处理表单的重大变革. 
使用异步转换的函数被称为Action(动作). Action自动管理数据的提交: 
"use server"

const submitData = async (userData) => {
    const newUser = {
        username: userData.get('username'),
        email: userData.get('email')
    }
    console.log(newUser)
}
const Form = () => {
    return <form action={submitData}>
        <div>
            <label>用户名</label>
            <input type="text" name='username'/>
        </div>
        <div>
            <label>邮箱</label>
            <input type="text" name="email" />
        </div>
        <button type='submit'>提交</button>
    </form>
}
]
文档元数据: 这是另一个备受期待的改进,让我们能够用更少的代码实现更多功能. 
[
  在做SEO时,我们需要在<meta>中处理title/keywords/description的信息. 

  title的权重最高,利用title提高页面权重
  keywords相对权重较低,作为页面的辅助关键词搜索
  description的描述一般会直接显示在搜索结果的介绍中

  经常借助编写自定义代码或使用像 react-helmet[5] 这样的包来处理路由更改并相应地更新元数据. 

  使用 React19后,我们可以直接在 React 组件中使用<title>和 <meta> 标签: 
Const HomePage = () => {
  return (
    <>
      <title>React19</title>
      <meta name="description" content="前端柒八九" />
      // 页面内容
    </>
  );
}
]
资源加载: 这将使资源在后台加载,从而提高应用程序的加载速度和用户体验. 
[
  在 React 19 中,当用户浏览当前页面时,图片和其他文件将「在后台加载」. 
此外,React 还引入了用于资源加载的生命周期 Suspense,包括script, 样式表和字体. 这个特性使 React 能够确定内容何时准备好显示,消除了任何FOUT的闪烁现象. 

]
Web Components: React 代码现在可以让我们集成 Web Components. 
[
  有一个功能需要多项目多框架使用,那么我们可以考虑一下,将此功能用Web Components实现. 
  Web 组件允许我们使用原生 HTML, CSS 和 JavaScript 创建自定义组件,无缝地将它们整合到我们的 Web 应用程序中,就像使用HTML 标签一样. 
  虽然 WebComponents 有三个要素,但却不是缺一不可的,WebComponents

    借助 shadow dom  来实现「样式隔离」,
    借助 templates 来「简化标签」的操作. 

在React19之前,在 React 中集成 Web Components并不直接. 通常,我们需要将 Web Components转换为 React 组件;
React 19 将帮助我们更轻松地将 Web Components整合到我们的 React 代码中
]
增强的 hooks: 引入了很多令人兴奋的新 hooks,将彻底改变我们的编码体验. 
[
  在 React 19 中,我们使用 useMemo, forwardRef, useEffect 和 useContext 的方式将会改变. 这主要是因为将引入一个新的 hook,即 use. 

  在 React19 之后,我们不再需要使用 useMemo() hook,因为 React编译器 将会自动进行记忆化. 

  ref 现在将作为props传递而不是使用 forwardRef() hook. 

React19 将引入一个新的 hook,名为 use(). 这个 hook 将简化我们如何使用 promises, async 代码和 context. 

useFormStatus() hook
在 React19 中,我们还有新的 hooks 来处理表单状态和数据. 这将使处理表单更加流畅和简单. 将这些 hooks 与 Action结合使用将使处理表单和数据更加容易. 

useOptimistic 也新发布的Hook,它允许我们在异步操作时显示不同的状态. 

]

JSX 是 JavaScript 语法的扩展,它允许编写类似于 HTML 的代码


```
