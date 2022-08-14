在开发者工具中的Network可以看到是get请求还是port请求。

get请求：

```python

import requests

url ="https://movie.douban.com/j/search_subjects"
dtat={
    "type": "movie",
    "tag": "热门",
    "sort": "recommend",
    "page_limit": "20",
    "page_start": "0",
}
header={"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.60 Safari/537.36"}
resp=requests.get(url,params=dtat,headers=header)
print(resp.json())
```

port请求:

```python
import requests

url="https://fanyi.baidu.com/sug"
data={
    "kw":input("请输入单词")
}
header={"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.60 Safari/537.36"}
resp=requests.post(url,data,headers=header)
print(resp.json())
print(resp.request.url)
```

