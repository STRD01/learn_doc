React 是一个用于构建用户界面的 JavaScript 库。它的核心思想是通过组件来构建 UI，并且每个组件可以拥有自己的状态和生命周期方法。React 的组件可以是函数组件或类组件，而 React Hooks 则提供了一种无需使用类就能在函数组件中使用状态和其他 React 特性的方式。

## React 基本介绍

### 核心概念

1. 组件（Components）：

组件是构建 React 应用的基本单位。每个组件可以独立运行并渲染自己的一部分 UI。

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

2. JSX：

JSX 是一种 JavaScript 的语法扩展，允许你在 JavaScript 代码中编写类似 HTML 的标签。

```jsx
const element = <h1>Hello, world!</h1>;
```

3. 状态（State）和属性（Props）：

- props 是只读的，用于从父组件传递数据到子组件。
- state 是组件内部的可变数据，决定了组件的行为和渲染。

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() };
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

4. 事件处理：

React 事件处理类似于 DOM 事件处理，但使用驼峰命名法，并且传递函数作为事件处理器。

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isToggleOn: true };

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```

## React Hooks

React Hooks 是 React 16.8 引入的一组新的 API，使函数组件可以使用状态和其他 React 特性。Hooks 的主要目的是在函数组件中实现状态逻辑的复用，同时保持代码的简洁和可读性。

### 常用的 Hooks

1. useState：

useState 是一个用于在函数组件中声明状态变量的 Hook。

```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

2. useEffect：

useEffect 允许你在函数组件中执行副作用操作，如数据获取、订阅或手动 DOM 操作。

```javascript
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

3. useContext：

useContext 使你可以在函数组件中使用上下文（context）。

```javascript
const MyContext = React.createContext();

function MyComponent() {
  const value = useContext(MyContext);
  return <div>{value}</div>;
}
```

4. useReducer：

useReducer 是一种替代 useState 的 Hook，适用于包含复杂逻辑的状态管理。

```javascript
import React, { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```

5. useRef：

useRef 返回一个可变的 ref 对象，其 .current 属性初始化为传入的参数值。

```javascript
import React, { useRef } from 'react';

function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    inputEl.current.focus();
  };
  return (
    <div>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </div>
  );
}
```

### 自定义 Hooks

自定义 Hooks 是一种将逻辑复用的模式，你可以将多个组件中重复使用的逻辑提取到自定义 Hook 中。

```javascript
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then(response => response.json())
      .then(data => {
        setData(data);
        setLoading(false);
      });
  }, [url]);

  return { data, loading };
}

function DataFetchingComponent() {
  const { data, loading } = useFetch('https://api.example.com/data');

  if (loading) return <p>Loading...</p>;
  return <div>{data}</div>;
}
```

### 优势

- 组件逻辑复用：使用自定义 Hooks 可以轻松复用组件逻辑，而不必依赖于高阶组件（HOC）或 render props。
- 状态管理简洁：useState 和 useReducer 提供了简洁的状态管理方式，不需要额外的类组件。
- 副作用管理：useEffect 统一了组件生命周期内的副作用处理，避免了类组件中生命周期方法的复杂性。

### 缺点

- 学习曲线：对于初学者来说，Hooks 的概念和用法可能比较复杂。
- 调试困难：由于函数组件在每次渲染时都会重新执行，调试可能会更加困难。


React 和 React Hooks 为开发者提供了强大且灵活的工具，用于构建现代的用户界面。React 的组件化、状态管理和 JSX 语法，使得代码可维护性和可重用性显著提高。React Hooks 通过简洁的 API，使函数组件能够具备类组件的所有特性，并且促进了逻辑复用和代码组织。掌握 React 和 Hooks，可以大大提升开发效率和代码质量。
