---
typora-root-url: 笔记图像
typora-copy-images-to: 笔记图像
---

# 1 基础语法

在html里面引用css：

~~~html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link href="css01.css" type="text/css" rel="stylesheet">
</head>
~~~



![image-20200123185553615](/image-20200123185553615.png)

几个元素定义相同的样式：

![image-20200123185836123](/image-20200123185836123.png)

*表示通配符，对所有的元素都起作用

## 1.1 属性的继承

body定义color为红，则嵌套在body‘里面的其他元素默认字体颜色也是红色

## 1.2 选择器

### 1.2.1派生选择器

~~~html
    <ul>
        <li><strong>li标签的样式</strong></li>
    </ul>
~~~

样式：ul与其中嵌套的strong用空格隔开

若A和B设置相同的css样式，则A，B{  }

~~~css
ul strong{
    color: aquamarine;
    font-size: 50px;
}
~~~

### 1.2.2 id选择器

id唯一但是class可以重复

<img src="/image-20200123191117775.png" alt="image-20200123191117775" style="zoom:67%;" />

id选择器与派生选择器同时使用：

~~~html
    <div id="div1">
        div的样式
        <p>p的样式</p>
    </div>
~~~

~~~css
#div1 p{
    color: blueviolet;
    font-size: 30px;
}
~~~

### 1.2.3 类选择器

<img src="/image-20200123191750543.png" alt="image-20200123191750543" style="zoom:67%;" />

<img src="/image-20200129124121348.png" alt="image-20200129124121348" style="zoom:67%;" />

~~~html
    <div class="class1">
        div的样式
        <p>p的样式</p>
    </div>
~~~

~~~css
.class1 p{
    color: cadetblue;
    font-size: 20px;
}
~~~



属性与类选择器结合：a.class

<img src="/image-20200129123710446.png" alt="image-20200129123710446" style="zoom:67%;" />

css：

<img src="/image-20200129123736223.png" alt="image-20200129123736223" style="zoom:67%;" />

此时只有a中的名为div 的class被加了样式，上面那个并没有



多类选择器：

<img src="/image-20200129123310930.png" alt="image-20200129123310930" style="zoom:67%;" />

p1空格p2，拥有p1和p2共同的性质，而且还拥有自己的性质

<img src="/image-20200129123415789.png" alt="image-20200129123415789" style="zoom:50%;" />

即p1 p2 这个类即有p1 的blue属性又有p2 的30px，还有自己的斜体属性

### 1.2.4 属性选择器

~~~html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style type="text/css">
        [title]{
            color: brown;
        }
        [title="t1"]{
            color: cornflowerblue;
        }
    </style>
</head>

<body>
    <p title="t1">属性选择器</p>
    <p title="t2">属性和值选择器</p>
</body>
~~~



模糊的匹配：

<img src="/image-20200129124409509.png" alt="image-20200129124409509" style="zoom:67%;" />

下面4个title中，只有含title字符串的才会被修改属性

### 1.2.5 冒号选择器

冒号选择器：功能强大 eg：a：hover 设置鼠标悬停时的样式变化

<img src="/image-20200123200003313.png" alt="image-20200123200003313" style="zoom:67%;" />

## 1.3  背景

<img src="/image-20200128152419305.png" alt="image-20200128152419305" style="zoom:67%;" />

<img src="/image-20200128152534901.png" alt="image-20200128152534901" style="zoom:67%;" />

## 1.4  文本

<img src="/image-20200128152704196.png" alt="image-20200128152704196" style="zoom: 80%;" />

![image-20200128153449017](/image-20200128153449017.png)

<img src="/image-20200128154133373.png" alt="image-20200128154133373" style="zoom: 67%;" />

<img src="/image-20200128161832595.png" alt="image-20200128161832595" style="zoom: 67%;" />

<img src="/image-20200129113101949.png" alt="image-20200129113101949" style="zoom: 50%;" />

## 1.5 列表

<img src="/image-20200128154651029.png" alt="image-20200128154651029" style="zoom:50%;" />

![image-20200128154536961](/image-20200128154536961.png)

效果如下：

<img src="/image-20200128154729526.png" alt="image-20200128154729526" style="zoom:80%;" />

display属性  ：inline 可以把列表变为一行用于做导航栏

## 1.6 表格

~~~html
    <table id="tb">
        <tr>
            <th>姓名</th>
            <th>年龄</th>
            <th>职业</th>
        </tr>
        <tr class="alt">
            <td>王刚</td>
            <td>22</td>
            <td>学生</td>
        </tr>
        <tr>
            <td>王刚</td>
            <td>22</td>
            <td>学生</td>
        </tr>
        <tr class="alt">
            <td>王刚</td>
            <td>22</td>
            <td>学生</td>
        </tr>
    </table>
~~~

~~~css
#tb{
    border-collapse: collapse;
    width: 500px;
}
#tb td,#tb th{
    border: 1px solid aquamarine;
    padding: 5px; /* 内边距为5px*/
}
#tb th{
    text-align: right;
    background-color: aquamarine;
    color: azure;
}
.alt td{   /*相邻两行颜色不一样*/
    color: black;
    background-color: antiquewhite;
}
~~~

效果如下：

<img src="/image-20200128161638457.png" alt="image-20200128161638457" style="zoom:80%;" />



# 2  定位

<img src="C:\Users\17229\AppData\Roaming\Typora\typora-user-images\image-20200128171218576.png" alt="image-20200128171218576" style="zoom:67%;" />

<img src="/image-20200128171306685.png" alt="image-20200128171306685" style="zoom:67%;" />

默认的普通流：按照html里的顺序在页面里面从上到下，从左到右排列

## 2.1 position属性

position ： absolute ，relative，fixed，static



### 2.1.1 relative:

 ~~~html
<html>
<head>
<style type="text/css">
h2.pos_left
{
position:relative;
left:-20px
}
h2.pos_right
{
position:relative;
left:20px
}
</style>
</head>

<body>
<h2>这是位于正常位置的标题</h2>
<h2 class="pos_left">这个标题相对于其正常位置向左移动</h2>
<h2 class="pos_right">这个标题相对于其正常位置向右移动</h2>
<p>相对定位会按照元素的原始位置对该元素进行移动。</p>
<p>样式 "left:-20px" 从元素的原始左侧位置减去 20 像素。</p>
<p>样式 "left:20px" 向元素的原始左侧位置增加 20 像素。</p>
</body>

</html>

 ~~~

<img src="/image-20200128170707922.png" alt="image-20200128170707922" style="zoom:67%;" />



### 2.1.2 absolute:

~~~html
<html>
<head>
<style type="text/css">
h2.pos_abs
{
position:absolute;
left:100px;
top:150px
}
</style>
</head>
<body>
<h2 class="pos_abs">这是带有绝对定位的标题</h2>
<p>通过绝对定位，元素可以放置到页面上的任何位置。下面的标题距离页面左侧 100px，距离页面顶部 150px。</p>
</body>
</html>
~~~

<img src="/image-20200128171047791.png" alt="image-20200128171047791" style="zoom:67%;" />

### 2.1.3 fixed: 

让某个东西固定在页面的某个位置，上下翻页面那个块也不动，例如哔哩哔哩的回到页面顶部功能块，某些网站的联系客服功能



### 2.1.4 z-index：

当几个元素叠在一起时，数字大的在上方



## 2.2 浮动

###   2.2.1 简介

eg：

<img src="/image-20200128173946831.png" alt="image-20200128173946831" style="zoom:67%;" />

<img src="/image-20200128174017318.png" alt="image-20200128174017318" style="zoom:67%;" />

三个div都向左浮动，再写个helloworld  也会继承向左浮动，去掉此继承效果：

<img src="/image-20200128174140075.png" alt="image-20200128174140075" style="zoom:67%;" />

### 2.2.2 浮动的应用

瀑布流效果

<img src="/image-20200128174731353.png" alt="image-20200128174731353" style="zoom:67%;" />

<img src="C:\Users\17229\AppData\Roaming\Typora\typora-user-images\image-20200128174805619.png" alt="image-20200128174805619" style="zoom:50%;" />

css：

其中*为通配符，对所有的元素都起作用，margin：外边距，padding：内边距

<img src="C:\Users\17229\AppData\Roaming\Typora\typora-user-images\image-20200128174833146.png" alt="image-20200128174833146" style="zoom:50%;" />

div1中 margin的参数：第一个表示高度，第二个表示宽度

瀑布流的实际效果：即等宽但是高度不相同： 这样的效果用js来实现更加完美

<img src="/image-20200129162318031.png" alt="image-20200129162318031" style="zoom:50%;" />

~~~html
    <div class="container">
        <div><img src="img/1.jpg"></div>
        <div><img src="img/2.jpg"></div>
        <div><img src="img/3.jpg"></div>
        <div><img src="img/4.jpg"></div>
        <div><img src="img/5.jpg"></div>
        <div><img src="img/6.jpg"></div>
        <div><img src="img/7.jpg"></div>
        <div><img src="img/8.jpg"></div>
        <div><img src="img/9.jpg"></div>
        <div><img src="img/10.jpg"></div>
        <div><img src="img/11.jpg"></div>
        <div><img src="img/12.jpg"></div>
    </div>
~~~

~~~css
.container{
    column-width: 250px;
    -webkit-column-width: 250px;   /*适配chrome浏览器*/
    column-gap: 5px;
    -webkit-column-gap: 5px;
}
.container div{
    width: 250px;
    margin: 5px 0;
}
~~~





## 2.3 盒子模型

### 2.3.1 基本内容

<img src="/image-20200128175227082.png" alt="image-20200128175227082" style="zoom:67%;" />

<img src="/image-20200128175442371.png" alt="image-20200128175442371" style="zoom:67%;" />

<img src="/image-20200128175609588.png" alt="image-20200128175609588" style="zoom:67%;" />

<img src="/image-20200128175710186.png" alt="image-20200128175710186" style="zoom:67%;" />

外边距合并原则：定义的两个盒子的外边距会自动合并，只剩下大的那个边距

<img src="/image-20200128180523399.png" alt="image-20200128180523399" style="zoom:67%;" />

### 2.3.2 盒子的应用

对以下网站的布局进行简单的实现，画出盒子模型

<img src="/image-20200128180958010.png" alt="image-20200128180958010" style="zoom:67%;" />

<img src="/image-20200128180924438.png" alt="image-20200128180924438" style="zoom:67%;" />

 ~~~html
<body>
    <div class="top">
        <div class="top_content"></div>
    </div>
    <div class="body">
        <div class="body_img"></div>
        <div class="body_content">
            <div class="body_news"></div>
        </div>
    </div>
    <div class="footing">
        <div class="foot_content"></div>
        <div class="foot_menu"></div>
    </div>
</body>
 ~~~

~~~css
*{
    margin: 0px;
    padding: 0px;
}
.top{
    width: 100%;
    height: 50px;
    background-color: black;
}
.top_content{
    width: 75%;
    height: 50px;
    margin: 0px auto; /* 上下0px，左右auto会居中*/
    background-color: blue;
}
.body{
    margin: 20px auto;
    width: 75%;
    height: 1500px;
    background-color: antiquewhite;
}
.body_img{
    width: 100%; /*这里的100%是body的100%，也就是整体的75%*/
    height: 400px;
    background-color: blueviolet;
}
.body_content{
    width: 100%;
    height: 1100px;
    background-color: chocolate;
}
.body_news{
    width: 100%;
    height: 50px;
    background-color: aquamarine;
}
.footing{
    margin: 0px auto;
    width: 75%;
    height: 330px;
    background-color: cadetblue;
}
.foot_content{
    width: 100%;
    height: 270px;
    background-color: darkslategray;
}
.foot_menu{
    width: 100%;
    height: 60px;
    background-color:darkkhaki;
}

~~~

# 3 实例、效果

## 3.0首页

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <link href="mycss.css"  type="text/css" rel="stylesheet">
</head>
<body>
    <div class="container">
        <div class="wrapper">
            <div class="heading">
                <div class="heading_navi">
                    <div class="heading_title">
                        在线图书馆
                    </div>

                    <div class="heading_navibar">
                        <ul>
                            <li><a href="#">首页</a></li>
                            <li><a href="#">馆藏</a></li>
                            <li><a href="#">个人中心</a></li>
                            <li><a href="#">帮助</a></li>
                        </ul>
                    </div>

                    <div  class="heading_img">
                        <img src="img/a.jpg">
                    </div>

                    <div class="heading_spotlight">
                        <form>
                            <input type="text">
                        </form>
                    </div>
                </div>
            </div>
<!--body-->
            <div class="body">
                <div class="body_title">
                    <h3>图书服务</h3>
                    <p>
                        在线借阅，归还，分享，评论书籍。
                        欢迎你的加入
                    </p>
                    <hr>
                    <hr>
                </div>
            </div>

        </div>

        <div class="footing">
            @dzy
        </div>
    </div>
</body>
</html>
~~~

~~~css
*{
    margin: 0px;
    padding: 0px;
}
body{
    background-color: snow;
}
.wrapper{
    width:80%;
    height: 1000px;
    background-color: #8eb845;
    margin: 0px auto;
}
.heading{
    margin: 0px auto;
    width: 100%;
    height: 120px;
    background-color: snow;
}
.heading_title{
    float: left;
    font-family: "Adobe Arabic";
    font-size: 50px;
    color: #8eb845;
}

.heading_navi{
    padding-bottom: 30px;
    padding-top: 30px;
    width: 100%;
    height:30px;
    position: relative;
}

ul{
    margin-left: 40px;
    float: left;
    list-style-type: none;
    padding-top: 13px;
    padding-bottom: 6px;
    font-size: 30px;
}

li{
    padding-left: 10px;
    display: inline;
}
a:link,a:visited{
    font-weight: bolder;
    color: #a08269;
    text-align: center;
    padding: 6px;
    text-decoration: none;
}
a:hover,a:active{
    color: #643425;
}

.heading_img img{
    border-radius: 30px;
    display: inline;
    width: 46px;
    height: 46px;
    box-shadow: 0 1px 1px #fffed7;
    float: right;
    margin-top: 10px;
}
.heading_spotlight form{
    float: right;
    width:150px;
    height:30px;
    position: relative;
    margin-right: 100px;
    margin-top: 20px;
}
form input{
    height: 30px;
    border-radius: 30px;
}

.body_title h3{
    font-size: 30px;
    font-family: "Adobe Arabic";
    color: #643425;
}
.body_title p{
    margin-top: 20px;
    margin-bottom: 20px;
    font-size: 20px;
    color: #643425;
}
.body{
    padding: 30px;
    height: auto;
    width: auto;
}

.footing{
    padding-top: 20px;
    padding-bottom: 20px;
    text-align: center;
    font-size: 15px;
    color: darkgray;
}
~~~

效果：

<img src="/image-20200211103020110.png" alt="image-20200211103020110" style="zoom:67%;" />

![image-20200211103040520](C:\Users\17229\AppData\Roaming\Typora\typora-user-images\image-20200211103040520.png)



## 3.1 导航栏

~~~html
    <ul>
        <li><a href="#">导航1</a></li>
        <li><a href="#">导航2</a></li>
        <li><a href="#">导航3</a></li>
        <li><a href="#">导航4</a></li>
    </ul>
~~~

垂直的导航栏：

~~~css
ul{
    list-style-type: none; /*去掉列表前面的小点*/
    margin: 0px;
    padding: 0px;
}
a:link,a:visited{
    text-decoration: none;   /*去掉a标签里的下划线，无论是未点击的还是已经点击的*/
    display: block; /*垂直导航栏需要设置这个*/
    background-color: blueviolet;
    color: azure;
    width: 50px;
    text-align: center;
}
a:active,a:hover{
    background-color: cadetblue;
}

~~~

![image-20200129114943479](/image-20200129114943479.png)

水平导航栏

~~~css
ul{
    list-style-type: none; /*去掉列表前面的小点*/
    margin: 0px;
    padding: 0px;
    background-color: chocolate;
    width: 250px;

}
a:link,a:visited{
    font-weight: bolder;
    text-decoration: none;   /*去掉a标签里的下划线，无论是未点击的还是已经点击的*/
    color: azure;
    width: 50px;
    text-align: center;
}
a:active,a:hover{
    background-color: cadetblue;
}
li{
    display: inline;
    padding-left: 5px;
    padding-right: 5px;
}

~~~

效果：

![image-20200129115617028](/image-20200129115617028.png)

## 3.2 图片排列

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>image</title>
    <link href="img.css" type="text/css" rel="stylesheet">
</head>
<body>
    <div class="img">
        <a href="#" target="_self">
            <img src="img/a.jpg" alt="风景" width="300px" height="230px">
        </a>
        <div class="text">夜晚的风景</div>
    </div>
    <div class="img">
        <a href="#" target="_self">
            <img src="img/a.jpg" alt="风景" width="300px" height="230px">
        </a>
        <div class="text">夜晚的风景</div>
    </div>
    <div class="img">
        <a href="#" target="_self">
            <img src="img/a.jpg" alt="风景" width="300px" height="230px">
        </a>
        <div class="text">夜晚的风景</div>
    </div>
    <div class="img">
        <a href="#" target="_self">
            <img src="img/a.jpg" alt="风景" width="300px" height="230px">
        </a>
        <div class="text">夜晚的风景</div>
    </div>
    <div class="img">
        <a href="#" target="_self">
            <img src="img/a.jpg" alt="风景" width="300px" height="230px">
        </a>
        <div class="text">夜晚的风景</div>
    </div>
    <div class="img">
        <a href="#" target="_self">
            <img src="img/a.jpg" alt="风景" width="300px" height="230px">
        </a>
        <div class="text">夜晚的风景</div>
    </div>
    <div class="img">
        <a href="#" target="_self">
            <img src="img/a.jpg" alt="风景" width="300px" height="230px">
        </a>
        <div class="text">夜晚的风景</div>
    </div>
    <div class="img">
        <a href="#" target="_self">
            <img src="img/a.jpg" alt="风景" width="300px" height="230px">
        </a>
        <div class="text">夜晚的风景</div>
    </div>
</body>
</html>
~~~

~~~css
body{
    margin: 100px auto;
    width:70%;
    height: auto;
    background-color: bisque;
}
.img{
    border: 1px solid darkgray;
    width: auto;
    height: auto;
    float:left;
    text-align: center;
    margin: 5px;
}
img{
    margin: 5px;
    opacity: 0.9;  /*透明度*/
}
.text{
    font-size: 12px;
}
~~~

![image-20200129121514896](/image-20200129121514896.png)

 

## 3.3 css动画

<img src="/image-20200129130055389.png" alt="image-20200129130055389" style="zoom: 33%;" />

### 3.3.1 2D效果

<img src="/image-20200129125815059.png" alt="image-20200129125815059" style="zoom:67%;" />

css：需要注明适配各种浏览器

<img src="/image-20200129125840049.png" alt="image-20200129125840049" style="zoom:67%;" />

效果：

<img src="/image-20200129125930254.png" alt="image-20200129125930254" style="zoom:33%;" />

### 3.3.2 过渡

<img src="/image-20200129154954065.png" alt="image-20200129154954065" style="zoom: 50%;" />

~~~html
    <div class="ttt">
        效果
    </div>
~~~

~~~css
.ttt{
    width: 200px;
    height: 200px;
    background-color: blueviolet;
    -webkit-transition: width 2s,height 2s,-webkit-transform 2s;
    transition: width 2s,height 2s,transform 2s;
    -webkit-transition-delay: 1s;
    transition-delay: 1s;
}
.ttt:hover{
    width: 200px;
    height:200px;
    transform: rotate(360deg);
    -webkit-transform: rotate(360deg);
}
~~~

效果：鼠标移到紫色的方块上面一秒钟后方块旋转360度