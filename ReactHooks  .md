## React Hooks  概述

> React 是主流的前端框架，v16.8 版本引入了全新的 API，叫做 [React Hooks](https://reactjs.org/docs/hooks-reference.html)，颠覆了以前的用法。这个 API 是 React 的未来，有必要深入理解。本文谈谈我的理解，通过一些简单的Demo。当然，如果你没有React的基础，不建议了解Hooks。

## React 组件类传统写法

```js
import React, { Component } from "react";

export default class Button extends Component {
  constructor() {
    super();
    this.state = { buttonText: "Click me, please" };
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState(() => {
      return { buttonText: "Thanks, been clicked!" };
    });
  }
  render() {
    const { buttonText } = this.state;
    return <button onClick={this.handleClick}>{buttonText}</button>;
  }
}
```

##### 缺点

- 大型组件很难拆分和重构，也很难测试。
- 业务逻辑分散在组件的各个方法之中，导致重复逻辑或关联逻辑。
- 组件类引入了复杂的编程模式，比如 `render` ` props` 和高阶组件。

## 函数组件写法

> React 团队希望，组件不要变成复杂的容器，最好只是数据流的管道。开发者根据需要，组合管道即可。 **组件的最佳写法应该是函数，而不是类。**React 其实早就支持[函数组件](https://reactjs.org/docs/components-and-props.html)，下面就是一个例子。

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

##### 缺点

这种写法有重大限制，必须是纯函数，不能包含状态，也不支持生命周期方法，因此无法取代类。

**React Hooks 的设计目的，就是加强版函数组件，完全不使用"类"，就能写出一个全功能的组件。**

## React Hooks Api概述

> Hook 这个单词的意思是"钩子"。**React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。** React Hooks 就是那些钩子。你需要什么功能，就使用什么钩子。React 默认提供了一些常用钩子，你也可以封装自己的钩子。所有的钩子都是为函数引入外部功能，所以 React 约定，钩子一律使用`use`前缀命名，便于识别。你要使用 xxx 功能，钩子就命名为 usexxx。

### 常用的五个 Hooks Api

|   FunctionName    |    Mean    |
| :---------------: | :--------: |
|   `useState()`    | 声明State  |
|  `useContext()`   | 使用上下文 |
| `createContext()` | 创建上下文 |
|  `useReducer()`   |  状态管理  |
|   `useEffect()`   |  生命周期  |

#### useState()：状态钩子

> `useState()`用于为函数组件引入状态（state）。纯函数不能有状态，所以把状态放在钩子里面。本文前面那个组件类，用户点击按钮，会导致按钮的文字改变，文字取决于用户是否点击，这就是状态。使用`useState()`重写如下。

```js
import React, { useState } from "react";

export default function  Button()  {
  const  [buttonText, setButtonText] =  useState("Click me,   please");

  function handleClick()  {
    return setButtonText("Thanks, been clicked!");
  }

  return  <button  onClick={handleClick}>{buttonText}</button>;
}
```

上面代码中，Button 组件是一个函数，内部使用`useState()`钩子引入状态。

`useState()`这个函数接受状态的初始值，作为参数，上例的初始值为按钮的文字。该函数返回一个数组，数组的第一个成员是一个变量（上例是`buttonText`），指向状态的当前值。第二个成员是一个函数，用来更新状态，约定是`set`前缀加上状态的变量名（上例是`setButtonText`）。

#### useContext()：共享状态钩子

> 如果需要在组件之间共享状态，可以使用`useContext()`。现在有两个组件 Navbar 和 Messages，他们是兄弟组件，我们希望它们之间共享状态。

```html
<div className="App">
  <Navbar/>
  <Messages/>
</div>
```

第一步就是使用 React Context API，在组件外部建立一个 Context。

```js
const AppContext = React.createContext({}); //建立上下文(父组件)
```

父组件使用如下

```js
<AppContext.Provider value={{
  username: 'superawesome'
}}>
  <div className="App">
    <Navbar/>
    <Messages/>
  </div>
</AppContext.Provider>
```

上面代码中，`AppContext.Provider`提供了一个 Context 对象，并包裹这两个组件，这个对象是可以被子组件共享的。

Navbar 组件的代码如下。

> ```javascript
> import React,{useContext} from 'react';
> const Navbar = () => {
>   const { username } = useContext(AppContext);//拿到username=>superawesome
>   return (
>     <div className="navbar">
>       <p>AwesomeSite</p>
>       <p>{username}</p>
>     </div>
>   );
> }
> ```

上面代码中，`useContext()`钩子函数用来引入 Context 对象，从中获取`username`属性。

Message 组件的代码也类似。

> ```javascript
> import React,{useContext} from 'react';
> const Messages = () => {
>   const { username } = useContext(AppContext)//拿到username=>superawesome
>   return (
>     <div className="messages">
>       <h1>Messages</h1>
>       <p>1 message for {username}</p>
>       <p className="message">useContext is awesome!</p>
>     </div>
>   )
> }
> ```

#### useEffect()钩子

> 相当于React三个生命周期的合集 `componentDidMount` `componentDidUpdate` `componentWillUnmount`

~~~js
useEffect(()  =>  {
  // Async Action
}, [dependencies])
~~~

上面用法中，`useEffect()`接受两个参数。第一个参数是一个函数，异步操作的代码放在里面。第二个参数是一个数组，用于给出 Effect 的依赖项，只要这个数组发生变化，`useEffect()`就会执行。第二个参数可以省略，这时每次组件渲染时，就会执行`useEffect()`。

##### demo

```js
import React,{useState,useEffect} from 'react';
const Person = ({ personId }) => {
  const [loading, setLoading] = useState(true);
  const [person, setPerson] = useState({});
  useEffect(() => {
    setLoading(true); 
    fetch(`https://swapi.co/api/people/${personId}/`)
      .then(response => response.json())
      .then(data => {
        setPerson(data);
        setLoading(false);
      });
  }, [personId])

  if (loading === true) {
    return <p>Loading ...</p>
  }
  return <div>
    <p>You're viewing: {person.name}</p>
    <p>Height: {person.height}</p>
    <p>Mass: {person.mass}</p>
  </div>
}
```

上面代码中，每当组件参数`personId`发生变化，`useEffect()`就会执行。组件第一次渲染时，`useEffect()`也会执行。

#### useReducer() 状态管理

> React 本身不提供状态管理功能，通常需要使用外部库。这方面最常用的库是 Redux。Redux 的核心概念是，组件发出 action 与状态管理器通信。状态管理器收到 action 以后，使用 Reducer 函数算出新的状态，Reducer 函数的形式是`(state, action) => newState`。

```js
  import React,{useReducer,useEffect} from 'react';
   const Counter = ()=> {
    // 定义中心化数据处理函数 第一个参数 action为触发器传过来的obj
    //(state, action) => newState
    const reducer = (state, action)=> {
        switch (action.type) {
            case 'increment':
                return {
                    count: state.count + 1
                };
            case 'decrement':
                return {
                    count: state.count - 1
                };
            case 'error':
                return {
                    count:0,
                    error: action.payload.error
                }
            default:
                throw new Error();
        }
    }
    //第二个参数 作为仓库的初始所有状态
    const initialState = {count: 0};
    // 返回值：定义state(最新的)和dispatch(触发器)函数
    const [state, dispatch] = useReducer(reducer, initialState);
    //useEffect=>React Hooks生命周期 第二个参数代表触发条件 这里当state变化就会执行 
    //当然 第一次渲染会无条件执行一次
    //相当于React三个生命周期的合集
    // componentDidMount，componentDidUpdate、componentWillUnmount
    useEffect(() => {
        //下面错误按钮单击时 打印{count: 0, error: "测试的错误"}
            console.log(state) 
    }, [state]);
        return (
            <>
                {/* useReducer会根据dispatch的action，
                返回最终的state，并触发render重新渲染 */}
                Count: {state.count}
                {/* dispatch 用来接收一个 
                action参数(reducer中的action)，
                用来触发reducer函数，更新最新的状态 */}
                <button onClick={() => dispatch({type: 'increment'})}>
                +</button>
                <button onClick={() => dispatch({type: 'decrement'})}>
                -</button>
                <button 
                onClick={() => dispatch(
                    {type: 'error',payload: { error: '测试的错误' }}
                )}> 错误按钮</button>
            </>
        );
    }
    export default Counter;
```

#### useContext() 和 useReducer 联合使用

```js
    // 定义初始化值
    const initState = {
        name: '',
        pwd: '',
        isLoading: false,
        error: '',
        isLoggedIn: false,
    }
    // 定义state[业务]处理逻辑 reducer函数
    function loginReducer(state, action) {
        switch(action.type) {
            case 'login':
                return {
                    ...state,
                    isLoading: true,
                    error: '',
                }
            case 'success':
                return {
                    ...state,
                    isLoggedIn: true,
                    isLoading: false,
                }
            case 'error':
                return {
                    ...state,
                    error: action.payload.error,
                    name: '',
                    pwd: '',
                    isLoading: false,
                }
            default: 
                return state;
        }
    }
    // 定义 context函数
    const LoginContext = React.createContext();
    function LoginPage() {
        const [state, dispatch] = useReducer(loginReducer, initState);
        const { name, pwd, isLoading, error, isLoggedIn } = state;
        const login = (event) => {
            event.preventDefault();
            dispatch({ type: 'login' });
            login({ name, pwd })
                .then(() => {
                    dispatch({ type: 'success' });
                })
                .catch((error) => {
                    dispatch({
                        type: 'error'
                        payload: { error: error.message }
                    });
                });
        }
        // 利用 context 共享dispatch
        return ( 
            <LoginContext.Provider value={dispatch}>
                <...>
                <LoginButton />
            </LoginContext.Provider>
        )
    }
    function LoginButton() {
        // 子组件中直接通过context拿到dispatch，出发reducer操作state
        const dispatch = useContext(LoginContext);
        const click = () => {
            if (error) {
                // 子组件可以直接 dispatch action
                dispatch({
                    type: 'error'
                    payload: { error: error.message }
                });
            }
        }
         return ( 
					<button onClick={click}>按钮</button>
        )
    }
```

可以看到在useReducer结合useContext，通过context把dispatch函数提供给组件树中的所有组件使用 ，而不用通过props添加回调函数的方式一层层传递。

@ xiaohuwei 2019 秋 于杭州

