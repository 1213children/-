# js的进阶

注意js中

```javascript
guid:this.gContext.dataset.guid   //这个是变量   变量赋值给guid。 this可以看成python的self

_rnd: x.getTimeStampStr()    	  //函数  x是一个闭包函数，闭包函数中有个getTimeStampStr()
								  //运行x中的getTimeStampStr()将结果赋值给—_rnd
```



## 1.定时器

```javascript
//定时器 第一种定时器        
var t=setTimeout(function (){    //4秒后弹窗。
        alert("吓唬我")
        },4000)                    //单位是毫秒。
        window.clearTimeout(t)     // 清除定时器
        //  第二种定时器
        var t=setInterval(function (){      //每过一秒弹一个
            alert("你好")
        },1000)
         window.clearInterval(t)       // 清除定时器
        var t = new Date()
        console.log(t.getFullYear())    //拿到当前年
        console.log(t.getMonth()+1)    //拿到月份，月份从零开始要加一才可以的。
        console.log(t.getDate())       // 拿到日期
        console.log(t.getTime())       // 拿到时间戳       js中是毫秒，python 中是秒。
     								//还可以有天，小时，秒
```

## 2.evel

```javascript
var a="console.log('哈哈')"     //将代码封装成字符串
eval(a)        //，运行字符串代码。a是字符串，也是代码
//作用
		//把evel中的内容复制到浏览器的console中。
        //var mm="复制的东西"
        //mm
        //获得明文
```

## 3.prototype

prototype是什么
prototype是js中里面的类增加功能扩展的一种模式
js中的类新版js中有class，但是旧版用的function(以前的人class还是用不惯)
在不动js类源码，给js类加薪的功能。**类似python中的装饰器。**

```javascript
function penser(name,age){
    this.name=name;
    this.age=age;
    this.chi=function (who){
        console.log(name,"和",who,"在一起吃饭")
    }
}
//给函数penser增加新的功能。用prototype
penser.prototype.haha=function lolo(){console.log("nihao")}
// 这就是类似python的class。(用python理解就是，类的实例化，创建两个实例化对象。在js中需要加new！运行那个函数时就（对象名称.函数名）和python一样，self变成了this，实例化时加new。)
p1=new penser("沈泽昊","21")
p2=new penser("沈泽昊2","22")
p1.haha()
p1.chi("z张")
```

## 4.变量提升

正常情况先要打印变量时需要先声明变量！

但是在js中打印变量时可以不先声明变量，(不报错)，但是打印出的变量时undefined.

```javascript
function name (){
    console.log(name)    //打印前没有声明变量，没有报错，打印出undefined.
    var name="沈泽昊"     //这个就是变量提升，就是先打印name在声明name。
}
name()
```

```javascript
在同一个作用域。
const   //这个声明变量后，变量不能改，变量名也不能重复使用(直接定死)
var     //这个声明变量后，变量能改，变量名也重复使用(灵活)
let     //声明变量后，变量能改，变量名也不能重复使用
```

## 5.闭包函数(难)

 闭包的特点: 1.内层函数使用外层函数的特点。
						2.会让变量常住在内存。

> 就是一个网页可能导入了好多个js文件，其中有些变量名可能会出现重复的，后加载的js代码，会将先加载的js代码中相同变量名覆盖掉。为了解决这个问题出现了闭包。
>
> 全局变量在函数内可以改，但是局部变量在全局是不可以被改的。(在python中想改需要加global)
>
> 我将我声明的变量放到局部，就可以了。

**类似python中的装饰器！** (灵活使用return)

### 第一种写法

```javascript
 var jiami = (function fn() {
    let key = "王力宏"
    var pro = function (shuju) {
        return shuju + "处理完毕"
    }
    return function (data) {
        data = pro(data)
        console.log("我要开始加密了，密钥匙：", key, "加密的密文是：", data)
        return "加密好的字节"
    }     //函数的返回值，>>>>>>>返回调运的位置。
})()
 // (函数(){})()  函数本身就运行着。jiami()就是运行return function函数，将return赋值给zhi。
zhi=jiami("沈")
console.log(zhi)
```

### 第二种写法

```javascript
var jiami = (function fn() {
    let key = "王力宏"
    let pro = function (shuju) {
        return shuju + "处理完毕"
    }
    return {
        resa: function (data) {
            data = pro(data)
            console.log("我要开始加密了，密钥匙：", key, "加密的密文是：", data)
            return "加密好的字节"

        },
        asa: function (data) {

            data = pro(data)
            console.log("我要开始加密了，密钥匙：", key, "加密的密文是：", data)
            return "加密好的字节"
        },
        md5: function (data) {

            data = pro(data)
            console.log("我要开始加密了，密钥匙：", key, "加密的密文是：", data)
            return "加密好的字节"
        },
        ppp : function (data) {
            // 运行asa()函数
            this.asa()
            data = pro(data)
            console.log("我要开始加密了，密钥匙：", key, "加密的密文是：", data)
            return "加密好的字节"
        }
    }})()
jiami.ppp("沈")
函数jiami本身就在运行着，只要在.函数名就行。
```

## 6.如何修改闭包里的变量。

```javascript
//在闭包中的变量如何传递给外面，并可以在外面进行修改。
function fn(){
            let name = "wusir";
            console.log(name);
            // 窗口对象-> 全局变量(nnnn)
            // 创建一个全局变量 -> 在闭包函数中向外界传递一些信息
            window.nnnn = name;
        }
        fn()
        console.log(nnnn);
```



## 7. JS原型和面向对象

```
ES5，不支持面向对象（居多）。
ES6，支持面向对象。

即使专业前端开发会用一些框架（ES6编写） --> 自动转换成ES5  --> 用户获取JS的面向对象的代码（ES5）。
```















