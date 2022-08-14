# JSONP

## 什么是JSONP？

正常的json是我们发送什么样请求，返回想要的数据，

jsonp是请求的后面有一个callback:f1，在返回数据时调用callback函数，
返回的数据中既有数据也有f1函数中的数据，类似回调函数。

![image-20210914203918541](https://s2.loli.net/2022/05/17/57VWUcNdn4khtwZ.png)