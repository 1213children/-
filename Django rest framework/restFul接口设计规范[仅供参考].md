## 1. 域名

应该尽量将API部署在专用域名之下。

```http
https://api.example.com  
```

如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下。

```http
https://www.example.org/api/
```



## 2. 版本（Versioning）

应该将API的版本号放入URL。

```http
http://www.example.com/app/1.0/foo

http://www.example.com/app/1.1/foo

http://www.example.com/app/2.0/foo
```

另一种做法是，将版本号放在HTTP头信息中，但不如放入URL方便和直观。[Github](https://developer.github.com/v3/media/#request-specific-version)就采用了这种做法。

因为不同的版本，可以理解成同一种资源的不同表现形式，所以应该采用同一个URL。版本号可以在HTTP请求头信息的Accept字段中进行区分（参见[Versioning REST Services](http://www.informit.com/articles/article.aspx?p=1566460)）：

```http
Accept: vnd.example-com.foo+json; version=1.0

Accept: vnd.example-com.foo+json; version=1.1

Accept: vnd.example-com.foo+json; version=2.0
```



## 3. 路径（Endpoint）

路径又称"终点"（endpoint），表示API的具体网址，每个网址代表一种资源（resource）

**(1) 资源作为网址，只能有名词，不能有动词，而且所用的名词往往与数据库的表名对应。**

举例来说，以下是不好的例子:

```http
/getProducts
/listOrders
/retreiveClientByOrder?orderId=1
```

对于一个简洁结构，你应该始终用名词。 此外，利用的HTTP方法可以分离网址中的资源名称的操作。

```http
GET /products ：将返回所有产品清单
POST /products ：将产品新建到集合
GET /products/4 ：将获取产品 4
PATCH（或）PUT /products/4 ：将更新产品 4
```

**(2) API中的名词应该使用复数。无论子资源或者所有资源。**

举例来说，获取产品的API可以这样定义

```http
获取单个产品：http://127.0.0.1:8080/AppName/rest/products/1
获取所有产品: http://127.0.0.1:8080/AppName/rest/products
```



## 3. HTTP动词

对于资源的具体操作类型，由HTTP动词表示。

常用的HTTP动词有下面四个（括号里是对应的SQL命令）。

- GET（SELECT）：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
- DELETE（DELETE）：从服务器删除资源。

```
CURD  Create、Update、Read、Delete 增删查改，这四个数据库的常用操作
```



还有三个不常用的HTTP动词。

- PATCH（UPDATE）：在服务器更新(更新)资源（客户端提供改变的属性）。
- HEAD：获取资源的元数据。
- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

下面是一些例子。

```http
GET /zoos：列出所有动物园
POST /zoos：新建一个动物园（上传文件）
GET /zoos/ID：获取某个指定动物园的信息
PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
DELETE /zoos/ID：删除某个动物园
GET /zoos/ID/animals：列出某个指定动物园的所有动物
DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物
```



## 4. 过滤信息（Filtering）

如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。

下面是一些常见的参数。query_string 查询字符串,地址栏后面问号后面的数据,格式: name=xx&sss=xxx

```
完整的URL地址格式：
协议://域名(IP):端口号/students/?查询字符串#锚点

查询字符串: query_srting
格式与请求体类型：
    username=xiaoming&class=301
```



```http
?limit=10：指定返回记录的数量
?offset=10：指定返回记录的开始位置。
?page=2&per_page=100：指定第几页，以及每页的记录数。
?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
?animal_type_id=1：指定筛选条件
```

参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，

GET /zoos/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。



## 6. 状态码（Status Codes）

```
1xx 表示当前本次请求还是持续，没结束
2xx 表示当前本次请求成功/完成了
3xx 表示当前本次请求成功，但是服务器进行代理操作/重定向
4xx 表示当前本次请求失败，主要是客户端发生了错误
5xx 表示当前本次请求失败，主要是服务器发生了错误
```



服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

> - 101 Switching Protocols - [*]  协议进行中，一般在http升级到websocket协议的时候就看到
> - 200 OK - [GET]：服务器成功返回用户请求的数据
> - 201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
> - 202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
> - 204 NO CONTENT - [DELETE]：用户删除数据成功。
> - 301 Moved Permanently - [*]: 永久重定向
> - 302 Move Temporarily - [*]: 临时重定向
> - 304 Not Modified - [*]: 命中缓存
> - 400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作
> - 401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
> - 403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
> - 404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
> - 406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
> - 410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
> - 422 Unprocesable entity - [POST/PUT/PATCH] 当创建/修改一个对象时，发生一个验证错误。
> - 500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
> - 507 Insufficient Storage [POST/PUT/PATCH] 数据存储出错，往往数据库操作错误出错，服务器就返回这个

状态码的完全列表参见[这里](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)或[这里](https://zh.wikipedia.org/wiki/HTTP%E7%8A%B6%E6%80%81%E7%A0%81)。



## 7. 错误处理（Error handling）

如果状态码是4xx或者5xx，服务器就应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可。一般格式都是json格式。

```json
{
    error: "Invalid API key"
}
```



## 8. 返回结果

restful针对不同操作，服务器向用户返回的结果应该符合以下规范。

- GET /collections：返回资源对象的**列表**（数组）
- GET /collections/ID：返回单个资源**字典**(json)
- POST /collections：返回**新生成**的资源字典(json)
- PUT /collections/ID：返回修改后的资源字典(json)
- DELETE /collections/ID：返回一个空文档(空字符串，空字典)



## 9. 超媒体（Hypermedia API）

RESTful API最好做到Hypermedia（即返回结果中提供链接，连向其他API方法），使得用户不查文档，也知道下一步应该做什么。

比如，Github的API就是这种设计，访问[api.github.com](https://api.github.com/)会得到一个所有可用API的网址列表。

```json
{
"current_user_url": "https://api.github.com/user",
"authorizations_url": "https://api.github.com/authorizations",
// ...
}
```

从上面可以看到，如果想获取当前用户的信息，应该去访问[api.github.com/user](https://api.github.com/user)，然后就得到了下面结果。

```json
{
  "message": "Requires authentication",
  "documentation_url": "https://developer.github.com/v3"
}
```

上面代码表示，服务器给出了提示信息，以及文档的网址。



## 10. 其他

服务器返回的数据格式，应该尽量使用JSON，避免使用XML。

XML（eXtend Markup Language，可扩展标记语言）是W3C为了替换HTML研发出来的，但是现在很明显失败了。

语法：

```xml
1. xml常用场景：配置文件 微信开发  小程序  安卓

2. xml就是网页文档，文件以 .xml结尾

3. xml文档必须以文档声明开头，所有的xml文档内容都必须写在自定义的根标签内，xml文档有且只有1个根标签。
<xml version="1.0" charset="utf-8">
<根标签>
    ..... xml文档内容
</根标签>

4. xml里面也是由标签组成页面的。标签分单标签和双标签。其中，
   单标签写法： <标签名/>
   双标签写法： <标签名></标签名>

5. xml标签名除了文档声明，其他标签名和属性全部是开发人员自己自定义的。

6. 标签有0个或多个属性，属性必须有属性值。而且属性值必须使用引号圈起来。
<标签名 属性名="属性值" .../>
<标签名 属性名="属性值" ...>标签内容</标签名>
```

xml文档举例：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<student-list>
<!-- 	<student>
	   <name>小红</name>
	   <age>17</age>
	   <sex>女</sex>
	   <class>301</class>
	</student>
	<student>
	   <name>小红</name>
	   <age>17</age>
	   <sex>女</sex>
	   <class>301</class>
	</student> -->
	<student age="17" sex="男" class="301">小明</student>
	<student age="17" sex="男" class="301">小明</student>
	<student age="17" sex="男" class="301">小明</student>
	<student age="17" sex="男" class="301">小明</student>
	<student age="17" sex="男" class="301">小明</student>
</student-list>
```

json是目前市面上最流行的数据传输格式。JavaScript Object Notation    js对象表示法

语法：

```json
# 1. json文件以 .json结尾，一个json文件中只能保存一个json对象或者一个json数组
# 2. json中的属性类型：
  数组    []                       # python->列表
  对象    {}                       # python->字典
  数值    整型，浮点型，布尔值   
  字符串  双引号圈起来的文本内容
  null    空

# 3. 数组和对象的成员之间必须以英文逗号隔开，并且最后一个子成员后面不能出现逗号
# 4. json对象只能有属性不能出现方法，而且属性名必须以字符串格式来编写
```

举例1：

```json
{
    "name": "张三",
    "sex": true,
    "age": 38,
    "money": 88.5,
    "child":{
        "name": "张思",
        "sex": false,
        "age": 4
    },
    "lve":["code","TV","swimming"]
}
```

举例2：

```json
[
    {
        "name":"小明",
        "sex":true,
    },{
        "name":"小灰",
        "sex":true,
    }
]
```

使用xml和json表述一本书的内容信息，书的字段：

name   varchar

price    float

author  varchar

pub_data  date

word_total   int

xml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<book>
	<name>金瓶梅</name>
	<price>9999</price>
	<author>内容~~~适合多看</author>
	<pub_data>2020-9-8</pub_data>
	<word_total>999</word_total>
</book>
```

json

```
{
	"name":"三国演义",
	"price":1,
	"author":"诸葛亮草船借箭。。。",
	"pub_data":"2000-1-1",
	"word_total":50
}
```

```
http://www.xx.com:80/students?username=xiaoming&pwd=123

协议名://域名:端口/路径?查询字符串
```



课外补充：

```python
1. python关于操作xml：  lxml
2. python关于操作json： json, ujson, orjson(rust)
```









