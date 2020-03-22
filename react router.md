---
typora-copy-images-to: 笔记图像
typora-root-url: 笔记图像
---

# 简介以及使用方法

在项目的根目录下下载：

~~~
yarn add react-router-dom
~~~

## 实例1

简单的跳转功能实现如下：

<img src="/image-20200214210400953.png" alt="image-20200214210400953" style="zoom: 80%;" />

两个子组件：首页和列表页

首页：

~~~js
import React, {Component} from 'react';

class Index extends Component {
    render() {
        return (
            <div>
                <h2>index</h2>
            </div>
        );
    }
}

export default Index;
~~~

列表页：

~~~js
import React, {Component} from 'react';

class List extends Component {
    render() {
        return (
            <div>
                <h2>list</h2>
            </div>
        );
    }
}

export default List;
~~~

承载他们的父组件：

~~~js
import React from "react";
import {BrowserRouter as Router,Route,Link}from 'react-router-dom'
import Index from "./pages";
import List from "./pages/list";

function AppRouter(){
    return (
        <Router>
            <ul>
                {/*link在router中相当于a标签*/}
                <li><Link to="/">首页</Link></li>
                <li><Link to="/list/">列表</Link></li>
            </ul>
            {/*exact是精确匹配，指在浏览器地址栏必须打"/"才能找到首页*/}
            {/*如果精确匹配，则只要包含"/"就可以找到首页*/}
            {/*一般首页需要精确匹配，其他页不太需要*/}
            <Route path="/" exact component={Index} />
            <Route path="/list/"  component={List} />
        </Router>
    )
}
export default AppRouter
~~~

## 动态传值

四步骤：

设置规则，传递值，接收值，显示内容

规则设置：修改上面的父组件（只改一行）

~~~js
<Route path="/list/:id"  component={List} />
~~~

即像许多网站某一页地址栏显示的id=xxxxxx一样，这里要求输入id才能找到list

修改首页：

~~~js
import React, {Component} from 'react';
import {Link} from "react-router-dom";

class Index extends Component {
    constructor(props) {
        super(props);
        this.state={
            list:[
                {cid:123,title:"dzy的个人博客-1"},
                {cid:456,title:"dzy的个人博客-2"},
                {cid:789,title:"dzy的个人博客-3"}
            ]
        }
    }
    render() {
        return (
            <div>
                <h2>index</h2>
                <ul>
                    {
                        this.state.list.map((item,index)=>{
                            return (
                                <li key={index}>
                                    <Link to={'/list/'+item.cid}>
                                        {item.title}
                                    </Link>
                                </li>
                            )
                        })
                    }
                </ul>
            </div>
        );
    }
}

export default Index;
~~~

列表页;

~~~js
import React, {Component} from 'react';

class List extends Component {
    constructor(props) {
        super(props);
        this.state={ }
    }
    render() {
        return (
            <div>
                <h2>list->{this.state.id}</h2>
            </div>
        );
    }

    componentDidMount() {
        console.log(this.props)
        let tempID=this.props.match.params.id
        this.setState({id:tempID})
    }

}

export default List;
~~~

效果：

<img src="/image-20200215114939887.png" alt="image-20200215114939887" style="zoom:67%;" />

点击每个博客的链接，都会给list传入一个id，从而显示不同的页面

<img src="/image-20200215115113131.png" alt="image-20200215115113131" style="zoom:80%;" />

## 重定向redirect

重定向,比如lol官网有活动，首页换成活动的页面，需要把首页重定向到这个新的页面：

方法：先增加新页面的路由：

新写一个Home.js  然后在父组件中增加：

~~~js
import Home from "./pages/Home";

......

<Route path="/Home"  component={Home} />
~~~

然后在首页中有两种方法把首页重定向为新页：



法1：直接在render，return中加一个Redirect标签（需要引入）

~~~js
<Redirect to='/home/'/>
~~~

法2：在首页的构造函数中加一句：

~~~js
this.props.history.push("/home/")
~~~



## 嵌套路由

效果：有一级导航，二级导航等不怎么变的，还有一直在变的部分

<img src="/image-20200215121421663.png" alt="image-20200215121421663" style="zoom: 50%;" />

案例效果：

<img src="/image-20200215160123689.png" alt="image-20200215160123689" style="zoom:80%;" />

实现的方法：一级菜单配置二级菜单的路由，二级菜单配置三级菜单的路由



## 后台动态获取路由进行配置

根据后台传来的数据构建页面：

模拟ajax传来的数据:

~~~js
let routeConfig=[
    {path:'/',title:'博客首页',exact:true,component:Index},
    {path:'/video',title:'视频教程',exact:false,component:Video},
    {path:'/workplace',title:'职场技能',exact:false,component:WorkPlace}
]
~~~

然后render出来：

~~~js
<ul>
    {
     routeConfig.map((item,index)=>{
          return(
          <li key={index}>
              <Link to={item.path}>{item.title}</Link>
          </li>
                                            )
                                        }
                                    )
    }
    下面是原本静态的写死的办法
    {/*<li><Link to="/">博客首页</Link></li>*/}
    {/*<li><Link to="/video">视频教程</Link></li>*/}
    {/*<li><Link to="/workplace">职场技能</Link></li>*/}
</ul>
~~~

