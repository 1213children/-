1. 找到先要的url，观察url，get请求和post请求，观察哪些参数是变的哪些是不变的。

2. 参数固定不变的，请求时直接填上就可以了，

3. 参数变的有两种情况1.上面的url返回的数据中携带的，2.算法生成的，

4. url返回的数据中携带的，需要请求url拿到数据，

5. 算法生成的，需要找j代码，分析js代码

   - ```
     当我们在js中找到算法的具体实现了，我们就有两种方式去实现这个算法：
     
     - 用Python代码直接实现。
     - 写JS + Python + node.js 可以直接去编译JS代码并的到结果。
     ```
   
     如何找到算法，根据关键字去搜索，



##  一，如何在js中找

找像这样的。

xxx = 

​	{
xxx:yyy
}

#### 方法一：搜索

看到Axios，vue ，react，jquery的js文件，大部分可以不用看。
这些都是前端框架。不会有人，动框架的源码。

![屏幕截图 2022-05-30 151720](https://s2.loli.net/2022/05/30/DC4yu1bzTYqtWVS.png)

#### 方法二：在initiator中找

看到Axios，vue ，react，jquery的js文件，大部分可以不用看。
这些都是前端框架。不会有人，动框架的源码。

![屏幕截图 2022-05-30 151627](https://s2.loli.net/2022/05/30/aHGBoIhKMq8UwdP.png)

### 方法三：Hook

**Hook就是，覆盖原函数**
**Hook是有时机的。**

如何解决：在第一个js文件的第一行下断断点，然后再控制台手动注入hook。(有些网页还是不行，就需要fiddler)

​	用fiddler代替





这个就是hook的代码来定位cookie，这个代码还可以加if来判断

```javascript
//当前版本hook工具只支持Content-Type为html的自动hook
//下面是一个示例:这个示例演示了hook全局的cookie设置点
(function() {
    //严谨模式 检查所有错误
    'use strict';
    //document 为要hook的对象   这里是hook的cookie
	var cookieTemp = "";
    Object.defineProperty(document, 'cookie', {
		//hook set方法也就是赋值的方法 
		set: function(val) {
				//这样就可以快速给下面这个代码行下断点
				//从而快速定位设置cookie的代码
				console.log('Hook捕获到cookie设置->', val);
				cookieTemp = val;
				return val;
		},
		//hook get方法也就是取值的方法 
		get: function()
		{
			return cookieTemp;
		}
    });
})()
```

## 方法四：

看到<form标签后，里面有一个onsubmit=""  里面是一个函数 ，去搜索这个函数

### 断点

断点最好打在函数开头(function)，var或者最后，
调试一个函数，最好在函数中打断点。

### stringify

看到这个词要注意

JSON.stringify() 方法用于将 JavaScript 值转换为 JSON 字符串。

对象变成json

### promise

看到这个单词就是异步的，

