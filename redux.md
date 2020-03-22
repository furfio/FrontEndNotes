---
typora-root-url: 笔记图像
typora-copy-images-to: 笔记图像
---

# redux简介（Flux的升级版本）

<img src="/image-20200213162629855.png" alt="image-20200213162629855" style="zoom: 50%;" />

Redux是一个用来管理管理数据状态和UI状态的JavaScript应用工具。随着JavaScript单页应用（SPA）开发日趋复杂，JavaScript需要管理比任何时候都要多的state（状态），Redux就是降低管理难度的。（Redux支持React，Angular、jQuery甚至纯JavaScript）

## 工作流程

<img src="/image-20200213163442062.png" alt="image-20200213163442062" style="zoom:50%;" />

解释上面的图：（客官）

<img src="/image-20200213163536302.png" alt="image-20200213163536302" style="zoom:50%;" />

或者：

<img src="/image-20200213163830125.png" alt="image-20200213163830125" style="zoom:50%;" />

## 注意事项

reducer必须是纯函数

先来看什么是纯函数，纯函数定义：

> 如果函数的调用参数相同，则永远返回相同的结果。它不依赖于程序执行期间函数外部任何状态或数据的变化，必须只依赖于其输入参数。

这个应该是大学内容，你可能已经忘记了，其实你可以简单的理解为返回的结果是由传入的值决定的，而不是其它的东西决定的。

它的返回结果，是完全由传入的参数`state`和`action`决定的，这就是一个纯函数。这个在实际工作中是如何犯错的？比如在`Reducer`里增加一个异步ajax函数，获取一些后端接口数据，然后再返回，这就是不允许的（包括你使用日期函数也是不允许的），因为违反了调用参数相同，返回相同的纯函数规则。

eg：

```js
export default (state = defaultState,action)=>{
    if(action.type === CHANGE_INPUT){
        let newState = JSON.parse(JSON.stringify(state)) //深度拷贝state
        newState.inputValue = action.value
        //newState.inputValue ='aaaa'  错
        //newState.inputValue = new Date 错
        return newState
    }
```

返回的newState完全由输入值action决定，不能自己乱改

那么，既然不能直接在reducer中获取后台数据然后改变state，该怎么获取后台数据呢？

其实也简单，就不在reducer中获取异步的后台数据就行了，而是在生命周期函数中获得好后传给reducer

先定义功能函数：

```js
export const getListAction  = (data)=>({
    type:GET_LIST,
    data
})
```

然后在生命周期函数中获取后台数据，把取得的数据传给reducer：

```js
componentDidMount(){
    axios.get('https://www.easy-mock.com/mock/5cfcce489dc7c36bd6da2c99/xiaojiejie/getList').then((res)=>{    
        const data = res.data
        const action = getListAction(data)
        store.dispatch(action)
    })
}
```

接下来reducer进行处理：

```js
if(action.type === GET_LIST ){ //根据type值，编写业务逻辑
        let newState = JSON.parse(JSON.stringify(state)) 
        newState.list = action.data.data.list //复制性的List数组进去
        return newState
    }
```



# 环境搭建

安装ant Design

学习antd官网：

https://ant.design/components/list-cn/



chrome 插件：redux devtools  插件的使用方法：

https://github.com/zalmoxisus/redux-devtools-extension

在项目根目录下

~~~shell
npm install -save antd
或者
yarn add antd
yarn add redux
~~~



# 简单案例演示

## 项目功能

输入框打字，点击按钮，输入的内容就会加入到列表中，点击列表项就会删除对应的项目

## 组件：

基本流程：组件中的函数（即如何改变state）dispatch，让reducer来具体处理，reducer复制state，再传回新的state，然后组件还需要subscribe这个变化，this.state才会真正的更新

~~~javascript
import React, {Component} from 'react';
import 'antd/dist/antd.css';
import {Input,Button,List} from "antd";
import store from "./store";

// 管理state的方法，reducer中放的数据=>store=>todolist
class TodoList extends Component {

    constructor(props) {
        super(props);
        // console.log(store.getState())
        this.state=store.getState()

        //绑定this与changeInputValue方法，使其能够对this中的数据做处理
        // 每一个方法都要这样绑定一下
        this.changeInputValue= this.changeInputValue.bind(this)
        this.storeChange=this.storeChange.bind(this)
        this.addItem=this.addItem.bind(this)
        //删除函数的绑定在下面，和输入的参数一起bind，这里就不用bind了
        //this.deleteItem=this.deleteItem.bind(this)

        //让store跟踪从reducer中传来的state的变化
        store.subscribe(this.storeChange)

    }

    render() {
        return (
            <div style={{margin:'10px'}}>
                <div>
                    <Input
                        placeholder={this.state.inputValue}
                        style={{width:'550px' ,marginRight:'10px'}}
                        onChange={this.changeInputValue}
                        value={this.state.inputValue}
                    />
                    <Button
                        type='primary'
                        onClick={this.addItem}
                    >增加</Button>
                </div>
                <div style={{margin:'10px',width:'300px'}}>
                    <List
                        bordered
                        dataSource={this.state.list}
                        //这里把index和this作为了参数，上面就不用bind了
                        renderItem={(item,index)=> (<List.Item onClick={this.deleteItem.bind(this,index)}>{item}</List.Item>)}
                        //以上是antd的官方写法
                    />
                </div>
            </div>
        );
    }

    changeInputValue(e){
        console.log(e.target.value)
        //e.target.value指输入的内容
        //下面就是redux中的action
        const action={
            type:'changeInput',//type就是action的名字
            value:e.target.value //value指要变成什么值，这里变成输入框中的输入
        }
        store.dispatch(action)
    }

    addItem(){
        const action={
            type:'addItem',//type就是action的名字
        }
        store.dispatch(action)
    }
    deleteItem(index){
        const action={
            type:'deleteItem',//type就是action的名字
            index
        }
        store.dispatch(action)
    }

    //下面函数的作用：reducer改变了state后，组件需要通过store更新组件自身的state
    storeChange(){
        this.setState(store.getState())
    }
}

export default TodoList;
~~~

## store

这部分代码基本是死的，就是作为reducer和组件的中间组件，其中windo那一行是开启chrome的redux插件

~~~javascript
import {createStore} from "redux";
import reducer from "./reducer";
const store=createStore(reducer,
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__())
export default store

~~~

## reducer

预定义state，state的改变也是在这里

~~~javascript
const defaultState={
    inputValue:'wwwwwwwww',
    list:[
        '8:00起床',
        '9:00吃饭',
        '10:00打游戏'
    ]
}
export default (state=defaultState,action)=>{

     console.log(state,action)
    //ruducer只能接收state，不能改变state，所以需要用局部变量深度拷贝state
    //然后改变此变量，再传回去
    switch (action.type) {
        case 'changeInput':{
            let newState=JSON.parse(JSON.stringify(state))
            newState.inputValue=action.value
            return newState
        }
        case 'addItem':{
            let newState=JSON.parse(JSON.stringify(state))
            newState.list.push(newState.inputValue)
            newState.inputValue='' //每次点完按钮后输入框变为空
            return newState
        }
        case 'deleteItem':{
            let newState=JSON.parse(JSON.stringify(state))
            newState.list.splice(action.index,1)
            return newState
        }

    }
    return state
}
~~~



