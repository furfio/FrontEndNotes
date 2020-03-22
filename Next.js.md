---
typora-copy-images-to: 笔记图像
typora-root-url: 笔记图像
---

# 简介

当前用react或者vue等做的页面大多是SPA（single page web application）

单一页面，首屏加载过慢，而且不能SEO（搜索引擎优化，否则搜索引擎搜不到）

于是出现了Next.js进行服务端渲染

- 完善的React项目架构，搭建轻松。比如：Webpack配置，服务器启动，路由配置，缓存能力，这些在它内部已经完善的为我们搭建完成了。
- 自带数据同步策略，解决服务端渲染最大难点。把服务端渲染好的数据，拿到客户端重用，这个在没有框架的时候，是非常复杂和困难的。有了Next.js，它为我们提供了非常好的解决方法，让我们轻松的就可以实现这些步骤。（啥也不用管，前后端自动同步）
- 丰富的插件帮开发人员增加各种功能。每个项目的需求都是不一样的，包罗万象。无所不有，它为我们提供了插件机制，让我们可以在使用的时候按需使用。你也可以自己写一个插件，让别人来使用。
- 灵活的配置，让开发变的更简单。它提供很多灵活的配置项，可以根据项目要求的不同快速灵活的进行配置。

# 创建项目

```s
npx create-next-app xxx
```

项目目录，执行`yarn dev`进行测试

## [项目结构介绍](https://jspang.com/detailed?id=51#toc39)

看到结果后，用VSCode打开目录，可以看到已经有了很多自动建立好的文件和文件夹，下面就简单的介绍一下这些它们的用处：

- components文件夹:这里是专门放置自己写的组件的，这里的组件不包括页面，指公用的或者有专门用途的组件。
- node_modules文件夹：Next项目的所有依赖包都在这里，一般我们不会修改和编辑这里的内容。
- pages文件夹：这里是放置页面的，这里边的内容会自动生成路由，并在服务器端渲染，渲染好后进行数据同步。
- static文件夹： 这个是静态文件夹，比如项目需要的图片、图标和静态资源都可以放到这里。
- .gitignore文件： 这个主要是控制git提交和上传文件的，简称就是忽略提交。
- package.json文件：定义了项目所需要的文件和项目的配置信息（名称、版本和许可证），最主要的是使用`npm install` 就可以下载项目所需要的所有包。

## [新建页面和访问路径](https://jspang.com/detailed?id=51#toc311)

直接在根目录下的`pages`文件夹下，新建一个`jspang.js`页面。然后写入下面的代码：

```js
function Jspang(){
    return (<button>技术胖</button>)
}

export default  Jspang;
```

只要写完上面的代码，`Next`框架就自动作好了路由，这个也算是Next的一个重要优点，给我们节省了大量的时间。

现在要作一个更深的页面，比如把有关博客的界面都放在这样的路径下`http://localhost:3000/blog/nextBlog`,其实只要在`pages`文件夹下再建立一个新的文件夹`blog`，然后进入`blog`文件夹，新建一个`nextBlog.js`文件，就可以实现了。

nextBlog.js文件内容,我们这里就用最简单的写法了

```js
export default ()=><div>nextBlog page</div>
```

写完后，就可以直接在浏览器中访问了，是不是发现Next框架真的减轻了我们大量的工作。

## [Component组件的制作](https://jspang.com/detailed?id=51#toc312)

制作组件也同样方便，比如要建立一个jspang组件，直接在`components`目录下建立一个文件`jspang.js`,然后写入下面代码:（{children}是把children解构出来，children是指此组件中包含的内容（子组件），下面调用时<Jspang>按钮</Jspang>按钮二字就是这里的children，所以两个字会传递给button上面显示的字）

```js
export default ({children})=><button>{children}</button>
```

组件写完后需要先引入，比如我们在Index页面里进行引入：

```js
import Jspang from '../components/jspang'
```

使用就非常简单了，直接写入标签就可以。

```js
<Jspang>按钮</Jspang>
```

一个自定义组件的创建和使用也是这么简单， 如果你React的基础很好，那这节课的内容对你来说就更加简单了。也就是说Next框架并没有给我们带来太多的学习成本，但是为我们减轻了很多配置工作。

# 路由基础和基本跳转

## 标签式跳转< Link >

在编写代码之前，先删除`index.js`中的代码，保证代码的最小化。使用标签式导航需要先进行引入，代码如下:

```js
import Link from 'next/link'
```

然后新建两个页面`jspangA.js`和`jspangB.js`，新建后写个最简单的页面，能标识出来A、B两个页面就好。

```js
//jspangA.js
import Link from 'next/link'

export default ()=>(
    <>
        <div>Jspang-A page .  </div>
        <Link href="/"><a>返回首页</a></Link>
    </>
)
```

写完A页面后，可以直接复制A页面的内容，然后修改一下就是B页面。

```js
//jspangB.js

import Link from 'next/link'

export default ()=>(
    <>
        <div>Jspang-B page .  </div>
        <Link href="/"><a>返回首页</a></Link>
    </>
)
```

有了两个页面后，可以编写首页的代码，实现跳转了。

```js
//index.js
import React from 'react'
import Link from 'next/link'


const Home = () => (
  <>
    <div>我是首页</div>
    <div><Link href="/jspangA"><a>去JspangA页面</a></Link></div>
    <div><Link href="/jspangB"><a>去JspangB页面</a></Link></div>

  </>
)

export default Home
```

用``标签进行跳转是非常容易的，但是又一个小坑需要你注意一下，就是他不支持兄弟标签并列的情况。

```js
 <div>
  <Link href="/jspangA">
    <span>去JspangA页面</span>
    <span>前端博客</span>
  </Link>
</div>
```

如果这样写会直接报错，报错信息如下

```s
client pings, but there's no entry for page: /_error
Warning: You're using a string directly inside <Link>. This usage has been deprecated. Please add an <a> tag as child of <Link>
```

但是你可以把这两个标签外边套一个父标签，就可以了，比如下面的代码就没有错误。

```js
<Link href="/jspangA">
  <a>
    <span>去JspangA页面</span>
    <span>前端博客</span>
  </a>
</Link>
```

通过标签跳转非常的简单，跟使用``标签几乎一样。那再来看看如何用编程的方式进行跳转。

## Router模块跳转

在`Next`框架中还可以使用Router模块进行编程式的跳转，使用前也需要我们引入`Router`，代码如下：

```js
import Router from 'next/router'
```

然后在`Index.js`页面中加入，直接使用Router进行跳转就可以了。

```js
 <div>
    <button onClick={()=>{Router.push('/jspangA')}}>去JspangA页面</button>
  </div>
```

这样写只是简单，但是还是耦合性太高，跟Link标签没什么区别，你可以修改一下代码，把跳转放到一个方法里，然后调用方法。

```js
import React from 'react'
import Link from 'next/link'
import Router from 'next/router'
const Home = () => {
  function gotoA(){
    Router.push('/jspangA')
  }
  return(
    <>
      <div>我是首页</div>
      <div>
        <Link href="/jspangA">
          <a>
            <span>去JspangA页面</span>
            <span>前端博客</span>
          </a>
        </Link>
      </div>
      <div><Link href="/jspangB"><a>去JspangB页面</a></Link></div>
      <div>
        <button onClick={gotoA}>去JspangA页面</button>
      </div>
    </>
  )

}
export default Home
```

# 路由-跳转时用query传递和接受参数

## 标签式

项目开发中一般都不是简单的静态跳转，而是需要动态跳转的。动态跳转就是跳转时需要带一个参数或几个参数过去，然后在到达的页面接受这个传递的参数，并根据参数不同显示不同的内容。比如新闻列表，然后点击一个要看的新闻就会跳转到具体内容。这些类似这样的需求都都是通过传递参数实现的。

在`Next.js`中只能通过通过query（`?id=1`）来传递参数，而不能通过(`path:id`)的形式传递参数，这个一定要记住，在工作中很容易就容易记混。

现在我们改写一下pages文件夹下的`index.js`文件。

```js
import React from 'react'
import Link from 'next/link'
import Router from 'next/router'
const Home = () => {
  return(
    <>
      <div>我是首页</div>
      <div>
        <Link href="/xiaojiejie?name=波多野结衣"><a>选波多野结衣</a></Link><br/>
        <Link href="/xiaojiejie?name=苍井空"><a>选苍井空</a></Link>
      </div>
    </>
  )

}
export default Home
```

这样编写query参数就可以进行传递过去了，接下来就是要接受参数了。

[接收传递过来的参数](https://jspang.com/detailed?id=51#toc318)

现在还没有小姐姐对应的页面，所以我们要创建`xiaojiejie.js`页面，并写下下面的代码。

```js
import { withRouter} from 'next/router'
import Link from 'next/link'

const Xiaojiejie = ({router})=>{
    return (
        <>
            <div>{router.query.name},来为我们服务了 .</div>
            <Link href="/"><a>返回首页</a></Link>
        </>
    )
}

export default withRouter(Xiaojiejie)
```

`withRouter`是Next.js框架的高级组件，用来处理路由用的，这里先学简单用法，以后还会学习的。通过这种方式就获得了参数，并显示在页面上了。

## 函数式

回了``这种标签式跳转传递参数的形式，那编程式跳转如何传递那，其实也可以简单使用`?加参数`的形式，代码如下：

```js
 <div>
  <button onClick={gotoXiaojiejie}>选井空</button>
</div>
// gotoXiaojiejie
function gotoXiaojiejie(){
    Router.push('/xiaojiejie?name=井空')
  }
```

这种形式跳转和传递参数是完全没有问题的，但是不太优雅（优雅这东西很难界定，其实你完全可以看成一种装X，这太简单了，我需要装个X），所以也可以写成`Object`的形式。

```js
 function gotoXiaojiejie(){
    Router.push({
      pathname:'/xiaojiejie',
      query:{
        name:'井空'
      }
    })
  }
```

嗯，这样写确实优雅很多(我们一定要面向对象编程，有对象比没对象要好)。

其实``标签也可以写成这种形式，比如我们把第一个修改成这种形式.

```js
<Link href={{pathname:'/xiaojiejie',query:{name:'结衣'}}}><a>选结衣</a></Link><br/>
```

在浏览器中预览一下，如果一切正常是可以顺利进行跳转，并接收到传递的值。这节课主要讲解了Next框架的路由跳转时带参数过去，然后用`withRouter`进行接收。



# 路由的6个钩子事件

路由的钩子事件，也就是当路由发生变化时，可以监听到这些变化事件，执行对应的函数。利用钩子事件是可以作很多事情的，比如转换时的加载动画，关掉页面的一些资源计数器.....。

## routeChangeStart路由发生变化时

在监听路由发生变化（即页面跳转时）时，我们需要用Router组件，然后用`on`方法来进行监听,在`pages`文件夹下的`index.js`，然后写入下面的监听事件，这里我们只打印一句话，就不作其他的事情了。代码如下：

```js
   Router.events.on('routeChangeStart',(...args)=>{
    console.log('1.routeChangeStart->路由开始变化,参数为:',...args)
  })
```

这个时路由发生变化时，时间第一时间被监听到，并执行了里边的方法。

## routeChangeComplete路由结束变化时

```js
  Router.events.on('routeChangeComplete',(...args)=>{
    console.log('routeChangeComplete->路由结束变化,参数为:',...args)
  })
```

## beforeHistoryChange浏览器history触发前

history就是HTML中的API，Next.js`路由变化默认都是通过history进行的，所以每次都会调用。 不适用history的话，也可以通过hash

```js
  Router.events.on('beforeHistoryChange',(...args)=>{
    console.log('3,beforeHistoryChange->在改变浏览器 history之前触发,参数为:',...args)
  })
```

## routeChangeError路由跳转发生错误时

```js
Router.events.on('routeChangeError',(...args)=>{
    console.log('4,routeChangeError->跳转发生错误,参数为:',...args)
  })
```

需要注意的是404找不到路由页面不算错误。

## 转变成hash路由模式

还有两种事件，都是针对hash的，所以现在要转变成hash模式。hash模式下的两个事件`hashChangeStart`和`hashChangeComplete`,就都在这里进行编写了。

```js
  Router.events.on('hashChangeStart',(...args)=>{
    console.log('5,hashChangeStart->hash跳转开始时执行,参数为:',...args)
  })

  Router.events.on('hashChangeComplete',(...args)=>{
    console.log('6,hashChangeComplete->hash跳转完成时,参数为:',...args)
  })
```

在下面的jsx语法部分，再增加一个链接,使用hash来进行跳转，代码如下：

```js
<div>
    <Link href="#jspang"><a>选JSPang</a></Link>
</div>
```



# 在getInitialProps中用Axios获取远端数据

注意：这东西在子组件中不能用，只能在pages目录下的页面中用

**Note: `getInitialProps` can not be used in children components. Only in `pages`.**

So when I fetcehd that data at `index.js`, it successfully get the data I want to have.



在`Next.js`框架中提供了`getInitialProps`静态方法用来获取远端数据，这个是框架的约定，所以你也只能在这个方法里获取远端数据。不要再试图在声明周期里获得，虽然也可以在`ComponentDidMount`中获得，但是用了别人的框架，就要遵守别人的约定。

`Axios`是目前最火的前端获取数据的插件了，也是由大神首推的数据接口请求插件，所以这里依然使用`Axios`来进行远端数据请求。在请求前需要先安装`Axios`插件。

打开终端，直接使用yarn命令进行安装。

```shell
yarn add axios
```

我使用的版本是`0.19.0`,可能你学习的时候会稍有变化。安装完成后，在需要的页面中用`import`引入`axios`，代码如下：

```js
import axios from 'axios'
```

引入后，就可以使用`getInitialProps`进行获取后端接口数据了。

在`xiaojiejie.js`页面中使用`getInitialProps`，因为是远程获取数据，所以我们采用异步请求的方式。数据存在了`Easy Mock`中

```js
import { withRouter} from 'next/router'
import Link from 'next/link'
import axios from 'axios'

//这里的list，下面的data.data都是远程的数据，由于使用了getInitialProps，这个list可以直接用
const Xiaojiejie = ({router,list})=>{
    return (
        <>
            <div>{router.query.name},来为我们服务了 .<br/>{list}</div>
            <Link href="/"><a>返回首页</a></Link>
        </>
    )
}

Xiaojiejie.getInitialProps = async ()=>{
    const promise =new Promise((resolve)=>{
            axios('https://www.easy-mock.com/mock/5cfcce489dc7c36bd6da2c99/xiaojiejie/getList').then(
                (res)=>{
                    console.log('远程数据结果：',res)
                    resolve(res.data.data)
                }
            )
    })
    return await promise
}

export default withRouter(Xiaojiejie)
```

# 用Style JSX编写css样式

## 基本

在`Next.js`中引入一个CSS样式是不可以用的，如果想用，需要作额外的配置。因为框架为我们提供了一个`style jsx`特性，也就是把CSS用JSX的语法写出来，语法与css也一样。

加入了`Style jsx`代码后，`Next.js`会自动加入一个随机类名，这样就防止了CSS的全局污染。比如我们把代码写成下面这样，然后在浏览器的控制台中进行查看，你会发现自动给我们加入了类名，类似`jsx-xxxxxxxx`。

```js
function Jspang(){
    return (
        <>
            <div>技术胖免费前端教程</div>
            <div className="jspang">技术胖免费前端教程</div>

            <style jsx>
                {`
                    div { color:blue;}
                    .jspang {color:red;}
                `}
            </style>
        </>
    )
}
export default Jspang
```

## 动态显示样式（css不是写死的）

`Next.js`使用了`Style jsx`,所以定义动态的CSS样式就非常简单，比如现在要作一个按钮，点击一下，字体颜色就由蓝色变成了红色。下面是实现代码。

```js
import React, {useState} from 'react'

function Jspang(){
    //关键代码----------start-------
    const [color,setColor] = useState('blue')

    const changeColor=()=>{

        setColor(color=='blue'?'red':'blue')
    }
     //关键代码----------end-------

    return (
        <>
            <div>技术胖免费前端教程</div>
            <div><button onClick={changeColor}>改变颜色</button></div>
            <style jsx>
                {`
                    div { color:${color};}
                `}
            </style>
        </>
    )
}
export default Jspang
```

这样就完成了CSS的动态显示，是不是非常容易。这节课主要学习了`Style jsx`的一些知识，有了这些知识，可以让我们的页面开始漂亮起来了。

​	

# lazy loading实现模块懒加载

当项目越来越大的时候，模块的加载是需要管理的，如果不管理会出现首次打开过慢，页面长时间没有反应一系列问题。这时候可用`Next.js`提供的`LazyLoading`来解决这类问题。让模块和组件只有在用到的时候在进行加载，一般我把这种东西叫做“懒加载”.它一般分为两种情况，一种是懒加载（或者说是异步加载）模块，另一种是异步加载组件。他们使用的方法也稍有不同，下面我们就来分别学习一下。

## 加载别人的模块

这里使用一个在开发中常用的模块`Moment.js`，它是一个JavaScript日期处理类库，使用前需要先进行安装，这里使用`yarn`来进行安装。

```js
yarn add momnet
```

然后在`pages`文件夹下，新建立一个`time.js`文件，并使用刚才的`moment`库来格式化时间，代码如下:

```js
import React, {useState} from 'react'
import moment from 'moment'

function Time(){

    const [nowTime,setTime] = useState(Date.now())

    const changeTime=()=>{
        setTime(moment(Date.now()).format())
    }
    return (
        <>
            <div>显示时间为:{nowTime}</div>
            <div><button onClick={changeTime}>改变时间格式</button></div>
        </>
    )
}
export default Time
```

这个看起来很简单和清晰的案例，缺存在着一个潜在的风险，就是如何有半数以上页面使用了这个`momnet`的库，那它就会以公共库的形式进行打包发布，就算项目第一个页面不使用`moment`也会进行加载，这就是资源浪费，对于我这样有代码洁癖的良好程序员是绝对不允许的。下面我们就通过`Lazy Loading`来进行改造代码。

```js
import React, {useState} from 'react'
//删除import moment
function Time(){

    const [nowTime,setTime] = useState(Date.now())

    const changeTime= async ()=>{ //把方法变成异步模式
        const moment = await import('moment') //等待moment加载完成
        setTime(moment.default(Date.now()).format()) //注意使用defalut
    }
    return (
        <>
            <div>显示时间为:{nowTime}</div>
            <div><button onClick={changeTime}>改变时间格式</button></div>
        </>
    )
}
export default Time
```

这时候就就是懒加载了，可以在浏览器中按F12，看一下`Network`标签，当我们点击按钮时，才会加载`1.js`,它就是`momnet.js`的内容。

## 懒加载自定义组件

懒加载组件也是非常容易的，我们先来写一个最简单的组件，在`components`文件夹下建立一个`one.js`文件，然后编写如下代码：

```js
export default ()=><div>Lazy Loading Component</div>
```

有了自定义组件后，先要在懒加载这个组件的文件中引入`dynamic`,我们这个就在上边新建的`time.js`文件中编写了。

```js
import dynamic from 'next/dynamic'
```

引入后就可以懒加载自定义模块了，代码如下：

```js
import React, {useState} from 'react'
import dynamic from 'next/dynamic'

const One = dynamic(import('../components/one'))

function Time(){

    const [nowTime,setTime] = useState(Date.now())

    const changeTime= async ()=>{
        const moment = await import('moment')

        setTime(moment.default(Date.now()).format())
    }
    return (
        <>
            <div>显示时间为:{nowTime}</div>
            <One/>
            <div><button onClick={changeTime}>改变时间格式</button></div>
        </>
    )
}
export default Time
```

写完代码后，可以看到自定义组件是懒加载的，只有在`jsx`里用到``时，才会被加载进来，如果不使用就不会被加载。

当我们作的应用存在首页打开过慢和某个页面加载过慢时，就可以采用`Lazy Loading`的形式，用懒加载解决这些问题。



# 自定义Head进行SEO优化

SEO，让自己制作的网页更容易被百度等搜索引擎搜到

方法：给各个页面加上<Head>标签

```js
import Head from 'next/head'
function Header(){ 
    return (
        <>
            <Head>
                <title>技术胖是最胖的！</title>
                <meta charSet='utf-8' />
            </Head>
            <div>JSPang.com</div>

        </> 
    )
}
export default Header
```

这时候再打开浏览器预览，你发现已经有了`title`。



# 配合使用Ant DesignUI

## 让Next.js支持css

先用`yarn`命令来安装`@zeit/next-css`包，它的主要功能就是让`Next.js`可以加载CSS文件，有了这个包才可以进行配置。

```
yarn add @zeit/next-css
```

包安装好以后就可以进行配置文件的编写了，建立一个`next.config.js`.这个就是`Next.js`的总配置文件（如果感兴趣可以自学一下）。

```js
const withCss = require('@zeit/next-css')

if(typeof require !== 'undefined'){
    require.extensions['.css']=file=>{}
}

module.exports = withCss({})
```

这段代码你有兴趣是可以看看的，其实我对配置文件基本不记忆的，因为配置文件就是别人规定的配置，你写就好。比如要使用CSS就可以把上面这段代码输入到放入到里边的就好了。

修改配置文件需要重新启一下服务，重启服务可以让配置生效，这时候你到浏览器中可以发现CSS文件已经生效了，字变成了绿色。



## 按需加载Ant Design

加载`Ant Design`在我们打包的时候会把`Ant Design`的所有包都打包进来，这样就会产生性能问题，让项目加载变的非常慢。这肯定是不行的，现在的目的是只加载项目中用到的模块，这就需要我们用到一个`babel-plugin-import`文件。

** 先来安装`Ant Design`库 **

直接使用yarn来安装就可以。

```shell
yarn add antd
```

** 安装和配置`babel-plugin-import` 插件 **

其实`babel-plugin-import`我讲Vue.js和Webpack.js的时候都一次讲过这个插件，这里我们就再来讲一下，先进行安装。

```shell
yarn add babel-plugin-import
```

安装完成后，在项目根目录建立`.babelrc`文件，然后写入如下配置文件。

```js
{
    "presets":["next/babel"],  //Next.js的总配置文件，相当于继承了它本身的所有配置
    "plugins":[     //增加新的插件，这个插件就是让antd可以按需引入，包括CSS
        [
            "import",
            {
                "libraryName":"antd",
                "style":"css"
            }
        ]
    ]
}
```

这样配置好了以后，`webpack`就不会默认把整个`Ant Design`的包都进行打包到生产环境了，而是我们使用那个组件就打包那个组件,同样CSS也是按需打包的。

通过上面的配置，就可以愉快的在`Next.js`中使用`Ant Desgin`，让页面变的好看起来。

可以在`header.js`里，引入``组件，并进行使用，代码如下。

```js
import Myheader from '../components/myheader'
import {Button} from 'antd'


import '../static/test.css'
function Header(){ 
    return (
        <>
            <Myheader />
            <div>JSPang.com</div>
            <div><Button>我是按钮</Button></div>

        </> 
    )
}
export default Header
```

然后到浏览器中查看一下结果，这时候`Ant Design`已经起作用了，我们也完成了在`Next.js`中，使用`Ant Design`的目的



# Next.js生产环境打包

其实Next.js大打包时非常简单的，只要一个命令就可以打包成功。但是当你使用了`Ant Desgin`后，在打包的时候会遇到一些坑。

> 打包 ：next build

> 运行：next start -p 80

先把这两个命令配置到`package.json`文件里，比如配置成下面的样子。

```js
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start -p 80"
},
```

然后在终端里运行一下`yarn build`，如果这时候报错，其实是我们在加入`Ant Design`的样式时产生的，这个已经在`Ant Design`的Github上被提出了，但目前还没有被修改，你可以改完全局引入CSS解决问题。

修改`.babelrc`文件，去掉"style":"css"这一行就可以打包成功

```js
{
    "presets":["next/babel"],  //Next.js的总配置文件，相当于继承了它本身的所有配置
    "plugins":[     //增加新的插件，这个插件就是让antd可以按需引入，包括CSS
        [
            "import",
            {
                "libraryName":"antd",
                //"style":"css"
            }
        ]
    ]
}
```

在page目录下，新建一个`_app.js`文件，然后写入下面的代码。

```js
import App from 'next/app'

import 'antd/dist/antd.css'

export default App
```

这样配置一下，就可以打包成功了，然后再运行`yarn start`来运行服务器，看一下我们的`header`页面，也是有样式的。说明打包已经成功了。