---
typora-copy-images-to: 笔记图像
typora-root-url: 笔记图像
---

# 简介

2019年React Hooks是React生态圈里边最火的新特性了。它改变了原始的React类的开发方式，改用了函数形式;它改变了复杂的状态操作形式，让程序员用起来更轻松;它改变了一个状态组件的复用性，让组件的复用性大大增加。

主要区别：hooks不再写class而是统一用function

对比传统的react写法与hooks写法：

功能，一个按钮，每点一下，count值加1

传统写法：

~~~js
import React, {Component} from 'react';

class Example extends Component {
    constructor(props) {
        super(props);
        this.state={count:0}
    }
    render() {
        return (
            <div>
                <p>你点击了{this.state.count}次</p>
                <button onClick={this.addCount.bind(this)}>加1</button>
            </div>
        );
    }

    addCount(){
        this.setState({count:this.state.count+1})
    }
}

export default Example;
~~~

hooks写法：（构造函数，生命周期函数都可以省略）

~~~js
import React, { useState } from 'react';
function Example(){
    const [ count , setCount ] = useState(0); //数组解构 
    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={()=>{setCount(count+1)}}>click me</button>
        </div>
    )
}
export default Example;
~~~

# 代替生命周期函数

## componentDidMount`和`componentDidUpdate`

useEffect函数代替两个生命周期函数`componentDidMount`和`componentDidUpdate`

原来的写法：

~~~js
import React, { Component } from 'react';

class Example3 extends Component {
    constructor(props) {
        super(props);
        this.state = { count:0 }
    }


    componentDidMount(){
        console.log(`ComponentDidMount=>You clicked ${this.state.count} times`)
    }
    componentDidUpdate(){
        console.log(`componentDidUpdate=>You clicked ${this.state.count} times`)
    }

    render() { 
        return (
            <div>
                <p>You clicked {this.state.count} times</p>
                <button onClick={this.addCount.bind(this)}>Chlick me</button>
            </div>
        );
    }
    addCount(){
        this.setState({count:this.state.count+1})
    }
}

export default Example3;
~~~

hooks写法：

~~~js
import React, { useState , useEffect } from 'react';
function Example(){
    const [ count , setCount ] = useState(0);
    //---关键代码---------start-------
    useEffect(()=>{
        console.log(`useEffect=>You clicked ${count} times`)
    })
    //---关键代码---------end-------

    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={()=>{setCount(count+1)}}>click me</button>
        </div>
    )
}
export default Example;
~~~

[useEffect两个注意点](https://jspang.com/detailed?id=50#toc311)

1. React首次渲染和之后的每次渲染都会调用一遍`useEffect`函数，而之前我们要用两个生命周期函数分别表示首次渲染(componentDidMonut)和更新导致的重新渲染(componentDidUpdate)。
2. useEffect中定义的函数的执行不会阻碍浏览器更新视图，也就是说这些函数时异步执行的，而`componentDidMonut`和`componentDidUpdate`中的代码都是同步执行的。个人认为这个有好处也有坏处吧，比如我们要根据页面的大小，然后绘制当前弹出窗口的大小，如果时异步的就不好操作了。

## componentWillUnmount解绑

在写React应用的时候，在组件中经常用到`componentWillUnmount`生命周期函数（组件将要被卸载时执行）。比如我们的定时器要清空，避免发生内存泄漏;比如登录状态要取消掉，避免下次进入信息出错。所以这个生命周期函数也是必不可少的

useEffect函数加入返回值就实现了componentWillUnmount：

```js
function Index() {
    useEffect(()=>{
        console.log('useEffect=>老弟你来了！Index页面')
        return ()=>{
            console.log('老弟，你走了!Index页面')
        }
    })
    return <h2>JSPang.com</h2>;
  }
```

但此时只要状态发生变化都会进行解绑，如果要实现当组件将被销毁时才进行解绑：就需要请出`useEffect`的第二个参数，它是一个数组，数组中可以写入很多状态对应的变量，意思是当状态值发生变化时，我们才进行解绑。但是当传空数组`[]`时，就是当组件将被销毁时才进行解绑，这也就实现了`componentWillUnmount`的生命周期函数。

~~~js
function Index() {
    useEffect(()=>{
        console.log('useEffect=>老弟你来了！Index页面')
        return ()=>{
            console.log('老弟，你走了!Index页面')
        }
    },[])
    return <h2>JSPang.com</h2>;
}
~~~

```js
function Example(){
    const [ count , setCount ] = useState(0);

    useEffect(()=>{
        console.log(`useEffect=>You clicked ${count} times`)

        return ()=>{
            console.log('====================')
        }
    },[count])

    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={()=>{setCount(count+1)}}>click me</button>

            <Router>
                <ul>
                    <li> <Link to="/">首页</Link> </li>
                    <li><Link to="/list/">列表</Link> </li>
                </ul>
                <Route path="/" exact component={Index} />
                <Route path="/list/" component={List} />
            </Router>
        </div>
    )
}
```

这时候只要`count`状态发生变化，都会执行解绑副作用函数，浏览器的控制台也就打印出了一串`=================`



# useContext父子组件传值

在用类声明组件时，父子组件的传值是通过组件属性和`props`进行的，那现在使用方法(Function)来声明组件，已经没有了`constructor`构造函数也就没有了props的接收，那父子组件的传值就成了一个问题。`React Hooks` 为我们准备了`useContext`。这节课就学习一下`useContext`，它可以帮助我们跨越组件层级直接传递变量，实现共享。需要注意的是`useContext`和`redux`的作用是不同的，一个解决的是组件之间值传递的问题，一个是应用中统一管理状态的问题，但通过和`useReducer`的配合使用，可以实现类似`Redux`的作用。

`Context`的作用就是对它所包含的组件树提供全局共享数据的一种技术。



```js
import React, { useState , createContext, useContext  } from 'react';
//===关键代码
const CountContext = createContext()
//子组件
function Counter(){
    const count = useContext(CountContext)  //一句话就可以得到count
    return (<h2>{count}</h2>)
}
//父组件：
function Example4(){
    const [ count , setCount ] = useState(0);

    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={()=>{setCount(count+1)}}>click me</button>
            {/*======关键代码 */把状态值传给子组件}
            <CountContext.Provider value={count}>
                <Counter />
            </CountContext.Provider>

        </div>
    )
}
export default Example4;
```

# useReducer代替redux

上节课学习了`useContext`函数，那这节课开始学习一下`useReducer`，因为他们两个很像，并且合作可以完成类似的Redux库的操作。在开发中使用`useReducer`可以让代码具有更好的可读性和可维护性，并且会给测试提供方便。那我们彻底的学习一下`useReducer`

reducer是什么？

```js
function countReducer(state, action) {
    switch(action.type) {
        case 'add':
            return state + 1;
        case 'sub':
            return state - 1;
        default: 
            return state;
    }
}
```

上面的代码就是Reducer，你主要理解的就是这种形式和两个参数的作用，一个参数是状态，一个参数是如何控制状态。

useReducer函数有两个输入参数，1个是上面的功能函数，第二个是默认值（初始值），有两个返回值，第一个是被处理的状态变量（这里是count），第二个是前面redux说过的dispatch:

```js
import React, { useReducer } from 'react';

function ReducerDemo(){
    const [ count , dispatch ] =useReducer((state,action)=>{
        switch(action){
            case 'add':
                return state+1
            case 'sub':
                return state-1
            default:
                return state
        }
    },0)
    return (
       <div>
           <h2>现在的分数是{count}</h2>
           <button onClick={()=>dispatch('add')}>Increment</button>
           <button onClick={()=>dispatch('sub')}>Decrement</button>
       </div>
    )

}

export default ReducerDemo
```



# 代替redux的案例

[理论上的可行性](https://jspang.com/detailed?id=50#toc322)

我们先从理论层面看看替代`Redux`的可能性，其实如果你对两个函数有所了解，只要我们巧妙的结合，这种替代方案是完全可行的。

`useContext`：可访问全局状态，避免一层层的传递状态。这符合`Redux`其中的一项规则，就是状态全局化，并能统一管理。

`useReducer`：通过action的传递，更新复杂逻辑的状态，主要是可以实现类似`Redux`中的`Reducer`部分，实现业务逻辑的可行性。

经过我们在理论上的分析是完全可行的，接下来我们就用一个简单实例来看一下具体的实现方法。那这节课先实现`useContext`部分（也就是状态共享），下节再继续讲解`useReducer`部分（控制业务逻辑）。

主页面：

~~~js
import React, { useReducer } from 'react';
import ShowArea from './showArea';
import Buttons from './Buttons';
import {Color} from "./color";

//父组件

function Example2(){
    return (
        <div>
        包含在color标签里的组件可以继承color里的属性
            <Color>
                <ShowArea />
                <Buttons />
            </Color>
        </div>
    )
}

export default Example2
~~~

color：

~~~js
import React, { createContext } from 'react';

export const ColorContext = createContext({})

export const Color = props=>{
    return (
        <ColorContext.Provider value={{color:"red"}}>
            {/*表示Color的子组件会继承当前的状态变量*/}
            {props.children}
        </ColorContext.Provider>
    )
}
~~~

组件：showarea  接受color里面的全局参数

```js
import React,{useContext} from 'react';
import {ColorContext} from "./color";

function ShowArea(){
    const {color}=useContext(ColorContext)
    return (<div style={{color:color}}>字体颜色为blue</div>)

}
export default ShowArea
```



现添加按钮功能：按第一个按钮字体变为黄色，按第二个字体变为蓝色，主页面和showarea组件不变

修改color.js:（这样就很方便，在这一个文件里定义所有的全局变量，以及修改他们的方法，其她组件想用，修改这些变量，就放到color标签里面，用const { xxx,xxx} = useContext(ColorContext)）来获取里面的变量或者方法，xxx既可以是变量，又可以是方法，而且名字和定义时名字一样就行 

~~~js
import React, { createContext,useReducer } from 'react';

export const ColorContext = createContext({})

export const UPDATE_COLOR = "UPDATE_COLOR"

const reducer= (state,action)=>{
    switch(action.type){
        case UPDATE_COLOR:
            return action.color
        default:
            return state
    }
}
export const Color = props=>{
    const [color,dispatch]=useReducer(reducer,'blue')
    return (
        <ColorContext.Provider value={{color,dispatch}}>
            {props.children}
        </ColorContext.Provider>
    )
}
~~~

按钮定义如下：

~~~js
import React ,{useContext} from 'react';
import {ColorContext,UPDATE_COLOR} from './color'

function Buttons(){
    const { dispatch } = useContext(ColorContext)
    return (
        <div>
            <button onClick={()=>{dispatch({type:UPDATE_COLOR,color:"red"})}}>红色</button>
            <button onClick={()=>{dispatch({type:UPDATE_COLOR,color:"yellow"})}}>黄色</button>
        </div>
    )
}

export default Buttons
~~~

# useMemo解决子组件重复更新

就等于之前的ShouldComponentUpdate生命周期函数