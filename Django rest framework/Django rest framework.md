drf用于django的前后端分离的

# 1.api接口

应用程序编程接口（Application Programming Interface，API接口），就是应用程序对外提供了一个操作数据的入口，这个入口可以是一个函数或类方法，也可以是一个url地址或者一个网络地址。当客户端调用这个入口，应用程序则会执行对应代码操作，给客户端完成相对应的功能。

当然，api接口在工作中是比较常见的开发内容，有时候，我们会调用其他人编写的api接口，有时候，我们也需要提供api接口给其他人操作。由此就会带来一个问题，api接口往往都是一个函数、类方法、或者url或其他网络地址，不断是哪一种，当api接口编写过程中，我们都要考虑一个问题就是这个接口应该怎么编写？接口怎么写的更加容易维护和清晰，这就需要大家在调用或者编写api接口的时候要有一个明确的编写规范！！！



为了在团队内部形成共识、防止个人习惯差异引起的混乱，我们都需要找到一种大家都觉得很好的接口实现规范，而且这种规范能够让后端写的接口，用途一目了然，减少客户端和服务端双方之间的合作成本。

目前市面上大部分公司开发人员使用的接口实现规范主要有：restful、RPC。

RPC（ Remote Procedure Call ）: 翻译成中文:远程过程调用[远程服务调用]. 从字面上理解就是访问/调用远程服务端提供的api接口。这种接口一般以服务或者过程式代码提供。

+ 服务端提供一个**唯一的访问入口地址**：http://api.xxx.com/ 或 http://www.xx.com/api 或者基于其他协议的地址

+ 客户端请求服务端的时候，所有的操作都理解为**动作**(action)，一般web开发时，对应的就是HTTP请求的post请求

+ 通过**请求体**参数，指定要调用的接口名称和接口所需的参数

  action=get_all_student&class=301&sex=1

  m=get_all_student&sex=1&age=22

  command=100&sex=1&age=22

+ 基本上现有rpc的数据格式：protobuf（gRPC）、json、xml

rpc接口多了,对应函数名和参数就多了,前端在请求api接口时难找.对于年代久远的rpc服务端的代码也容易出现重复的接口

restful: 翻译成中文: 资源状态转换.(表征性状态转移)

+ 把服务端提供的所有的数据/文件都看成资源， 那么通过api接口请求数据的操作，本质上来说就是对资源的操作了.

  因此，restful中要求，我们把当前接口对外提供哪种资源进行操作，就把**资源的名称写在url地址**。

  /students/

  /avatars/

+ web开发中操作资源，最常见的最通用的无非就是增删查改，所以restful要求在地址栏中声明要操作的资源是什么。然后通过**http请求动词**来说明对该资源进行哪一种操作.

  POST http://www.xxx.com/api/students/   添加学生数据

  GET    http://www.xxx.com/api/students/   获取所有学生

  DELETE http://www.xxx.com/api/students/<pk>/   删除id=pk的一个学生

  PUT   http://www.xxx.com/api/students/<pk>/       修改一个学生的全部信息 [id,name,sex,age,]

  PATCH  http://www.xxx.com/api/students/<pk>/    修改一个学生的部分信息[age]

也就是说，我们仅需要通过url地址上的资源名称结合HTTP请求动作，就可以说明当前api接口的功能是什么了。

**restful是以资源为主的api接口规范，体现在地址上就是资源就是以名词表达。**

**rpc则以动作为主的api接口规范，体现在接口名称上往往附带操作数据的动作。**

# 2. RESTful API规范



REST全称是Representational State Transfer，中文意思是表述（编者注：通常译为表征）性状态转移。 它首次出现在2000年Roy Fielding的博士论文中。

RESTful是一种专门为Web 开发而定义API接口的设计风格，尤其适用于前后端分离的应用模式中。

而对于数据资源分别使用POST、DELETE、GET、UPDATE等请求动作来表达对数据的增删查改。

| GET      | /students      | 获取所有学生       |
| -------- | -------------- | ------------------ |
| 请求方法 | 请求地址       | 后端操作           |
| POST     | /students      | 增加学生           |
| GET      | /students/<pk> | 获取编号为pk的学生 |
| PUT      | /students/<pk> | 修改编号为pk的学生 |
| DELETE   | /students/<pk> | 删除编号为pk的学生 |

restful规范是一种通用的规范，不限制语言和开发框架的使用。事实上，我们可以使用任何一门语言，任何一个框架都可以实现符合restful规范的API接口。

参考文档：http://www.runoob.com/w3cnote/restful-architecture.html

## 幂等性

接口实现过程中，会存在**幂等性**。所谓幂等性是指代**客户端发起多次同样请求时，是否对于服务端里面的资源产生不同结果**。如果**多次请求**，服务端**结果**还是**一样**，则属于**幂等接口**，如果多次请求，服务端产生结果是不一样的，则属于非幂等接口。

| 请求方式  | 是否幂等 | 是否安全 |
| --------- | -------- | -------- |
| GET       | 幂等     | 安全     |
| POST      | 不幂等   | 不安全   |
| PUT/PATCH | 幂等     | 不安全   |
| DELETE    | 幂等     | 不安全   |

# 3. 序列化

api接口开发，最核心最常见的一个代码编写过程就是序列化，所谓序列化就是把**数据转换格式**。常见的序列化方式：

json，pickle，base64，struct，….

序列化可以分两个阶段：

**序列化**： 把我们识别的数据转换成指定的格式提供给别人。

例如：我们在django中获取到的数据默认是模型对象，但是模型对象数据无法直接提供给前端或别的平台使用，所以我们需要把数据进行序列化，变成字符串或者json数据，提供给别人。

**反序列化**：把别人提供的数据转换/还原成我们需要的格式。

例如：前端js提供过来的json数据，对于python而言json就是字符串，我们需要进行反序列化换成字典，然后接着字典再进行转换成模型对象，这样我们才能把数据保存到数据库中。



# 4. Django Rest Framework

核心思想: 大量缩减编写api接口的代码

Django REST framework是一个建立在Django基础之上的Web 应用开发框架，可以快速的开发REST API接口应用。在REST framework中，提供了序列化器Serialzier的定义，可以帮助我们简化序列化与反序列化的过程，不仅如此，还提供丰富的类视图、扩展类、视图集来简化视图的编写工作。REST framework还提供了认证、权限、限流、过滤、分页、接口文档等功能支持。REST framework还提供了一个调试API接口 的Web可视化界面来方便查看测试接口。

中文文档：https://q1mi.github.io/Django-REST-framework-documentation/#django-rest-framework

github: https://github.com/encode/django-rest-framework/tree/master

### 特点

- 提供了定义序列化器Serializer的方法，可以快速根据 Django ORM 或者其它库自动序列化/反序列化；
- 提供了丰富的类视图、Mixin扩展类，简化视图的编写；
- 丰富的定制层级：函数视图、类视图、视图集合到自动生成 API，满足各种需要；
- 多种身份认证和权限认证方式的支持；[jwt]
- 内置了限流系统；
- 直观的 API web 界面；【方便我们调试开发api接口】
- 可扩展性，插件丰富

# 5. 环境安装与配置

```bash
pip install djangorestframewor
```

DRF需要以下依赖：

- Python (3.5 以上)
- Django (2.2 以上)

**DRF是以Django子应用的方式提供的，所以我们可以直接利用已有的Django环境而无需从新创建。（若没有Django环境，需要先创建环境安装Django）**

#  6.添加rest_framework应用

在**settings.py**的**INSTALLED_APPS**中添加'rest_framework'。

```python
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

在项目中如果使用rest_framework框架实现API接口，主要有以下三个步骤：

- 将请求的数据（如JSON格式）转换为模型类对象
- 操作数据库
- 将模型类对象转换为响应的数据（如JSON格式）

# 6.创建序列化器-Serializer

## 6.1创建序列化器的常见参数

**常用字段类型**：

| 字段                    | 字段构造方式                                                 |
| ----------------------- | ------------------------------------------------------------ |
| **BooleanField**        | BooleanField()                                               |
| **NullBooleanField**    | NullBooleanField()                                           |
| **CharField**           | CharField(max_length=None, min_length=None, allow_blank=False, trim_whitespace(是否去掉两边空格)=True) |
| **EmailField**          | EmailField(max_length=None, min_length=None, allow_blank(是否为空)=False) |
| **RegexField**          | RegexField(regex, max_length=None, min_length=None, allow_blank=False) |
| **SlugField**           | SlugField(max*length=50, min_length=None, allow_blank=False) 正则字段，验证正则模式 [a-zA-Z0-9*-]+ |
| **URLField**            | URLField(max_length=200, min_length=None, allow_blank=False) |
| **UUIDField**           | UUIDField(format='hex_verbose')  format:  1) `'hex_verbose'` 如`"5ce0e9a5-5ffa-654b-cee0-1238041fb31a"`  2） `'hex'` 如 `"5ce0e9a55ffa654bcee01238041fb31a"`  3）`'int'` - 如: `"123456789012312313134124512351145145114"`  4）`'urn'` 如: `"urn:uuid:5ce0e9a5-5ffa-654b-cee0-1238041fb31a"` |
| **IPAddressField**      | IPAddressField(protocol='both', unpack_ipv4=False, **options) |
| **IntegerField**        | IntegerField(max_value=None, min_value=None)                 |
| **FloatField**          | FloatField(max_value=None, min_value=None)                   |
| **DecimalField**        | DecimalField(max_digits, decimal_places, coerce_to_string=None, max_value=None, min_value=None) max_digits: 最多位数 decimal_palces: 小数点位置 |
| **DateTimeField**       | DateTimeField(format=api_settings.DATETIME_FORMAT, input_formats=None) |
| **DateField**           | DateField(format=api_settings.DATE_FORMAT, input_formats=None) |
| **TimeField**           | TimeField(format=api_settings.TIME_FORMAT, input_formats=None) |
| **DurationField**       | DurationField()                                              |
| **ChoiceField**         | ChoiceField(choices) choices与Django的用法相同               |
| **MultipleChoiceField** | MultipleChoiceField(choices)                                 |
| **FileField**           | FileField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL) |
| **ImageField**          | ImageField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL) |
| **ListField**           | ListField(child=, min_length=None, max_length=None)          |
| **DictField**           | DictField(child=)                                            |

**选项参数：**

| 参数名称            | 作用             |
| ------------------- | ---------------- |
| **max_length**      | 最大长度         |
| **min_lenght**      | 最小长度         |
| **allow_blank**     | 是否允许为空     |
| **trim_whitespace** | 是否截断空白字符 |
| **max_value**       | 最小值           |
| **min_value**       | 最大值           |

通用参数：

| 参数名称           | 说明                                          |
| ------------------ | --------------------------------------------- |
| **read_only**      | 表明该字段仅用于序列化输出，默认False         |
| **write_only**     | 表明该字段仅用于反序列化输入，默认False       |
| **required**       | 表明该字段在反序列化时必须输入，默认True      |
| **default**        | 反序列化时使用的默认值                        |
| **allow_null**     | 表明该字段是否允许传入None，默认False         |
| **validators**     | 该字段使用的验证器                            |
| **error_messages** | 包含错误编号与错误信息的字典                  |
| **label**          | 用于HTML展示API页面时，显示的字段名称         |
| **help_text**      | 用于HTML展示API页面时，显示的字段帮助提示信息 |

## 6.2序列化

作用：

```
1. 序列化,序列化器会把模型对象转换成字典,经过response以后变成json字符串
2. 反序列化,把客户端发送过来的数据,经过request以后变成字典,序列化器可以把字典转成模型
3. 反序列化,完成数据校验功能
```

作用：

例如，在django项目中创建学生子应用。

```python
python manage.py startapp students
```

配置这个students应用和数据库

`models`
在数据库中手动添加数据

```python
from django.db import models


# Create your models here.
class Student(models.Model):
    """学生信息"""
    name = models.CharField(max_length=255, verbose_name="姓名")
    sex = models.BooleanField(default=1, verbose_name="性别")
    age = models.IntegerField(verbose_name="年龄")
    classmate = models.CharField(max_length=5, verbose_name="班级编号")
    description = models.TextField(max_length=1000, verbose_name="个性签名")

    class Meta:
        db_table = "tb_students"
        verbose_name = "学生"
        verbose_name_plural = verbose_name
```

在syudents应用目录中新建serializers.py用于保存该应用的序列化器。
序列化和反序列化可以在一个类下。

`serializers`      

```python
from rest_framework import serializers
"""
serializers 是drf提供给开发者调用的序列化器模块类
里面是声明了所有的可用序列化器的基类
Serializer         序列化器基类，drf中所有的序列化器类都必须继承Serializer
ModelSerializer    模型序列化器类，是序列化器基类的子类，在工作中除了Serializer基类以外，最长用的序列化器类基类
"""
class Stuend1Serializer(serializers.Serializer):
    """学生信息序列化器"""
    # 1. 转换的字段声明
    # 字段 = serializers.字段类型(选项=选项值)
    id=serializers.IntegerField()
    name=serializers.CharField()
    sex=serializers.BooleanField()
    age=serializers.IntegerField()
    description=serializers.CharField()
    # 2. 如果序列化器集成的是ModelSerializer，则需要声明调用的模型信息

    # 3. 验证代码

    # 4. 编写添加和更新模型的代码
```

`views`

```python
import json
from django.shortcuts import render
from django.views import View
from .serializers import Stuend1Serializer
from django.http.response import JsonResponse
# Create your views here.
from .models import Student

class StudentView(View):
    def get(self,request):
        """序列化器-序列化阶段的调用"""
        """序列化多个转换对象 many=True """
        # 1.获取数据集
        student_list=Student.objects.all()
        # 2.实例化序列化器，得到序列化对象 序列化多个需要many=True，一个不用
        serializer=Stuend1Serializer(instance=student_list,many=True)
        # 3.调用序列化对象的data属性方法获取转换后的数据
        data=serializer.data
        # 4.响应数据
        return JsonResponse(data=data,status=200,safe=False,json_dumps_params={"ensure_ascii":False})

    def get(self,request):
        """序列化器-序列化阶段的调用"""
        """序列化一个转换对象"""
        # 1.获取数据集
        student_list=Student.objects.first()
        # 2.实例化序列化器，得到序列化对象 序列化多个需要many=True，一个不用
        serializer=Stuend1Serializer(instance=student_list)
        # 3.调用序列化对象的data属性方法获取转换后的数据
        data=serializer.data
        # 4.响应数据
        return JsonResponse(data=data,status=200,safe=False,json_dumps_params={"ensure_ascii":False})
```

`usrls`

在总路由配置

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('sers/',include("sers.urls"))
]
```

在局部配置

```python
from django.urls import path
from . import views

urlpatterns=[
    path('student',views.StudentView.as_view()),
]
```

## 6.3反序列化

在syudents应用目录中新建serializers.py用于保存该应用的序列化器。
序列化和反序列化可以在一个类下。

`serializers`    

```python
class Stuend1Serializer(serializers.Serializer):
    """学生信息序列化器"""
    # 1. 转换的字段声明
    # 字段 = serializers.字段类型(选项=选项值)
    id=serializers.IntegerField(read_only=True)   # read_only=True 在客户端提交数据(反序列化阶段不会要求字段有值)
    name=serializers.CharField(required=True)     # required=True 反序列化比填
    sex=serializers.BooleanField(default=True)    # default=True  反序列化阶段客户端没有提交，则默认为Ture
    age=serializers.IntegerField(max_value=100,min_value=0,error_messages={"min_value":"The age filed must be 0<=age<=100"}) # age在反序列化阶段必须是 0<=age<=100    error_messages=报错时的提示
    description=serializers.CharField(allow_null=True,allow_blank=True)   # 允许客户端不填写内容(None)，或者值为""
    # 2. 如果序列化器集成的是ModelSerializer，则需要声明调用的模型信息

    # 3. 验证代码
    def validate_name(self, attrs):
        """验证单个字段
        方法名的格式必须以 validate_<字段名>为名称",否则序列化器不识别!
        validate开头的方法,会自动被is_valid调用
        attrs就是客户端传来的name数据"""
        print(f"name:{attrs}")
        if attrs in ["python","django"]:
            raise serializers.ValidationError(detail="学生姓名名不能是python或者django",code="validate_name")
        return attrs  # 没有写返回值时,name会时None.
    def validate(self,data):
        """
        验证客户端的所有数据,
        类似:密码和确认密码,只能在validate方法中校验
        validate是固定的方法名
        参数:data,是在序列化实例化的data选项数据
        """
        if data["name"]=="shenzeaho" and data["age"]== 21:
            raise serializers.ValidationError(detail="就是不让你加入",code="validate")
        return data
    # 4. 编写添加和更新模型的代码
```

`views`

```python
class StudentView1(View):    
    def get3(self,request):
        """反序列化-采用字段选项来验证数据"""
        # 1.接受客户端提交的数据
        # 模拟来自客户端的数据
        data={
            "name":"shenzehao",
            "age":12,
            "sex":True,
            "classname":"201",
            "description":"这个人很懒",
        }
        # data=json.dumps(request.body)
        # 2.实例化序列化器，获取实例化对象
        serializer=Stuend1Serializer(data=data)
        # 3.调用序列化器进行数据验证。
        ret=serializer.is_valid()
        # 4.验证数据获取验证以后的结果
        if ret:
            print(serializer.validated_data)
            return JsonResponse(dict(serializer.validated_data))
        else:
            print(serializer.errors)
            return JsonResponse(dict(serializer.errors))
        # 5.操作数据库
        # 6.返回数据
        # return JsonResponse({})

    def get(self,request):
        """反序列化-采用字段选项来验证数据"""
        # 1.接受客户端提交的数据
        # data=json.dumps(request.body)
        # 模拟来自客户端的数据
        data={
            "name":"shenzeaho",
            "age":22,
            "sex":True,
            "classname":"201",
            "description":"这个人很懒",
        }
        # 2.实例化序列化器，获取实例化对象
        serializer=Stuend1Serializer(data=data)
        # 3.调用序列化器进行数据验证。
        serializer.is_valid(raise_exception=True)  # 页面抛出异常(直接出黄色框)不在向下运行.
        # 4.验证数据获取验证以后的结果
        print(serializer.validated_data)
        # 5.操作数据库
        # 6.返回数据
        return JsonResponse({})
```

### 6.3.1数据的修改和添加

#### 添加数据

路由和上面的序列化一样

`serializers`   

```python
def create(self, validated_data):
    """添加数据
    添加数据操作，方法名固定为create，固定参数validated_data就是验证成功以后的结果"""
    # validated_data就是前端传入的数据。
    student = Student.objects.create(**validated_data)
    return student
```

`views`

```python
class StudentView1(View):
    def get5(self,request):
        """反序列化-验证成功后让数据入库"""
        # 1.接受客户端提交的数据
        # 模拟来自客户端的数据
        data={
            "name":"shenzeaho",
            "age":22,
            "sex":True,
            "classmate":"201",
            "description":"这个人很懒",
        }
        # data=json.dumps(request.body)
        # 2.实例化序列化器，获取实例化对象
        serializer=Stuend1Serializer(data=data)
        # 3.调用序列化器进行数据验证。
        serializer.is_valid(raise_exception=True)  # 页面抛出异常(直接出黄色框)不在向下运行.
        # 4.验证数据获取验证以后的结果，操作数据库
        serializer.save()   # 会根据实例化序列化器的时候，石头传入instance属性来自动调用create或者update方法，传入instance属性，自动调用update方法，没有传入instacne，会自动调用create方法。
        # 5.返回数据
        return JsonResponse(data=serializer.validated_data,status=201,json_dumps_params={"ensure_ascii":False})
```

#### 修改数据

`views`

```python
def get(self,request):
    """反系列化--验证数据以后更新数据库"""
    # 1.根据客户端访问的url地址，获取pk值
    pk=1
    try:
        student=Student.objects.get(pk=pk)
    except:
        return JsonResponse({"errors":"当前学生不存在！"},status=400)
    # 2.接受客户端提交的数据
    # 模拟来自客户端的数据
    data={
        "name":"cao",
        "age":22,
        "sex":True,
        "classmate":203,
        "description":"这个人很懒",
    }
    # 3.修改操作中的实例化序列化对象
    serializer=Stuend1Serializer(instance=student,data=data)
    # 4.验证数据
    serializer.is_valid(raise_exception=True)
    # 5.入库
    serializer.save() # 会根据实例化序列化器的时候，石头传入instance属性来自动调用create或者update方法，传入instance属性，自动调用update方法，没有传入instacne，会自动调用create方法。
    # 返回结果
    return JsonResponse(serializer.data,status=201)
```

`serializers`

```python
def update(self, instance, validated_data):
    """
    更新数据操作
    方法名是固定为update
    固定参数instance，实例化序列化器对象时，必须传入模型对象
    固定参数validated_data就是验证成功以后的结果。
    更新数据时就自动实现从字典变成模型对象的过程
    """
    print(instance)  # 模型类对象
    print(validated_data)   # 修改后的全部数据
    instance.name = validated_data["name"]
    instance.age = validated_data["age"]
    instance.sex = validated_data["sex"]
    instance.classmate = validated_data["classmate"]
    instance.description = validated_data["description"]
    instance.save()  # 调用模型对象的save方法，和视图中的serialzier.save()不是同一个类的方法
    return instance
```

#### 6.3.2 附加说明

1） 在对序列化器进行save()保存时，可以额外传递数据，这些数据可以在create()和update()中的validated_data参数获取到

```python
# request.user 是django中记录当前登录用户的模型对象
serializer.save(owner=request.user)
```

2）默认序列化器必须传递所有required的字段，否则会抛出验证异常。但是我们可以使用partial参数来允许部分字段更新

```python
# Update `comment` with partial data
serializer = CommentSerializer(comment, data={'content': u'foo bar'}, partial=True)
```

# 7.模型类序列化器-ModelSerializer

ModelSerializer与常规的Serializer相同，但提供了：

- 基于模型类自动生成一系列字段
- 基于模型类自动为Serializer生成validators，比如unique_together
- 包含默认的create()和update()的实现

`serializers`

```python
class StuendModelSerializer(serializers.ModelSerializer):
    """学生信息序列化器"""
    # 1.转换字段声明
    nickname = serializers.CharField(read_only=True)

    # 2. 如果序列化器集成的是ModelSerializer，则需要声明调用的模型信息。
    # 必须给Mate声明两个属性
    class Meta():
         # model = 模型             # 必填
        # fields= 字段列表         # 必填  `__all__`表名包含所有字段
        # read_only_fields = []   # 选填，只读字段列表，表示设置的字段只会在序列化阶段采用。
        # extra_kwargs={          # 选填  字段额外选项声明。
        #     "字段名":{
        #         "选项":选项值
        #     }
        # }
        # exclude = ('image',)    和fields的意思相反(使用exclude可以明确排除掉哪些字段)
        model = Student
        fields = ["id", "name", "age", "sex", "nickname", "classmate","description"]
        extra_kwargs = {
            "age": {
                "max_value": 20,
                "min_value": 5,
                "error_messages": {
                    "min_value": "年龄最小值必须大于等于5",
                    "max_value": "年龄最大值不能超过20"
                }
            }
        }
        read_only_fields=["id",""]
    # def create(self, validated_data):
    #     """ModelSerializer的源码中有添加数据的代码，
    #     但是我们可以重写create的方法"""
    #     pass
    # def update(self, instance, validated_data):
    #     """ModelSerializer的源码中有更新数据的代码，
    #     但是我们可以重写update的方法"""
    #     pass
```

views中的代码，导包后将Stuend1Serializer变成StuendModelSerializer其他的不变.

# 8.http请求响应

代码：

`views`

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

# Create your views here.


class StudentAPIView(APIView):
    def get(self, request):
        print("drf",request.data)
# 这个的request，是属于drf单独声明的请求处理对象，与django提供的HttpRequset不是同一个，甚至可以说毫无关系。
        print(request.data)
        print(request.query_params)
        print(request._request)
        # 获取请求体
        return Response({"msg": "OK"})
    def post(self, request):
        """添加数据
        获取请求体数据"""

        return Response({"msg":"OK"},status=status.HTTP_200_OK)
```

路由

总路由

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
path('req/', include("req.urls")),]
```

局部路由

```python
from django.urls import path
from . import views
urlpatterns = [
    path("students/",views.StudentAPIView.as_view())
]
```

记住在settings中配置

```python
INSTALLED_APPS = [
	"sers",
    "req",
    "rest_framework"

]
```



内容协商：drf在django原有的基础上，新增了一个request对象继承到了APIVIew视图类，并在django原有的HttpResponse响应类的基础上实现了一个子类rest_framework.response.Response响应类。这两个类，都是基于内容协商来完成数据的格式转换的。

request->parser解析类->识别客户端请求头中的Content-Type来完成数据转换成->类字典(QueryDict，字典的子类)

response->renderer渲染类->识别客户端请求头的"Accept"来提取客户端期望的返回数据格式，-> 转换成客户端的期望格式数据

![image-20211020101829885](https://s2.loli.net/2022/08/13/JlxmuPGNK4Hhbfi.png)

## 8.1 Request

```python
from rest_framework.views import APIView
```

framework提供的扩展了HttpRequest类的**Request**类的对象。

REST framework 提供了**Parser**解析器，在接收到请求后会自动根据Content-Type指明的请求数据类型（如JSON、表单等）将请求数据进行parse解析，解析为类字典[QueryDict]对象保存到**Request**对象中。

**Request对象的数据是自动根据前端发送数据的格式进行解析之后的结果。**

无论前端发送的哪种格式的数据，我们都可以以统一的方式读取数据。

## 8.2 Request常用属性

#### 1）.data

`request.data` 返回解析之后的**请求体**数据。类似于Django中标准的`request.POST`和 `request.FILES`属性，但提供如下特性：

- 包含了解析之后的文件和非文件数据
- 包含了对POST、PUT、PATCH请求方式解析后的数据
- 利用了REST framework的parser解析器，不仅支持表单类型数据，也支持JSON数据

#### 2）.query_params

query_params，查询参数，也叫查询字符串(query string )

`request.query_params`与Django标准的`request.GET`相同，只是更换了更正确的名称而已。

#### 3）request._request 

获取django封装的Request对象

## 8.3 Response

```python
from rest_framework.response import Response
```

REST framework提供了一个响应类`Response`，使用该类构造响应对象时，响应的具体数据内容会被转换（renderer渲染器）成符合前端需求的类型。

REST framework提供了`Renderer` 渲染器，用来根据请求头中的`Accept`（接收数据类型声明）来自动转换响应数据到对应格式。如果前端请求中未进行声明Accept，则会采用Content-Type方式处理响应数据，我们可以通过配置来修改默认响应格式。

可以在**rest_framework.settings**查找所有的drf默认配置项

```python
REST_FRAMEWORK = {
    'DEFAULT_RENDERER_CLASSES': (  # 默认响应渲染类
        'rest_framework.renderers.JSONRenderer',  # json渲染器，返回json数据
        'rest_framework.renderers.BrowsableAPIRenderer',  # 浏览器API渲染器，返回调试界面
    )
}
```

#### 构造方式

```python
Response(data, status=None, template_name=None, headers=None, content_type=None)
```

drf的响应处理类和请求处理类不一样，Response就是django的HttpResponse响应处理类的子类。

`data`数据不要是render处理之后的数据，只需传递python的内建类型数据即可，REST framework会使用`renderer`渲染器处理`data`。

`data`不能是复杂结构的数据，如Django的模型类对象，对于这样的数据我们可以使用`Serializer`序列化器序列化处理后（转为了Python字典类型）再传递给`data`参数。

参数说明：

- `data`: 为响应准备的序列化处理后的数据；
- `status`: 状态码，默认200；
- `template_name`: 模板名称，如果使用`HTMLRenderer` 时需指明；
- `headers`: 用于存放响应头信息的字典；
- `content_type`: 响应数据的Content-Type，通常此参数无需传递，REST framework会根据前端所需类型数据来设置该参数。

####  response对象的属性

工作少用，

##### 1）.data

传给response对象的序列化后，但尚未render处理的数据

##### 2）.status_code

状态码的数字

##### 3）.content

经过render处理后的响应数据

####  状态码

```python
from rest_framework import status
```

##### 1）信息告知 - 1xx

```python
HTTP_100_CONTINUE
HTTP_101_SWITCHING_PROTOCOLS
```

##### 2）成功 - 2xx

```python
HTTP_200_OK
HTTP_201_CREATED
HTTP_202_ACCEPTED
HTTP_203_NON_AUTHORITATIVE_INFORMATION
HTTP_204_NO_CONTENT
HTTP_205_RESET_CONTENT
HTTP_206_PARTIAL_CONTENT
HTTP_207_MULTI_STATUS
```

##### 3）重定向 - 3xx

```python
HTTP_300_MULTIPLE_CHOICES
HTTP_301_MOVED_PERMANENTLY
HTTP_302_FOUND
HTTP_303_SEE_OTHER
HTTP_304_NOT_MODIFIED
HTTP_305_USE_PROXY
HTTP_306_RESERVED
HTTP_307_TEMPORARY_REDIRECT
```

##### 4）客户端错误 - 4xx

##### 

```python
HTTP_400_BAD_REQUEST
HTTP_401_UNAUTHORIZED
HTTP_402_PAYMENT_REQUIRED
HTTP_403_FORBIDDEN
HTTP_404_NOT_FOUND
HTTP_405_METHOD_NOT_ALLOWED
HTTP_406_NOT_ACCEPTABLE
HTTP_407_PROXY_AUTHENTICATION_REQUIRED
HTTP_408_REQUEST_TIMEOUT
HTTP_409_CONFLICT
HTTP_410_GONE
HTTP_411_LENGTH_REQUIRED
HTTP_412_PRECONDITION_FAILED
HTTP_413_REQUEST_ENTITY_TOO_LARGE
HTTP_414_REQUEST_URI_TOO_LONG
HTTP_415_UNSUPPORTED_MEDIA_TYPE
HTTP_416_REQUESTED_RANGE_NOT_SATISFIABLE
HTTP_417_EXPECTATION_FAILED
HTTP_422_UNPROCESSABLE_ENTITY
HTTP_423_LOCKED
HTTP_424_FAILED_DEPENDENCY
HTTP_428_PRECONDITION_REQUIRED
HTTP_429_TOO_MANY_REQUESTS
HTTP_431_REQUEST_HEADER_FIELDS_TOO_LARGE
HTTP_451_UNAVAILABLE_FOR_LEGAL_REASONS
```

##### 5）服务器错误 - 5xx

```python
HTTP_500_INTERNAL_SERVER_ERROR
HTTP_501_NOT_IMPLEMENTED
HTTP_502_BAD_GATEWAY
HTTP_503_SERVICE_UNAVAILABLE
HTTP_504_GATEWAY_TIMEOUT
HTTP_505_HTTP_VERSION_NOT_SUPPORTED
HTTP_507_INSUFFICIENT_STORAGE
HTTP_511_NETWORK_AUTHENTICATION_REQUIRED
```





