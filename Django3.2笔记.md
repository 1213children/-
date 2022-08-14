# django

## django是什么？

Django是重量级选手中最有代表性的一位。许多成功的网站和APP都基于Django。

Django 采用了 MVT 的软件设计模式，即模型（Model），视图（View）和模板（Template）。（MVT模型）

官网: `http://www.djangoproject.com`
官方文档`https://docs.djangoproject.com/zh-hans/3.2/`

## MVT模型和MVC模型

### MVC 模式

MVC 模式（Model–view–controller）是软件工程中的一种软件架构模式，把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller）。

MVC 以一种插件式的、松耦合的方式连接在一起。

- 模型（M）- 编写程序应有的功能，负责业务对象与数据库的映射(ORM)。
- 视图（V）- 图形界面，负责与用户的交互(页面)。
- 控制器（C）- 负责转发请求，对请求进行处理。

![img](https://s2.loli.net/2022/07/14/ZB7IyekMJtUDr3T.png)

用户操作流程图：



![img](https://s2.loli.net/2022/07/14/vqUL4D7hpKkztci.png)



### MVT 模型

> M，Model，模型，是一个类或者函数，里面的代码就是用于完成操作数据库的。
>
> V，View，视图，里面的代码就是用于展示给客户端的页面效果。可以是一个具有特殊语法的HTML文件，也可以是一个数据结构。
>
> C，Controller，控制器，是一个类或者函数，里面的代码就是用于项目功能逻辑的，一般用于调用模型来获取数据，获取到的数据通过调用视图文件返回给客户端。

MVT 模型Django 的 MTV 模式本质上和 MVC 是一样的，也是为了各组件间保持松耦合关系，只是定义上有些许不同，Django 的 MTV 分别是指：

- M 表示模型（Model）：编写程序应有的功能，负责业务对象与数据库的映射(ORM)。
- T 表示模板 (Template)：负责如何把页面(html)展示给用户。
- V 表示视图（View）：负责业务逻辑，并在适当时候调用 Model和 Template。

除了以上三层之外，还需要一个 URL 分发器，它的作用是将一个个 URL 的页面请求分发给不同的 View 处理，View 再调用相应的 Model 和 Template，MTV 的响应模式如下所示：

![](https://s2.loli.net/2022/07/14/BEXvLdIYsyGwMqp.png)

用户操作流程图：

![img](https://s2.loli.net/2022/07/14/7yVZdNvzT1SP8on.png)

**解析：**

用户通过浏览器向我们的服务器发起一个请求(request)，这个请求会去访问视图函数：

- a.如果不涉及到数据调用，那么这个时候视图函数直接返回一个模板也就是一个网页给用户。
- b.如果涉及到数据调用，那么视图函数调用模型，模型去数据库查找数据，然后逐级返回。

视图函数把返回的数据填充到模板中空格中，最后返回网页给用户

## 运行Django

### 第一种

pycharm下面的终端terminal中使用命令运行django

```python
python manage.py runserver (这里写端口，如8080)
python manage.py runserver 8080
```

### 第二种

点击箭头

## 目录结构

在创建django文件时没有视图文件，需要自己创建一个

`views`创建在dome的文件下面。

```python
│─ manage.py    # 终端脚本命令,提供了一系列用于生成文件或者目录的命					 令,也叫脚手架
└─ dome/        # 主应用开发目录,保存了项目中的所有开发人员编写的代					  码, 目录是生成项目时指定的
    │- asgi.py      # django3.0以后新增的，用于让django运行在异						步编程模式的一个web应用对象
    │- settings.py  # 默认开发配置文件
    │- urls.py      # 路由列表目录,用于绑定视图和url的映射关系
    │- wsgi.py      # wsgi就是项目运行在wsgi服务器时的入口文件,相					  当于manage.py
    └- __init__.py
```

### 创建子应用

```bash
python manage.py startapp 子应用名称 
```

子应用的名称将来会作为目录名而存在,所以不能出现特殊符号,不能出现中文等多字节的字符.

就是在目录中在建一个文件夹。

## 路由

路由简单的来说就是根据用户请求的 URL 链接来判断对应的处理程序，并返回处理结果，也就是 URL 与 Django 的视图建立映射关系。

（路由就是我们在百度输入的url。输入的url不同返回给我们东西就不同。）

Django 路由在` urls.py` 配置

```python
from django.contrib import admin
from django.urls import path
from app01.views import index     
urlpatterns = [
    path('admin/', admin.site.urls),
    path("", views.index),
]
```

**在写路由的时候`path("", views.index),`没有参数，也是要在视图函数中写参数request，**

**当有几个参数，就写几个参数`path('timer/xxxx/xxx',views.timer)`,这里视图函数就需要传三个值。**

### 正则匹配路由

#### 无名分组

无名分组就相当于时python函数的位置传参，不能将参数顺序写错。

```python
from django.urls import path,re_path
urlpatterns = [
    path('admin/', admin.site.urls),
    path("index", views.index),
    # 在括号内进行分组，一定要写括号，在括号内写正则。不写会报错
    # 错误article_year() missing 1 required positional argument: 'year'
    re_path('article/(\d{4})',views.article_year)
    # 正则匹配有时会匹配上其他相同的字符串，所以用^ 和$来固定字符串。
    re_path('^article/(\d{4}$)',views.article_year)
]
```

#### 有名分组

有名分组的就相当于时python函数的指定传参，

```python
from django.urls import path,re_path
urlpatterns = [
    path('admin/', admin.site.urls),
    path("index", views.index),
    # 在括号内进行分组，一定要写括号，在括号内写正则。不写会报错
    # 错误article_year() missing 1 required positional argument: 'year'
    re_path('article/(?P<year>\d{4})/(?P<month>\d{2})',views.article_month)
]
```

### 路由分发

要是有1000多个的路由，别不能再全局`urls.py`中写1000多个。
所以有了路由分发，

```python
# 在全局urls.py中设置路由分发。
# 导入include
from django.urls import include, path 
urlpatterns = [
	path('article/', include('app01.urls'))
]
# path('这个还是url的拼装',include('那个文件夹下的url.py'))
# 就是将article/和app01.urls.py下的url路由进行拼接。所以app01.urls.py不用写前缀article/
```

一般我们建的app文件没有`urls.py`文件，需要自己创建。

```python
urlpatterns = [
    re_path('(^\d{4}$)',views.article_year),
    re_path('(?P<year>^\d{4})/(?P<month>\d{2}$)',views.article_month),
]
```

## 视图

`views.py `

一个视图函数，简称视图，是一个简单的 Python 函数，它接受 Web 请求并且返回 Web 响应。

响应可以是一个 HTML 页面、一个 404 错误页面、重定向页面、XML 文档、或者一张图片...

无论视图本身包含什么逻辑，都要返回响应。代码写在哪里都可以，只要在 Python 目录下面，一般放在项目的 views.py 文件中。

每个视图函数都负责返回一个 HttpResponse 对象，对象中包含生成的响应。

视图层中有两个重要的对象：请求对象(request)与响应对象(HttpResponse)。

```python
# 就是写一个函数，路由匹配到函数了，就响应函数的逻辑
# HttpResponse返回文本，
# render可以返回模板文件(html文件)，可以占位并将值传给模板文件。
from django.shortcuts import HttpResponse,render
def index(request):
    return HttpResponse("OK")
def timer(request):
    q=datetime.datetime.now()
    return render(request, "index.html", {"now": q})
```

django的视图主要有2种,分别是**函数视图**和**类视图**.现在刚开始学习django,我们先学习函数视图(FBV),后面再学习类视图(CBV).

### 4.1、请求方式

web项目运行在http协议下,默认肯定也支持用户通过不同的http请求发送数据来.

```
常用的http请求:
POST        添加/上传
GET         获取/下载
PUT/PATCH   修改,其中PUT表示修改整体数据,而PATCH表示修改部分数据
DELETE      删除,废弃
```

django支持让客户端只能通过指定的Http请求来访问到项目的视图

### 4.2、请求对象

django将请求报文中的请求行、首部信息、内容主体封装成 HttpRequest 类中的属性。 除了特殊说明的之外，其他均为只读的。

#### （1）请求方式

```
print(request.method)
```

#### （2）请求数据

```python
# 1.HttpRequest.GET:一个类似于字典的对象，包含 HTTP GET 的所有参数。详情请参考 QueryDict 对象。

# 2.HttpRequest.POST:一个类似于字典的对象，如果请求中包含表单数据，则将这些数据封装成 QueryDict 对象。
   # 注意：键值对的值是多个的时候,比如checkbox类型的input标签，select标签，需要用： request.POST.getlist("hobby")
    
# 3.HttpRequest.body:一个字符串，代表请求报文的请求体的原数据。
```

#### （3）请求路径

```python
# HttpRequest.path:表示请求的路径组件（不含get参数）
# HttpRequest.get_full_path()：含参数路径
```

#### （4）请求头

```python
# HttpRequest.META:一个标准的Python 字典，包含所有的HTTP 首部。具体的头部信息取决于客户端和服务器
```

### 4.3响应对象

#### HttpResponse

返回文本

```python
def index(request):
    return HttpResponse("OK")
```

#### render

返回一个html文件，并可以向html文件中注入数据。

render方法就是将一个模板页面中的模板语法进行渲染，最终渲染成一个html页面作为响应体。

```python
def timer(request):
    q=datetime.datetime.now()
    return render(request, "index.html", {"now": q})
```

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 style="color: red">hello 沈泽昊{{ now }}</h1>
</body>
</html>
````

#### redirect(重定向)

返回一个新的页面(需要配置路由)

```python
def login(request):
    ...
	return redirect("/timer/")
# 登录成功后跳转到timer/页面，
# 必须是前一个/必须加
```

当然也可以是一个完整的网址

```python
def my_view(request):
    ...
    return redirect("http://www.baidu.com")
```

传递一个视图的名称(reverse来反向解析url)

```python
def my_view(request):
    ...
    return redirect(reverse("url的别名"))　
```



## 模板文件

创建创建一个新的子应用tem

```python
python manage.py startapp app01
```

settings.py，注册子应用，代码：

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 开发者创建的子应用，这填写就是子应用的导包路径
     'app01.apps.App01Config',
     # z
     'app01',  
]
```

模板文件写在templates内，如果前后端分离这个用的少。

占位就是{{ }} 两个大括号，在括号内写，在`views.py `中{"now": q}给值

`app01.views`

```python
def timer(request):
    now_time=datetime.datetime.now()
    dic_li={"nihao":1,
            "小说":2,
             "lo":3
    }
    list_li=[1,2,3,4,5]
    book_list = [
        {"id": 10, "price": 9.90, "name": "python3天入门到挣扎", },
        {"id": 11, "price": 19.90, "name": "python7天入门到垂死挣扎", },
    ]
    a=1
    return render(request, "index.html", {"now": now_time,"list_li":list_li,"dic":dic_li,"book_list":book_list,"a":a})
```

`templates.index`

```html
<p>{{ now|date:"D d M Y"  }}</p>
<p>{{ a|add:1 }}</p><!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 style="color: red">hello 沈泽昊
{{ now }}</h1>
<p>{{ list_li|last }}</p>
<p>{{ dic.lo }}</p>
<p>{{ book_list.0.name }}</p>
</body>
</html>
```

> 1.DTL模板文件与普通html文件的区别在哪里？
>
> ```python
> DTL模板文件是一种带有特殊语法的HTML文件，这个HTML文件可以被Django编译，可以传递参数进去，实现数据动态化。在编译完成后，生成一个普通的HTML文件，然后发送给客户端。
> ```
>
> 2.开发中，我们一般把开发中的文件分2种，分别是静态文件和动态文件。
>
> ```python
> * 静态文件，数据保存在当前文件，不需要经过任何处理就可以展示出去。普通html文件，图片，视频，音频等这一类文件叫静态文件。
> * 动态文件，数据并不在当前文件，而是要经过服务端或其他程序进行编译转换才可以展示出去。 编译转换的过程往往就是使用正则或其他技术把文件内部具有特殊格式的变量转换成真实数据。 动态文件，一般数据会保存在第三方存储设备，如数据库中。django的视图，模板文件，就属于动态文件。
> ```

### 模板语法

#### 变量渲染之深度查询

`app01.views`

```python
def timer(request):
    now_time=datetime.datetime.now()
    dic_li={"nihao":1,
            "小说":2,
             "lo":3
    }
    list_li=[1,2,3,4,5]
    book_list = [
        {"id": 10, "price": 9.90, "name": "python3天入门到挣扎", },
        {"id": 11, "price": 19.90, "name": "python7天入门到垂死挣扎", },
    ]
    a=1
    return render(request, "index.html", {"now": now_time,"list_li":list_li,"dic":dic_li,"book_list":book_list,"a":a})
```

模板代码，`templates/index.html`：
通过句点符号深度查询

```python
<p>{{ now|date:"D d M Y"  }}</p>
<p>{{ a|add:1 }}</p><!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 style="color: red">hello 沈泽昊
{{ now }}</h1>
<p>{{ list_li|last }}</p>
<p>{{ dic.lo }}</p>
<p>{{ book_list.0.name }}</p>
</body>
</html>
```

#### 变量渲染之内置过滤器

语法：

```python
{{obj|过滤器名称:过滤器参数}}
```

内置过滤器

| 过滤器          | 用法                                     | 代码                            |
| --------------- | ---------------------------------------- | ------------------------------- |
| last            | 获取列表/元组的最后一个成员              | {{liast \| last}}               |
| first           | 获取列表/元组的第一个成员                | {{list\|first}}                 |
| length          | 获取数据的长度                           | {{list \| length}}              |
| defualt         | 当变量没有值的情况下, 系统输出默认值,    | {{str\|default="默认值"}}       |
| safe            | 让系统不要对内容中的html代码进行实体转义 | {{htmlcontent\| safe}}          |
| upper           | 字母转换成大写                           | {{str \| upper}}                |
| lower           | 字母转换成小写                           | {{str \| lower}}                |
| title           | 每个单词首字母转换成大写                 | {{str \| title}}                |
| date            | 日期时间格式转换                         | `{{ value| date:"D d M Y" }}`   |
| `cut`           | 从内容中截取掉同样字符的内容             | {{content \| cut:"hello"}}      |
| list            | 把内容转换成列表格式                     | {{content \| list}}             |
| `escape`        | 把内容中的HTML特殊符号转换成实体字符     | {{content \| escape }}          |
| filesizeformat  | 把文件大小的数值转换成单位表示           | {{filesize \| filesizeformat}}  |
| `join`          | 按指定字符拼接内容                       | {{list\| join("-")}}            |
| `random`        | 随机提取某个成员                         | {list \| random}}               |
| `slice`         | 按切片提取成员                           | {{list \| slice:":-2"}}         |
| `truncatechars` | 按字符长度截取内容                       | {{content \| truncatechars:30}} |
| `truncatewords` | 按单词长度截取内容                       | 同上                            |

### 标签

#### （1）if 标签

`app01.views`视图函数传值

```python
def timer(request):
 user=None
  return render(request, 'index.html',{"user":user})
```

和python语法一样
`templates.index.html`

```python
如果user有值页面返回用户名(user),没有值就返回<p>标签，
{% if user %}
    <p>用户名{{ user }}</p>
{% else %}
    <p><a href="">注册</a></p>
{% endif %}
```

#### （2）for标签

`app01.views`视图函数传值

```python
def timer(request):
    list_li=["沈","泽","昊","哈","哈"]
     return render(request,"index.html","list_li":list_li）
```

`templates.index.html`

```python
#  forloop就是一个计数的。forloop.counter就是列表标出1，2，3，的
<ul>
{% for book in list_li %}
    <li>{{ forloop.counter }}{{ book }}</li>
    {% endfor %}
</ul>
```

循环中, 模板引擎提供的forloop对象,用于给开发者获取循环次数或者判断循环过程的.

| 属性                | 描述                                      |
| ------------------- | ----------------------------------------- |
| forloop.counter     | 显示循环的次数,从1开始                    |
| forloop.counter0    | 显示循环的次数,从0开始                    |
| forloop.revcounter0 | 倒数显示循环的次数,从0开始                |
| forloop.revcounter  | 倒数显示循环的次数,从1开始                |
| forloop.first       | 判断如果本次是循环的第一次,则结果为True   |
| forloop.last        | 判断如果本次是循环的最后一次,则结果为True |
| forloop.parentloop  | 在嵌套循环中，指向当前循环的上级循环      |

### 模板嵌套和继承

传统的模板分离技术,依靠{%include "模板文件名"%}实现,这种方式,虽然达到了页面代码复用的效果,但是由此也会带来大量的碎片化模板,导致维护模板的成本上升.因此, 大部分框架中除了提供这种模板分离技术以外,还并行的提供了 模板继承给开发者.

> {% extends "继承html文件" %}
>
> {% block content %} 
> 独立内容
> {% endblock  content%}
>
> 在子html文件中和父html文件中都写，不想继承父html文件中的代码，就在子html文件{% block content %} {% endblock  content%}中重写。

#### (1)  继承父模板的公共内容

> {% extends "base.html" %}   继承base.html中的所有代码

子模板index.html,代码:

```python
{% extends "base.html" %}
{% block title %}index3的标题{% endblock  %}
{% block content %}
    {{ block.super }} {# 父级模板同名block标签的内容 #}
    <h1>index3.html的独立内容</h1>
    {{ block.super }}
{% endblock %}
```



> - 如果你在模版中使用 `{% extends %}` 标签，它必须是模版中的第一个标签。其他的任何情况下，模版继承都将无法工作。
> - 在base模版中设置越多的 `{% block %}` 标签越好。请记住，子模版不必定义全部父模版中的blocks，所以，你可以在大多数blocks中填充合理的默认内容，然后，只定义你需要的那一个。多一点钩子总比少一点好。
> - 为了更好的可读性，你也可以给你的 `{% endblock %}` 标签一个 *名字* 。例如：`{``%` `block content``%``}``...``{``%` `endblock content``%``},`在大型模版中，这个方法帮你清楚的看到哪一个　 `{% block %}` 标签被关闭了。
> - 不能在一个模版中定义多个相同名字的 `block` 标签。

### 静态文件

在新建一个软件包，取名statics(名字随便取)

在`settings`文件中配置

```python
STATIC_URL = '/static/'   # 这个是访问时的路径
STATICFILES_DIRS = [ 
    os.path.join(BASE_DIR, "statics"),  # 这个是将文件添加到环境中 
]
```

在模板文件中导入时是

```python
# 在导入时第一个文件名，不能写我们创建的那个，要写/static/
<link rel="stylesheet" href="/static/bootstrap/dist/css/bootstrap.css">
```

注意：项目上线以后，关闭debug模式时，django默认是不提供静态文件的访问支持，项目部署的时候，我们会通过收集静态文件使用nginx这种web服务器来提供静态文件的访问支持。

## ORM

Django 模型使用自带的 ORM。

对象关系映射（Object Relational Mapping，简称 ORM ）用于实现面向对象编程语言里不同类型系统的数据之间的转换。

ORM 在业务逻辑层和数据库层之间充当了桥梁的作用。

ORM 是通过使用描述对象和数据库之间的映射的元数据，将程序中的对象自动持久化到数据库中

![img](https://s2.loli.net/2022/07/21/iIBMADfxp2QGPTu.png)

使用 ORM 的好处：

> - 简单高效，提高开发效率。
> - 不同数据库可以平滑切换。
> - 数据模型类都在一个地方定义，更容易更新和维护，也利于重用代码。
> - ORM 有现成的工具，很多功能都可以自动完成，比如数据消除、预处理、事务等等。
>
> - 它迫使你使用 MVC 架构，ORM 就是天然的 Model，最终使代码更清晰。
>
> - 基于 ORM 的业务代码比较简单，代码量少，语义性好，容易理解。
>
> - 新手对于复杂业务容易写出性能不佳的 SQL,有了ORM不必编写复杂的SQL语句, 只需要通过操作模型对象即可同步修改数据表中的数据.
>
> - 开发中应用ORM将来如果要切换数据库.只需要切换ORM底层对接数据库的驱动【修改配置文件的连接地址即可】

使用 ORM 的缺点：

> - ORM 代码转换为 SQL 语句时，需要花费一定的时间，执行效率会有所降低。
> - 长期写 ORM 代码，会降低编写 SQL 语句的能力。
> - ORM 库不是轻量级工具，需要花很多精力学习和设置，甚至不同的框架，会存在不同操作的ORM。
> - 对于复杂的业务查询，ORM表达起来比原生的SQL要更加困难和复杂。
> - ORM操作数据库的性能要比使用原生的SQL差。
> - ORM 抽象掉了数据库层，开发者无法了解底层的数据库操作，也无法定制一些特殊的 SQL。【自己使用pymysql另外操作即可，用了ORM并不表示当前项目不能使用别的数据库操作工具了。】

ORM 解析过程:

- 1、ORM 会将 Python 代码转成为 SQL 语句。
- 2、SQL 语句通过 pymysql 传送到数据库服务端。
- 3、在数据库中执行 SQL 语句并将结果返回。

ORM 对应关系表：

![img](https://s2.loli.net/2022/07/21/FfdISDrEYqLBX2p.png)

通过下面步骤来使用django的数据库操作

创建 MySQL 数据库( ORM 无法操作到数据库级别，只能操作到数据表)，所有要自己建库。

```sql
create database 数据库名称 default charset=utf8; # 防止编码问题，指定为 utf8
```

```python
1. 配置数据库连接信息
2. 在models.py中定义模型类
3. 生成数据库迁移文件并执行迁文件[注意：数据迁移是一个独立的功能，这个功能在其他web框架未必和ORM一块的]
4. 通过模型类对象提供的方法或属性完成数据表的增删改查操作
```

我们在项目的 settings.py 文件中找到 DATABASES 配置项，将其信息修改为：

```python
# 将原本的DATABASES注释掉。
DATABASES = { 
    'default': 
    { 
        'ENGINE': 'django.db.backends.mysql',    # 数据库引擎
        'NAME': 'db1', # 数据库名称
        'HOST': '127.0.0.1', # 数据库地址，本机 ip 地址 127.0.0.1 
        'PORT': 3306, # 端口 
        'USER': 'root',  # 数据库用户名
        'PASSWORD': '123456', # 数据库密码
    }  
}
```

告诉 Django 使用 pymysql 模块连接 mysql 数据库：

```python
# 在与 settings.py 同级目录下的 __init__.py 中引入模块和进行配置
import pymysql
pymysql.install_as_MySQLdb()
```

#### 写入MySQL

要在setting中提前配置

```python
INSTALLED_APPS = [    
    'app01.apps.App01Config',
    'app02.apps.App02Config',
]
```

在`app01.models`中写

模型类如果未指明表名db_table，Django默认以 **小写app应用名_小写模型类名** 为数据库表名。

```python
from django.db import models
from datetime import datetime

# 模型类必须要直接或者间接继承于 models.Model
class BaseModel(models.Model):
	"""公共模型[公共方法和公共字段]"""
	# created_time = models.IntegerField(default=0, verbose_name="创建时间")
	created_time = models.DateTimeField(auto_now_add=True, verbose_name="创建时间")
	# auto_now_add 当数据添加时设置当前时间为默认值
	# auto_now= 当数据添加/更新时, 设置当前时间为默认值
	updated_time = models.DateTimeField(auto_now=True)
	class Meta(object):
		abstract = True # 设置当前模型为抽象模型, 当系统运行时, 不会认为这是一个数据表对应的模型.
        # 抽象类就是只能继承，不能实例。
        
        
# 模型类必须要直接或者间接继承于 models.Model
class Student(BaseModel):
# 也可以不继承，直接写Student类
class Student(models.Model)
    id=models.IntegerField(primary_key=True,auto_created=True)
    name=models.CharField(max_length=32,unique=True)
    age=models.IntegerField(null=True)
	#2. 数据表结构信息
	class Meta:
		db_table = 'tb_student'  # 指明数据库表名,如果没有指定表明,则默认为子应用目录名_模型名称,例如: users_student
		verbose_name = '学生信息表'  # 在admin站点中显示的名称
		verbose_name_plural = verbose_name  # 显示的复数名称

	#3. 自定义数据库操作方法
	def __str__(self):
		"""定义每个数据对象的显示信息"""
		return "<User %s>" % self.name
```

#### 字段类型

| 类型             | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| AutoField        | 自动增长的IntegerField，通常不用指定，不指定时Django会自动创建属性名为id的自动增长属性 |
| BooleanField     | 布尔字段，值为True或False                                    |
| NullBooleanField | 支持Null、True、False三种值                                  |
| CharField        | 字符串，参数max_length表示最大字符个数，对应mysql中的varchar |
| TextField        | 大文本字段，一般大段文本（超过4000个字符）才使用。           |
| IntegerField     | 整数                                                         |
| DecimalField     | 十进制浮点数， 参数max_digits表示总位数， 参数decimal_places表示小数位数,常用于表示分数和价格  Decimal(max_digits=7, decimal_places=2)  ==> 99999.99~ 0.00 |
| FloatField       | 浮点数                                                       |
| DateField        | 日期<br>参数auto_now表示每次保存对象时，自动设置该字段为当前时间。<br>参数auto_now_add表示当对象第一次被创建时自动设置当前。<br>参数auto_now_add和auto_now是相互排斥的，一起使用会发生错误。 |
| TimeField        | 时间，参数同DateField                                        |
| DateTimeField    | 日期时间，参数同DateField                                    |
| FileField        | 上传文件字段,django在文件字段中内置了文件上传保存类, django可以通过模型的字段存储自动保存上传文件, 但是, 在数据库中本质上保存的仅仅是文件在项目中的存储路径!! |
| ImageField       | 继承于FileField，对上传的内容进行校验，确保是有效的图片      |

#### 约束选项

| 选项        | 说明                                                         |
| :---------- | ------------------------------------------------------------ |
| null        | 如果为True，表示允许为空，默认值是False。相当于python的None  |
| blank       | 如果为True，则该字段允许为空白，默认值是False。  相当于python的空字符串，“” |
| db_column   | 字段的名称，如果未指定，则使用属性的名称。                   |
| db_index    | 若值为True, 则在表中会为此字段创建索引，默认值是False。 相当于SQL语句中的key |
| default     | 默认值，当不填写数据时，使用该选项的值作为数据的默认值。     |
| primary_key | 如果为True，则该字段会成为模型的主键，默认值是False，一般不用设置，系统默认设置。 |
| unique      | 如果为True，则该字段在表中必须有唯一值，默认值是False。相当于SQL语句中的unique |

**注意：null是数据库范畴的概念，blank是表单验证范畴的**

### 数据迁移

将模型类定义表架构的代码转换成SQL同步到数据库中，这个过程就是数据迁移。django中的数据迁移，就是一个类，这个类提供了一系列的终端命令，帮我们完成数据迁移的工作。

#### (1)生成迁移文件

```python
# 所谓的迁移文件, 是类似模型类的迁移类,主要是描述了数据表结构的类文件.
# 会在app01.migrations文件下创建一个文件，将python代码转换成sql代码。
python manage.py makemigrations     
```

#### (2)同步到数据库中

经验在db2中使用过`python manage.py migrate`这个，那就不能使用了，

会报出  No migrations to apply.

```python
# 执行上面代码生成的文件，写入到mysql数据库中。
python manage.py migrate
```

## 数据库基本操作

### 添加记录

#### (1)save方法

```python
# 先导入
from app01.models import Student 
s1=Student(id=1,name="沈泽昊",age=21)
s1.save() # 将数据写入mysql 
print(student.id)# 判断是否新增有ID
```

#### (2)create方法

### 通过模型类.objects.create()保存。

```python
from app01.models import Student
Student.objects.create(id=2,name="曹璐璇",age=21)
Student.objects.create(id=3, name="董睿洋",age=21)
# 不用在写代码提交数据
```

### 查询记录

ORM中针对查询结果的数量限制,提供了一个查询集[QuerySet].这个QuerySet,是ORM中针对查询结果进行保存数据的一个类型,我们可以通过了解这个QuerySet进行使用,达到查询优化,或者限制查询结果数量的作用。

查询集，也称查询结果集、QuerySet，表示从数据库中获取的对象集合。当调用如下过滤器方法时，Django会返回查询集（而不是简单的列表）：

#### all()  

```python
student_list=Student.objects.all()
print("student_list",student_list,type(student_list))  
# 类型<class 'django.db.models.query.QuerySet'>
for i in student_list:
    print(i.name)
    print(i.age)
"""
找到所有的数据，并输出
"""
```

#### filter()

```python
q=Student.objects.filter(name="沈泽昊")    # filter相当于where过滤
for i in q:
    print(i.name)
"""
和where差不多，一个过滤条件。
"""
```

#### get()

```python
q=Student.objects.get(name="沈泽昊")   
# 当数据库中只有一个值时可以拿出来，有两个以上，或者没有值时，就会报错。
print(q.name)
```

#### first(),last()

```python
q = Student.objects.all().first().name
print(q)

"""
# 没有结果返回none，如果有多个结果，则返回模型对象
拿到第一个，或者最后一个
"""
```

#### order_by()

```python
# # 年龄倒序，年龄一致按id进行正序
student_list = Student.objects.order_by("-age", "id").all()
for i in student_list:
    print(i.name)
"""
# order_by("字段")  # 按指定字段正序显示，相当于 asc  从小到大
# order_by("-字段") # 按字段倒序排列，相当于 desc 从大到小
# order_by("第一排序","第二排序",...)
"""
```

#### count()

```python
# count统计结果数量
ret = Student.objects.filter(name="沈泽昊").count()
print(ret)
```

#### exists()

```python
判断查询集中是否有数据，如果有则返回True，没有则返回False
```

#### values()、values_list()

* `value()`把结果集中的模型对象转换成**字典**,并可以设置转换的字段列表，达到减少内存损耗，提高性能
* `values_list()`: 把结果集中的模型对象转换成**列表**，并可以设置转换的字段列表（元祖），达到减少内存损耗，提高性能

```python
# values()  不写，默认把所有字段全部转换并返回。
q=Student.objects.values("name","age") 
for i in q:
    print(i,type(i))     # <class 'dict'>
q=Student.objects.values_list("name","age") 
print(q)
for i in q:
    print(i,type(i))   # 'tuple'
```

#### distinct()

```python
# 去重
q=Student.objects.all().distinct()
print(q)
```

### 更新数据

**使用模型类.objects.filter().update()**，会返回受影响的行数

```python
# update是全局更新，只要符合更新的条件，则全部更新，因此强烈建议加上条件！！！
student = Student.objects.filter(name="沈泽昊",age=22).update(name="刘大炮",age=22)
print(student)
```

### 删除记录

删除有两种方法

#### （1）模型类对象.delete

```python
student = Student.objects.get(id=13)
student.delete()
```

#### （2）模型类.objects.filter().delete()(推荐)

```python
Student.objects.filter(id=14).delete() 
```

```python
# 1. 先查询到数据模型对象。通过模型对象进行删除
student = Student.objects.filter(name="沈泽昊").first()
student.delete()

# 2. 直接删除
ret = Student.objects.filter(name="沈泽昊").delete()
print(ret)
# 务必写上条件，否则变成了清空表了。
#ret = Student.objects.filter().delete()
```

### 模糊查询

#### （1）模糊查询之contains

> 说明：如果要包含%无需转义，直接写即可。

```python
# 例：查询姓名包含'华'的学生。
Student.objects.filter(name__contains='华')
```

#### （2）模糊查询之startswith、endswith

查询到开头或者结尾

> 以上运算符都区分大小写，在这些运算符前加上i表示不区分大小写，如iexact、icontains、istartswith、iendswith.

```python
# 例：查询姓名以'文'结尾的学生
Student.objects.filter(name__endswith='文')
```

#### （3）模糊查询之isnull

判断某个字段是否为空

```python
res = Student.objects.filter(description__isnull=True)
```

#### （4）模糊查询之in

例：查询编号为1或3或5的学生

```python
Student.objects.filter(id__in=[1, 3, 5])
```

#### （5）模糊查询之比较查询

- **gt** 大于 (greater then)
- **gte** 大于等于 (greater then equal)
- **lt** 小于 (less then)
- **lte** 小于等于 (less then equal)

例：查询编号大于3的学生

```python
Student.objects.filter(id__gt=3)
```

#### （6）模糊查询之日期查询

**year、month、day、week_day、hour、minute、second：对日期时间类型的属性进行运算。**

例：查询2010年被添加到数据中的学生。

```python
Student.objects.filter(born_date__year=1980)
```

例：查询2016年6月20日后添加的学生信息。

```python
from django.utils import timezone as datetime
student_list = Student.objects.filter(created_time__gte=datetime.datetime(2016,6,20),created_time__lt=datetime.datetime(2016,6,21)).all()
print(student_list)
```

### 进阶查询

#### （1） F查询

之前的查询都是对象的属性与常量值比较，两个属性的比较是用到F

```python
"""F对象：2个字段的值比较"""
# 获取从添加数据以后被改动过数据的学生
from django.db.models import F
# SQL: select * from db_student where created_time=updated_time;
student_list = Student.objects.exclude(created_time=F("updated_time"))
print(student_list)
```

#### （2） Q查询

**多个过滤器逐个调用表示逻辑与关系，同sql语句中where部分的and关键字。**

**在django中需要实现逻辑或or的查询，需要使用Q()对象结合|运算符**，Q对象被义在django.db.models中。

```python
Q(属性名__运算符=值)
Q(属性名__运算符=值) | Q(属性名__运算符=值)
```

例：查询年龄小于19或者大于20的学生，使用Q对象如下。

```python
from django.db.models import Q
student_list = Student.objects.filter( Q(age__lt=19) | Q(age__gt=20) ).all()
```

**Q对象可以使用&、|连接，&表示逻辑与，|表示逻辑或，~表示非**

例：查询年龄大于20，或编号小于30的学生，只能使用Q对象实现

```python
Student.objects.filter(Q(age__gt=20) | Q(pk__lt=30))
```

`Q对象左边可以使用~操作符，表示非not。但是工作中，我们只会使用Q对象进行或者的操作，只有多种嵌套复杂的查询条件才会使用&和~进行与和非得操作`。

### （3）聚合查询

使用aggregate()过滤器调用聚合函数。聚合函数包括：**Avg** 平均，**Count** 数量，**Max** 最大，**Min** 最小，**Sum** 求和，被定义在django.db.models中。

例：查询学生的平均年龄

```python
from django.db.models import Sum,Count,Avg,Max,Min

Student.objects.aggregate(Avg('age'))
```

注意：aggregate的返回值是一个字典类型，格式如下：

```python
  {'属性名__聚合类小写':值}
```

使用count时一般不使用aggregate()过滤器。

例：查询学生总数。

```python
Student.objects.count() # count函数的返回值是一个数字。
```

### （4）分组查询

**返回值：**

- 分组后，用 values 取值，则返回值是 QuerySet 数据类型里面为一个个字典；
- 分组后，用 values_list 取值，则返回值是 QuerySet 数据类型里面为一个个元组。

MySQL 中的 limit 相当于 ORM 中的 QuerySet 数据类型的切片。

**注意：**

##### annotate 里面放聚合函数。

- **values 或者 values_list 放在 annotate 前面：**values 或者 values_list 是声明以什么字段分组，annotate 执行分组。
- **values 或者 values_list 放在annotate后面：** annotate 表示直接以当前表的pk执行分组，values 或者 values_list 表示查询哪些字段， 并且要将 annotate 里的聚合函数起别名，在 values 或者 values_list 里写其别名。

```python
objects.values("id").annotate(in_price=Max("age"))
以id进行分组，
输出{'age': 22, 'in_price': 22}{'age': 23, 'in_price': 23}{'age': 2, 'in_price': 2}{'age': 3, 'in_price': 3}
```

```python
objects.values("id").annotate(in_price=Max("age")).values("name","age")
这个会将in_price改名字。
输出{'name': '曹璐璇', 'age': 22}{'name': '董睿洋', 'age': 23}{'name': 'shen', 'age': 22}{'name': 'cao', 'age': 2}{'name': 'dong', 'age': 3}
```



```python
QuerySet对象.annotate()
# annotate() 进行分组统计，按前面select 的字段进行 group by
# annotate() 返回值依然是 queryset对象，增加了分组统计后的键值对
模型对象.objects.values("id").annotate(course=Count('course__sid')).values('id','course')
# 查询指定模型， 按id分组 , 将course下的sid字段计数，返回结果是 name字段 和 course计数结果 

# SQL原生语句中分组之后可以使用having过滤，在django中并没有提供having对应的方法，但是可以使用filter对分组结果进行过滤
# 所以filter在annotate之前，表示where，在annotate之后代表having
# 同理，values在annotate之前，代表分组的字段，在annotate之后代表数据查询结果返回的字段
```

### （5）原生查询

执行原生SQL语句,也可以直接跳过模型,才通用原生pymysql.

```python
res = Student.objects.raw("SELECT id,name,age FROM  pp_student").query
# 可以配合佛循环
for item in res:
    print(item)
```

## 关联型数据库

```python
class Book(models.Model):
    title = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=5, decimal_places=2)
    pub_date = models.DateField()
    # 与Publish建立一对多的关系,外键字段建立在多的一方
    # on_delete= 关联关系的设置
    # models.CASCADE    删除主键以后, 对应的外键所在数据也被删除
    # models.DO_NOTHING 删除主键以后, 对应的外键不做任何修改
    # 反向查找字段 related_name
    publish = models.ForeignKey("Publish", on_delete=models.CASCADE)
        # 与Author表建立多对多的关系,ManyToManyField可以建在两个模型中的任意一个，自动创建第三张表
    authors = models.ManyToManyField("Author")


class Publish(models.Model):
    name = models.CharField(max_length=32)
    city = models.CharField(max_length=64)
    email = models.EmailField()


class Author(models.Model):
    name = models.CharField(max_length=32)
    age = models.SmallIntegerField()
       # 与AuthorDetail建立一对一的关系
    au_detail = models.OneToOneField("AuthorDetail", on_delete=models.CASCADE)


class AuthorDetail(models.Model):
    gender_choices = (
        (0, "女"),
        (1, "男"),
        (2, "保密")
    )
    gender = models.SmallIntegerField(choices=gender_choices)
    tel = models.CharField(max_length=32)
    addr = models.CharField(max_length=64)
    birthday = models.DateField()
```

### 一对一

```python
class Book(models.Model):
    book_name=models.CharField(max_length=32)
    price=models.DecimalField(max_digits=5, decimal_places=2)
    pub_date= models.DateField()


class Author(models.Model):
    name = models.CharField(max_length=32)
    age = models.SmallIntegerField()
    birth=models.DateField()
    Bo_or = models.OneToOneField("Book", on_delete=models.CASCADE)
```

在author表中会建一个Bo_or_id的字段与book表的id字段关联

```sql
mysql> desc app01_book;
+-----------+--------------+------+-----+---------+----------------+
| Field     | Type         | Null | Key | Default | Extra          |
+-----------+--------------+------+-----+---------+----------------+
| id        | bigint       | NO   | PRI | NULL    | auto_increment |
| book_name | varchar(32)  | NO   |     | NULL    |                |
| price     | decimal(5,2) | NO   |     | NULL    |                |
| pub_date  | date         | NO   |     | NULL    |                |
+-----------+--------------+------+-----+---------+----------------+


mysql> desc app01_author;
+----------+-------------+------+-----+---------+----------------+
| Field    | Type        | Null | Key | Default | Extra          |
+----------+-------------+------+-----+---------+----------------+
| id       | bigint      | NO   | PRI | NULL    | auto_increment |
| name     | varchar(32) | NO   |     | NULL    |                |
| age      | smallint    | NO   |     | NULL    |                |
| birth    | date        | NO   |     | NULL    |                |
| Bo_or_id | bigint      | NO   | UNI | NULL    |                |
+----------+-------------+------+-----+---------+----------------+
```

#### 写入数据 

写入前要先写入book，先写app01_author会报错，因为book表中没有数据，Bo_or_id没有关联的对象。

```python
#写入desc app01_book表
book=Book.objects.create(book_name="西游记",price="19.9",pub_date="2022-12-12")
# 第一种写入Author表的方法
Bo_or_id=Book.objects.get(id=3)
Author.objects.create(name="吴承恩", age="18", birth="2001-12-12",Bo_or=Bo_or_id)
# 第二种写入Author表的方法
author=Author.objects.create(name="罗贯中", age="18", birth="2001-12-12", Bo_or_id=5)
book=Book.objects.create(book_name="三国演艺", price="29.9", pub_date="2012-11-11")
# 查数据
book=Book.objects.get(id=3)
可以通过关联对象来查询。
print(book.author.name)
print(book.author.age)

# 不能这样查：print(author.book.book_name)
```

### 一对多

```python
class Book(models.Model):
    title = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=5, decimal_places=2)
    pub_date = models.DateField()
    publish = models.ForeignKey("Publish", on_delete=models.CASCADE)



class Publish(models.Model):
    name = models.CharField(max_length=32)
    city = models.CharField(max_length=64)
    email = models.EmailField()
```

在book表中会建一个publish_id的字段与book表的id字段关联

```sql
mysql> desc app02_publish;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | bigint       | NO   | PRI | NULL    | auto_increment |
| name  | varchar(32)  | NO   |     | NULL    |                |
| city  | varchar(64)  | NO   |     | NULL    |                |
| email | varchar(254) | NO   |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+


mysql> desc app02_book;
+------------+--------------+------+-----+---------+----------------+
| Field      | Type         | Null | Key | Default | Extra          |
+------------+--------------+------+-----+---------+----------------+
| id         | bigint       | NO   | PRI | NULL    | auto_increment |
| title      | varchar(32)  | NO   |     | NULL    |                |
| price      | decimal(5,2) | NO   |     | NULL    |                |
| pub_date   | date         | NO   |     | NULL    |                |
| publish_id | bigint       | NO   | MUL | NULL    |                |
+------------+--------------+------+-----+---------+----------------+
```

#### 写入数据

写入前要先写入publish，先入book表会报错，因为book表中没有数据，publish_id没有关联的对象。

```python
Publish.objects.create(name="西瓜出版社",city="北京",email="wwww.121212@qq.com")
Publish.objects.create(name="橙子出版社", city="上海", email="wwww.898989@qq.com")
Book.objects.create(title="西游记", price="19.9", pub_date="2022-02-19", publish_id=1)
Book.objects.create(title="三国演艺", price="29.9", pub_date="2012-02-19", publish_id=1)
Book.objects.create(title="水浒传", price="9.9", pub_date="2002-02-19", publish_id=2)
Book.objects.create(title="红楼梦", price="9.9", pub_date="2002-02-19", publish_id=2)
#######查询数据#######
book=Book.objects.get(id=2)
publish=Publish.objects.get(id=1)
print(book.publish.name)

#    这样写会报错    print(publish.book.publish.name)
```

### 多对多

```python
class Book(models.Model):
    name = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=5, decimal_places=2)
    pub_date = models.DateField()
    authors = models.ManyToManyField("Author")



class Author(models.Model):
    name = models.CharField(max_length=32)
    age = models.SmallIntegerField()
```

不用建立第三张表，会自己建立张表，名字是book_authors,名字是由Book表的名字和这个表中关联关系的返回值，而创建。

在第三个表中会有三个字典，id，book_id，author_id。

```sql
mysql> desc app03_author;
+-------+-------------+------+-----+---------+----------------+
| Field | Type        | Null | Key | Default | Extra          |
+-------+-------------+------+-----+---------+----------------+
| id    | bigint      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(32) | NO   |     | NULL    |                |
| age   | smallint    | NO   |     | NULL    |                |
+-------+-------------+------+-----+---------+----------------+


mysql> desc app03_book;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | bigint       | NO   | PRI | NULL    | auto_increment |
| name     | varchar(32)  | NO   |     | NULL    |                |
| price    | decimal(5,2) | NO   |     | NULL    |                |
| pub_date | date         | NO   |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+


mysql> desc app03_book_authors;
+-----------+--------+------+-----+---------+----------------+
| Field     | Type   | Null | Key | Default | Extra          |
+-----------+--------+------+-----+---------+----------------+
| id        | bigint | NO   | PRI | NULL    | auto_increment |
| book_id   | bigint | NO   | MUL | NULL    |                |
| author_id | bigint | NO   | MUL | NULL    |                |
+-----------+--------+------+-----+---------+----------------+
```

#### 写入数据

```python
# 向book表和author表添加数据
Book.objects.create(name="红楼梦", price="29.9", pub_date="2010-02-12")
Book.objects.create(name="三国演艺", price="39.9", pub_date="2021-02-22")
Book.objects.create(name="水浒传", price="49.9", pub_date="2028-1-26")
Author.objects.create(name="吴承恩", age=300)
Author.objects.create(name="罗贯中", age=299)


    # 添加记录
    # 第一种写法
    # 在book表中拿到数据，用来向第三表添加数据
    book=Book.objects.get(pk=1)
    luo=Author.objects.get(name="罗贯中")
    shi=Author.objects.get(name="施耐庵")
    # 这个是authors的原因的是在建表是关联添加的命名是authors！！！
    book.authors.add(luo,shi)
    # 第二种写法
    book = Book.objects.get(pk=1)
    book.authors.add(2,3)
    # 第三种写法
    # 用的多
    book = Book.objects.get(pk=1)
    l = [2]
    book.authors.add(*l)
# 删除数据 
# 删除book_id=1，author_id=2的数据
book = Book.objects.get(pk=1)
book.authors.remove(2)
# book表中id=1，将所有第三个表的book_id=1的数据都删除了。
book = Book.objects.get(pk=1)
book.authors.clear()
# 删除加更改
# 将book_id=1的数据删除掉，在添加book_id=1，author_id=2和book_id=1，author_id=3
book.authors.set([2,3])
    # 查询
    # 在book表中找到西游记，拿到西游记的id的值，在去第三表中找到book_id=西游记的id,拿到author_id,的作者表中，找到作者的信息。
    #book=Book.objects.get(name="西游记")
    # a=book.authors.all().values("name","age")
    # print(a)
```

### 反向查询

我们再写`models`时在那个类中写关联条件，那就只能在这类类查，	

如：

`models`

```python
class Book(models.Model):
    name = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=5, decimal_places=2)
    pub_date = models.DateField()
    authors = models.ManyToManyField("Author")
    # 加上related_name=就可以反向查询
    authors = models.ManyToManyField("Author",related_name="book_list")

class Author(models.Model):
    name = models.CharField(max_length=32)
    age = models.SmallIntegerField()
```

`views`中

```python
#这样可以
book=BOOK.objects.get(name="西游记")
book.authors.all().values("name")
# 但是这样会报错
author=Author.objects.get(name="施耐庵")
a=author.book.all().values("name")
#####################################author.写related_name等于的值。
# 当在写关联条件时加上related_name就可以查了，
    author=Author.objects.get(name="施耐庵")
    a=author.book_list.all().values("name")
```

## AJAX

AJAX（Asynchronous Javascript And XML）翻译成中文就是“异步Javascript和XML”。即使用Javascript语言与服务器进行异步交互，传输的数据为XML（当然，传输的数据不只是XML,现在更多使用json数据）。

AJAX的优点：

- AJAX使用Javascript技术向服务器发送异步请求；
- AJAX无须刷新整个页面（局部刷新）；
- 因为服务器响应内容不再是整个页面，而是页面中的局部，所以AJAX性能高；

应用：

### json数据

```python
'''
Supports the following objects and types by default:
+-------------------+---------------+
| Python            | JSON          |
+===================+===============+
| dict              | object        |
+-------------------+---------------+
| list, tuple       | array         |
+-------------------+---------------+
| str               | string        |
+-------------------+---------------+
| int, float        | number        |
+-------------------+---------------+
| True              | true          |
+-------------------+---------------+
| False             | false         |
+-------------------+---------------+
| None              | null          |
+-------------------+---------------+
'''
```

### python支持的序列化和反序列化方法

```python
import json

dic = {"name": "yuan"}
dic_json = json.dumps(dic)
dic = json.dumps(dic_json)
```

#### 一个例子

`view`

```python
from django.shortcuts import render, HttpResponse
from app04.models import userinfo
# Create your views here.
import json

from django.http import JsonResponse


def reg(request):
    return render(request, "reg.html")


def ajax_reg(request):
    if request.method == "POST":
        print(request.POST)
        user = request.POST.get("user")
        res = {"state": False, "msg": None}
        if userinfo.objects.filter(usename=user):
            res["state"] = True
            res["msg"] = "该用户已经存在！"
        # 使用JsonResponse直接传入字典就行。
        return JsonResponse(res)
    	# 使用HttpResponse(需要将数据序列化，在返回) 
        # return HttpResponse(json.dumps(res,ensure_ascii=False))
    else:
        return HttpResponse("ok")
```

`reg.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.js"></script>
</head>
<body>
<q>AJAX</q>
<input id="i1" type="text" onblur="foo()">
<span class="err"></span>
<script>
    function foo() {
        //发送ajax请求
        $.ajax({
                url: "/ajax_reg/",
                type: "post",
                data: {
                    user: $("#i1").val(),
                },
                {#当请求发送了，返回的值#}
                success: function (res) {
                    console.log(res)
                    // 反序列化
                    {#var data = JSON.parse(res)#}
                    var data = res
                    if (data.state) {
                        $("#i1").css("border", "lpx solid red")
                        // 显示错误信息、
                        $(".err").html(data.msg)
                    }
                },
                    // k
               // error: function (XMLHttpRequest, textStatus, errorThrown) {
                    // 状态码
                 //   console.log(XMLHttpRequest.status);
                    // 状态
                   // console.log(XMLHttpRequest.readyState);
                    // 错误信息
                   // console.log(textStatus);
                //}

            }
        )
    }
</script>
</body>
</html>





```

> 自己的理解
>
> 写两个视图函数，访问第一个视图函数，视图函数中的模板函数有Ajax请求。
> Ajax请求就是访问另一个视图函数，将前端数据发送给后端，后端做完校验后，在将校验结果返回给前端。做到局部刷新。

## form组件

校验作用，对前端数据到后端进行校验。

`html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

</head>
<body>

<form action="" method="post">
    {% csrf_token %}
    <p>用户名<input type="text" name="name"></p>
    <p>密码<input type="password" name="pwd"></p>
    <p>确认密码<input type="password" name="r_pwd"></p>
    <p>邮箱<input type="text" name="email"></p>
    <p>手机号<input type="text" name="tel"></p>
    <input type="submit">
</form>

</body>
</html>
```

`views.py`

> 使用form = UserForm(request.POST)时，一定要保证UserForm类的变量名和模板函数中的name值一样，否则会报错。

```python
from django import forms
class UserForm(forms.Form):
    name = forms.CharField(min_length=4)
    pwd = forms.CharField(min_length=4)
    r_pwd = forms.CharField(min_length=4)
    email = forms.EmailField()
    tel = forms.CharField()
 
def reg(request):
    if request.method == "POST":
        # print(request.POST)
        # form=UserForm({"name":"yuan","email":"123"})
        # 校验时只对UserForm类中的数据进行校验，有其他数据，不会报错
        # request.POST中有token，但是token并没有在UserForm类中所有不用校验。
        form = UserForm(request.POST)  # form表单的name属性必须和forms组件的变量名一样
        if form.is_valid():  # 所有字段检验成功 返回布尔值(Ture,Flase)
            print(form.cleaned_data) # 正确的数据会在这个字典中
        else:
            print(form.errors)   # 将错误放到这个字典里面
            # print(type(form.errors))
            print(form.errors.get("__all__")[0])
        return HttpResponse("ok")
    return render(request, "register.html")
```

### 局部钩子和全局钩子

就是对数据进行更加精细的校验

##### 局部钩子

```python
# 这个函数名字是固定的，clean_
def clean_name(self):
    val = self.cleaned_data.get("name")
    if Book.objects.filter(book_name=val):
        raise ValidationError("该用户已注册")
    else:
        return val

def clean_tel(self):
    val = self.cleaned_data.get("tel")
    if len(val) == 11:
        return val
    else:
        raise ValidationError("手机号格式错误")
```

##### 全局钩子

```python
def clean(self):
    pwd=self.cleaned_data.get("pwd")
    r_pwd=self.cleaned_data.get("r_pwd")
    if pwd==r_pwd:
        return self.cleaned_data
    else:
        raise ValidationError('两次密码不一致!')
```

可以将form组件单独写在一个文件中。

`My_forms.py`

```python
from django import forms
from app01.models import Book,Author
from django.core.exceptions import ValidationError
class UserForm(forms.Form):
    name = forms.CharField(min_length=4)
    pwd = forms.CharField(min_length=4)
    r_pwd = forms.CharField(min_length=4)
    email = forms.EmailField()
    tel = forms.CharField()

    # 局部钩子
    def clean_name(self):
        val = self.cleaned_data.get("name")
        if Book.objects.filter(book_name=val):
            raise ValidationError("该用户已注册")
        else:
            return val

    def clean_tel(self):
        val = self.cleaned_data.get("tel")
        if len(val) == 11:
            return val
        else:
            raise ValidationError("手机号格式错误")
```

`views`

```python
from app01.My_forms import *

def reg(request):
    if request.method == "POST":
        # print(request.POST)
        # form=UserForm({"name":"yuan","email":"123"})
        form = UserForm(request.POST)  # form表单的name属性必须和forms组件的变量名一样
        if form.is_valid():  # 所有字段检验成功 返回布尔值(Ture,Flase)
            print(form.cleaned_data)
        else:
            print(form.errors)   # 将错误放到这个字典里面
            # print(type(form.errors))
            print(form.errors.get("__all__"))
        return HttpResponse("ok")
    return render(request, "register.html")

```

## Cookie和Session

![image-20210812145309289](D:\笔记\Django\assets\image-20210812145309289.png)

无状态的意思是每次请求都是独立的，它的执行情况和结果与前面的请求和之后的请求都无直接关系，它不会受前面的请求响应情况直接影响，也不会直接影响后面的请求响应情况。

**就是我们在访问的时候，浏览器不道是我们访问以前访问过没有，就可能出现没访问一次浏服务端就需要重新登录。**

##### 什么是Cookie

Cookie具体指的是一段小信息，它是服务器发送出来存储在浏览器上的一组组键值对，下次访问服务器时浏览器会自动携带这些键值对，以便服务器提取有用信息。

##### Cookie的原理

cookie的工作原理是：由服务器产生内容，浏览器收到请求后保存在本地；当浏览器再次访问时，浏览器会自动带上Cookie，这样服务器就能通过Cookie的内容来判断这个是“谁”了。

##### cookie的设置和读取

配置好路由后

```python
# response=HttpResponse("登录成功")# 这里写render,HttpResponse,redirect都可以
# rep.set_cookie(key,value,...)
# 设置cookice
response=HttpResponse("登录成功")
response.set_cookie("is_login", True)
```

`views`

```python
def login(request):
    if request.method=="POST":
        use=request.POST.get("user")
        pwd=request.POST.get("pwd")
        if userinfo.objects.filter(use=use,pwd=pwd).first():
            response=HttpResponse("登录成功")
            # response.set_cookie("is_login",True,max_age=10)
            response.set_cookie("is_login", True,path='/index')
            import datetime
            # 这里的时间需要用户现在的时间，减去8，
            # date=datetime.datetime(year=2022,month=8,day=4,hour=9,minute=3)
            response.set_cookie("username",userinfo.use)
            return response
        else:
            return HttpResponse("失败")
    return render(request,"login.html")
```

`views`

```python
def index(request):
    # print(request.COOKIES)
    is_login=request.COOKIES.get("is_login")
    user=request.COOKIES.get("username")
    # print(user)
    if is_login:
        return render(request,"index2.html",{"user":user})
    else:
        return redirect("/login/")
```

#### cookie的参数

```python
# 主要用待到这三个
set_cookie(self, key, value='', max_age=None, expires=None, path='/',）
```

> max_age就是cookie的有效时间(秒)
>
> expires也是有效时间但是需要的格式是2022-12-12 15:12:22，一般用datetime
>
> path指定路径，指定那一个路径可以用这条cookie

## session

session会存到django_session中，并对数据进行加密。 

配置好路由

```python
# 1、设置Sessions值
       request.session['session_name'] ="ok"
# 2、获取Sessions值
       session_name = request.session["session_name"]
# 3、删除Sessions值
       del request.session["session_name"]
# 4、flush()
  # 删除当前的会话数据并删除会话的Cookie。这用于确保前面的会话数据不可以再次被用户的浏览器访问
```

`views`

```python
def Cookie_session(request):
    if request.method=="POST":
        use=request.POST.get("user")
        pwd=request.POST.get("pwd")
        if userinfo.objects.filter(use=use,pwd=pwd).first():
            request.session["is_login"]=True
            request.session["suername"]=use
            """
            1.生成一个随机字符串，如：12145asdgfds312
            2.response.set_cookie("sessionid",12145asdgfds312)
            3.django-sessiuon表中会创建一个记录(表中的数据是加密的)
                session-key            session-data
                12145asdgfds312        {is_login:True,"suername","yuan"}   
            """
            return HttpResponse("登录成功")
        else:
            return HttpResponse("失败")
    return render(request,"login.html")
```

`views`

```python
def index_session(request):
    is_login=request.session.get("is_login")
    if not is_login:
        return  redirect("/login/")
    username=request.session.get("suername")
    return HttpResponse(username)
```



> 1、设置Sessions值
>           request.session['session_name'] ="admin"
> 2、获取Sessions值
>           session_name = request.session["session_name"]
> 3、删除Sessions值
>           del request.session["session_name"]
> 4、flush()
>      删除当前的会话数据并删除会话的Cookie。
>      这用于确保前面的会话数据不可以再次被用户的浏览器访问
> 5、get(key, default=None)
>   fav_color = request.session.get('fav_color', 'red')  
> 6、pop(key)
>   fav_color = request.session.pop('fav_color')  
> 7、keys()
> 8、items()  
> 9、setdefault()  
> 10 用户session的随机字符串
>         request.session.session_key
>
> 将所有Session失效日期小于当前日期的数据删除
>
> ​				request.session.clear_expired()
>
> 检查 用户session的随机字符串 在数据库中是否
>
> ​				request.session.exists("session_key")
>
> 删除当前用户的所有Session数据
>
> ​				request.session.delete("session_key")
>
> ​				request.session.set_expiry(value)
> ​    * 如果value是个整数，session会在些秒数后失效。
> ​        * 如果value是个datatime或timedelta，session就会在这个时间后失效。
> ​        * 如果value是0,用户关闭浏览器session就会失效。
> ​        * 如果value是None,session会依赖全局session失效策略。

#### session配置

connkie实在后面的参数中，session的配置在setings中。

Django默认支持Session，并且默认是将Session数据存储在数据库中，即：django_session 表中。

#### 配置 settings.py

```python
  = 'django.contrib.sessions.backends.db'   # 引擎（默认）
   
SESSION_COOKIE_NAME ＝ "sessionid"               # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串（默认）
SESSION_COOKIE_PATH ＝ "/"                               # Session的cookie保存的路径（默认）
SESSION_COOKIE_DOMAIN = None                             # Session的cookie保存的域名（默认）
SESSION_COOKIE_SECURE = False                            # 是否Https传输cookie（默认）
SESSION_COOKIE_HTTPONLY = True                           # 是否Session的cookie只支持http传输（默认）
SESSION_COOKIE_AGE = 1209600                             # Session的cookie失效日期（2周）（默认）(秒)
SESSION_EXPIRE_AT_BROWSER_CLOSE = False                  # 是否关闭浏览器使得Session过期（默认）
SESSION_SAVE_EVERY_REQUEST = False                       # 是否每次请求都保存Session，默认修改之后才保存（默认）
```

## auth

Django 用户认证（Auth）组件

Django 用户认证（Auth）组件一般用在用户的登录注册上，用于判断当前的用户是否合法，并跳转到登陆成功或失败页面。

Django 用户认证（Auth）组件需要导入 auth 模块



## 中间件

中间件顾名思义，是介于request与response处理之间的一道处理过程，相对比较轻量级，并且在全局上改变django的输入与输出。因为改变的是全局，所以需要谨慎实用，用不好会影响到性能。

Django默认的`Middleware`：

每一个中间件都有具体的功能。

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

#### 自定义中间件

这四较为常用

```python
from django.utils.deprecation import MiddlewareMixin
```

```python
process_request

process_view

process_exception

process_response
```

#### process_request和process_response

```python
"""
process_request  一般不需要返回值。如果添加返回值会将视图函数中的返回值修改
process_response 需要返回值
"""

class M1(MiddlewareMixin):
    def process_request(self,request):
        print("M1的process_request.....这是我自己设立的中间件。")
		# return HttpResponse('ASDASASD')
    def process_response(self,request,response):
        print("Md2返回")
        return response

class M2(MiddlewareMixin):
    def process_request(self,request):
        print("M2的process_request.....这是我自己设立的中间件。")
    def process_response(self,request,response):
        print("Md2返回")
        return response
```

在settings中建立

```python
MIDDLEWARE = [
    'app05.my_middiewares.M1',
    'app05.my_middiewares.M2',
]
```

#### 结果

```python
"输出"
M1的process_request.....这是我自己设立的中间件。
M2的process_request.....这是我自己设立的中间件。
Md2返回
Md2返回
```

![图一](https://s2.loli.net/2022/08/05/GSN4z3kCEUqlRMo.png)

### process_view

```python
from django.utils.deprecation import MiddlewareMixin
class M1(MiddlewareMixin):
    def process_request(self,request):
        print("M1的process_request.....这是我自己设立的中间件。")

    def process_response(self,request,response):
        print("Md2返回")
        return response
    def process_view(self, request, callback, callback_args, callback_kwargs):
        print("1111111process_view")
        # 一般不加
        # return HttpResponse("hello")

class M2(MiddlewareMixin):
    def process_request(self,request):
        print("M2的process_request.....这是我自己设立的中间件。")
    def process_response(self,request,response):
        print("Md2返回")
        return response
    def process_view(self, request, callback, callback_args, callback_kwargs):
        print("2222222process_view")
```

运行顺序

![图二](https://s2.loli.net/2022/08/05/V5xAwhC3FaTEiqt.png)

```python
"输出"

M1的process_request.....这是我自己设立的中间件。
M2的process_request.....这是我自己设立的中间件。
1111111process_view
2222222process_view
Md2返回
Md2返回
```

注意：process_view如果有返回值，会越过其他的process_view以及视图函数，但是所有的process_response都还会执行。

```python
"输出"
M1的process_request.....这是我自己设立的中间件。
M2的process_request.....这是我自己设立的中间件。
1111111process_view
Md2返回
Md2返回
"""
并且在网页上也会显示hello
"""
```

### process_exception

当程序报错时显示，return HttpResponse(exception)，网页不会有黄框，就将错误信息写在网页上。

![图三](https://s2.loli.net/2022/08/05/8ikMJWLYBtGxCT4.png)

```python
from django.shortcuts import render, HttpResponse, redirect
from django.utils.deprecation import MiddlewareMixin


class M1(MiddlewareMixin):
    def process_request(self, request):
        print("M1的process_request.....这是我自己设立的中间件。")

    def process_response(self, request, response):
        print("Md2返回")
        return response

    def process_view(self, request, callback, callback_args, callback_kwargs):
        print("1111111process_view")

    def process_exception(self,request,exception):
        print("md1 process_exception...")



class M2(MiddlewareMixin):
    def process_request(self, request):
        print("M2的process_request.....这是我自己设立的中间件。")

    def process_response(self, request, response):
        print("Md2返回")
        return response

    def process_view(self, request, callback, callback_args, callback_kwargs):
        print("222222process_view")

    def process_exception(self,request,exception):
        print("md2 process_exception...")
        # return HttpResponse(exception)
```

输出

```python
输出
# 程序有错误时
M1的process_request.....这是我自己设立的中间件。
M2的process_request.....这是我自己设立的中间件。
1111111process_view
222222process_view
md2 process_exception...
md1 process_exception...
Md2返回
Md2返回
# 程序没有错误时
M1的process_request.....这是我自己设立的中间件。
M2的process_request.....这是我自己设立的中间件。
1111111process_view
222222process_view
Md2返回
Md2返回
```

















