                    第一章 项目准备工作

本项目是一个bootstrap的教学案例，前台使用bootstrap，后台采用了django框架。

一、 下载bootstrap框架

尝试了各种方法下载和配置bootstrap所需要的css和js，最后发现使用node.js来搞定这些是最方便的了。
那么我们就需要先来安装node.js.

登录node.js官网，下载合适的版本进行安装，非常简单！关键是node.js有一个工具非常有用，那就是npm！
用npm可以下载bootstrap所需要的css和js。

1. 在d：盘建立一个新的目录，命名为‘bd’，进入‘d:/bd',执行如下命令：

    1) npm install bootstrap 下载bootstrap的css和js
    2）npm install jquery  下载jquery
    3）npm install font-awesome 下载字体
    4）npm install django-social 下载图标
    5）npm install popper.js --save 下载popper.js

二、建立django项目和应用

1. 在d：盘下新建一个目录www，在该目录下建立虚拟开发环境
    python -m venv webvenv

2. 激活该虚拟环境
    d:\www\webvenv\scripts\activate

3. 安装django
    pip install django

4. 使用pycharm建立项目
注意一定要使用刚刚建立的虚拟环境建立项目！

5. 进入pycharm，打开Terminal创建应用myapp，不要忘记在项目settings.py 中添加该应用。

6. 在应用myapp下建立static文件夹，将下载好的bootstrap相关文件都拷贝到这里。

7. 在应用myapp建立templates文件夹，然后再建立myapp子文件夹。接下来最为重要的一步，
就是在templates构建整个项目的基础模板base-4.1.1.html

8. 其实，所谓的bootstrap框架，无非就是在网页里引入那么几个css和js，没有想象的那么高大上。
base-4.1.1.html代码如下：

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

这个文件不仅仅集成了bootstrap框架，同时还使用了django的模板语言。是整个项目的基础。

9.在myapp中放入index.html，目前该网页没有使用任何bootstrap样式。

10. 编写myapp应用视图，修改views.py，添加如下代码：

from django.shortcuts import render
from django.views.generic.base import View


class IndexView(View):
    def get(self, request):
        return render(request, 'myapp/index.html')

11. 添加应用myapp的路由文件urls.py, 写入下面的代码

from django.urls import path
from . import views


urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
]

12. 把myapp的路由文件添加到项目总路由文件中，修改项目下的urls.py如下：

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),
]

启动服务器，目前看到的是没有使用bootstrap的页面，简单、丑陋