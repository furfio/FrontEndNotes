---
typora-copy-images-to: 笔记图像
typora-root-url: 笔记图像
---

# 1、下载使用

[下载Vue2.0的两个版本：](https://jspang.com/detailed?id=21#toc33)

官方网站：

- 开发版本：包含完整的警告和调试模式
- 生产版本：删除了警告，进行了压缩

[项目结构搭建](https://jspang.com/detailed?id=21#toc34)

~~~
npm init
~~~



[live-server使用](https://jspang.com/detailed?id=21#toc35)

用npm进行全局安装

```shell
npm install live-server -g
```

在项目目录中开启服务（类似于apache吧，在本地8080端口看到index.html）

```
live-server
```

[编写第一个HelloWorld代码：](https://jspang.com/detailed?id=21#toc36)

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>Helloworld</title>
</head>
<body>
    <h1>Hello World</h1>
    <hr>
    <div id="app">
        {{message}}
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                message:'hello Vue!'
            }
        })
    </script>
</body>
</html>
```

# 2、vue的内部指令

## 2.1、v-if, v-else, v-show

[v-if的使用：](https://jspang.com/detailed?id=21#toc38)

v-if:是vue 的一个内部指令，指令用在我们的html中。

v-if用来判断是否加载html的DOM，比如我们模拟一个用户登录状态，在用户登录后现实用户名称。

关键代码：

```html
 <div v-if="isLogin">你好，JSPang！</div>
```

完整html代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>v-if & v-show & v-else</title>
</head>
<body>
    <h1>v-if 判断是否加载</h1>
    <hr>
    <div id="app">
        <div v-if="isLogin">你好：JSPang</div>
        <div v-else>请登录后操作</div>

    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
               isLogin:false
            }
        })
    </script>
</body>
</html>
```

这里我们在vue的data里定义了isLogin的值，当它为true时，网页就会显示：你好：JSPang，如果为false时，就显示请登录后操作。

[2.v-show的使用：](https://jspang.com/detailed?id=21#toc39)

调整css中display属性，DOM已经加载，只是CSS控制没有显示出来。

```html
<div v-show="isLogin">你好：JSPang</div>
```

[v-if 和v-show的区别：](https://jspang.com/detailed?id=21#toc310)

- v-if： 判断是否加载，可以减轻服务器的压力，在需要时加载。
- v-show：调整css dispaly属性，可以使客户端操作更加流畅。

## 2.2、v-for

v-for指令是循环渲染一组data中的数组，v-for 指令需要以 item in items 形式的特殊语法，items 是源数据数组并且item是数组元素迭代的别名。

[一、基本用法：](https://jspang.com/detailed?id=21#toc312)

模板写法

```html
 <li v-for="item in items">
        {{item}}
</li>
```

js写法

```javascript
var app=new Vue({
     el:'#app',
     data:{
         items:[20,23,18,65,32,19,54,56,41]
     }
})
```

完整代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>V-for 案例</title>
</head>
<body>
    <h1>v-for指令用法</h1>
    <hr>
    <div id="app">
       <ul>
           <li v-for="item in items">
                {{item}}
           </li>
       </ul>
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                items:[20,23,18,65,32,19,54,56,41]
            }
        })
    </script>
</body>
</html>
```

这是一个最基础的循环，先在js里定义了items数组，然后在模板中用v-for循环出来，需要注意的是，你需要那个html标签循环，v-for就写在那个上边。

[二、排序](https://jspang.com/detailed?id=21#toc313)

我们已经顺利的输出了我们定义的数组，但是我需要在输出之前给数组排个序，那我们就用到了Vue的computed:属性。

```javascript
computed:{
    sortItems:function(){
          return this.items.sort();
    }
}
```

我们在computed里新声明了一个对象sortItems，如果不重新声明会污染原来的数据源，这是Vue不允许的，所以你要重新声明一个对象。

如果一切顺利的话，你已经看到了结果，但是这个小程序还是有个小Bug的，现在我把数组修改成这样。

```
items:[20,23,18,65,32,19,5,56,41]
```

我们把其中的54修改成了5，我们再看一下结果，发现排序结果并不是我们想要的。

我们可以自己编写一个方法sortNumber，然后传给我们的sort函数解决这个Bug。

```
function sortNumber(a,b){
            return a-b
  }
```

用法

```
computed:{
    sortItems:function(){
    return this.items.sort(sortNumber);
    }
 }
```

经过一番折腾，我们终于实现了真正的数字排序，这是在工作中非常常用的，一定要学好，记住。

[三、对象循环输出](https://jspang.com/detailed?id=21#toc314)

我们上边循环的都是数组，那我们来看一个对象类型的循环是如何输出的。

我们先定义个数组，数组里边是对象数据

```javascript
students:[
  {name:'jspang',age:32},
  {name:'Panda',age:30},
  {name:'PanPaN',age:21},
  {name:'King',age:45}
]
```

在模板中输出

```javascript
<ul>
   <li v-for="student in students">
       {{student.name}} - {{student.age}}
   </li>
</ul>
```

加入索引序号：

```javascript
//数组对象方法排序:
function sortByKey(array,key){
    return array.sort(function(a,b){
      var x=a[key];
      var y=b[key];
      return ((x<y)?-1:((x>y)?1:0));
   });
}
```

有了数组的排序方法，在computed中进行调用排序

```javascript
sortStudent:function(){
     return sortByKey(this.students,'age');
}
```

注意：vue低版本中 data里面的items和computed里面可以一样，但是高版本，是不允许相同名称。有很多小伙伴踩到了这个坑，这里提醒学习的小伙伴，根据自己版本的不同，请修改代码。（感谢网友：tannnb的指正）。

## 2.3、v-text和v-html

我们已经会在html中输出data中的值了，我们已经用的是{{xxx}},这种情况是有弊端的，就是当我们网速很慢或者javascript出错时，会暴露我们的{{xxx}}。Vue给我们提供的v-text,就是解决这个问题的。我们来看代码：

```html
<span>{{ message }}</span>=<span v-text="message"></span><br/>
```

如果在javascript中写有html标签，用v-text是输出不出来的，这时候我们就需要用`v-html`标签了。

```html
<span v-html="msgHtml"></span>
```

双大括号会将数据解释为纯文本，而非HTML。为了输出真正的HTML，你就需要使用v-html 指令。 需要注意的是：在生产环境中动态渲染HTML是非常危险的，因为容易导致`XSS`攻击。所以只能在可信的内容上使用v-html，永远不要在用户提交和可操作的网页上使用。 完整代码：

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>v-text & v-html 案例</title>
</head>
<body>
    <h1>v-text & v-html 案例</h1>
    <hr>
    <div id="app">
        <span>{{ message }}</span>=<span v-text="message"></span><br/>
        <span v-html="msgHtml"></span>
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                message:'hello Vue!',
                msgHtml:'<h2>hello Vue!</h2>'
            }
        })
    </script>
</body>
</html>
```

## 2.4、v-on

v-on 就是监听事件，可以用v-on指令监听DOM事件来触发一些javascript代码。

一、使用绑定事件监听器，编写一个加分减分的程序。

程序代码

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>v-on事件监听器</title>
</head>
<body>
    <h1>v-on 事件监听器</h1>
    <hr>
    <div id="app">
       本场比赛得分： {{count}}<br/>
       <button v-on:click="jiafen">加分</button>
       <button v-on:click="jianfen">减分</button>

    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                count:1,
                secondCount:2
            },
            methods:{
                jiafen:function(){
                    this.count++;
                },
                jianfen:function(){
                    this.count--;
                }
            }
        })
    </script>
</body>
</html>
```

我们的v-on 还有一种简单的写法，就是用@代替。

```
<button @click="jianfen">减分</button>
```

我们除了绑定click之外，我们还可以绑定其它事件，比如键盘回车事件v-on:keyup.enter,现在我们增加一个输入框，然后绑定回车事件，回车后把文本框里的值加到我们的count上。 绑定事件写法：

```
<input type="text" v-on:keyup.enter="onEnter" v-model="secondCount">
```

javascript代码：

```javascript
onEnter:function(){
     this.count=this.count+parseInt(this.secondCount);
}
```

因为文本框的数字会默认转变成字符串，所以我们需要用`parseInt()函数`进行整数转换。

你也可以根据键值表来定义键盘事件： 

## 2.5、v-model

v-model指令，我理解为绑定数据源。就是把数据绑定在特定的表单元素上，可以很容易的实现双向数据绑定。

[一、我们来看一个最简单的双向数据绑定代码：](https://jspang.com/detailed?id=21#toc318)

html文件

```html
<div id="app">
    <p>原始文本信息：{{message}}</p>
    <h3>文本框</h3>
    <p>v-model:<input type="text" v-model.lazy="message"></p>
</div>
```

javascript代码：

```js
var app=new Vue({
  el:'#app',
  data:{
       message:'hello Vue!'
  }
 })
```

[二、修饰符](https://jspang.com/detailed?id=21#toc319)

- .lazy：取代 imput 监听 change 事件（其实就是不会实时绑定输入内容，而是在鼠标离开输入框后才把message变量改为输入内容）
- .number：输入字符串转为数字。（也是懒加载，输入为2222rrrrr时，只会加载为2222，要是rrrrr2222，就会保留前面的rrrrr）
- .trim：输入去掉首尾空格。

[三、文本区域加入数据绑定](https://jspang.com/detailed?id=21#toc320)

```html
<textarea  cols="30" rows="10" v-model="message"></textarea>
```

[四、多选按钮绑定一个值](https://jspang.com/detailed?id=21#toc321)

```html
<h3>多选按钮绑定一个值</h3>
<input type="checkbox" id="isTrue" v-model="isTrue">
<label for='isTrue'>{{isTrue}}</label>

...

var app=new Vue({
  el:'#app',
  data:{
       isTrue:true
  }
 })
```

[五、多选绑定一个数组](https://jspang.com/detailed?id=21#toc322)

功能：三个单选，当前选中的数据的value会显示在数组中

![image-20200311112107132](/image-20200311112107132.png)

```html
<h3>多选绑定一个数组</h3>
       <p>
            <input type="checkbox" id="JSPang" value="JSPang" v-model="web_Names">
            <label for="JSPang">JSPang</label><br/>
            <input type="checkbox" id="Panda" value="Panda" v-model="web_Names">
            <label for="JSPang">Panda</label><br/>
            <input type="checkbox" id="PanPan" value="PanPan" v-model="web_Names">
            <label for="JSPang">PanPan</label>
            <p>{{web_Names}}</p>
       </p>

...

var app=new Vue({
  el:'#app',
  data:{
       web_Names:[]
  }
 })
```

[六、单选按钮绑定数据](https://jspang.com/detailed?id=21#toc323)

```html
<h3>单选按钮绑定</h3>
<input type="radio" id="one" value="男" v-model="sex">
<label for="one">男</label>
<input type="radio" id="two" value="女" v-model="sex">
<label for="two">女</label>
<p>{{sex}}</p>
```

## 2.6、v-bind

v-bind用于绑定标签属性，如html的src属性等

html文件：

```
<div id="app">
    <img v-bind:src="imgSrc"  width="200px">
</div>
```

在html中我们用v-bind:src=”imgSrc”的动态绑定了src的值，这个值是在vue构造器里的data属性中找到的。

js文件：

```
var app=new Vue({
    el:'#app',
    data:{
          imgSrc:'http://baidu.com/wp-content/uploads/2017/02/vue01-2.jpg'
     }
})
```

我们在data对象在中增加了imgSrc属性来供html调用。

[v-bind 缩写](https://jspang.com/detailed?id=21#toc325)

```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

[绑定CSS样式](https://jspang.com/detailed?id=21#toc326)

在工作中我们经常使用v-bind来绑定css样式：（就可以动态修改css啦）

在绑定CSS样式是，绑定的值必须在vue中的data属性中进行声明。

 1、直接绑定class样式

```html
<div :class="className">1、绑定classA</div>

...

<style>
    .classA{
        A类样式
    }
    .classB{
        B类样式
    }
</style>

...

var app=new Vue({
  el:'#app',
  data:{
       className:'classA'
  }
 })
```

2、绑定classA并进行判断，在isOK为true时显示样式，在isOk为false时不显示样式。isOK也是在data里面定义

```html
<div :class="{classA:isOk}">2、绑定class中的判断</div>
```

3、绑定class中的数组, 即同时具有A和B中定义的样式

```
<div :class="[classA,classB]">3、绑定class中的数组</div>

data:{
	classA:'classA',
	classB:'classB'
}
```

4、绑定class中使用三元表达式判断

```
<div :class="isOk?classA:classB">4、绑定class中的三元表达式判断</div>
```

5、绑定style

```
<div :style="{color:red,fontSize:font}">5、绑定style</div>
```

6、用对象绑定style样式

```
<div :style="styleObject">6、用对象绑定style样式</div>
var app=new Vue({
   el:'#app',
   data:{
       styleObject:{
           fontSize:'24px',
           color:'green'
            }
        }
})
```

## 2.7、其他内部指令v-pre，v-cloak，v-once

[v-pre指令](https://jspang.com/detailed?id=21#toc328)

在模板中跳过vue的编译，直接输出原始值。就是在标签中加入v-pre就不会输出vue中的data值了。

```
<div v-pre>{{message}}</div>
```

这时并不会输出我们的message值，而是直接在网页中显示{{message}}

[v-cloak指令](https://jspang.com/detailed?id=21#toc329)

在vue渲染完指定的整个DOM后才进行显示。它必须和CSS样式一起使用，

```
[v-cloak] {
  display: none;
}
<div v-cloak>
  {{ message }}
</div>
```

[v-once指令](https://jspang.com/detailed?id=21#toc330)

在第一次DOM时进行渲染，渲染完成后视为静态内容，跳出以后的渲染过程。就算之后message变量再变化，这里显示的都不会再变

```
<div v-once>第一次绑定的值：{{message}}</div>
<div><input type="text" v-model="message"></div>
```



# 3、全局API

## 3.1、Vue.directive自定义指令

[一、什么是全局API？](https://jspang.com/detailed?id=22#toc32)

全局API并不在构造器里，而是先声明全局变量或者直接在Vue上定义一些新功能，Vue内置了一些全局API，比如我们今天要学习的指令Vue.directive。说的简单些就是，在构造器外部用Vue提供给我们的API函数来定义新的功能。

[二、Vue.directive自定义指令](https://jspang.com/detailed?id=22#toc33)

我们在第一季就学习了内部指令，我们也可以定义一些属于自己的指令，比如我们要定义一个v-jspang的指令，作用就是让文字变成红色。

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>hi dzy</title>
    <script type="text/javascript" src="../assets/js/vue.js"></script>
</head>
<body>
    <h1>hi dzy</h1>
    <div id="app">
        <div v-jspang="color">{{message}}</div>
    </div>

    <script type="text/javascript">
        Vue.directive('jspang',function(el,binding,vnode){
            el.style='color:'+binding.value;
        });

        var app=new Vue({
            el:'#app',
            data:{
                message:'helloWorld',
                color:'red'
            }
            }

        )
    </script>
</body>
</html>
```

[三、自定义指令中传递的三个参数](https://jspang.com/detailed?id=22#toc34)

- el: 指令所绑定的元素，可以用来直接操作DOM。这里指

  ~~~
  <div style="color: red;">helloWorld</div>
  ~~~

- binding: 一个对象，包含指令的很多信息。

  ~~~
  Object
  def: {bind: ƒ, update: ƒ}
  expression: "color"
  modifiers: {}
  name: "jspang"
  rawName: "v-jspang"
  value: "red"
  __proto__: Object
  ~~~

- vnode: Vue编译生成的虚拟节点。

[四、自定义指令的生命周期](https://jspang.com/detailed?id=22#toc35)

自定义指令有五个生命周期（也叫钩子函数），分别是 bind,inserted,update,componentUpdated,unbind

1. bind:只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个绑定时执行一次的初始化动作。
2. inserted:被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于document中）。
3. update:被绑定于元素所在的模板更新时调用，而无论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。
4. componentUpdated:被绑定元素所在模板完成一次更新周期时调用。
5. unbind:只调用一次，指令与元素解绑时调用。

```js
 Vue.directive('jspang',{
bind:function(){//被绑定
     console.log('1 - bind');
},
inserted:function(){//绑定到节点
      console.log('2 - inserted');
},
update:function(){//组件更新
      console.log('3 - update');
},
componentUpdated:function(){//组件更新完成
      console.log('4 - componentUpdated');
},
unbind:function(){//解绑
      console.log('1 - bind');
}
 });

```

## 3.2、vue.set全局操作

[一、引用构造器外部数据：](https://jspang.com/detailed?id=22#toc311)

什么是外部数据，就是不在Vue构造器里里的data处声明，而是在构造器外部声明，然后在data处引用就可以了。外部数据的加入让程序更加灵活，我们可以在外部获取任何想要的数据形式，然后让data引用。 看一个简单的代码：

```
//在构造器外部声明数据
 var outData={
    count:1,
    goodName:'car'
};
var app=new Vue({
    el:'#app',
    //引用外部数据
    data:outData
})
```

二、在外部改变数据的三种方法：

1、用Vue.set改变

```
function add(){
       Vue.set(outData,'count',4);
 }
```

2、用Vue对象的方法添加

```
function add(){
       app.count++;
 }
```

3、直接操作外部数据

```
function add(){
      outData.count++;
 }
```

其实这三种方式都可以操作外部的数据，Vue也给我们增加了一种操作外部数据的方法。

[三、为什么要有Vue.set的存在?](https://jspang.com/detailed?id=22#toc312)

由于Javascript的限制，Vue不能自动检测以下变动的数组。

- 当你利用索引直接设置一个项时，vue不会为我们自动更新。
- 当你修改数组的长度时，vue不会为我们自动更新。

看一段代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>Vue.set 全局操作</title>
</head>
<body>
    <h1>Vue.set 全局操作</h1>
    <hr>
    <div id="app">
        <ul>
            <li v-for=" aa in arr">{{aa}}</li>
        </ul>

    </div>
    <button onclick="add()">外部添加</button>

    <script type="text/javascript">

        function add(){
            console.log("我已经执行了");
           app.arr[1]='ddd';
           //Vue.set(app.arr,1,'ddd');
        }
        var outData={
            arr:['aaa','bbb','ccc']
        };
        var app=new Vue({
            el:'#app',
            data:outData
        })
    </script>
</body>
</html>
```

这时我们的界面是不会自动跟新数组的，我们需要用Vue.set(app.arr,1,’ddd’)来设置改变，vue才会给我们自动更新，这就是Vue.set存在的意义。

## 3.3、Vue 的生命周期函数（钩子函数）

<img src="/lifecycle.png" alt="lifecycle" style="zoom:67%;" />

我们直接来看一段代码：

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>构造器的声明周期</title>
</head>
<body>
    <h1>构造器的声明周期</h1>
    <hr>
    <div id="app">
        {{message}}
        <p><button @click="jia">加分</button></p>
    </div>
        <button onclick="app.$destroy()">销毁</button>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                message:1
            },
            methods:{
                jia:function(){
                    this.message ++;
                }
            },
            beforeCreate:function(){
                console.log('1-beforeCreate 初始化之前');
            },
            created:function(){
                console.log('2-created 创建完成');
            },
            beforeMount:function(){
                console.log('3-beforeMount 挂载之前');
            },
            mounted:function(){
                console.log('4-mounted 被挂载之后');
            },
            beforeUpdate:function(){
                console.log('5-beforeUpdate 数据更新前');
            },
            updated:function(){
                console.log('6-updated 被更新后');
            },
            activated:function(){
                console.log('7-activated');
            },
            deactivated:function(){
                console.log('8-deactivated');
            },
            beforeDestroy:function(){
                console.log('9-beforeDestroy 销毁之前');
            },
            destroyed:function(){
                console.log('10-destroyed 销毁之后')
            }

        })
    </script>
</body>
</html>
```

常用方法：

created中加载数据，播放加载动画

mounted中结束加载，结束加载动画

## 3.4、Template制作模板

[一、直接写在选项里的模板](https://jspang.com/detailed?id=22#toc315)

直接在构造器里的template选项后边编写。这种写法比较直观，但是如果模板html代码太多，不建议这么写。

javascript代码：

```
var app=new Vue({
     el:'#app',
     data:{
         message:'hello Vue!'
      },
     template:`
        <h1 style="color:red">我是选项模板</h1>
     `
})
```

这里需要注意的是模板的标识不是单引号和双引号，而是，就是Tab上面的键。

[二、写在template标签里的模板](https://jspang.com/detailed?id=22#toc316)

这种写法更像是在写HTML代码，就算不会写Vue的人，也可以制作页面。

```
   <template id="demo2">
             <h2 style="color:red">我是template标签模板</h2>
    </template>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                message:'hello Vue!'
            },
            template:'#demo2'
        })
    </script>
```

[三、写在script标签里的模板](https://jspang.com/detailed?id=22#toc317)

这种写模板的方法，可以让模板文件从外部引入。

```
<script type="x-template" id="demo3">
        <h2 style="color:red">我是script标签模板</h2>
    </script>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                message:'hello Vue!'
            },
            template:'#demo3'
        })
    </script>
```

**这节课我们学习了Template的三种写法，以后学习到vue-cli的时候还会学到一种xxx.vue的写法。**

# 4、组件 Vue.component

## 4.1、初识

其实组件就是制作自定义的标签，这些标签在HTML中是没有的。

[一、全局化注册组件](https://jspang.com/detailed?id=22#toc319)

全局化就是在构造器的外部用Vue.component来注册，我们注册现在就注册一个的组件来体验一下。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>component-1</title>
</head>
<body>
    <h1>component-1</h1>
    <hr>
    <div id="app">
        <jspang></jspang>
    </div>

    <script type="text/javascript">
        //注册全局组件
        Vue.component('jspang',{
            template:`<div style="color:red;">全局化注册的jspang标签</div>`
        })
        var app=new Vue({
            el:'#app',
            data:{
            }
        })
    </script>
</body>
</html>
```

我们在javascript里注册了一个组件，在HTML中调用了他。这就是最简单的一个组件的编写方法，并且它可以放到多个构造器的作用域里。

[二、局部注册组件局部](https://jspang.com/detailed?id=22#toc320)

局部注册组件局部注册组件和全局注册组件是向对应的，局部注册的组件只能在组件注册的作用域里进行使用，其他作用域使用无效。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>component-1</title>
</head>
<body>
    <h1>component-1</h1>
    <hr>
    <div id="app">
      <panda></panda>
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            components:{
                "panda":{
                    template:`<div style="color:red;">局部注册的panda标签</div>`
                }
            }
        })
    </script>
</body>
</html>
```

从代码中你可以看出局部注册其实就是写在构造器里，但是你需要注意的是，构造器里的components 是加s的，而全局注册是不加s的。

[三、组件和指令的区别](https://jspang.com/detailed?id=22#toc321)

组件注册的是一个标签，而指令注册的是已有标签里的一个属性。在实际开发中我们还是用组件比较多，指令用的比较少。因为指令看起来封装的没那么好，这只是个人观点

## 4.2、组件props属性设置

props选项就是设置和获取标签上的属性值的，例如我们有一个自定义的组件,这时我们想给他加个标签属性写成 意思就是熊猫来自中国，当然这里的China可以换成任何值。定义属性的选项是props。

[一、定义属性并获取属性值](https://jspang.com/detailed?id=22#toc323)

定义属性我们需要用props选项，加上数组形式的属性名称，例如：props:[‘here’]。在组件的模板里读出属性值只需要用插值的形式，例如{{ here }}.

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>component-2</title>
</head>
<body>
    <h1>component-2</h1>
    <hr>
    <div id="app">
      <panda here="China"></panda>
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            components:{
                "panda":{
                    template:`<div style="color:red;">Panda from {{ here }}.</div>`,
                    props:['here']
                }
            }
        })
    </script>
</body>
</html>
```

上面的代码定义了panda的组件，并用props设置了here的属性值，在here属性值里传递了China给组件。 最后输出的结果是红色字体的Panda from China.

[二、属性中带’-‘的处理方式](https://jspang.com/detailed?id=22#toc324)

我们在写属性时经常会加入’-‘来进行分词，比如：，那这时我们在props里如果写成props:[‘form-here’]是错误的，我们必须用小驼峰式写法props:[‘formHere’]。

html文件：

```
<panda from-here="China"></panda>
```

javascript文件：

```
 var app=new Vue({
            el:'#app',
            components:{
                "panda":{
                    template:`<div style="color:red;">Panda from {{ here }}.</div>`,
                    props:['fromHere']
                }
            }
        })
```

PS：因为这里有坑，所以还是少用-为好

[三、在构造器里向组件中传值](https://jspang.com/detailed?id=22#toc325)

把构造器中data的值传递给组件，我们只要进行绑定就可以了。就是我们第一季学的v-bind:xxx.

我们直接看代码:

Html文件：

```
<panda v-bind:here="message"></panda>
```

javascript文件：

```
 var app=new Vue({
            el:'#app',
            data:{
               message:'SiChuan' 
            },
            components:{
                "panda":{
                    template:`<div style="color:red;">Panda from {{ here }}.</div>`,
                    props:['here']
                }
            }
        })
```

## 4.3、父子组件关系

在实际开发中我们经常会遇到在一个自定义组件中要使用其他自定义组件，这就需要一个父子组件关系。

[一、构造器外部写局部注册组件](https://jspang.com/detailed?id=22#toc327)

上面上课我们都把局部组件的编写放到了构造器内部，如果组件代码量很大，会影响构造器的可读性，造成拖拉和错误。

我们把组件编写的代码放到构造器外部或者说单独文件。

我们需要先声明一个对象,对象里就是组件的内容。

```
var jspang = {
   template:`<div>Panda from China!</div>`
}
```

声明好对象后在构造器里引用就可以了。

```javascript
components:{
    "jspang":jspang
}
```

html中引用

```
 <jspang></jspang>
```

二、父子组件的嵌套 我们先声明一个父组件，比如叫jspang，然后里边我们加入一个city组件，我们来看这样的代码如何写。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>component-3</title>
</head>
<body>
    <h1>component-3</h1>
    <hr>
    <div id="app">
      <jspang></jspang>  
    </div>
    <script type="text/javascript">
       var city={
           template:`<div>Sichuan of China</div>`
       }
        var jspang = {
            template:`<div>
                    <p> Panda from China!</p>
                    <city></city>
            </div>`,
            components:{
                "city":city
            }
        }
        var app=new Vue({
            el:'#app',
            components:{
                "jspang":jspang
            }

        })
    </script>
</body>
</html>
```

## 4.4、Component标签

标签是Vue框架自定义的标签，它的用途就是可以动态绑定我们的组件，根据数据的不同更换不同的组件。

1.我们先在构造器外部定义三个不同的组件，分别是componentA,componentB和componentC.

```
 var componentA={
     template:`<div>I'm componentA</div>`
}
 var componentB={
      template:`<div>I'm componentB</div>`
}
var componentC={
    template:`<div>I'm componentC</div>`
}
```

2.我们在构造器的components选项里加入这三个组件。

```
components:{
    "componentA":componentA,
    "componentB":componentB,
    "componentC":componentC,
}
```

3.我们在html里插入component标签，并绑定who数据，根据who的值不同，调用不同的组件。

```
<component v-bind:is="who"></component>
```

这就是我们的组件标签的基本用法。我们提高以下，给页面加个按钮，每点以下更换一个组件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>component-4</title>
</head>
<body>
    <h1>component-4</h1>
    <hr>
    <div id="app">
       <component v-bind:is="who"></component>
       <button @click="changeComponent">changeComponent</button>
    </div>

    <script type="text/javascript">
        var componentA={
            template:`<div style="color:red;">I'm componentA</div>`
        }
        var componentB={
            template:`<div style="color:green;">I'm componentB</div>`
        }
        var componentC={
            template:`<div style="color:pink;">I'm componentC</div>`
        }

        var app=new Vue({
            el:'#app',
            data:{
                who:'componentA'
            },
            components:{
                "componentA":componentA,
                "componentB":componentB,
                "componentC":componentC,
            },
            methods:{
                changeComponent:function(){
                    if(this.who=='componentA'){
                        this.who='componentB';
                    }else if(this.who=='componentB'){
                        this.who='componentC';
                    }else{
                        this.who='componentA';
                    }
                }
            }
        })
    </script>
</body>
</html>
```

# 5、vue-router（详见6 cli）

## 5.1、入门

简介： 由于Vue在开发时对路由支持的不足，后来官方补充了`vue-router`插件，它在Vue的生态环境中非常重要，在实际开发中只要编写一个页面就会操作`vue-router`。要学习`vue-router`就要先知道这里的路由是什么？这里的路由并不是指我们平时所说的硬件路由器，这里的路由就是SPA（单页应用）的路径管理器。再通俗的说，`vue-router`就是我们WebApp的链接路径管理系统。

有的小伙伴会有疑虑，为什么我们不能像原来一样直接用``标签编写链接哪？因为我们用Vue作的都是单页应用，就相当于只有一个主的index.html页面，所以你写的``标签是不起作用的，你必须使用`vue-router`来进行管理。

**安装vue-router**

vue-router是一个插件包，所以我们还是需要用npm来进行安装的。打开命令行工具，进入你的项目目录，输入下面命令。



# 6、vue-cli（重点：代码实际的样子）

## 6.1、安装

The package name changed from `vue-cli` to `@vue/cli`. If you have the previous `vue-cli`(1.x or 2.x) package installed globally, you need to uninstall it first with `npm uninstall vue-cli -g` or `yarn global remove vue-cli`.

```bash
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

vue -V 查看版本 （当前4.2.3）

 **使用图形化界面**

你也可以通过 `vue ui` 命令以图形化界面创建和管理项目：

```bash
vue ui
```

上述命令会打开一个浏览器窗口，并以图形化界面将你引导至项目创建的流程。

![image-20200314102349183](/image-20200314102349183.png)

选这三个包，把Linter/Formatter代码校验的去掉



打开history模式：

![image-20200314102517797](/image-20200314102517797.png)

**什么是HTML5 History 模式**

`vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

如果不想要很丑的 hash，我们可以用路由的 **history 模式**，这种模式充分利用 `history.pushState` API 来完成 URL 跳转而无须重新加载页面。

```js
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

当你使用 history 模式时，URL 就像正常的 url，例如 `http://yoursite.com/user/id`，也好看！

不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 `http://oursite.com/user/id` 就会返回 404，这就不好看了。

所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 app 依赖的页面。

创建完项目后ctrlC关闭命令行，用ws或IJ打开项目，（记得安装vue.js插件）

之后直接

~~~
npm run serve
~~~

## 6.2、项目路径

- App.vue: 项目主页面
- /router/index.js路由配置
- Babel解释器的配置文件，存放在根目录下。Babel是一个转码器，项目里需要用它将ES6代码转为ES5代码。



## 6.3、实际代码案例（路由）：

主页面App.vue

~~~html
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
      <router-link to="/hi">Hi</router-link> |
      <router-link to="/hi/hi1">Hi1</router-link> |
      <router-link to="/hi/hi2">Hi2</router-link>
    </div>
    aaaaaaaaaaaaaaaaaaaaaaaa
    <router-view/>
    bbbbbbbbbbbbbbbbbbbbbbbb
<!--    这里的<router-view/>类似于一个占位符，代指当前页面的子页面中的内容-->
  </div>
</template>
~~~

效果如下：（注意页面中aaaaaaaa和bbbbbbbb的位置，中间夹着的内容就是当前选择的子页面的内容）

<img src="/image-20200314215251922.png" alt="image-20200314215251922" style="zoom:50%;" />



路由index.js

~~~js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'
import hi from '../views/hi'
import hi1 from "../views/hi1";
import hi2 from "../views/hi2";

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  },
  {
    path:'/hi',
    name:'hi',
    component:hi,
    children:[
      {path:'/',component:hi},   //这行把hi组件本身也当作了他的一个子组件
      {path:'hi1',component:hi1}, //这里不能写/hi1,否则hi与hi1的内容都看不见了
      {path:'hi2',component:hi2},
    ]
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router

~~~



组件hi.vue:

~~~html
<template>

    <div class="div1" id="dzy">
        <p>{{myData}}</p>
        <router-view/>
    </div>

</template>

<script>
    export default {
        name: "hi",
        data(){
            return{
                myData:'hidzy'
            }
        }
    }
</script>

<style scoped>
    .div1{
        color: #42b983;
    }
    .div1 p{
        color: chocolate;
    }
</style>
~~~

点击主页的hi链接：由于在路由配置里hi把自己又当作了自己的子组件，于是这里有两个hidzy

![image-20200314223148061](/image-20200314223148061.png)

hi1和hi2的代码类似：

~~~html
<template>
    <div class="div1">
        <h1>hi 1</h1>
    </div>
</template>

<script>
    export default {
        name: "hi1",
    }
</script>

<style scoped>
    .div1{
        color: #42b983;
    }
</style>
~~~

点击hi1效果：可以看到其父组件hi依然显示，而贯穿全文，App.vue（hi的父组件）里的东西一直在显示

![image-20200314223348028](/image-20200314223348028.png)

## 6.4、路由传参数

我们用``标签中的to属性进行传参，需要您注意的是这里的to要进行一个绑定，写成:to。先来看一下这种传参方法的基本语法：

```
<router-link :to="{name:xxx,params:{key:value}}">valueString</router-link>
```

这里的to前边是带冒号的，然后后边跟的是一个对象形势的字符串.

- `name`：就是我们在路由配置文件中起的name值。
- `params`：就是我们要传的参数，它也是对象形势，在对象里可以传递多个值。

了解基本的语法后，我们改造一下我们的src/App.vue里的``标签,我们把hi1页面的``进行修改。

```
 <router-link :to="{name:'hi1',params:{username:'jspang'}}">Hi页面1</router-link>
```

把`src/reouter/index.js`文件里给hi1配置的路由起个name,就叫hi1.

```
{path:'/hi1',name:'hi1',component:Hi1},
```

最后在模板里(src/components/Hi1.vue)用$route.params.username进行接收.

```
{{$route.params.username}}
```

## 6.5、单页面多路由

这节课我们讲“单页面多路由区域操作”，实际需求是这样的，在一个页面里我们有2个以上``区域，我们通过配置路由的js文件，来操作这些区域的内容。例如我们在src/App.vue里加上两个``标签。我们用`vue-cli`建立了新的项目，并打开了src目录下的App.vue文件，在``下面新写了两行``标签,并加入了些CSS样式。

```
<router-view ></router-view>
 <router-view name="left" style="float:left;width:50%;background-color:#ccc;height:300px;"></router-view>
 <router-view name="right" style="float:right;width:50%;background-color:#c0c;height:300px;"></router-view>
```

现在的页面中有了三个``标签，也就是说我们需要在路由里配置这三个区域，配置主要是在components字段里进行。

```
import Vue from 'vue'
import Router from 'vue-router'
import Hello from '@/components/Hello'
import Hi1 from '@/components/Hi1'
import Hi2 from '@/components/Hi2'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      components: {
        default:Hello,
        left:Hi1,
        right:Hi2
      }
    },{
      path: '/Hi',
      components: {
        default:Hello,
        left:Hi2,
        right:Hi1
      }
    }

  ]
})
```

上边的代码我们编写了两个路径，一个是默认的`‘/’`，另一个是`’/Hi’`.在两个路径下的`components`里面，我们对三个区域都定义了显示内容。 定义好后，我们需要在component文件夹下，新建Hi1.vue和Hi2.vue页面就可以了。 ** Hi1.vue **

```
<template>
    <div>
        <h2>{{ msg }}</h2> 
    </div>
</template>

<script>
export default {
  name: 'hi1',
  data () {
    return {
      msg: 'I am Hi1 page.'
    }
  }
}
</script>
```

Hi2.vue

```
<template>
    <div>
        <h2>{{ msg }}</h2>
    </div>
</template>

<script>
export default {
  name: 'hi2',
  data () {
    return {
      msg: 'I am Hi2 page.'
    }
  }
}
</script>
```

最后在App.vue中配置我们的``就可以了

```
<router-link to="/">首页</router-link> | 
<router-link to="/hi">Hi页面</router-link> |
```

## 6.6、利用url传递参数

在实际开发也是有很多用URL传值的需求，比如我们在新闻列表中有很多新闻标题整齐的排列，我们需要点击每个新闻标题打开不同的新闻内容，这时在跳转路由时跟上新闻编号就十分实用。

** :冒号的形式传递参数 ** 我们可以在理由配置文件里以:冒号的形式传递参数，这就是对参数的绑定。 -. 在配置文件里以冒号的形式设置参数。我们在`/src/router/index.js`文件里配置路由。

```
{
    path:'/params/:newsId/:newsTitle',
     component:Params
}
```

我们需要传递参数是新闻ID`（newsId）`和新闻标题`（newsTitle`）.所以我们在路由配置文件里制定了这两个值。

- 在`src/components`目录下建立我们`params.vue`组件，也可以说是页面。我们在页面里输出了url传递的的新闻ID和新闻标题。

```
<template>
    <div>
        <h2>{{ msg }}</h2>
        <p>新闻ID：{{ $route.params.newsId}}</p>
        <p>新闻标题：{{ $route.params.newsTitle}}</p>
    </div>
</template>

<script>
export default {
  name: 'params',
  data () {
    return {
      msg: 'params page'
    }
  }
}
</script>
```

- 在App.vue文件里加入我们的``标签。这时候我们可以直接利用url传值了。

```
<router-link to="/params/198/jspang website is very good">params</router-link> |
```

我们已经实现了以url方式进行传值，这在实际开发中经常使用，必须完全了解。我希望你看完视频后或者学完文章后能多练习两边，并在实际项目中充分使用。

**正则表达式在URL传值中的应用**

上边的例子，我们传递了新闻编号，现在需求升级了，我们希望我们传递的新闻ID只能是数字的形式，这时候我们就需要在传递时有个基本的类型判断，vue是支持正则的。

加入正则需要在路由配置文件里（`/src/router/index.js`）以圆括号的形式加入。

```
path:'/params/:newsId(\\d+)/:newsTitle',
```

加入了正则，我们再传递数字之外的其他参数，params.vue组件就没有办法接收到。

## 6.7、重定向redirect

开发中有时候我们虽然设置的路径不一致，但是我们希望跳转到同一个页面，或者说是打开同一个组件。这时候我们就用到了路由的重新定向redirect参数。

** redirect基本重定向 **

我们只要在路由配置文件中（`/src/router/index.js`）把原来的component换成redirect参数就可以了。我们来看一个简单的配置。

```
export default new Router({
  routes: [
    {
      path: '/',
      component: Hello
    },{
      path:'/params/:newsId(\\d+)/:newsTitle',
      component:Params
    },{
      path:'/goback',
      redirect:'/'
    }

  ]
})
```

这里我们设置了goback路由，但是它并没有配置任何component（组件），而是直接redirect到path:’/’下了，这就是一个简单的重新定向。

** 重定向时传递参数**

我们已经学会了通过url来传递参数，那我们重定向时如果也需要传递参数怎么办？其实vue也已经为我们设置好了，我们只需要在ridirect后边的参数里复制重定向路径的path参数就可以了。可能你看的有点晕，我们来看一段代码：

```
{
  path:'/params/:newsId(\\d+)/:newsTitle',
  component:Params
},{
  path:'/goParams/:newsId(\\d+)/:newsTitle',
  redirect:'/params/:newsId(\\d+)/:newsTitle'
}
```

已经有了一个params路由配置，我们在设置一个`goParams`的路由重定向，并传递了参数。这时候我们的路由参数就可以传递给`params.vue`组件了。参数接收方法和正常的路由接收方法一样。

## 6.8、路由的过渡动画

在开发中有一种需求叫高端、大气、上档次。所以作为一个大前端有责任让你的程序开起来更酷炫。可以在页面切换时我们加入一些动画效果，提升我们程序的动效设计。这节课我们就学习一下路由的过渡动画效果制作。

``标签

想让路由有过渡动画，需要在``标签的外部添加``标签，标签还需要一个`name`属性。

```
<transition name="fade" mode="out-in">
  <router-view ></router-view>
</transition>
```

我们在/src/App.vue文件里添加了``标签，并给标签起了一个名字叫`fade`。

** css过渡类名： ** 组件过渡过程中，会有四个CSS类名进行切换，这四个类名与transition的name属性有关，比如name=”fade”,会有如下四个CSS类名：

1. fade-enter:进入过渡的开始状态，元素被插入时生效，只应用一帧后立刻删除。
2. fade-enter-active:进入过渡的结束状态，元素被插入时就生效，在过渡过程完成后移除。
3. fade-leave:离开过渡的开始状态，元素被删除时触发，只应用一帧后立刻删除。
4. fade-leave-active:离开过渡的结束状态，元素被删除时生效，离开过渡完成后被删除。

从上面四个类名可以看出，fade-enter-active和fade-leave-active在整个进入或离开过程中都有效，所以CSS的transition属性在这两个类下进行设置。

那我们就在App.vue页面里加入四种CSS样式效果，并利用CSS3的transition属性控制动画的具体效果。代码如下：

```
.fade-enter {
  opacity:0; 透明度
}
.fade-leave{
  opacity:1;
}
.fade-enter-active{
  transition:opacity .5s;
}
.fade-leave-active{
  opacity:0;
  transition:opacity .5s;
}
```

上边的代码设置了改变透明度的动画过渡效果，但是默认的`mode`模式是`in-out`模式，这并不是我们想要的。下面我们学一下`mode`模式。

**过渡模式mode：**

- `in-ou`t:新元素先进入过渡，完成之后当前元素过渡离开。
- `out-in`:当前元素先进行过渡离开，离开完成后新元素过渡进入。

这节课只能算是一个简单的过渡入门，教会大家原理，如果想做出实用酷炫的过渡效果，你需要有较强的动画制作能力。

## 6.9、mode的设置和404页面的处理

在学习过渡效果的时候，我们学了mode的设置，但是在路由的属性中还有一个mode。这节课我们就学习一下另一个mode模式和404页面的设置。

** mode的两个值 **

1. `histroy`:当你使用 `history` 模式时，URL 就像正常的 url，例如 `http://jsapng.com/lms/`，也好看！
2. `hash`:默认’hash’值，但是hash看起来就像无意义的字符排列，不太好看也不符合我们一般的网址浏览习惯。

（这个现在不用设置，在构建项目时已经选了history模式）

** 404页面的设置： **

用户会经常输错页面，当用户输错页面时，我们希望给他一个友好的提示，为此美工都会设计一个漂亮的页面，这个页面就是我们常说的404页面。vue-router也为我们提供了这样的机制.

1.设置我们的路由配置文件（`/src/router/index.js`）：

```
{
   path:'*',
   component:Error
}
```

这里的`path:’*’`就是找不到页面时的配置，`componen`t是我们新建的一个`Error.vue`的文件。

2.新建404页面：

在`/src/components/`文件夹下新建一个`Error.vue`的文件。简单输入一些有关错误页面的内容。

```
<template>
    <div>
        <h2>{{ msg }}</h2>
    </div>
</template>
<script>
export default {
  data () {
    return {
      msg: 'Error:404'
    }
  }
}
</script>
```

3.我们在用``瞎写一个标签的路径。

```
<router-link to="/bbbbbb">我是瞎写的</router-link> |
```

预览一下我们现在的结果，就已经实现404页面的效果。

## 6.10、编程式导航

这是这篇文章的最后一节，前10节课的导航都是用``标签或者直接操作地址栏的形式完成的，那如果在业务逻辑代码中需要跳转页面我们如何操作？这就是我们要说的编程式导航，顾名思义，就是在业务逻辑代码中实现导航。

** this.$router.go(-1) 和 this.$router.go(1) **

这两个编程式导航的意思是后退和前进，功能跟我们浏览器上的后退和前进按钮一样，这在业务逻辑中经常用到。比如条件不满足时，我们需要后退。

`router.go(-1)`代表着后退，我们可以让我们的导航进行后退，并且我们的地址栏也是有所变化的。

1.我们先在`app.vue`文件里加入一个按钮，按钮并绑定一个`goback( )`方法。

```
<button @click="goback">后退</button>
```

2.在我们的script模块中写入goback()方法，并使用this.$router.go(-1),进行后退操作。

```
<script>
export default {
  name: 'app',
  methods:{
    goback(){
      this.$router.go(-1);
    }
  }
}
</script>
```

打开浏览器进行预览，这时我们的后退按钮就可以向以前的网页一样后退了。

`router.go(1)`:代表着前进，用法和后退一样，我在这里就不重复码字了（码字辛苦希望大家理解）。



```
this.$router.push(‘/xxx ‘)
```

这个编程式导航都作用就是跳转，比如我们判断用户名和密码正确时，需要跳转到用户中心页面或者首页，都用到这个编程的方法来操作路由。

我们设置一个按钮，点击按钮后回到站点首页。

1.先编写一个按钮，在按钮上绑定`goHome( )`方法。

```
<button @click="goHome">回到首页</button>
```

2.在`>模块里加入`goHome`方法，并用`this.$router.push(‘/’)`导航到首页

```
export default {
  name: 'app',
  methods:{
    goback(){
      this.$router.go(-1);
    },
    goHome(){
      this.$router.push('/');
    }
  }
}
```



# 7、vuex状态管理器

## 7.1、基本

这篇文章我们主要讲解vuex。 vuex是一个专门为vue.js设计的集中式状态管理架构。状态？我把它理解为在data中的属性需要共享给其他vue组件使用的部分，就叫做状态。简单的说就是data中需要共用的属性。比如：我们有几个页面要显示用户名称和用户等级，或者显示用户的地理位置。如果我们不把这些属性设置为状态，那每个页面遇到后，都会到服务器进行查找计算，返回后再显示。在中大型项目中会有很多共用的数据，所以尤大神给我们提供了vuex。

在路径/store下有一个index.js来集中管理状态

~~~js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count:1
  },
  mutations: {
    add(state){
      state.count++;
    },
    reduce(state){
      state.count--;
    }
  },
  actions: {
  },
  modules: {
  }
})

~~~

在任意一个组件中，无需import即可使用：

~~~
<p>{{$store.state.count}}</p>
~~~

现在调用mutation中改变状态的方法：

~~~
<div>
    <button @click="$store.commit('add')">+</button>
    <button @click="$store.commit('reduce')">-</button>
</div>
这样进行预览就可以实现对vuex中的count进行加减了。
~~~

## 7.2、state访问

今天我们主要学习状态对象赋值给内部对象，也就是把stroe中的值，赋值给我们模板里data中的值。我们有三种赋值方式，我们一个一个来学习一下。

** 一、通过computed的计算属性直接赋值**

computed属性可以在输出前，对data中的值进行改变，我们就利用这种特性把`store.js`中的state值赋值给我们模板中的data值。

```js
export default {
        name: "hi",
        data(){
            return{
                myData:'hidzy'
            }
        },
        computed:{
            count(){
                return this.$store.state.count;
            }
        }
    }
```

然后就可以直接使用

~~~
<p>{{count}}</p>
~~~

这里需要注意的是return this.$store.state.count这一句，一定要写this，要不你会找不到$store的。这种写法很好理解，但是写起来是比较麻烦的，那我们来看看第二种写法。



**二、通过mapState的对象来赋值**

我们首先要用import引入mapState。

```
import {mapState} from 'vuex';
```

然后还在computed计算属性里写如下代码：

```
computed:mapState({
        count:state=>state.count
 })
```

这里我们使用ES6的箭头函数来给count赋值。

** 三、通过mapState的数组来赋值 **

```
 computed:mapState(["count"])
```

这个算是最简单的写法了，在实际项目开发当中也经常这样使用。

这就是三种赋值方式，是不是很简单，虽然简单，但是在实际项目中经常使用，一定要自己动手练习两遍啊。

## 7.3 、Mutation修改state

上面的加减只是一个最简单的修改状态的操作，在实际项目中我们常常需要在修改状态时传值。比如上边的例子，是我们每次只加1，而现在我们要通过所传的值进行相加。其实我们只需要在`Mutations`里再加上一个参数，并在`commit`的时候传递就就可以了。我们来看具体代码：

现在`/store/index.js`文件里给`add`方法加上一个参数n。

```
const mutations={
    add(state,n){
        state.count+=n;
    },
    reduce(state){
        state.count--;
    }
}
```

在Count.vue里修改按钮的`commit( )`方法传递的参数，我们传递10，意思就是每次加10.

```
<p>
   <button @click="$store.commit('add',10)">+</button>
   <button @click="$store.commit('reduce')">-</button>
</p>
```

这样两个简单的修改我们就完成了传值，我们可以在浏览器中实验一下了。

** 模板获取Mutations方法 **

实际开发中我们也不喜欢看到`$store.commit( )`这样的方法出现，我们希望跟调用模板里的方法一样调用。

例如：`@click=”reduce”` 就和没引用`vuex`插件一样。

要达到这种写法，只需要简单的两部就可以了：

1.在模板count.vue里用import 引入我们的mapMutations：

```
import { mapState,mapMutations } from 'vuex';
```

2.在模板的``标签里添加methods属性，并加入mapMutations

```
 methods:mapMutations([
        'add','reduce'
]),
```

通过上边两部，我们已经可以在模板中直接使用我们的reduce或者add方法了，就像下面这样。

```
<button @click="reduce">-</button>
```

## 7.4、actions异步修改状态

actions和之前讲的Mutations功能基本一样，不同点是，actions是异步的改变state状态，而Mutations是同步改变状态。

**在`/store/index.js`中声明actions** actions是可以调用Mutations里的方法的，我们还是继续在上节课的代码基础上进行学习，在actions里调用add和reduce两个方法。这里加减用了两种不同的写法

```
actions: {
  addAction(context){
    context.commit('add')
  },
  reduceAction({commit}){
    commit('reduce')
  }
```

在hi.vue组件中调用同步和异步的方法：

~~~js
<template>

    <div class="div1" id="dzy">
        <p>{{myData}}</p>
        <p>{{$store.state.count}}</p>
        <p>{{count}}</p>
        <div>
            <button @click="$store.commit('add')">+</button>
            <button @click="$store.commit('reduce')">-</button>
        </div>
        <div>
            <button @click="addAction">异步+</button>
            <button @click="reduceAction">异步-</button>
        </div>
        <router-view/>
    </div>

</template>

<script>
    import {mapActions, mapMutations, mapState} from "vuex";

    export default {
        name: "hi",
        data(){
            return{
                myData:'hidzy'
            }
        },
        computed:mapState(["count"]),
        methods:{
            ...mapMutations([
                'add','reduce'
            ]),
            ...mapActions(['addAction','reduceAction'])
        }
    }
</script>

<style scoped>
    .div1{
        color: #42b983;
    }
    .div1 p{
        color: chocolate;
    }
</style>
~~~

## 7.5、module模块组

随着项目的复杂性增加，我们共享的状态越来越多，这时候我们就需要把我们状态的各种操作进行一个分组，分组后再进行按组编写。那今天我们就学习一下module：状态管理器的模块组操作。