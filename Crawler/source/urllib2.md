# urlli2

## 发送请求

### 以GET形式发送数据


```python
import urllib.request

response = urllib.request.urlopen('http://www.baidu.com')
print(response.read().decode("utf-8"))
```

### 以POST形式发送数据

```python
import urllib.parse
import urllib.request

# POST类型需要传输data参数
data = bytes(urllib.parse.urlencode({"world":"hello"}), encoding = 'utf-8')
response = urllib.request.urlopen('http://httpbin.org/post', data = data)
print(response.read())
```

### timeout参数
如果访问超时，抛出异常。

```python
import socket
import urllib.request
import urllib.error

try:
    response = urllib.request.urlopen("http://httpbin.org/get", timeout = 0.1)
except urllib.error.URLError as e:
    if isinstance(e.reason, socket.timeout):
        print("TIME OUT")
```

## 响应请求

### 响应类型

```python
import urllib.request

response = urllib.request.urlopen("http://www.python.org")
print(type(response))
```

### 状态码、响应头

```python
import urllib.request

response = urllib.request.urlopen("http://www.python.org")
print(response.status)
print(response.getheaders())
print(response.getheader('Server'))
```

### 获取响应体内容

```python
import urllib.request

response = urllib.request.urlopen("http://www.python.org")
print(response.read().decode('utf-8'))
```

### 使用Request对象

```python
import urllib.request

request = urllib.request.Request("http://python.org")
response = urllib.request.urlopen(request)
print(response.read().decode('utf-8'))
```
这样操作实现结果和上面写的**获取响应体**内容实现结果是一样的。不过，我们使用Request对象之后可以进行一些更复杂的操作。


```python
from urllib import request, parse

url = 'http://httpbin.org/post'
headers = {
    'User-Agent':'Mozilla/4.0', 'Host':'httpbin.org'
}
dict = {
    'name':'Germey'
}
data = bytes(parse.urlopen(dict)), encoding = 'utf-8')
req = request.Requests(url = url, data = data, headers = headers, method = 'POST')
response = request.urlopen(req)
print(response.read().decode('utf-8'))
```

## 使用Handler
许多高级操作，比如说FTP或者操作cache需要使用Handler来进行。

### 代理

```python
import urllib.request

proxy_handler = urllib.request.ProxyHandler({
    'http':'http://127.0.0.1:9743',
    'https':'https://127.0.0.1:9743'
})
opener = urllib.request.bulid_opener(proxy_handler)
response = opener.open('http://www.baid.com')
print(response.read())
```

### Cookie
Cookie是在客户端保留，用来记录用户身份的文本文件。在爬虫中，使用Cookie主要用来维护登录状态的一个机制。

```python
import http.cookiejar, urllib.request

cookie = http.cookiejar.CookieJar() #使用cookie首先需要声明为一个CookieJar的对象
handler = urllib.request.HTTPCookieProcessor(cookie)
opener = urllib.request.build_opener(handler)
response = opener.open('http://www.baidu.com')

# 打印cookie
for item in cookie:
    print(item.name +"="+ item.value)
```

因为，cookie是用来维持用户登录信息的，所以，我们常常把cookie保存为一个文本文件。用爬虫请求的时候把Cookie发送给服务器。下面展示一下如何把cookie保存成为一个文本文件。`Mozilla`样式的cookie保存格式

```python
import http.cookiejar, urllib.request

filename = 'cookie.txt'
cookie = http.cookiejar.MozillaCookieJar(filename) #需要声明为CookieJar的一个子类对象，声明好会带有一个save方法
handler = urllib.request.HTTPCookieProcessor(cookie)
opener = urllib.request.build_opener(handler)
response = opener.open('http://www.baidu.com')
cookie.save(ignore_discard = True, ignore_expires = True)
```
也可以保存为`LWP`Cookie保存格式：

```python
import http.cookiejar, urllib.request

filename = 'cookie.txt'
cookie = http.cookiejar.LWPCookieJar(filename) #需要声明为CookieJar的一个子类对象，声明好会带有一个save方法
handler = urllib.request.HTTPCookieProcessor(cookie)
opener = urllib.request.build_opener(handler)
response = opener.open('http://www.baidu.com')
cookie.save(ignore_discard = True, ignore_expires = True)
```
不过，无论使用哪种格式，其实没有太大关系，使用哪种格式来存cookie，最后使用哪种格式来读取cookie就好了。

```python
import http.cookiejar, urllib.request

filename = 'cookie.txt'
cookie = http.cookiejar.MozillaCookieJar(filename) 

cookie.load(filename, ignore_discard = True, ignore_expires = True)
handler = urllib.request.HTTPCookieProcessor(cookie)
opener = urllib.request.build_opener(handler)
response = opener.open('http://www.baidu.com')
print(response.read().decode('utf-8'))
```

## 异常处理

```python
from urllib import request, error
try:
    response = request.urlopen('http://cuiqingcai.com/index.htm')
except error.URLError as e:
    print(e.reason) #使用e.reason打印异常原因
```
`urllib.error`一共有两种error，分别是`URLError`和`HTTPError`，其中URLError只有一个reason方法，而HTTPError有三种方法，一般捕获异常的时候回这样写：

```python
from urllib import request, error
try:
    response = request.urlopen('http://cuiqingcai.com/index.htm')
except error.HTTPError as e:
    print(e.reason, e.code, e.headers, sep = '\n')
except error.URLError as e:
    print(e.reason)
else:
    print('Request Successfully')
```
还可以用这种方法来验证异常：

```python
import socket
import urllib.request
import urllib.error

try:
    response = urllib.request.urlopen("http://httpbin.org/get", timeout = 0.1)
except urllib.error.URLError as e:
    if isinstance(e.reason, socket.timeout):
        print("TIME OUT")
```
超时异常的话e.reason是
`<class 'socket.timeout>`类型，通过`isinstance`进行类型匹配来验证异常。

## URL解析

### urlparse

**urllib.parse.urlparse(urlstring, scheme='', allow_fragments=True)**

urlparse的功能就是对url进行分割，分割成不同的部分。

```python
from urllib.parse import urlparse

result = urlparse('http://www.baidu.com/index.html;user?id=5#comment')
print(type(result), result)
```
结果是：
<class 'urllib.parse.ParseResult'> ParseResult(scheme='http', netloc='www.baidu.com', path='/index.html', params='user', query='id=5', fragment='comment')

还可以指定解析的协议

```python
from urllib.parse import urlparse

result = urlparse('www.baidu.com/index.html;user?id=5#comment', scheme='https')
print(result)
```
但是如果网页本身有协议类型，这时候用`scheme`参数指定协议类型就不会生效了。


```python
from urllib.parse import urlparse

result = urlparse('http://www.baidu.com/index.html;user?id=5#comment', allow_fragments=False)
print(result)
```
这样，fragment就会拼接到前面的位置去，输出结果为：

ParseResult(scheme='http', netloc='www.baidu.com', path='/index.html', params='user', query='id=5#comment', fragment='')

如果`query`也是空的话，fragment的值如果是false的话，值会拼接到更前面去。


### urlunparse

用这个方法把片段拼成一个url

```python
from urllib.parse import urlunparse

data = ['http', 'www.baidu.com', 'index.html', 'user', 'a=6', 'comment']
print(urlunparse(data))
```

### urljoin

这个也是用来拼URL，但是更为强大。

```python
from urllib.parse import urljoin

print(urljoin('http://www.baidu.com', 'FAQ.html'))
print(urljoin('http://www.baidu.com', 'https://cuiqingcai.com/FAQ.html'))
print(urljoin('http://www.baidu.com/about.html', 'https://cuiqingcai.com/FAQ.html'))
print(urljoin('http://www.baidu.com/about.html', 'https://cuiqingcai.com/FAQ.html?question=2'))
print(urljoin('http://www.baidu.com?wd=abc', 'https://cuiqingcai.com/index.php'))
print(urljoin('http://www.baidu.com', '?category=2#comment'))
print(urljoin('www.baidu.com', '?category=2#comment'))
print(urljoin('www.baidu.com#comment', '?category=2'))
```
输出结果

```
http://www.baidu.com/FAQ.html #这是将两个片段拼在一起
https://cuiqingcai.com/FAQ.html #后面的字段名会自动覆盖掉前面的字段
https://cuiqingcai.com/FAQ.html
https://cuiqingcai.com/FAQ.html?question=2
https://cuiqingcai.com/index.php
http://www.baidu.com?category=2#comment
www.baidu.com?category=2#comment
www.baidu.com?category=2
```
前面可以看到，整个URL可以分成6个字段，如果后面的字段不完成，不能构成一个URL，最后就会用前面的字段来补充。如果后面的是存在的，那么就以后面的为基准。

### urlencode
可以把一个字典对象转换成get请求参数。按照`&`进行分割。因为很多情况下，URL都是以字典形式构造的。

```python
from urllib.parse import urlencode

params = {
    'name': 'germey',
    'age': 22
}
base_url = 'http://www.baidu.com?'
url = base_url + urlencode(params)
print(url)
```
结果：http://www.baidu.com?name=germey&age=22

