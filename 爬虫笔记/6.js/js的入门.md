# js的入门

**程序是由上到下加载的!!!!**

## 一.js导入的方法

js可以写在head里，也可以body中。

### 1.第一种

在head里，也可以body中<script>写在这个标签里面</script>

### 2.第二种

外部脚本。<script src="myScript.js"></script>

### 3.第三种

```javascript
//第一种
<script>
    // alert("我叫沈泽昊")
</script>
//第二种
<script src="ii.js">
</script>
```



## 二.输出

JavaScript 能够以不同方式“显示”数据：

- 使用 `window.alert()` 写入警告框
- 使用 `document.write()` 写入 HTML 输出
- 使用 `innerHTML` 写入 HTML 元素
- 使用 `console.log()` 写入浏览器控制台

### innerHTML演示

```javascript
<!DOCTYPE html>
<html>
<meta charset="UTF-8">
<body>

<h1>我的第一张网页</h1>

<p>我的第一个段落</p>

<p id="demo">asdw</p>

<script>
 document.getElementById("demo").innerHTML = 5 + 26;
</script>

</body>
</html> 
```

### document.write()演示

```javascript
<!DOCTYPE html>
<html>
<meta charset="UTF-8">
<body>

<h1>我的第一张网页</h1>

<p>我的第一个段落</p>

<script>
document.write(5 + 6);
</script>

</body>
</html>
```

**注意：**在 HTML 文档完全加载后使用 `document.write()` 将*删除所有已有的 HTML* ：

这个主要document.write()方法仅用于测试

### window.alert()演示

提示框

```javascript
<!DOCTYPE html>
<html>
<body>

<h1>我的第一张网页</h1>

<p>我的第一个段落</p>

<script>
window.alert(5 + 6);
</script>

</body>
</html
```

### console.log()演示

在浏览器中，您可使用 `console.log()` 方法来显示数据。

请通过 F12 来激活浏览器控制台，并在菜单中选择“控制台”。（Console）

```javascript
<!DOCTYPE html>
<html>
<body>

<h1>我的第一张网页</h1>

<p>我的第一个段落</p>

<script>
console.log(5 + 6);
</script>

</body>
</html>
```

## 三.JavaScript 关键词

| 关键词        | 描述                                              |
| :------------ | :------------------------------------------------ |
| break         | 终止 switch 或循环。                              |
| continue      | 跳出循环并在顶端开始。                            |
| debugger      | 停止执行 JavaScript，并调用调试函数（如果可用）。 |
| do ... while  | 执行语句块，并在条件为真时重复代码块。            |
| for           | 标记需被执行的语句块，只要条件为真。              |
| function      | 声明函数。                                        |
| if ... else   | 标记需被执行的语句块，根据某个条件。              |
| return        | 退出函数。                                        |
| switch        | 标记需被执行的语句块，根据不同的情况。            |
| try ... catch | 对语句块实现错误处理。                            |
| var           | 声明变量。                                        |
| let           | 声明变量。                                        |
| const         | 声明变量。                                        |

## var  let  const  三个关键字的区别

#### 在 ES2015 之前，JavaScript 只有两种类型的作用域：

#### *全局作用域*  和 *函数(局部)作用域*。

***全局*（在函数之外）声明的变量拥有*全局作用域*。**

**局部（函数内）声明的变量拥有*函数作用域*。**

##### var

 在全局定义后，局部和全局都可以调用，在函数定义后，局部和全局也都可以调用。

##### let 

在全局定义后，局部和全局都可以调用，在函数定义后，函数内可用，全局不可用。

##### const

定义完变量后，不可以改，会报错

## 4.js语法

### JavaScript 对大小写敏感

```javascript
// 下面两个是不同的变量
lastName = "Gates";
lastname = "Jobs"; 
```

### JavaScript 与驼峰式大小写

ABcdcEF=

### 缩进

JavaScript 的代码可以不用缩进，但是为了可读性我们都是缩进的。

## js的数据类型

**字符串值，数值，布尔值，数组，对象。**

数值就是整数和浮点数

数组就是列表

对象就是字典

| **简单数据类型** | **说明**                                        | **默认值**            |
| ---------------- | ----------------------------------------------- | --------------------- |
| Number           | 数字型，包含整型值和浮点型值，如21，0.21        | 0                     |
| string           | 字符串类型，如“张三”                            | “”                    |
| Boolean          | 布尔值类型，如true，false ，等价于1和false      | false                 |
| Undefined        | var a; 声明了变量a但是没有赋值，此时a=undefined | undefined（未定义的） |
| Null             | var a = null;声明了变量a为空值                  | null                  |

## 定义变量(js赋值)

```javascript
var a=22     // 定义一个变量
var b=2
var name="张三"
console.log(typeof(c))   //看变量的类型。
var c=parseInt(b)    // 将b变成number赋值给c
var d=a+""     // 野路子的将number变成string
var f=toString(a)   //官方 将a变成字符串。
console.log(name+a)    //只要有一个变量是字符串就会+号就是拼接将前一个变量和后一个变量进行拼接。类型字符串。
```

## and，or，not

```javascript
        //在python中and or not 在js中是没有的。分别表示为&& ||  !
        console.log((a>b && 3>4))
        console.log(a==b)   //这样的是不比较数据类型是比较数据的大小
        console.log(a===b)   //这样的是即比较数据类型是比较数据的大小

```

## 三元表达式

```javascript
var c=a<b ? a:b    //三元表达式 a>b成立就等于a，不成立c就等于b
console.log(c)
```

## i ++ 和++i(这个难懂)

```javascript
  i++   //表示让i自增1，i+=1
  ++i   //表示让i自增1，i+=1
  //减法也是一样的。
   		var i=10
        //②后置递增运算符
        //使用口诀:先返回原值，后自加
        var j=i++
        console.log(i)   //i 变成了11
        console.log(j)   // j还是10
        //①前置递增运算符
        //使用口诀:先自加，后返回值
        var j=++i
        console.log(i)     // i变成了11
        console.log(j)     // j变成了是11
```

## 流程控制

### if语句

```javascript
// if(判断条件){代码块}
var i =10 
if (i<5){
    console.log("i比我大");
}else if (i>1) {
    console.log("没有我大");
}
```

### switch语法

```javascript
 var i=1
switch (i){
    case 1:
        console.log("星期一")
        break
    case 2:
        console.log("星期二")
        default:
            console.log("没有了")
    }
//注意 没有break会发生穿透。当
```

### while循环

```javascript
//while循环和python种的语法相似。while(判断条件){代码体；改变循环变量}
var a=0
while (a<=10){
    console.log(a)
    a++
}
```

##  for循环

```javascript
/*
for(初始化代表试;判断条件;改变循环变量){
    循环体
    }
 for 循环
 1.初始化代表试
 2.判断条件
 3.执行循环体
 4.改变循环变量
 5.回到第二部
 */
//for循环的第一种写法。
    for (var i=0;i<=10;i++){
        console.log(i)

    }
    //里面可以加break和continue
//for循环数组的第一种写法
        var arr=[11,22,33,44]
        //可以理解成 arr={0:11,1:22,2:33,3:44}
        for(var item in arr){
            console.log(item)    //这样拿到的是索引,是数组的下标,
        }
/*                                                                                  */
        var arr=[11,22,33,44]
        for(var item in arr){
            console.log(arr[item])    //这样拿到的是数组里面的值.
        }
//for循环数组的第二种写法
var wangfen={
    name:"wnagfen",
    age:14,
    wife:"章老师"
}
for (var key in wangfen){
    console.log(wangfen[key])
}
//for循环数组的第二种写法。(这是数组特有的，爬虫看到的多)
    //    数组(列表)特有的一个方案
         var arr=[11,22,33,44]     //让arr中的每一个去执行一次里面的函数，把被循环的内容一个一个的传递给函数。
         arr.forEach(function (item){
             console.log(item)
         })
```

## 数组（就是列表）

```javascript
//如何定义一个列表

//第一种定义数组的方式！
        var arr=[1,2,3,4]
        console.log(arr)     //建议用第一个
//第二种定义数组的方式！
        var arr2=new Array(1,2,3,4); //官方(麻烦)
        console.log(arr2)
//第三种定义数组的方式！
        var arr3=new Array(3)   //这个数字就是假的
        arr3[0]="张无忌"
        arr3[1]="涿鹿县"
        arr3[2]="沈泽昊"
        arr3[3]="沈泽昊"
        arr3[4]="沈泽昊"
        console.log(arr3)

    for (var i=0;i<arr.length;i++){           //这个必须看得懂.
        console.log(arr[i])
    }
```

## 

```javascript
//就是字典
var p =  {
     name:"张无忌",
    age:18,
     wife:"赵敏",
 chi:function (){
         console.log("吃饭")
 }
 };
 console.log(p["name"])
 console.log(p["age"])
 console.log(p.chi())
```

## 函数(重点)

```javascript
function fn(形参1,形参2,){
    函数体
    可以有return
}
function func(a,b){
    return a+b
}
number=func(2,3)
console.log(number)
var a = function (a) {      //将函数直接赋值给一个变量,js的语法非常的灵活.可以将函数赋值给变量,特殊符号,下划线都可以.js的非常的灵活.
    console.log(a)          //函数赋值不可以是数字.
}
a(1)
//在前端，函数可以在后面加括号后直接赋值
//(函数(形参){})(实参)
(function (a,b){
    console.log("这是我的",a,"这是我的",b)
})(1,2)

c=(function  (){
    return {
    name: "沈泽昊",
    age: "21",
    chi: function (a) {
        console.log(a, "开始吃饭了");
    }
}
})()
console.log(c.age)
console.log(c.xijiao("沈泽昊"))
```
