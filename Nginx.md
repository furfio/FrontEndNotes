---
typora-copy-images-to: 笔记图像
typora-root-url: 笔记图像
---

# 1、简介

"Nginx是一款轻量级的HTTP服务器，采用事件驱动的异步非阻塞处理方式框架，这让其具有极好的IO性能，时常用于服务端的反向代理和负载均衡。"

**Nginx的优点**

- 支持海量高并发：采用IO多路复用epoll。官方测试Nginx能够支持5万并发链接，实际生产环境中可以支撑2-4万并发连接数。

- 内存消耗少：在主流的服务器中Nginx目前是内存消耗最小的了，比如我们用Nginx+PHP，在3万并发链接下，开启10个Nginx进程消耗150M内存。

- 免费使用可以商业化：Nginx为开源软件，采用的是2-clause BSD-like协议，可以免费使用，并且可以用于商业。

- 配置文件简单：网络和程序配置通俗易懂，即使非专业运维也能看懂。

  

**Nginx版本说明**

- Mainline version ：开发版,主要是给广大Nginx爱好者，测试、研究和学习的，但是不建议使用于生产环境。
- Stable version : 稳定版,也就是我们说的长期更新版本。这种版本一般比较成熟，经过长时间的更新测试，所以这种版本也是主流版本。
- legacy version : 历史版本，如果你需要以前的版本，Nginx也是有提供的。

# 2、安装

**基于Yum的方式安装Nginx**

我们可以先来查看一下yum是否已经存在，命令如下：

```
yum list | grep nginx
```

如果出现类似下面的内容，说明yum源是存在的。 ![alt](http://www.jspang.com/static/upload/20181007/ljnFrPYc23562AtUSLvSIYpQ.png)

(细心的小伙伴可以发现系统原来的源只支持1.1.12版本，这版本有些低)

如果不存在，或者不是你需要的版本，那我们可以自行配置yum源，下面是官网提供的源，我们可以放心大胆的使用。

```shell
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/OS/OSRELEASE/$basearch/
gpgcheck=0
enabled=1
```

复制上面的代码，然后在终端里输入：

```shell
vim /etc/yum.repos.d/nginx.repo
```

然后把代码复制进去，这里你可能需要一些Vim的操作知识，如果不熟悉，可以自行学习一下，当然我视频中也是有讲解的。

赋值完成后，你需要修改一下对应的操作系统和版本号，因为我的是centos和7的版本，所以改为这样。

```
baseurl=http://nginx.org/packages/centos/7/$basearch/
```

你可以根据你的系统或需要的版本进行修改。

如果都已经准备好了，那就可以开始安装了，安装的命令非常简单：

```shell
yum install nginx
```

安装完成后可以使用命令，来检测Nginx的版本。

```shell
nginx -v
```

# 3、Nginx基本配置文件讲解

**查看Nginx的安装目录**

在使用`yum`安装完Nginx后，需要知道系统中多了那些文件，它们都安装到了那里。可以使用下面的命令进行查看：

```
rpm -ql nginx
```

rpm 是linux的rpm包管理工具，-q 代表询问模式，-l 代表返回列表，这样我们就可以找到nginx的所有安装位置了。

列表列出的内容还是比较多的，我们尽量给大家进行讲解，我们这节先来看看重要的文件。

**nginx.conf文件解读**

nginx.conf 文件是Nginx总配置文件，在我们搭建服务器时经常调整的文件。

进入etc/nginx目录下，然后用vim进行打开

```
cd /etc/nginx
vim nginx.conf
```

下面是文件的详细注释，我几乎把每一句都进行了注释，你可以根据你的需要来进行配置。

```
#运行用户，默认即是nginx，可以不进行设置
user  nginx;
#Nginx进程，一般设置为和CPU核数一样
worker_processes  1;   
#错误日志存放目录
error_log  /var/log/nginx/error.log warn;
#进程pid存放位置
pid        /var/run/nginx.pid;


events {
    worker_connections  1024; # 单个后台进程的最大并发数
}


http {
    include       /etc/nginx/mime.types;   #文件扩展名与类型映射表
    default_type  application/octet-stream;  #默认文件类型
    #设置日志格式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;   #nginx访问日志存放位置

    sendfile        on;   #开启高效传输模式
    #tcp_nopush     on;    #减少网络报文段的数量，默认关闭

    keepalive_timeout  65;  #保持连接的时间，也叫超时时间

    #gzip  on;  #开启gzip压缩，可以减少前端图片等文件的加载时间

    include /etc/nginx/conf.d/*.conf; #包含的子配置项位置和文件
```

**default.conf 配置项讲解** 我们看到最后有一个子文件的配置项，那我们打开这个include子文件配置项看一下里边都有些什么内容。

进入conf.d目录，然后使用`vim default.conf`进行查看。

```
server {
    listen       80;   #配置监听端口
    server_name  localhost;  //配置域名

    #charset koi8-r;     
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;     #服务默认启动目录
        index  index.html index.htm;    #默认访问文件
    }

    #error_page  404              /404.html;   # 配置404页面

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;   #错误状态码的显示页面，配置后需要重启
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

明白了这些配置项，我们知道我们的服务目录放在了`/usr/share/nginx/html`下，可以使用命令进入看一下目录下的文件。

```
cd /usr/share/nginx/html
ls 
```

可以看到目录下面有两个文件，50x.html 和 index.html。我们可以使用vim进行编辑。

学到这里，其实可以预想到，我们的nginx服务器已经可以为html提供服务器了。

~~~
nginx   #开启nginx服务
~~~

我们可以打开浏览器，输入阿里云服务器的ip，但是由于阿里云服务器默认关闭80端口，需要按如下方法打开

**阿里云的安全组配置**

如果你使用的是阿里云，记得到ECS实例一下打开端口。

步骤如下：

1. 进入阿里云控制台，并找到ECS实例。
2. 点击实例后边的“更多”
3. 点击“网络和安全组” ，再点击“安全组配置”
4. 右上角添加“安全组配置”
5. 配置规则
6. 进行80端口的设置，具体设置如图就好。（http（80），地址段访问）
7. 授权对象0.0.0.0/0 表示允许所有人访问）

<img src="/image-20200224164531479.png" alt="image-20200224164531479" style="zoom:67%;" />

# 4、Nginx服务的启动，停止，重启

**启动Nginx服务**

默认的情况下，Nginx是不会自动启动的，需要我们手动进行启动，当然启动Nginx的方法也不是单一的。

**nginx直接启动**

在CentOS7.4版本里（低版本是不行的），是可以直接直接使用`nginx`启动服务的。

```
nginx
```

**使用systemctl命令启动**

还可以使用个Linux的命令进行启动，我一般都是采用这种方法进行使用。因为这种方法无论启动什么服务，都是一样的，只是换一下服务的名字（不用增加额外的记忆点）。

```
systemctl start nginx.service
```

输入命令后，没有任何提示，那我们如何知道Nginx服务已经启动了哪？可以使用Linux的组合命令，进行查询服务的运行状况。

```
ps aux | grep nginx
```



**停止Nginx服务的四种方法**

停止Nginx 方法有很多种，可以根据需求采用不一样的方法，我们一个一个说明。

- 立即停止服务

```
nginx  -s stop
```

这种方法比较强硬，无论进程是否在工作，都直接停止进程。

- 从容停止服务

```
nginx -s quit
```

这种方法较stop相比就比较温和一些了，需要进程完成当前工作后再停止。

- killall 方法杀死进程

这种方法也是比较野蛮的，我们直接杀死进程，但是在上面使用没有效果时，我们用这种方法还是比较好的。

```
killall nginx
```

- systemctl 停止

```
systemctl stop nginx.service
```

**重启Nginx服务**

有时候我们需要重启Nginx服务，这时候可以使用下面的命令。

```
systemctl restart nginx.service
```

**重新载入配置文件**

在重新编写或者修改Nginx的配置文件后，都需要作一下重新载入而不用重启，这时候可以用Nginx给的命令。

```
nginx -s reload
```

**查看端口号**

在默认情况下，Nginx启动后会监听80端口，从而提供HTTP访问，如果80端口已经被占用则会启动失败。我么可以使用

~~~
netstat -tlnp
~~~

命令查看端口号的占用情况。

# 5、自定义错误页面和访问设置

一个好的网站会武装到牙齿，任何错误都有给用户友好的提示。比如当网站遇到页面没有找到的时候，我们要提示页面没有找到，并给用户可返回性。错误的种类有很多，所以真正的好产品会给顾客不同的返回结果。

**多错误指向一个页面**

在/etc/nginx/conf.d/default.conf 是可以看到下面这句话的。

```
error_page   500 502 503 504  /50x.html;
```

error_page指令用于自定义错误页面，500，502，503，504 这些就是HTTP中最常见的错误代码，/50.html 用于表示当发生上述指定的任意一个错误的时候，都是用网站根目录下的/50.html文件进行处理。



**单独为错误置顶处理方式**

有些时候是要把这些错误页面单独的表现出来，给用户更好的体验。所以就要为每个错误码设置不同的页面。设置方法如下：

```
error_page 404  /404_error.html;
```

然后到网站目录下新建一个404_error.html 文件，并写入一些信息。

```
<html>
<meta charset="UTF-8">
<body>
<h1>404页面没有找到!</h1>
</body>
</html>
```

然后重启我们的服务，再进行访问，你会发现404页面发生了变化。



**把错误码换成一个地址**

处理错误的时候，不仅可以只使用本服务器的资源，还可以使用外部的资源。比如我们将配置文件设置成这样。

```
error_page  404 http://jspang.com;
```

我们使用了技术胖的博客地址作为404页面没有找到的提示，就形成了，没有找到文件，就直接跳到了技术胖的博客上了。



**简单实现访问控制**

有时候我们的服务器只允许特定主机访问，比如内部OA系统，或者应用的管理后台系统，更或者是某些应用接口，这时候我们就需要控制一些IP访问，我们可以直接在`location`里进行配置。

可以直接在default.conf里进行配置。

```
 location / {
        deny   123.9.51.42;  #deny all 表示除了allow的别人都不允许访问
        allow  45.76.202.231;
    }
```

配置完成后，重启一下服务器就可以实现限制和允许访问了。这在工作中非常常用，一定要好好记得。

# 6、访问权限详解

**指令优先级**

我们先来看一下代码：

```
 location / {
        allow  45.76.202.231;
        deny   all;
    }
```

上面的配置表示只允许`45.76.202.231`进行访问，其他的IP是禁止访问的。但是如果我们把`deny all`指令，移动到 `allow 45.76.202.231`之前，会发生什么那？会发现所有的IP都不允许访问了。**这说明了一个问题：就是在同一个块下的两个权限指令，先出现的设置会覆盖后出现的设置（也就是谁先触发，谁起作用）**。



**复杂访问控制权限匹配**

在工作中，访问权限的控制需求更加复杂，例如，对于网站下的img（图片目录）是运行所有用户访问，但对于网站下的admin目录则只允许公司内部固定IP访问。这时候仅靠deny和allow这两个指令，是无法实现的。我们需要location块来完成相关的需求匹配。

上面的需求，配置代码如下：

```
    location =/img{
        allow all;
    }
    location =/admin{
        deny all;
    }
```

`=`号代表精确匹配，使用了`=`后是根据其后的模式进行精确匹配。这个直接关系到我们网站的安全，一定要学会。

**使用正则表达式设置访问权限**

只有精确匹配有时是完不成我们的工作任务的，比如现在我们要禁止访问所有php的页面，php的页面大多是后台的管理或者接口代码，所以为了安全我们经常要禁止所有用户访问，而只开放公司内部访问的。

代码如下：

```
 location ~\.php$ {
        deny all;
    }
```

这样我们再访问的时候就不能访问以php结尾的文件了。是不是让网站变的安全很多了那？

# 7、Nginx设置虚拟主机

虚拟主机是指在一台物理主机服务器上划分出多个磁盘空间，每个磁盘空间都是一个虚拟主机，每台虚拟主机都可以对外提供Web服务，并且互不干扰。在外界看来，虚拟主机就是一台独立的服务器主机，这意味着用户能够利用虚拟主机把多个不同域名的网站部署在同一台服务器上，而不必再为建立一个网站单独购买一台服务器，既解决了维护服务器技术的难题，同时又极大地节省了服务器硬件成本和相关的维护费用。

配置虚拟主机可以基于端口号、基于IP和基于域名，这节课我们先学习基于端口号来设置虚拟主机。

**基于端口号配置虚拟主机**

基于端口号来配置虚拟主机，算是Nginx中最简单的一种方式了。原理就是Nginx监听多个端口，根据不同的端口号，来区分不同的网站。

我们可以直接配置在主文件里`etc/nginx/nginx.conf`文件里， 也可以配置在子配置文件里`etc/nginx/conf.d/default.conf`。我这里为了配置方便，就配置在子文件里了。当然你也可以再新建一个文件，只要在conf.d文件夹下就可以了。

~~~
vim 8001.conf  #z在conf.d文件夹中
~~~



修改配置文件中的server选项，这时候就会有两个server。

```
server{
        listen 8001;
        server_name localhost;
        root /usr/share/nginx/html/html8001;
        index index.html;
}
```

编在`usr/share/nginx/html/html8001/`目录下的index.html文件并查看结果。

```
<h1>welcome port 8001</h1>
```

最后在浏览器中分别访问地址和带端口的地址。看到的结果是不同的。

然后我们就可以在浏览器中访问`http://112.74.164.244:8001`了，当然你的IP跟这个肯定不一样，这个IP过几天就会过期的。

**基于IP的虚拟主机**

基于IP和基于端口的配置几乎一样，只是把`server_name`选项，配置成IP就可以了。

比如上面的配置，我们可以修改为：

```
server{
        listen 80;
        server_name 112.74.164.244;
        root /usr/share/nginx/html/html8001;
        index index.html;
}
```

这种演示需要多个IP的支持，由于我们的阿里ECS只提供了一个IP，所以这里就不给大家演示了，如果工作中用到，只要安装这种方法配置就可以了。



# 8、使用域名设置虚拟主机

在真实的上线环境中，一个网站是需要域名和公网IP才可以访问的。这也是比较真实的一节课，我们在实际工作中配置最多的就是设置这种虚拟主机。

先要对域名进行解析，这样域名才能正确定位到你需要的IP上。 我这里在阿里云的控制台新建了两个解析，分别是:

- nginx.jspang.com :这个域名映射到默认的Nginx首页位置。
- nginx2.jspang.com : 这个域名映射到原来的8001端口的位置。

**配置以域名为划分的虚拟主机**

我们修改`etc/nginx/conf.d`目录下的default.conf 文件，把原来的80端口虚拟主机改为以域名划分的虚拟主机。代码如下：

```
server {
    listen       80;
    server_name  nginx.jspang.com;
```

我们再把同目录下的`8001.conf`文件进行修改，改成如下：

```
server{
        listen 80;  #由于域名不一样了，可以用这个80端口了，不会与上一个冲突
        server_name nginx2.jspang.com;
        location / {
                root /usr/share/nginx/html/html8001;
                index index.html index.htm;
        }
}
```

然后我们用平滑重启的方式，进行重启，这时候我们在浏览器中访问这两个网页。

其实域名设置虚拟主机也非常简单，主要操作的是配置文件的server_name项，还需要域名解析的配合。



# 9、反向代理的设置

虚拟主机学习完成了，作为一个前端必会的一个技能是反向代理。大家都知道，我们现在的web模式基本的都是标准的CS结构，即Client端到Server端。那代理就是在Client端和Server端之间增加一个提供特定功能的服务器，这个服务器就是我们说的代理服务器。



**正向代理：**如果你觉的反向代理不好理解，那先来了解一下正向代理。我相信作为一个手速远超正常人的程序员来说，你一定用过翻墙工具，它就是一个典型的正向代理工具。它会把我们不让访问的服务器的网页请求，代理到一个可以访问该网站的代理服务器上来，一般叫做proxy服务器，再转发给客户。

简单来说就是你想访问目标服务器的权限，但是没有权限。这时候代理服务器有权限访问服务器，并且你有访问代理服务器的权限，这时候你就可以通过访问代理服务器，代理服务器访问真实服务器，把内容给你呈现出来。



**反向代理：**反向代理跟代理正好相反（需要说明的是，现在基本所有的大型网站的页面都是用了反向代理），客户端发送的请求，想要访问server服务器上的内容。发送的内容被发送到代理服务器上，这个代理服务器再把请求发送到自己设置好的内部服务器上，而用户真实想获得的内容就在这些设置好的服务器上。

通过对比，应该看出一些区别，这里proxy服务器代理的并不是客户端，而是服务器,即向外部客户端提供了一个统一的代理入口，客户端的请求都要先经过这个proxy服务器。具体访问那个服务器server是由Nginx来控制的。再简单点来讲，一般代理指代理的客户端，反向代理是代理的服务器。



**反向代理的用途和好处**

- 安全性：正向代理的客户端能够在隐藏自身信息的同时访问任意网站，这个给网络安全代理了极大的威胁。因此，我们必须把服务器保护起来，使用反向代理客户端用户只能通过外来网来访问代理服务器，并且用户并不知道自己访问的真实服务器是那一台，可以很好的提供安全保护。
- 功能性：反向代理的主要用途是为多个服务器提供负债均衡、缓存等功能。负载均衡就是一个网站的内容被部署在若干服务器上，可以把这些机子看成一个集群，那Nginx可以将接收到的客户端请求“均匀地”分配到这个集群中所有的服务器上，从而实现服务器压力的平均分配，也叫负载均衡。



最简单的反向代理**

现在我们要访问`http://nginx2.jspang.com`然后反向代理到`jspang.com`这个网站。我们直接到`etc/nginx/con.d/8001.conf`进行修改。

修改后的配置文件如下：

```
server{
        listen 80;
        server_name nginx2.jspang.com;
        location / {
               proxy_pass http://jspang.com;
        }
}
```

一般我们反向代理的都是一个IP，但是我这里代理了一个域名也是可以的。其实这时候我们反向代理就算成功了，我们可以在浏览器中打开`http://nginx2.jspang.com`来测试一下，发现会自动反向代理到http://jspang.com即浏览器地址显示的是nginx2.jspang.com，但里面的内容却是jspang.com里的东西

**其它反向代理指令**

反向代理还有些常用的指令，我在这里给大家列出：

- proxy_set_header :在将客户端请求发送给后端服务器之前，更改来自客户端的请求头信息。
- proxy_connect_timeout:配置Nginx与后端代理服务器尝试建立连接的超时时间。
- proxy_read_timeout : 配置Nginx向后端服务器组发出read请求后，等待相应的超时时间。
- proxy_send_timeout：配置Nginx向后端服务器组发出write请求后，等待相应的超时时间。
- proxy_redirect :用于修改后端服务器返回的响应头中的Location和Refresh。

# 10、 Nginx适配pc或者移动端设备

现在很多网站都是有了PC端和H5站点的，因为这样就可以根据客户设备的不同，显示出体验更好的，不同的页面了。这样的需求有人说拿css自适应就可以搞定，比如我们常说的bootstrap和24格布局法，这些确实是非常好的方案，但是无论是复杂性和易用性上面还是不如分开编写的好，比如我们常见的淘宝、京东......这些大型网站就都没有采用自适应，而是用分开制作的方式。

那分开制作如何通过配置Nginx来识别出应该展示哪个页面那？我们这节课就来学习一下。

**$http_user_agent的使用：**

Nginx通过内置变量`$http_user_agent`，可以获取到请求客户端的userAgent，就可以用户目前处于移动端还是PC端，进而展示不同的页面给用户。

操作步骤如下：

1. 在/usr/share/nginx/目录下新建两个文件夹，分别为：pc和mobile目录

   ```
   cd /usr/share/nginx
   mkdir pc
   mkdir mobile
   ```

2. 在pc和miblic目录下，新建两个index.html文件，文件里下面内容

   ```
   <h1>I am pc!</h1>
   ```

   ```
   <h1>I am mobile!</h1>
   ```

3. 进入`etc/nginx/conf.d`目录下，修改8001.conf文件，改为下面的形式:

   ```
   server{
        listen 80;
        server_name nginx2.jspang.com;
        location / {
         root /usr/share/nginx/pc;
         if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
            root /usr/share/nginx/mobile;
         }  # *指不区分大小写
         index index.html;
        }
   }
   ```

# 11、Nginx的Gzip压缩配置

Gzip是网页的一种网页压缩技术，经过gzip压缩后，页面大小可以变为原来的30%甚至更小。更小的网页会让用户浏览的体验更好，速度更快。gzip网页压缩的实现需要浏览器和服务器的支持。

当浏览器支持gzip压缩时，会在请求消息中包含Accept-Encoding:gzip,这样Nginx就会向浏览器发送听过gzip后的内容，同时在相应信息头中加入Content-Encoding:gzip，声明这是gzip后的内容，告知浏览器要先解压后才能解析输出。

**gzip的配置项**

Nginx提供了专门的gzip模块，并且模块中的指令非常丰富。

- gzip : 该指令用于开启或 关闭gzip模块。
- gzip_buffers : 设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。
- gzip_comp_level : gzip压缩比，压缩级别是1-9，1的压缩级别最低，9的压缩级别最高。压缩级别越高压缩率越大，压缩时间越长。
- gzip_disable : 可以通过该指令对一些特定的User-Agent不使用压缩功能。
- gzip_min_length:设置允许压缩的页面最小字节数，页面字节数从相应消息头的Content-length中进行获取。
- gzip_http_version：识别HTTP协议版本，其值可以是1.1.或1.0.
- gzip_proxied : 用于设置启用或禁用从代理服务器上收到相应内容gzip压缩。
- gzip_vary : 用于在响应消息头中添加Vary：Accept-Encoding,使代理服务器根据请求头中的Accept-Encoding识别是否启用gzip压缩。

**gzip最简单的配置**

~~~
cd /etc/nginx
vim nginx.conf
~~~



```
http {
   .....
    gzip on;
    gzip_types text/plain application/javascript text/css;
   .....
}
```

`gzip on`是启用gizp模块，下面的一行是用于在客户端访问网页时，对文本、JavaScript 和CSS文件进行压缩输出。

配置好后，我们就可以重启Nginx服务，让我们的gizp生效了。

如果你是windows操作系统，你可以按F12键打开开发者工具，单机当前的请求，在标签中选择Headers，查看HTTP响应头信息。你可以清楚的看见`Content-Encoding`为gzip类型。

# 12、Nginx配置图片服务器

在etc/nginx/conf.d/里新建一个pic.conf

~~~shell
server{
        listen 8888;
        server_name localhost;
        location ~ .*\.(gif|jpg|jpeg|png)$ { 
        
      expires 24h;  
      root /www/wwwroot/myblog/img;#指定图片存放路径  
      access_log /www/wwwroot/myblog/logs/images.log;#日志存放路径  
      proxy_store on;  
      proxy_store_access user:rw group:rw all:rw;  
      proxy_temp_path     /www/wwwroot/myblog/img;#图片访问路径  
      proxy_redirect     off;  
      proxy_set_header    Host 127.0.0.1:8888;  
      client_max_body_size  10m;  
      client_body_buffer_size 1280k;  
      proxy_connect_timeout  900;  
      proxy_send_timeout   900;  
      proxy_read_timeout   900;  
      proxy_buffer_size    40k;  
      proxy_buffers      40 320k;  
      proxy_busy_buffers_size 640k;  
      proxy_temp_file_write_size 640k;  
      if ( !-e $request_filename)  
      {  
         proxy_pass http://127.0.0.1:8888;#默认80端口  
      }  
  }  

        
}
~~~

就可以在访问到    服务器地址:8888/xxx.jpg 了





