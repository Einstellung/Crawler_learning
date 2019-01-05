# Requests
Requests是用Python语言编写的，基于urllib，但是它比urllib更加方便，可以节约我们大量的工作，完全满足HTTP测试需要。

## GET请求

### 基本请求
```python
import requests

response = requests.get('http://httpbin.org/get')
print(response.text)
```
就可以打印基本请求信息

```python
{
  "args": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.20.0"
  }, 
  "origin": "222.197.179.9", 
  "url": "http://httpbin.org/get"
}
```

### 带参数GET

```python
import requests
response = requests.get("http://httpbin.org/get?name=germey&age=22")
print(response.text)
```
和下面的写法是完全一样的，下面写法的好处是不用写各种符号了。

```python
import requests

data = {
    'name': 'germey',
    'age': 22
}
response = requests.get("http://httpbin.org/get", params=data)
print(response.text)
```

### 解析json
这个操作可以直接将获取的结果转化为json格式。

```python
import requests
import json

response = requests.get("http://httpbin.org/get")
print(type(response.text))
print(response.json())  # response.json实际上就是执行了一个json.loads方法，和下面语句效果是一样的
print(json.loads(response.text))
print(type(response.json()))
```

### 获取二进制数据
获取二进制数据在下载图片和下载视频的时候作为常用方法。

```python
import requests

response = requests.get("https://github.com/favicon.ico")
print(type(response.text), type(response.content))
print(response.text)
print(response.content) #使用response.content来获取图片的二进制内容
```
保存的话，可以使用这种方式来保存：

```python
import requests

response = requests.get("https://github.com/favicon.ico")
with open('favicon.ico', 'wb') as f:
    f.write(response.content)
    f.close()
```

### 添加headers

```python
import requests

headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36'
}
response = requests.get("https://www.zhihu.com/explore", headers=headers)
print(response.text)
```

## POST请求
使用POST请求一定要发一个Form Data，我们可以用data字典来传入表单。

```python
import requests

data = {'name': 'germey', 'age': '22'}
headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36'
}
response = requests.post("http://httpbin.org/post", data=data, headers=headers)
print(response.json())
```

## 响应

### response属性
下面列出常用的response属性。

```python
import requests

response = requests.get('http://www.jianshu.com')
print(type(response.status_code), response.status_code)
print(type(response.headers), response.headers)
print(type(response.cookies), response.cookies)
print(type(response.url), response.url)
print(type(response.history), response.history)
```

### 状态码判断
下面给出状态码的各个编号对应的名字，我们在实际使用过程中不需要记住状态码的具体数字，使用名字是一样的效果。

```python
100: ('continue',),
101: ('switching_protocols',),
102: ('processing',),
103: ('checkpoint',),
122: ('uri_too_long', 'request_uri_too_long'),
200: ('ok', 'okay', 'all_ok', 'all_okay', 'all_good', '\\o/', '✓'),
201: ('created',),
202: ('accepted',),
203: ('non_authoritative_info', 'non_authoritative_information'),
204: ('no_content',),
205: ('reset_content', 'reset'),
206: ('partial_content', 'partial'),
207: ('multi_status', 'multiple_status', 'multi_stati', 'multiple_stati'),
208: ('already_reported',),
226: ('im_used',),

# Redirection.
300: ('multiple_choices',),
301: ('moved_permanently', 'moved', '\\o-'),
302: ('found',),
303: ('see_other', 'other'),
304: ('not_modified',),
305: ('use_proxy',),
306: ('switch_proxy',),
307: ('temporary_redirect', 'temporary_moved', 'temporary'),
308: ('permanent_redirect',
      'resume_incomplete', 'resume',), # These 2 to be removed in 3.0

# Client Error.
400: ('bad_request', 'bad'),
401: ('unauthorized',),
402: ('payment_required', 'payment'),
403: ('forbidden',),
404: ('not_found', '-o-'),
405: ('method_not_allowed', 'not_allowed'),
406: ('not_acceptable',),
407: ('proxy_authentication_required', 'proxy_auth', 'proxy_authentication'),
408: ('request_timeout', 'timeout'),
409: ('conflict',),
410: ('gone',),
411: ('length_required',),
412: ('precondition_failed', 'precondition'),
413: ('request_entity_too_large',),
414: ('request_uri_too_large',),
415: ('unsupported_media_type', 'unsupported_media', 'media_type'),
416: ('requested_range_not_satisfiable', 'requested_range', 'range_not_satisfiable'),
417: ('expectation_failed',),
418: ('im_a_teapot', 'teapot', 'i_am_a_teapot'),
421: ('misdirected_request',),
422: ('unprocessable_entity', 'unprocessable'),
423: ('locked',),
424: ('failed_dependency', 'dependency'),
425: ('unordered_collection', 'unordered'),
426: ('upgrade_required', 'upgrade'),
428: ('precondition_required', 'precondition'),
429: ('too_many_requests', 'too_many'),
431: ('header_fields_too_large', 'fields_too_large'),
444: ('no_response', 'none'),
449: ('retry_with', 'retry'),
450: ('blocked_by_windows_parental_controls', 'parental_controls'),
451: ('unavailable_for_legal_reasons', 'legal_reasons'),
499: ('client_closed_request',),

# Server Error.
500: ('internal_server_error', 'server_error', '/o\\', '✗'),
501: ('not_implemented',),
502: ('bad_gateway',),
503: ('service_unavailable', 'unavailable'),
504: ('gateway_timeout',),
505: ('http_version_not_supported', 'http_version'),
506: ('variant_also_negotiates',),
507: ('insufficient_storage',),
509: ('bandwidth_limit_exceeded', 'bandwidth'),
510: ('not_extended',),
511: ('network_authentication_required', 'network_auth', 'network_authentication'),
```
下面两种写法实现功能是一样的：

```python
import requests

response = requests.get('http://www.jianshu.com/hello.html')
exit() if not response.status_code == requests.codes.not_found else print('404 Not Found')
```

```python
import requests

response = requests.get('http://www.jianshu.com')
exit() if not response.status_code == 200 else print('Request Successfully')
```
## 高级操作

### 文件上传
文件上传需要使用POST操作。我们使用open把文件读取出来，然后赋值给files参数就可以实现文件上传操作了。

```python
import requests

files = {'file': open('favicon.ico', 'rb')}
response = requests.post("http://httpbin.org/post", files=files)
print(response.text)
```

### 获取cookie
我们可以直接从cookies属性就可以获取cookie的具体信息了。

```python
import requests

response = requests.get("https://www.baidu.com")
print(response.cookies)
for key, value in response.cookies.items():
    print(key + '=' + value)
```

### 会话维持
有了cookies就可以模拟维持一个登录状态了。可以用来做模拟登陆。

需要注意的是，我们如果想要设置一个cookies，然后读出这个cookies需要设置一个Session对象。如果不设置Session对象，那么相当于两个浏览器在进行get。第二个浏览器是拿不到第一个浏览器的请求的。通过Session这样来维持一个登录会话。

```python
import requests

s = requests.Session()
s.get('http://httpbin.org/cookies/set/number/123456789')
response = s.get('http://httpbin.org/cookies')
print(response.text)
```

### 证书验证
我们在浏览网页的时候，网页证书可能是错误的，直接使用requests就会报出一个SSL认证的错误。我们爬网页的时候如果这个网页是https协议的话，他会首先检查一下证书的合法性。如果证书不合法，就会报出SSL Error，那么程序就会中断掉。如果想要避免这个问题，关掉证书验证功能就可以了。

```python
import requests

response = requests.get('https://www.12306.cn', verify=False)
print(response.status_code)
```
但是，依旧会有警告信息，如果想要看起来好看一些，可以添加下面代码把警告信息消除掉。

```python
import requests
from requests.packages import urllib3
urllib3.disable_warnings()
response = requests.get('https://www.12306.cn', verify=False)
print(response.status_code)
```

### 代理设置

```python
import requests

proxies = {
  "http": "http://127.0.0.1:9743",
  "https": "https://127.0.0.1:9743",
}

response = requests.get("https://www.taobao.com", proxies=proxies)
print(response.status_code)
```
如果代理需要用户名和密码的话，可以这样写，其中user的位置写用户名，password的位置写密码。@作为分隔符。

```python
import requests

proxies = {
    "http": "http://user:password@127.0.0.1:9743/",
}
response = requests.get("https://www.taobao.com", proxies=proxies)
print(response.status_code)
```
如果你的代理是ShadowSocks的话，那么可以这样：


```python
pip3 install 'requests[socks]'

import requests

proxies = {
    'http': 'socks5://127.0.0.1:9742',
    'https': 'socks5://127.0.0.1:9742'
}
response = requests.get("https://www.taobao.com", proxies=proxies)
print(response.status_code)
```

### 超时设置

```python
import requests
from requests.exceptions import ReadTimeout #引入Timeout类
try:
    response = requests.get("http://httpbin.org/get", timeout = 0.5)
    print(response.status_code)
except ReadTimeout:
    print('Timeout')
```

### 认证设置
有些网站需要输入用户名和密码，登录的时候需要进行认证。可以通过给requests传入一个`auth`参数来做到登录验证。

```python
import requests
from requests.auth import HTTPBasicAuth

r = requests.get('http://120.27.34.24:9001', auth=HTTPBasicAuth('user', '123'))
print(r.status_code)
```
或者也可以写成这样：

```python
import requests

r = requests.get('http://120.27.34.24:9001', auth=('user', '123'))
print(r.status_code)
```

### 异常处理

这些具体异常Error的定义和其他有什么Error可以从Requests官方文档中找到。

```python
import requests
from requests.exceptions import ReadTimeout, ConnectionError, RequestException
try:
    response = requests.get("http://httpbin.org/get", timeout = 0.5)
    print(response.status_code)
except ReadTimeout:
    print('Timeout')
except ConnectionError:
    print('Connection error')
except RequestException:
    print('Error')
```
















