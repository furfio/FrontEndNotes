---
typora-root-url: 笔记图像
typora-copy-images-to: 笔记图像
---

# 1 指令

创建项目的指令：

~~~shell
create-react-app xxx
在当前目录中创建一个新文件夹xxx，并在里面下载react脚手架文件
~~~



npm小知识：以下四种添加依赖的区别

<img src="/image-20200206221144349.png" alt="image-20200206221144349" style="zoom:67%;" />

第一种没有参数，只安装到项目的node_modules目录中，不会添加依赖，即不会在package.json中写入依赖

第二种-g  ： 把包安装到全局，即安装到电脑磁盘上

第三种 -save ： 安装到node_modules中，且添加了依赖，写入json（生产环境）

第四种：-dev指开发模式中的依赖





rcc 一键生成如下代码：（即组件）

~~~javascript
import React, {Component} from 'react';

class GaigaiItem extends Component {
    render() {
        return (
            <div>

            </div>
        );
    }
}

export default GaigaiItem;
~~~

- this.state :数据
- this.props: 属性（可以传递属性与方法）



## 1.2工具

![image-20200205202540564](/image-20200205202540564.png)

react官方调试工具

如果网站使用react开发的，eg：知乎，这个插件会变成蓝色，如果是测试环境下的react界面，图标会变成红色

然后打开f12，会看到有对应的react项目调试栏，component等





# 2 目录介绍

![image-20200203122444055](/image-20200203122444055.png)

package.json：介绍项目的名称，版本，导入的包等

.lock文件：锁定项目创建时各导入的包的版本方便协作开发，防止大家不一样

.gitignore:  规定在上传到github时哪些文件不用上传（node_modules太大不用上传)

public文件夹里面的manifes.json是移动端的配置



主要工作区域：

![image-20200203123014757](/image-20200203123014757.png)

首先进入index.js  它又引入app,.js(简写为app)

logo.svg是一个动态图

serviceWorker.js用于设置第一次在线浏览过网页后，第二次即使没有网络也可以观看，（谷歌的PWA标准）



## 页面显示关系：

首先是public目录下的index.html 里有一个div：

~~~html
<div id="root"></div>
~~~

然后再src里的index.js里把各种组件挂载到首页的这个div里：这里挂载一个App.js

~~~javascript
ReactDOM.render(<App />,document.getElementById( 'root'))
//把app.js这一个组件挂载到public/index.html里的id为root的div中
~~~

然后就是App.js

~~~javascript
import React,{Component} from "react";//ES6的语法
//这句话=两句话：
//import React from 'react'
//const Component = React.Component

class App extends Component{
    render() {
        return(
            <div>
                Hello dzy
                {false?'来啦来啦':'dddd'}
            </div>
        )
    }
}

export default App
~~~



# 3 基本概念

## 3.1 单向数据流

即父组件传给子组件的数据，子组件不能修改，只能使用，如果非要改变，可以通过父组件传递一个方法来改变这个数据，例如5.2节中的案例，父组件把delete方法传给子组件

## 3.2 PropTypes校验传递值

父组件传给子组件的值，子组件应该加以验证，防止业务复杂时值错误

以下是对5.2中子组件的修改：其实就是在类的下面加一个校验的东西

~~~javascript
import React, {Component} from 'react';
import PropTypes from 'prop-types'

/*
* by dzy
* 组件实现的功能：输入框中输入要增加的菜品，点击按钮后，内容被添加到列表里面
* 点击列表项,删除对应的项目
* gaigai文件的组件拆分: 列表部分(子组件)
* */

class GaigaiItem extends Component {

    constructor(props) {
        super(props);
        //在构造方法里面绑定函数后,就不用在函数调用处绑定了
        //绑定是为了在函数里使用this.xxx
        this.handleClick=this.handleClick.bind(this)
    }

    render() {
        return (
            <div>
                <li onClick={this.handleClick}>
                    {this.props.content}
                </li>
            </div>
        );
    }

    handleClick(){
        //console.log(this.props.index)
        //调用父组件里的删除函数,删除点击的列表项
        this.props.deleteItem(this.props.index)
    }
}

//以下是对父组件传过来的值进行校验
GaigaiItem.propTypes={
    content:PropTypes.string,
    index:PropTypes.number,
    deleteItem:PropTypes.func
}

export default GaigaiItem;
~~~



## 3.3 ref属性

用于绑定页面元素与this.元素

详见 5.2中的父组件中的两处ref

## 3.4 React生命周期

<img src="/image-20200205213837386.png" alt="image-20200205213837386" style="zoom:110%;" />

初始化——DOM挂载——更新——DOM卸载

生命周期函数：在某一时刻可以自动执行的函数（如render）

<img src="C:\Users\17229\AppData\Roaming\Typora\typora-user-images\image-20200206215003608.png" alt="image-20200206215003608" style="zoom:67%;" />

<img src="/image-20200206215025748.png" alt="image-20200206215025748" style="zoom:67%;" />

可以在组件中手写以下几个函数：发现他们确实按顺序自动执行

<img src="/image-20200206215107202.png" alt="image-20200206215107202" style="zoom:67%;" />

开发中可以在这两个函数中添加打印日志或者查询数据库的功能

### 生命周期函数的应用

下述案例5.2的缺陷，render函数一直在渲染，造成性能浪费，渲染的东西多了会造成卡顿

修改，可以在render函数前面加以下函数：表示只有输入框的内容改变了才去渲染，否则不渲染

<img src="/image-20200206220839088.png" alt="image-20200206220839088" style="zoom:67%;" />



# 4 JSX语法介绍

jsx：javascript 和xml

即可以在js中写html，混搭两种语言

原则：遇到 () 当作html

遇到{}  当作JavaScript

jsx组件，如App, 要求首字母大写

eg：在这一段里混搭着html和js

~~~javascript
render() {
        return(
            <div>
                Hello dzy
            	{false?'来啦来啦':'dddd'}
            </div>
        )
    }
~~~

# 5 案例学习

## 5.1  增加，删除菜谱

~~~javascript
import React,{Component,Fragment} from "react";
import './gaigai.css'

/*
* by dzy
* 组件实现的功能：输入框中输入要增加的菜品，点击按钮后，内容被添加到列表里面
* 点击列表项,删除对应的项目
* */
class Gaigai extends Component{
    //用构造函数来初始化组件用到的数据this.state
    //react改变页面元素的内容就是依靠这些数据，
    //而不是像传统的js一样靠getElementbyID然后修改
    constructor(props) {
        super(props);
        this.state={
            inputValue:'菜名',
            list:['过油肉','酸菜鱼']
        }
    }

    render() {
        return(
           <Fragment>
                <div>
                    {/*绑定数据：输入框中的内容绑定thisthis.state.inputValue*/}
                    {/*输入框内容变化绑定下面的inputChange函数*/}
                    {/*htmlFor替换原来的for,className替换原来的class,因为与关键字冲突了*/}
                    {/*这里的label文本与输入框绑定一下,点一下文本光标就移动到输入框里面去了*/}
                    <label htmlFor="shuru">请输入菜名</label>
                    <input id="shuru" className="input" value={this.state.inputValue} onChange={this.inputChange.bind(this)}/>
                    <button onClick={this.addList.bind(this)}> 增加菜谱</button>
                </div>
                <ul>
                    {
                        // 遍历list中的内容: item和index(数组下标)
                        this.state.list.map((item,index)=>{
                            //这里的index+item是为了防止索引重复,用index+item降低重复概率
                            return(
                                <li key={index+item}
                                    onClick={this.deleteItem.bind(this,index)}
                                >
                                    {item}
                                </li>
                            )
                        })
                    }
                </ul>
           </Fragment>
        )
    }
    //函数功能:输入框的内容随着输入变化
    inputChange(e){
        // console.log(e.target.value) //可以在控制台看到输入内容的变化
        //用this.setState修改数据内容
        this.setState({
            inputValue:e.target.value
        })
    }

    //函数功能:点击按钮增加列表项
    addList(){
        this.setState({
            // "...this.state.list"是扩展运算符(ES6),相当于list中的全部内容
            //  即用inputValue扩展list中的内容
            list:[...this.state.list,this.state.inputValue],
            inputValue:''  //每次点击按钮之后,输入框应该清空,方便继续输入
        })
    }

    //函数功能:点击列表项删除列表项,
    //输入参数:list中的数组下标index
    deleteItem(index){
        //这里有一个坑: 删除数据时应该先把this.state.list数据传递给一个局部变量,修改局部变量
        //后再返回给this.state.list,而不是直接修改this.state.list值
        let list = this.state.list
        list.splice(index,1) //删除1位
        this.setState({
            list:list
        })
    }
}

export default Gaigai
~~~

## 5.2 把上诉案例拆分

为输入框部分和列表部分两个组件：

父组件（输入框部分）只改了ul里面的return的内容

~~~javascript
import React,{Component,Fragment} from "react";
import './gaigai.css'
import GaigaiItem from "./gaigai_item";

/*
* by dzy
* 组件实现的功能：输入框中输入要增加的菜品，点击按钮后，内容被添加到列表里面
* 点击列表项,删除对应的项目，控制台可以打印当前的列表项数
* gaigai文件的组件拆分: 输入框部分(即父组件)
* */
class Gaigai extends Component{
    //用构造函数来初始化组件用到的数据this.state
    //react改变页面元素的内容就是依靠这些数据，
    //而不是像传统的js一样靠getElementbyID然后修改
    constructor(props) {
        super(props);
        this.state={
            inputValue:'菜名',
            list:['过油肉','酸菜鱼']
        }
    }

    render() {
        return(
            <Fragment>
                <div>
                    {/*绑定数据：输入框中的内容绑定thisthis.state.inputValue*/}
                    {/*输入框内容变化绑定下面的inputChange函数*/}
                    {/*htmlFor替换原来的for,className替换原来的class,因为与关键字冲突了*/}
                    {/*这里的label文本与输入框绑定一下,点一下文本光标就移动到输入框里面去了*/}
                    <label htmlFor="shuru">请输入菜名</label>
                    <input id="shuru"
                           className="input"
                           value={this.state.inputValue}
                           onChange={this.inputChange.bind(this)}
                           // 下面这句把输入内容与this.input绑定
                           // 用于inputchange函数里，实时根据用户输入改变输入框中的内容
                           ref = {(input)=>{this.input=input}}
                    />
                    <button onClick={this.addList.bind(this)}> 增加菜谱</button>
                </div>
                //下面这个用ref把this.ul与ul进行绑定，用于显示ul中有几项li
                <ul ref={(ul)=>{this.ul=ul}}>
                    {
                        // 遍历list中的内容: item和index(数组下标)
                        this.state.list.map((item,index)=>{
                            //这里的index+item是为了防止索引重复,用index+item降低重复概率
                            return(
                                //把父组件里的内容传给子组件
                                <GaigaiItem
                                    key={index+item}
                                    content={item}
                                    index={index}
                                    deleteItem={this.deleteItem.bind(this)}
                                />
                            )
                        })
                    }
                </ul>
            </Fragment>
        )
    }
    //函数功能:输入框的内容随着输入变化
    inputChange(){
        // console.log(e.target.value) //可以在控制台看到输入内容的变化
        //用this.setState修改数据内容
        this.setState({
            inputValue:this.input.value
        })
    }

    //函数功能:点击按钮增加列表项
    addList(){
        this.setState({
            // "...this.state.list"是扩展运算符(ES6),相当于list中的全部内容
            //  即用inputValue扩展list中的内容
            list:[...this.state.list,this.state.inputValue],
            inputValue:''  //每次点击按钮之后,输入框应该清空,方便继续输入
        }, 
            //以下是一个回调函数，用于显示ul中有几项li
            //之所以用回调是因为setstate改变数据是一个异步的方法，它的执行是需要时间的
            //它还没执行，下面的打印数字到控制台就已经执行了，所以会少数一项
            ()=>{
            console.log(this.ul.querySelectorAll('li').length)
            }
        )
    }

    //函数功能:点击列表项删除列表项,
    //输入参数:list中的数组下标index
    deleteItem(index){
        //这里有一个坑: 删除数据时应该先把this.state.list数据传递给一个局部变量,修改局部变量
        //后再返回给this.state.list,而不是直接修改this.state.list值
        let list = this.state.list
        list.splice(index,1) //删除1位
        this.setState({
            list:list
        })
    }
}

export default Gaigai
~~~



子组件部分：（即列表部分）

~~~javascript
import React, {Component} from 'react';

/*
* by dzy
* 组件实现的功能：输入框中输入要增加的菜品，点击按钮后，内容被添加到列表里面
* 点击列表项,删除对应的项目
* gaigai文件的组件拆分: 列表部分(子组件)
* */
class GaigaiItem extends Component {

    constructor(props) {
        super(props);
        //在构造方法里面绑定函数后,就不用在函数调用处绑定了
        //绑定是为了在函数里使用this.xxx
        this.handleClick=this.handleClick.bind(this)
    }

    render() {
        return (
            <div>
                <li onClick={this.handleClick}>
                    {this.props.content}
                </li>
            </div>
        );
    }

    handleClick(){
        //console.log(this.props.index)
        //调用父组件里的删除函数,删除点击的列表项
        this.props.deleteItem(this.props.index)
    }
}

export default GaigaiItem;
~~~



# 6 动画

![image-20200206222614006](/image-20200206222614006.png)



