---
typora-copy-images-to: 笔记图像
typora-root-url: 笔记图像
---

# 简介

ECMAScript6，即JavaScript的标准

## 1 let

以前js变量的作用域：全局和函数作用域

现在又有了块级作用域，块就是用 {} 包起来的范围

<img src="/image-20200206110039522.png" alt="image-20200206110039522" style="zoom:67%;" />

会报错，因为fruit只在{} 里面

## 2 const

恒量

<img src="/image-20200206110227226.png" alt="image-20200206110227226" style="zoom:67%;" />

会报错，因为const不能重复赋值，但只是限制再次赋值的动作，并不会限制其值的改变

eg：下面的不会报错，打印出的数组有两个元素

<img src="/image-20200206110453268.png" alt="image-20200206110453268" style="zoom:67%;" />

但是下面的就会报错，因为给const重新赋值了

<img src="/image-20200206110552055.png" alt="image-20200206110552055" style="zoom:50%;" />

## 3 解构数组

老的办法（ES5）：按下标

<img src="/image-20200206110736568.png" alt="image-20200206110736568" style="zoom:67%;" />

现在的方法：

<img src="/image-20200206110840471.png" alt="image-20200206110840471" style="zoom:67%;" />

##  4 解构对象

把对象里的属性（绿色的）传递给3个变量（红色的）

<img src="/image-20200206111117634.png" alt="image-20200206111117634" style="zoom:50%;" />



## 5 数据的拷贝

![image-20200206120635014](/image-20200206120635014.png)



## 6 扩展运算符（展开操作符）

### 基本定义

#### 展开操作符：

<img src="/image-20200206122151138.png" alt="image-20200206122151138" style="zoom:67%;" />

输出：可以看出...把原数组里的元素展开来了

<img src="/image-20200206122213429.png" alt="image-20200206122213429" style="zoom:50%;" />

#### 剩余操作符

用于函数参数的传递，此处表示调用函数时，前两个变量会传递给前两个参数，其余的变量都会传递给

...foods

<img src="/image-20200206122610070.png" alt="image-20200206122610070" style="zoom:67%;" />

输出：

<img src="/image-20200206122745780.png" alt="image-20200206122745780" style="zoom:67%;" />

若想展开输出中的数组：

<img src="/image-20200206122807841.png" alt="image-20200206122807841" style="zoom:67%;" />

<img src="/image-20200206122835058.png" alt="image-20200206122835058" style="zoom:67%;" />



### 对象的扩展运算符

**`对象中的扩展运算符(...)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中`**

![image-20200206120342789](/image-20200206120342789.png)



### 数组的扩展运算符

- 可以将数组转换为参数序列

```javascript
function add(x, y) {
  return x + y;
}

const numbers = [4, 38];
add(...numbers) // 42
```

![image-20200206121037879](/image-20200206121037879.png)





## 7 模板字符串

以前的表达式：

<img src="/image-20200206111430047.png" alt="image-20200206111430047" style="zoom:67%;" />

ES6：注意`今天的早餐是 ${dessert} 与 ${drink} !`是反斜杠包住的，即esc键下面那个键

使用${} 符号，里面的东西会自动被替换为括号里变量的值，括号里可以包含一些表达式

~~~javascript
'use strict'

let dessert='cake'
let drink='milk'

let breakfast= `今天的早餐是 ${dessert} 与 ${drink} !`
console.log(breakfast)

~~~

若用普通单引号，输出的是：

今天的早餐是 ${dessert} 与 ${drink} !



## 8 带标签的模板字符串

<img src="/image-20200206114141196.png" alt="image-20200206114141196" style="zoom:67%;" />

kitchen就是一个标签，在下面一个kitchen函数可以接收被kitchen打了标签的字符串里的两个数组

strings和values

打印输出如下：其中strings指上面那句话中的字符串，values则指其中的变量，都变成了数组格式

<img src="/image-20200206121339881.png" alt="image-20200206121339881" style="zoom:67%;" />

## 8 为函数设置默认参数

<img src="/image-20200206121819705.png" alt="image-20200206121819705" style="zoom:80%;" />



## 9 箭头函数

箭头左边是函数的参数，右边是返回值、没有参数用一个空白的括号

<img src="/image-20200206123403673.png" alt="image-20200206123403673" style="zoom:67%;" />

相当于：

<img src="C:\Users\17229\AppData\Roaming\Typora\typora-user-images\image-20200206123419847.png" alt="image-20200206123419847" style="zoom:67%;" />

如果函数不是简单地要返回一个值，而是有具体的js语句操作，可以在返回值处加一个{}，里面写具体的功能

<img src="/image-20200206123838148.png" alt="image-20200206123838148" style="zoom:67%;" />



## 10 对象属性

<img src="/image-20200206124425197.png" alt="image-20200206124425197" style="zoom:67%;" />

给对象的属性赋值时，属性如果带有空格，直接赋值会报错，应该改为：

<img src="/image-20200206124522897.png" alt="image-20200206124522897" style="zoom:67%;" />



## 11 prototype，__proto__,和constructor

![image-20200206131808145](/image-20200206131808145.png)

### __proto__

![image-20200206131946133](C:\Users\17229\AppData\Roaming\Typora\typora-user-images\image-20200206131946133.png)

eg: 设置对象的__proto__为breakfast

<img src="/image-20200206132133220.png" alt="image-20200206132133220" style="zoom:50%;" />



### prototype

![image-20200206132039307](/image-20200206132039307.png)

eg：左端是输出

![image-20200206132754775](/image-20200206132754775.png)



## 12 迭代器（Iterators）

迭代器每次执行会返回一个对象：

<img src="/image-20200206133130756.png" alt="image-20200206133130756" style="zoom:50%;" />

value表示返回来的东西，done表示还有没有可以迭代的东西，没有了就是true，即完成了迭代

next方法：迭代器中每次执行这一方法都会返回上述对象，如果没有可迭代的东西，再执行next方法，value值就会变为undifined，done会变为true

 手动定义一个迭代器：

<img src="/image-20200206133631007.png" alt="image-20200206133631007" style="zoom: 50%;" />

运行迭代器：

<img src="/image-20200206133729752.png" alt="image-20200206133729752" style="zoom:67%;" />

利用generators生成迭代器：运行方法同上

<img src="/image-20200206133905986.png" alt="image-20200206133905986" style="zoom:67%;" />

或用函数表达式的方式创建生成器：

<img src="/image-20200206134217417.png" alt="image-20200206134217417" style="zoom:67%;" />

结果：

<img src="/image-20200206133953078.png" alt="image-20200206133953078" style="zoom:50%;" />



## 13 class

赶脚和Java里的类的定义差不多

<img src="/image-20200206134450486.png" alt="image-20200206134450486" style="zoom: 50%;" />

加上get和set方法：

<img src="/image-20200206134605372.png" alt="image-20200206134605372" style="zoom: 50%;" />

static ：若把函数定义为静态方法，则无需实例化就可以使用

继承：

<img src="/image-20200206134920616.png" alt="image-20200206134920616" style="zoom:50%;" />

## 14 Set

set是一堆东西的集合，有点像Array，但是Set里面的元素不能重复，其基本操作如下

<img src="/image-20200206135418634.png" alt="image-20200206135418634" style="zoom:67%;" />

## 15 Map

比对象更加灵活的键值对形式：键值可以是各种形式：对象，函数，字符串等

![image-20200206140135375](/image-20200206140135375.png)

## 16 Modules

模块化编程

写一个小模块：

<img src="/image-20200206140826102.png" alt="image-20200206140826102" style="zoom:67%;" />

在另一个模块里引用：

<img src="/image-20200206140904342.png" alt="image-20200206140904342" style="zoom:67%;" />

或者：

<img src="/image-20200206140925495.png" alt="image-20200206140925495" style="zoom:67%;" />

默认导出的东西：

<img src="/image-20200206141149700.png" alt="image-20200206141149700" style="zoom:50%;" />

这样在导入时就可以不写明导入了什么，只给导入的东西起一个名字：

<img src="/image-20200206141258456.png" alt="image-20200206141258456" style="zoom:50%;" />

再或者：

<img src="/image-20200206141328264.png" alt="image-20200206141328264" style="zoom:50%;" />



# 17 Promise

S6中的promise的出现给我们很好的解决了回调地狱的问题，在使用ES5的时候，在多层嵌套回调时，写完的代码层次过多，很难进行维护和二次开发，比如各个函数之间具有先后逻辑关系，得等上一个返回执行成功的提示后写一个函数才可以执行，层层嵌套，烦。

ES6认识到了这点问题，现在promise的使用，完美解决了这个问题。

**promise的基本用法**

promise执行多步操作非常好用，那我们就来模仿一个多步操作的过程，那就以吃饭为例吧。要想在家吃顿饭，是要经过三个步骤的。

1. 洗菜做饭。
2. 坐下来吃饭。
3. 收拾桌子洗碗。



这个过程是有一定的顺序的，你必须保证上一步完成，才能顺利进行下一步。我们可以在脑海里先想想这样一个简单的过程在ES5写起来就要有多层的嵌套。那我们现在用promise来实现。

```js
let state=1;

function step1(resolve,reject){
    console.log('1.开始-洗菜做饭');
    if(state==1){
        resolve('洗菜做饭--完成');
    }else{
        reject('洗菜做饭--出错');
    }
}


function step2(resolve,reject){
    console.log('2.开始-坐下来吃饭');
    if(state==1){
        resolve('坐下来吃饭--完成');
    }else{
        reject('坐下来吃饭--出错');
    }
}


function step3(resolve,reject){
    console.log('3.开始-收拾桌子洗完');
     if(state==1){
        resolve('收拾桌子洗完--完成');
    }else{
        reject('收拾桌子洗完--出错');
    }
}
//初始化：new Promise(step1).then().then().then()即3个then代表等待三个步骤执行
//val就是resolve或者reject返回的值
new Promise(step1).then(function(val){//开始执行第一步
    console.log(val);
    return new Promise(step2);//开始执行第二步

}).then(function(val){
     console.log(val);
    return new Promise(step3);//开始执行第三步
}).then(function(val){
    console.log(val);
    return val;
});
```