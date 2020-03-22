---
typora-root-url: 笔记图像
typora-copy-images-to: 笔记图像
---

# html5

## 1、基本结构



```html
<!DOCTYPE html><!--  这个标明是html5版本-->
<html lang="en"><!--  en 英文 zh中文  也可以不写-->
<head>    
    <meta charset="UTF-8">    
    <title>Title</title>
    <link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
<body>    
    league of legends
    <p>定义一段文字</p>
    <a href="https://www.baidu.com">百度</a>
</body>
</html>
```

## 2、常见元素及其属性

***html中的大多数元素都是可以相互嵌套的***

###  2.1  a标签	

```html
<a href ="xxx.html" target="_blank"> 打开本地html文件，target是打开的位置 _blank表示打开新的页面，self表示当前页面 还有top，parent，在3.3 iframe处讲解</a>
```

~~~ html
<a href="https://www.baidu.com">
        <img src="img/a.jpg" width="100 height="100" alt="html5logo">
        点击图片跳转 alt表示如果图片找不到或者无法正常显示，就在图片位置显示一行字
</a>
~~~

~~~html
<a name="top">top</a>利用name属性指定文档内的链接，类似于网页内跳转位置（比如回到顶部啥的）
<a href="#top">回到页面中的top处</a>
~~~



### 2.2  h1 到h6

~~~
<h1 align="center">标题1 居中</h1>
~~~



### 2.3  body

bgcolor，background(背景图片)



### 2.4  表格标签

<img src="/image-20200113160928935.png" alt="image-20200113160928935" style="zoom:67%;" />

~~~html
<table border="1" cellpadding="10" cellspacing="2" bgcolor="#ffebcd" background="img/a.jpg">  
    border是表格边框，cellpadding是单元格大小，cellspaceing是单元格之间的间距
        <caption>表格1</caption>  表格标题
        <tr>
            <th>1</th>    单元格标题
            <th>2</th>
            <th>3</th>
        </tr>
        <tr>
            <td>单元格1</td>   行中的每一列
            <td>单元格2</td>
            <td>单元格3</td>
        </tr>
        <tr>
            <td>
                <ul>
                    <li>苹果</li>
                    <li>梨</li>
                    <li>香蕉</li>
                </ul>
            </td>
            <td>单元格5</td>
            <td>单元格6</td>
        </tr>
    </table>
~~~

![image-20200113162943098](/image-20200113162943098.png)



### 2.5  列表

<img src="/image-20200113162631511.png" alt="image-20200113162631511" style="zoom: 33%;" />

<img src="/image-20200114091615155.png" alt="image-20200114091615155" style="zoom: 67%;" />



~~~html
	<ul type="square">  square表示无序列表前面的小圆点改为方块
        <li>苹果</li>
        <li>梨</li>
        <li>香蕉</li>
	</ul>
    <ol type="a"> a表示把默认的1，2，3改为a，b，c， I表示罗马数字 start="10",表示从10开始
        <li>苹果</li>
        <li>梨</li>
        <li>香蕉</li>
    </ol>
~~~



<img src="/image-20200114091826200.png" alt="image-20200114091826200" style="zoom: 80%;" />

~~~html
    <dl>
        <dt>hello</dt>
            <dd>啦啦啦</dd>
        <dt>hello</dt>
            <dd>啦啦啦</dd>
        <dt>hello</dt>
            <dd>啦啦啦</dd>
    </dl>
~~~

![image-20200114093135531](/image-20200114093135531.png)

### 2.6  块元素

<img src="/image-20200114093323685.png" alt="image-20200114093323685" style="zoom:67%;" />

div不指定块里面的内容，span指定里面是文本内容 一般是div里面嵌套span使用



## 3、HTML布局

### 3.1、使用div布局

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>布局</title>
    <style type="text/css">
        body{
            margin: 0px;  <!-- 让内容铺满这个页面，不留白色的边距   -->
        }
        div#container{
            width: 100%;  <!-- 可以用比例 -->
            height: 950px;
            background-color: bisque;
        }
        div#heading{
            width: 100%;
            height: 10%;
            background-color: darkcyan;
        }
        div#menu{
            width: 30%;
            height: 80%;
            background-color: blueviolet;
            float: left;   <!-- 这个很有必要，否则body跑到menu下面去了， -->
                           <!--  都向左浮动会让他们在同一行  -->
        }
        div#body{
            width: 70%;
            height: 80%;
            background-color: brown;
            float: left;
        }
        div#foot{
            width: 100%;
            height: 10%;
            background-color: chartreuse;
            clear: both;  <!-- 这个很有必要，否则foot还是跟着menu和body在浮动 -->
        }
    </style>
</head>
<body>
    <div id="container">
        <div id="heading">heading</div>
        <div id="menu">菜单</div>
        <div id="body">body
            <img src="img/a.jpg">
        </div>
        <div id="foot">foot</div>
    </div>

</body>
</html>
~~~

<img src="/image-20200114101255648.png" alt="image-20200114101255648" style="zoom:50%;" />

发现和讲的视频不太一样，规定的height貌似没有用，页面高度只会随着内容增加



### 3.2、使用table布局

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>布局2</title>
</head>
<body marginheight="0px" marginwidth="0px">
    <table width="100%" height="950px" style="background-color: antiquewhite">
        <tr>
            <td colspan="2" width="100%" style="background-color: aquamarine">头部</td>
            <!--  这里布局的表格分三行，第一，三行1个格子，第二行2个格子         -->
        </tr>
        <tr>
            <td width="30%" height="80%" style="background-color: brown">左菜单</td>
            <td width="70%" height="80%" style="background-color: yellow">右菜单</td>
        </tr>
        <tr>
            <td colspan="2" width="70%" height="80%" style="background-color: blue">foot</td>
        </tr>
    </table>
</body>
</html>
~~~

这样舒服了，注意核心就是td的那个colspan设置

![image-20200114103821479](/image-20200114103821479.png)



### 3.3  iframe

内联框架iframe，用于在一个页面里嵌套子页面

<img src="/image-20200116095212205.png" alt="image-20200116095212205" style="zoom: 50%;" />

关于A标签，如果target选self：当前页面打开

<img src="/image-20200116095400489.png" alt="image-20200116095400489" style="zoom:50%;" />

如果target=parent，则会在当前页面的父页面上打开链接，即在红色的frameB中打开百度

如果=top，则在白色的当前页面中打开



当前页面的代码如下：当前页面（白色，打开framec.html）

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <iframe src="frameC.html" frameborder="0" width="800" height="800">

    </iframe>
</body>
</html>
~~~

然后frameC的代码：（承载frameB，以此类推）

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body bgcolor="aqua">
frameC
<br>
    <iframe src="frameB.html" frameborder="0" width="600" height="600">

    </iframe>
</body>
</html>
~~~





## 表单

### 基本元素



<img src="/image-20200115224338993.png" alt="image-20200115224338993" style="zoom:67%;" />

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <form>
        用户名：
        <input type="text">
        密码：
        <input type="password">
        </br>
        复选框：你喜欢的水果有：
        </br>
        苹果<input type="checkbox">
        香蕉<input type="checkbox">
        西红柿<input type="checkbox">
        </br>
        单选框：请选择性别：
        男<input type="radio" name="sex">
        女<input type="radio" name="sex">
        <!--   只有男，女都指定同一个name，才能实现他们两个中只能选一个-->
        <br>
        以下是下拉框
        <select>
            <option>1111</option>
            <option>2222</option>
            <option>3333</option>
        </select>
        <br>
        以下是按钮：
        <br>
        <input type="button" value="按钮">
        <input type="submit">
    </form>

    <br>
    以下是文本域：
    <textarea cols="30" rows="30">请提交留言 </textarea>

</body>
</html>
~~~



<img src="/image-20200115230320366.png" alt="image-20200115230320366" style="zoom:80%;" />

### 与php交互

前端代码：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    action 指定将表单提交到哪个服务器上
    <br>
    这里的后台环境是由xampp开启的apache服务，把learning_php.php放在xampp规定目录下就可再localhost访问到
    <form action="http://localhost:8080/learning_php.php" method="get">
        用户名：
        <input type="text" name="name">
        密码：
        <input type="password" name="password">
        <br>
        <input type="submit" value="提交">
        这个submit相当关键，有了它才把内容提交给服务器
    </form>
</body>
</html>
~~~

后台代码：

~~~php
<?php
echo "helloworld";
echo "用户名：".$_GET['name']."<br>密码：".$_GET['password'];
?>

~~~

输入用户名密码之后，点击提交，跳转到php服务器文件：

![image-20200115232732184](/image-20200115232732184.png)

get与post区别：

- 如上图的地址栏处，get会在后台处显示提交的name，password，不如post安全

- 对于不需要保护的，也不是太长的内容来说，get比post更简单方便

- get可以做资源定位，但是post不能：

  eg：get方法.php后面，加了一串具体的信息，把这个地址复制给别人，他们还是能打开，但post方法不行，因为后面没有这些附加的信息，只会显示xxxx.php，后面不加 "？xxx=xxx......"

  <img src="/image-20200115233625038.png" alt="image-20200115233625038" style="zoom: 67%;" />



## 常见属性

class 类名

id 元素的唯一ID

style 规定元素的样式

title  规定元素的额外信息

## 格式化使用

<img src="/image-20200113153120574.png" alt="image-20200113153120574" style="zoom:50%;" />

<img src="/image-20200113153200858.png" alt="image-20200113153200858" style="zoom:50%;" />

## HTML样式（详见印象笔记）

**其中的link标签写在head标签里面**

<img src="/image-20200113153541246.png" alt="image-20200113153541246" style="zoom: 67%;" />