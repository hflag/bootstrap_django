# 第一章 项目准备工作
                   

本项目是一个bootstrap的教学案例，前台使用bootstrap，后台采用了django框架。

## 一、 下载bootstrap框架

尝试了各种方法下载和配置bootstrap所需要的css和js，最后发现使用node.js来搞定这些是最方便的了。
那么我们就需要先来安装node.js.

登录node.js官网，下载合适的版本进行安装，非常简单！关键是node.js有一个工具非常有用，那就是npm！
用npm可以下载bootstrap所需要的css和js。

### 1. 在d：盘建立一个新的目录，命名为‘bd’，进入‘d:/bd',执行如下命令：

    1) npm install bootstrap 下载bootstrap的css和js
    2）npm install jquery  下载jquery
    3）npm install font-awesome 下载字体
    4）npm install django-social 下载图标
    5）npm install popper.js --save 下载popper.js

## 二、建立django项目和应用

### 1. 在d：盘下新建一个目录www，在该目录下建立虚拟开发环境
    python -m venv webvenv

### 2. 激活该虚拟环境
    d:\www\webvenv\scripts\activate

### 3. 安装django
    pip install django

### 4. 使用pycharm建立项目
注意一定要使用刚刚建立的虚拟环境建立项目！

### 5. 进入pycharm，打开Terminal创建应用myapp，不要忘记在项目settings.py 中添加该应用。

### 6. 在应用myapp下建立static文件夹，将下载好的bootstrap相关文件都拷贝到这里。

### 7. 在应用myapp建立templates文件夹，然后再建立myapp子文件夹。接下来最为重要的一步，
就是在templates构建整个项目的基础模板base-4.1.1.html

### 8. 其实，所谓的bootstrap框架，无非就是在网页里引入那么几个css和js，没有想象的那么高大上。
base-4.1.1.html代码如下：
```html
<!DOCTYPE html>
{% load static %}
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="{% static 'bootstrap/dist/css/bootstrap.min.css' %}">
    <link rel="stylesheet" href="{% static 'font-awesome/css/font-awesome.min.css' %}">
    <link rel="stylesheet" href="{% static 'bootstrap-social-gh-pages/bootstrap-social.css' %}">
    <script src="{% static 'jquery/dist/jquery.min.js' %}"></script>
    {% block custom_css %}{% endblock %}
    <title>{% block title %}Hello, world!{% endblock %}</title>
  </head>
  <body>
    {% block content %}
    {% endblock %}
    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="{% static 'jquery/dist/jquery.slim.min.js' %}"></script>
    <script src="{% static 'popper.js/dist/popper.min.js' %}"></script>
    <script src="{% static 'bootstrap/dist/js/bootstrap.min.js' %}"></script>
  </body>
</html>
```
这个文件不仅仅集成了bootstrap框架，同时还使用了django的模板语言。是整个项目的基础。

### 9.在myapp中放入index.html，目前该网页没有使用任何bootstrap样式。

### 10. 编写myapp应用视图，修改views.py，添加如下代码：
```python
from django.shortcuts import render
from django.views.generic.base import View


class IndexView(View):
    def get(self, request):
        return render(request, 'myapp/index.html')
```
### 11. 添加应用myapp的路由文件urls.py, 写入下面的代码
```python
from django.urls import path
from . import views


urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
]
```
### 12. 把myapp的路由文件添加到项目总路由文件中，修改项目下的urls.py如下：
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),
]
```
启动服务器，目前看到的是没有使用bootstrap的页面，简单、丑陋

# 第二章 响应式布局和bootstrap网格系统

Bootstrap的设计是移动优先的，并且能够根据不同设备进行适当的自适应，以便达到跨设备的目标。之所以bootstrap
能够做到这一点，关键的就是媒体查询，我们在<head></head>部分已经添加如下的媒体查询的代码：
```html
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
```
接下来，我们就可以使用bootstrap强大的网格系统（grid system）了。

## 一、使用container类
对一个块级标签使用container的作用是保持内容显示在屏幕上一个固定尺寸的宽度。另一个选择是使用container-fluid，
这个类能使内容自动散开填满整个屏幕的宽度。
### 1. 首先对</header>下的div添加这个类，代码如下：
    <div class='container'>...

## 二、把内容划分成rows
对从container内的下一级div添加row类，这样我们可以把内容按照行来组织。
    <div class='row'>...
这里中间部分，也就是标记为container的div下有4个子div，都要添加这个类。

## 三、对header标签添加jumboron类。这使得header元素转换成了bootstrap的jumbotron组件。
同样的紧邻的下一级div添加container类。

    <header class='jumbotron'>...
 
## 四、对footer下的一级div也添加类container

经过上面的处理，目前所有网页内容显示在一个container所确定的固定宽度内。

## 五、对每个row使用列类

1. 对于header，添加如下代码
```html
    <div class='col-12 col-sm-6'>...</div>
    <div class='col-12 col-sm’>...</div>
```
2. 中间部分的三行，添加如下的代码：
```html
    <div class="col-12 col-sm-4 col-md-3"> ... </div>

    <div class="col col-sm col-md"> ... </div>
```
3. footer中列的定义如下：
```html
    <div class="col-4 col-sm-2"> ... </div>

    <div class="col-7 col-sm-5"> ... </div>

    <div class="col-12 col-sm-4"> ... </div>

    <div class="col-auto"> ... </div>
```
## 六、使用order和offset
1. 为了使显示格式的多样化，我们队中间那个container中三个div中的第一个和第三个添加下面的代码
```html
    <div class="col-12 col-sm-4 order-sm-last col-md-3"> ... </div>

    <div class="col col-sm order-sm-first col-md"> ... </div>
```
2. 对包含<ul>标签的div，更新如下：
```html
    <div class="col-4 offset-1 col-sm-2">
```
Ok，基本的bootstrap网格系统搞定，赶快刷新网页看看效果吧！

# 第三章 添加自定义css

尽管bootstrap很强大了，但是对于我们自己的需求，也许有时还是常常不能满足，需要自定义一些样式，那么如何自定义呢？

## 一、新建自定义css文件
1. 在应用myapp下，新建css文件夹，将自己的css放置在这里，代码内容如下：
```css
.row-header{
    margin: 0px auto;
    padding: 0;
}

.row-content{
    margin: 0px auto;
    padding: 0px 0px 20px 0px;
}

.footer{
    background-color: #e9ecef;
    margin: 0px auto;
    padding: 29px 0px 20px 0px;
}
address{
    font-size:80%;
    margin:0px;
    color:#0f0f0f;
}
```
2. 在index.html中引入和使用style.css
```html
{% extends "base-4.1.1.html" %}
{% load static %}

{% block custom_css %}
    <link rel="stylesheet" href="{% static 'css/style.css' %}">
{% endblock %}
```
同时添加应用如下：
```html
    <div class="row row-header"> ... </div>

    <div class="row row-content"> ... </div>

    <div class="row row-content"> ... </div>

    <div class="row row-content"> ... </div>

    <footer class="footer"> ... </footer>
```
## 二、垂直居中显示内容

1. 在内容区，更新所有row的类
    <div class='row row-content align-items-center'>

2. 在footer区，更新第三个列div，就是包括社会媒体链接的那个
    <div class='col-12 col-sm-4 align-self-center'>

3. 更新包含社会媒体链接的那个div的类
    <div class='text-center'>

## 三、水平居中显示内容
更新footer中包含内容段落的div：
```html
    <div class="row justify-content-center">
                <div class="col-auto">
```

## 四、设置ul样式
    <ul class="list-unstyled"> ... </ul>

至此，一个页面基本完成。同时上去第二个页面aboutus.html.

                第四章 aboutus.html

我们这里的about.html算是对前面网格系统的一个复习。通过对about.html使用网格系统，
使其成为响应式页面。
为了使页面aboutus.html能够显示，首先要修改视图，在myapp应用中的views.py，添加代码：
```python
class AboutView(View):
    def get(self, request):
        return render(request, 'myapp/aboutus.html')
```
另外，为这个页面建立路由，打开myapp下的urls.py, 添加一条路由：
```python
    urlpatterns = [
        path('', views.IndexView.as_view(), name='index'),
        path('about/', views.AboutView.as_view(), name='about'),
    ]
```
一、header和footer部分直接使用index.html中相应的部分，它们就不用在修改

二、中间内容区，最外层div添加container类，注意这个类的作用使得其中内容以一个固定宽度
占据页面。

三、container内有三个div，都添加row类

1. 在第一个行内 <div class="col-12">
2. 第二行内的两个div分别添加
    <div class="col-12 col-sm-6">
    <div class="col-12 col-sm">
3. 第三个div内添加 <div class="col-12">。同时为了使其内容在小屏幕下只显示标题，而大屏幕下
标题和段落都显示，需要对其中的每个<p>添加类：<p class="d-none d-sm-block">

这样我们很快就完成了aboutus.html的网格系统设置，刷新浏览器看看效果吧。


                第五章 添加响应式导航条

所谓响应式导航条，就是在大屏幕时能够完整呈现所有导航菜单，在小屏幕时能够成为一个折叠菜单。

一、构建导航菜单
1. 基本导航菜单的添加，在页面中的header之前，添加<nav>...</nav>构成一个导航条组件，代码如下
```html
    <nav class="navbar navbar-expand-sm navbar-dark bg-primary fixed-top">
        <div class="container">
            <a class="navbar-brand" href="#">Ristorante Con Fusion</a>
            <ul class="navbar-nav mr-auto">
                <li class="nav-item active">
                    <a class="nav-link" href="#">
                        Home
                    </a>
                </li>
                <li class="nav-item ">
                    <a class="nav-link" href="#">
                        About
                    </a>
                </li>
                <li class="nav-item ">
                    <a class="nav-link" href="#">
                        Menu
                    </a>
                </li>
                <li class="nav-item ">
                    <a class="nav-link" href="#">
                        Contact
                    </a>
                </li>
            </ul>
        </div>
    </nav>
```
注意，到了这里就是一个普通的导航条，是不具备响应式特性的！

2.增加响应式代码

在上面的container下添加button，代码如下：
```html
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#Navbar">
         <span class="navbar-toggler-icon"></span>
    </button>
```
在ul外添加一个div，设置如下：
```html
            <div class="collapse navbar-collapse" id="Navbar">
                <ul class="navbar-nav mr-auto">
                ...
                </ul>
            </div>
```
ok, 刷新页面，看看在小屏幕和大屏幕下是否有了区别，导航条部分。

二、修改body的css
因为添加导航条后，需要修改body的css，为显示导航条预留空间，在style.css中添加如下内容：
```css
body{
    padding:50px 0px 0px 0px;
    z-index: 0;
}
```
三、在导航中添加具体地址，在index.html页面内，修改about页面的href
```html
<li class="nav-item active">
    <a class="nav-link" href="{% url 'index' %}">
        Home
    </a>
</li>
 <li class="nav-item ">
    <a class="nav-link" href="{% url 'about' %}">
        About
    </a>
 </li>
```
这样点击相应菜单时，就可以正确跳转了。

四、拷贝index.html中的nav部分，添加到aboutus.html页面，是aboutus.html页面也有导航条。
接下来修改当前导航菜单，把包含Home的li标签的active删去，添加到包含About的li标签中。

                第六章 为页面添加Icon Fonts

页面中添加精美的icon fonts后能添加好多灵气，如果要在页面中使用icon fonts。必须有font-awesome
和 bootstrap-social支持。我们的base-4.1.1模板中已经集成了支持。

一、在页面中使用font icons，起到装饰的作用，更新navbar中的ul
```html
                <ul class="navbar-nav mr-auto">
                    <li class="nav-item active">
                        <a class="nav-link" href="{% url 'index' %}">
                            <span class="fa fa-home fa-lg"></span>
                            Home
                        </a>
                    </li>
                    <li class="nav-item ">
                        <a class="nav-link" href="{% url 'about' %}">
                            <span class="fa fa-info fa-lg"></span>
                            About
                        </a>
                    </li>
                    <li class="nav-item ">
                        <a class="nav-link" href="#">
                            <span class="fa fa-list fa-lg"></span>
                            Menu
                        </a>
                    </li>
                    <li class="nav-item ">
                        <a class="nav-link" href="#">
                            <span class="fa fa-address-card fa-lg"></span>
                            Contact
                        </a>
                    </li>
                </ul>
```
相应的也要更新aboutus.html中的navbar

二、修改index和aboutus页面中footer内的address，使用下面的代码取代'Tel', 'Fax'和'Email'
```html
<i class="fa fa-phone fa-lg"></i>: +852 1234 5678<br>
<i class="fa fa-fax fa-lg"></i>: +852 8765 4321<br>
<i class="fa fa-envelope fa-lg"></i>:
 <a href="mailto:confusion@food.net">confusion@food.net</a>
```
三、使用bootstrap-social CSS类来创建social按钮
在index和aboutus中的footer部分，修改social链接如下：
```html
<div class="text-center">
  <a class="btn btn-social-icon btn-google" href="http://google.com/+"><i class="fa fa-google-plus"></i></a>
  <a class="btn btn-social-icon btn-facebook" href="http://www.facebook.com/profile.php?id="><i class="fa fa-facebook"></i></a>
  <a class="btn btn-social-icon btn-linkedin" href="http://www.linkedin.com/in/"><i class="fa fa-linkedin"></i></a>
  <a class="btn btn-social-icon btn-twitter" href="http://twitter.com/"><i class="fa fa-twitter"></i></a>
  <a class="btn btn-social-icon btn-google" href="http://youtube.com/"><i class="fa fa-youtube"></i></a>
  <a class="btn btn-social-icon" href="mailto:"><i class="fa fa-envelope-o"></i></a>
</div>
```
至此完成icon fonts使用，赶快刷新页面看看吧


                第七章 添加contactus.html页面

在应用myapp中添加contactus.html页面，并对其进行视图和路由设置

一、添加contactus.html
1. 在应用myapp中修改views.py
``python
class ContactView(View):
    def get(self, request):
        return render(request, 'myapp/contactus.html')
```
2. 添加路由，打开应用下的urls.py添加下面的代码
```python
urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
    path('about/', views.AboutView.as_view(), name='about'),
    path('contact/', views.ContactView.as_view(), name='contact'),
]
```
二、在页面里使用buttons
1. 添加button bar。在页面contactus.html中，找到“button group goes here",
使用下面的代码取代：
```html
<div class="btn-group" role="group">
    <a role="button" class="btn btn-primary" href="tel:+85212345678"><i class="fa fa-phone"></i> Call</a>
    <a role="button" class="btn btn-info"><i class="fa fa-skype"></i> Skype</a>
    <a role="button" class="btn btn-success" href="mailto:confusion@food.net"><i class="fa fa-envelope-o"></i> Email</a>
</div>
```
效果图：
![screen-shot](https://github.com/hflag/bootstrap_django/blob/master/screen_shots/screenshot-buttons.png)
