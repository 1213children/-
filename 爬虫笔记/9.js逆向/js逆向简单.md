# 简单的js逆向

**实验的百度翻译**

先上代码

```python
import requests
import execjs
input=input("请输入你先要查对的词")
with open(r'fanyi.js', "r", encoding="utf-8") as f:
    f = f.read()
    js = execjs.compile(f)
    a = js.call("e",input)
    print(a)
url = "https://fanyi.baidu.com/v2transapi?from=en&to=zh"
data = {
    "from": "en",
    "to": "zh",
    "query": input,
    "transtype": "translang",
    "simple_means_flag": 3,
    "sign": a,
    "token": "604b6a8269fc5df6a3b343adb45edf8b",
    "domain": "common",
}
headers = {
    "Referer": "https://fanyi.baidu.com/?aldtype=16047",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.127 Safari/537.36",
    "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
    'Cookie': 'BIDUPSID=36E6C7EE8BEC38986A3CB4E31CDC86C2; PSTM=1640439532; BAIDUID=36E6C7EE8BEC38987AF6C678E70AD59A:FG=1; __yjs_duid=1_17684c25859712b24b6a6ec6a90472491640572807378; SOUND_SPD_SWITCH=1; REALTIME_TRANS_SWITCH=1; HISTORY_SWITCH=1; FANYI_WORD_SWITCH=1; SOUND_PREFER_SWITCH=1; APPGUIDE_10_0_2=1; BDUSS=WFaaWs4djVzdmk0ZTB-MUs2U3lJRDNsdk5qS0h5T0F1SGQ5QWFhazJ6QS1GQ2hpRVFBQUFBJCQAAAAAAQAAAAEAAADjKs8gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD6HAGI-hwBia; BDUSS_BFESS=WFaaWs4djVzdmk0ZTB-MUs2U3lJRDNsdk5qS0h5T0F1SGQ5QWFhazJ6QS1GQ2hpRVFBQUFBJCQAAAAAAQAAAAEAAADjKs8gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD6HAGI-hwBia; BDSFRCVID_BFESS=0vkOJexroG0RMW5D0khbKFGMC2KK0gOTDYrEbmI_j_DajMLVgQA_EG0PtENUNzF-oxnIogKKBmOTHnuF_2uxOjjg8UtVJeC6EG0Ptf8g0M5; H_BDCLCKID_SF_BFESS=tRk8oDt5fCv5j4Fk5-of5DCShG4j-tn9WDTOQJ7TtUoYSI5eD-Rs-xPzQPcmJfnitIv9-pbw-q5DDqT3MlnU-tIIXbjMQxod3mkjbPbNBf5FJlLzhn7_yt4syPRG2xRnWIotKfA-b4ncjRcTehoM3xI8LNj405OTbIFO0KJzJCcjqR8Ze5t5DxK; RT="z=1&dm=baidu.com&si=f0f2yjxb4kj&ss=l2zvuly6&sl=2&tt=2r4&bcn=https%3A%2F%2Ffclog.baidu.com%2Flog%2Fweirwood%3Ftype%3Dperf&ul=1yd1&hd=1yfg"; BA_HECTOR=a00k8k2h258g2184ni1h7m26b0r; delPer=0; PSINO=2; Hm_lvt_64ecd82404c51e03dc91cb9e8c025574=1651290161,1651917558,1652013122,1652231367; H_PS_PSSID=36426_31660_34813_36424_36167_34584_35978_36073_36055_36296_36344_26350; Hm_lpvt_64ecd82404c51e03dc91cb9e8c025574=1652241083',
}

res = requests.post(url, data=data, headers=headers)
print(res.json())
print(res.json()["trans_result"]['data'][0]['dst'])
```

和js代码(fanyi.js)

```javascript
var i = null;
function a(r) {
    if (Array.isArray(r)) {
        for (var o = 0, t = Array(r.length); o < r.length; o++)
            t[o] = r[o];
        return t
    }
    return Array.from(r)
}
function n(r, o) {
    for (var t = 0; t < o.length - 2; t += 3) {
        var a = o.charAt(t + 2);
        a = a >= "a" ? a.charCodeAt(0) - 87 : Number(a),
        a = "+" === o.charAt(t + 1) ? r >>> a : r << a,
        r = "+" === o.charAt(t) ? r + a & 4294967295 : r ^ a
    }
    return r
}
function e(r) {
    var o = r.match(/[\uD800-\uDBFF][\uDC00-\uDFFF]/g);
    if (null === o) {
        var t = r.length;
        t > 30 && (r = "" + r.substr(0, 10) + r.substr(Math.floor(t / 2) - 5, 10) + r.substr(-10, 10))
    } else {
        for (var e = r.split(/[\uD800-\uDBFF][\uDC00-\uDFFF]/), C = 0, h = e.length, f = []; h > C; C++)
            "" !== e[C] && f.push.apply(f, a(e[C].split(""))),
            C !== h - 1 && f.push(o[C]);
        var g = f.length;
        g > 30 && (r = f.slice(0, 10).join("") + f.slice(Math.floor(g / 2) - 5, Math.floor(g / 2) + 5).join("") + f.slice(-10).join(""))
    }
    var u = void 0
      // , l = "" + String.fromCharCode(103) + String.fromCharCode(116) + String.fromCharCode(107);
    u = null !== i ? i : (i = "320305.131321201" || "") || "";
    for (var d = u.split("."), m = Number(d[0]) || 0, s = Number(d[1]) || 0, S = [], c = 0, v = 0; v < r.length; v++) {
        var A = r.charCodeAt(v);
        128 > A ? S[c++] = A : (2048 > A ? S[c++] = A >> 6 | 192 : (55296 === (64512 & A) && v + 1 < r.length && 56320 === (64512 & r.charCodeAt(v + 1)) ? (A = 65536 + ((1023 & A) << 10) + (1023 & r.charCodeAt(++v)),
        S[c++] = A >> 18 | 240,
        S[c++] = A >> 12 & 63 | 128) : S[c++] = A >> 12 | 224,
        S[c++] = A >> 6 & 63 | 128),
        S[c++] = 63 & A | 128)
    }
    for (var p = m, F = "" + String.fromCharCode(43) + String.fromCharCode(45) + String.fromCharCode(97) + ("" + String.fromCharCode(94) + String.fromCharCode(43) + String.fromCharCode(54)), D = "" + String.fromCharCode(43) + String.fromCharCode(45) + String.fromCharCode(51) + ("" + String.fromCharCode(94) + String.fromCharCode(43) + String.fromCharCode(98)) + ("" + String.fromCharCode(43) + String.fromCharCode(45) + String.fromCharCode(102)), b = 0; b < S.length; b++)
        p += S[b],
        p = n(p, F);
    return p = n(p, D),
    p ^= s,
    0 > p && (p = (2147483647 & p) + 2147483648),
    p %= 1e6,
    p.toString() + "." + (p ^ m)
}
```

## 思路

![屏幕截图 2022-05-11 152129](https://s2.loli.net/2022/05/11/SBuLI9U3T5gAYDv.png)

1. 找到需要抓的包，点击Initator,这个的加载是从下向上加载的，但是我们从上面的开始找。点开后是 函数名.send().就不是，在点第二个。依次，找到ajax，或着找到函数。
2. 开始断点，调试，进去到函数里面，找到函数并，分析函数。
3. 拿到函数的数据在execjs，或者在网页Console运行，拿到后面的数据，
4. 在python中运行。拿到结果，