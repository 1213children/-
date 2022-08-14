# Jquery和ajax

## 1.js和页面交互

```javascript
<script>
    function fn(){
        console.log("你好孤傲")
    }// 用onclick
//页面加载时，执行的功能。等到下面的<body>中的代码执行完了，在执行下面的js代码。
window.onload = function (){
    let btn =document.getElementById("ll")     
    // 通过id的值来获取到页面元素(标签)
    btn.addEventListener("click",function (){
        //监听clickd
        //监听这个html代码点击后运行这行js代码。
        console.log("你敢点我")
        
            document.getElementById("mydiv").innerText="你好";
        								//点击Id是mydiv的代码后会
    })
    }
</script>
```

一段前端代码！

```html
<body>
<!--老前端是这样的用onclick="fn()"，现在用的多的是下面的方法。-->
<input type="button" onclick="fn()" value="来点我，快快">
<input type="button" id="ll"  value="我是按钮，点我">
    <div id="mydiv" style="width:100px;height:100px;background: #574247;"></div>
</body>
<script>//
    //而这样的不是很好，代码的执行时从上向下执行的。所以下面的代码只能放到后面，如果有好多下面的代码不好写。
    //     let btn =document.getElementById("ll")
    //     btn.addEventListener("click",function (){
    //         console.log("你敢点我")
    //     })
</script>
```

- **AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。**

- AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。
- **Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。**
- 就和国内百度的搜索框一样!

- 传统的网页(即不用ajax技术的网页)，想要更新内容或者提交一个表单，都需要重新加载整个网页。
- 使用ajax技术的网页，通过在后台服务器进行少量的数据交换，就可以实现异步局部更新。
- 使用Ajax，用户可以创建接近本地桌面应用的直接、高可用、更丰富、更动态的Web用户界面。



