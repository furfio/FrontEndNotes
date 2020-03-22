---
typora-root-url: 笔记图像
typora-copy-images-to: 笔记图像
---

# 1

## 1.2 基本用法

<img src="/image-20200130115704520.png" alt="image-20200130115704520" style="zoom: 50%;" />

html调用：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>test01</title>
    <script src="test01.js"> </script>
</head>
<body>

</body>
</html>
~~~

test01.js:

~~~javascript
document.write("hello dzy");
~~~

或者：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>test01</title>
    <script>
        document.write("<h1>hello world</h1>");
    </script>
</head>
<body>

</body>
</html>
~~~

又eg：利用id，用js改变html元素的内容：网页上将显示吼吼吼而不是啦啦啦

~~~javascript
    <p id="ppp">啦啦啦</p>
    <script>
        document.getElementById("ppp").innerHTML="吼吼吼";
    </script>
~~~

## 1.3 变量，语句

### 1.3.1 变量

变量的定义：

<img src="/image-20200130123630921.png" alt="image-20200130123630921" style="zoom:67%;" />

数组还可以：

<img src="/image-20200130123716458.png" alt="image-20200130123716458" style="zoom:67%;" />

还可以动态地赋值，不规定数组的大小：

<img src="/image-20200130123802484.png" alt="image-20200130123802484" style="zoom:67%;" />

eg：点击按钮，显示加和

~~~javascript
    <p id="ppp"></p>
    <button onclick="sum()">结果</button>
    <script>
        function sum(){
            var i=1;
            var j=2;
            var m=i+j;
            document.getElementById("ppp").innerHTML=m;
        }
    </script>
~~~

eg：类型转换，任何类型与字符串类型相加，都会转化为字符串类型：结果为55

<img src="/image-20200130124723744.png" alt="image-20200130124723744" style="zoom:67%;" />

局部变量和全局变量：

<img src="/image-20200130132359524.png" alt="image-20200130132359524" style="zoom:67%;" />

### 1.3.2 运算符

以下结果为true

<img src="/image-20200130124952443.png" alt="image-20200130124952443" style="zoom:67%;" />

但若改为三等号：结果为false，===要求类型也必须一致

<img src="/image-20200130125044157.png" alt="image-20200130125044157" style="zoom:67%;" />

类似的有!=和！==

三目运算符：条件运算符

<img src="/image-20200130125409749.png" alt="image-20200130125409749" style="zoom:67%;" />

### 1.3.3函数调用

两种方式：在script中，在html中

<img src="/image-20200130131849421.png" alt="image-20200130131849421" style="zoom:67%;" />

# 2 JavaScript异常、事件

## 2.1 异常

eg：

<img src="/image-20200131164935430.png" alt="image-20200131164935430" style="zoom:67%;" />

经常与throw一起使用，抛出用户自定义的错误，输入为空时弹出提示

![image-20200131165334386](/image-20200131165334386.png)



## 2.2 事件

### 2.2.1 简介

指可以被JavaScript侦测到的行为；

eg：

<img src="/image-20200131165619210.png" alt="image-20200131165619210" style="zoom: 50%;" />

eg1：鼠标经过div以及移出div时显示不同字，其中this指代当前的函数

![image-20200131165957517](/image-20200131165957517.png)

eg2：

~~~html
<body onload="alert('网页内容加载完毕')">
~~~

### 2.2.2 事件流

<img src="/image-20200131180500397.png" alt="image-20200131180500397" style="zoom:67%;" />

即事件流分为两种顺序：具体元素→不具体元素    不具体元素→具体元素

冒泡更为常见

不具体到具体：文档→html→body→具体元素

eg：冒泡：按钮事件最先由button标签捕获，然后是div，body html document

<img src="/image-20200131180746336.png" alt="image-20200131180746336" style="zoom:50%;" />

### 2.2.3 事件处理

<img src="/image-20200131181141909.png" alt="image-20200131181141909" style="zoom:50%;" />

<img src="/image-20200131181202374.png" alt="image-20200131181202374" style="zoom: 67%;" />

即最普通的事件处理方式。



<img src="/image-20200131181540405.png" alt="image-20200131181540405" style="zoom:67%;" />

eg：3会覆盖1和2

<img src="/image-20200131181650060.png" alt="image-20200131181650060" style="zoom:67%;" />



<img src="/image-20200131181611675.png" alt="image-20200131181611675" style="zoom:67%;" />



<img src="/image-20200131182311382.png" alt="image-20200131182311382" style="zoom:50%;" />

关于浏览器的适配方法：用if语句判断支持哪个dom事件级别

<img src="/image-20200131182207053.png" alt="image-20200131182207053" style="zoom:67%;" />

### 2.2.4 事件对象

<img src="/image-20200131182453577.png" alt="image-20200131182453577" style="zoom:33%;" />

eg：网页弹出click

<img src="/image-20200131182735333.png" alt="image-20200131182735333" style="zoom: 67%;" />

网页弹出

<img src="/image-20200131182853001.png" alt="image-20200131182853001" style="zoom:67%;" />

<img src="/image-20200131182815129.png" alt="image-20200131182815129" style="zoom:67%;" />



eg：事件的冒泡（div包含button）

<img src="/image-20200131183020866.png" alt="image-20200131183020866" style="zoom:67%;" />

点击按钮后也会触发div的函数，弹出两次对话框

阻止：

<img src="/image-20200131183254269.png" alt="image-20200131183254269" style="zoom:67%;" />





## 2.3 DOM

### 2.3.1 什么是DOM

<img src="/image-20200131171311644.png" alt="image-20200131171311644" style="zoom: 50%;" />

### 2.3.2 DOM操作HTML

<img src="/image-20200131171359198.png" alt="image-20200131171359198" style="zoom:67%;" />

<img src="/image-20200131174201385.png" alt="image-20200131174201385" style="zoom:67%;" />

eg：这样world不会覆盖掉hello

<img src="/image-20200131173702102.png" alt="image-20200131173702102" style="zoom:67%;" />

但是如下点击按钮后就会覆盖掉：因为是在文档加载完成后才点的按钮：

<img src="/image-20200131173759875.png" alt="image-20200131173759875" style="zoom:67%;" />



通过id来找到元素：

<img src="/image-20200131173953938.png" alt="image-20200131173953938" style="zoom:67%;" />

通过标签来找到：如果有多个相同的标签，则指第一个

<img src="/image-20200131174307432.png" alt="image-20200131174307432" style="zoom:67%;" />

修改元素的属性：按下按钮之后链接被篡改

<img src="/image-20200131174516852.png" alt="image-20200131174516852" style="zoom:50%;" />



方法：（全）其中子节点就是指在元素中嵌套的元素

<img src="/image-20200131203244259.png" alt="image-20200131203244259" style="zoom:50%;" />

设置属性：

<img src="/image-20200131204028267.png" alt="image-20200131204028267" style="zoom:50%;" />

获得子节点，父节点

<img src="/image-20200131204256199.png" alt="image-20200131204256199" style="zoom: 67%;" />

添加元素（即节点） 例为在网页中用js添加一个按钮，注意append是在末尾添加

<img src="/image-20200131204423502.png" alt="image-20200131204423502" style="zoom:67%;" />

在特定的地方添加，如某元素之前：在div中的pid之前添加一个元素

<img src="/image-20200131204841808.png" alt="image-20200131204841808" style="zoom:67%;" />

删除子节点：

<img src="/image-20200131205111643.png" alt="image-20200131205111643" style="zoom:67%;" />

获取网页尺寸：offset不包含滚动条，scroll包括，其中||是考虑浏览器的兼容性用两种写法

<img src="/image-20200131205352576.png" alt="image-20200131205352576" style="zoom: 67%;" />



### 2.3.3 DOM操作css

基本语法：
<img src="/image-20200131175054280.png" alt="image-20200131175054280" style="zoom:67%;" />

eg：通过按钮事件触发js方法，改变div 的背景颜色

![image-20200131175127065](/image-20200131175127065.png)

### 2.3.4 DOM EventListener

<img src="/image-20200131175334717.png" alt="image-20200131175334717" style="zoom:50%;" />

eg：添加事件监听器   按下按钮就执行函数，监听按钮是否按下

![image-20200131175626924](/image-20200131175626924.png)

添加事件监听器的好处，可以给这个按钮添加多个句柄，句柄之间不会冲突

eg：按钮按第一下弹出hello，然后弹出world

<img src="/image-20200131175910502.png" alt="image-20200131175910502" style="zoom: 67%;" />

移除句柄：

![image-20200131180211012](/image-20200131180211012.png)



# 3 JavaScript对象

## 3.1 简介

<img src="/image-20200131183636775.png" alt="image-20200131183636775" style="zoom: 50%;" />

定义对象的方法：

<img src="/image-20200131183907252.png" alt="image-20200131183907252" style="zoom:67%;" />

2：

<img src="/image-20200131183927346.png" alt="image-20200131183927346" style="zoom:67%;" />

3，用函数定义对象（js中不存在类的概念，所以用function来模拟类）



<img src="/image-20200131184133580.png" alt="image-20200131184133580" style="zoom:67%;" />

另外的定义对象的方法

<img src="/image-20200201113456273.png" alt="image-20200201113456273" style="zoom:67%;" />



## 3.2 JavaScript内置对象

### 3.2.1 String

<img src="/image-20200131185121752.png" alt="image-20200131185121752" style="zoom:67%;" />

![image-20200131184517834](/image-20200131184517834.png)

定义一个str后，它具有各种属性

str.length

str.indexof("xxx")  返回xxx在str中的起始位置不存在返回-1

str.match("xxx" )  字符串匹配，存在xxx就返回xxx，不存在就返回null

repalce（a,b）把a换为b

toUpper

split

### 3.2.2 Date

<img src="/image-20200131185350962.png" alt="image-20200131185350962" style="zoom:67%;" />

eg：

<img src="/image-20200131185415188.png" alt="image-20200131185415188" style="zoom:67%;" />

### 3.3.3 Array

<img src="/image-20200131202334892.png" alt="image-20200131202334892" style="zoom: 50%;" />

升序

<img src="/image-20200131202607588.png" alt="image-20200131202607588" style="zoom:50%;" />

降序

<img src="/image-20200131202634499.png" alt="image-20200131202634499" style="zoom:50%;" />

### 3.3.4 Math

<img src="/image-20200131202937995.png" alt="image-20200131202937995" style="zoom:50%;" />

## 3.3 JavaScript浏览器对象

### 3.3.1 window对象

<img src="/image-20200131210042298.png" alt="image-20200131210042298" style="zoom: 50%;" />

即window.document .write() =document.write()

![image-20200131210552712](/image-20200131210552712.png)

效果如下：规定新的页面的打开位置和大小

<img src="/image-20200131210611314.png" alt="image-20200131210611314" style="zoom:50%;" />

### 3.3.2 计时器

<img src="/image-20200131211022597.png" alt="image-20200131211022597" style="zoom:67%;" />

eg：页面每隔1秒刷新一次时间，按按钮后停止刷新

<img src="/image-20200131211324909.png" alt="image-20200131211324909" style="zoom:67%;" />

### 3.3.3  History对象

<img src="/image-20200201104433178.png" alt="image-20200201104433178" style="zoom: 50%;" />

### 3.3.4 Location

<img src="/image-20200201110027280.png" alt="image-20200201110027280" style="zoom: 50%;" />

### 3.3.5 Screen

可用于屏幕适配

<img src="/image-20200201111533921.png" alt="image-20200201111533921" style="zoom:67%;" />