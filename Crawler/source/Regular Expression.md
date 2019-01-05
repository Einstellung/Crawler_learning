# 正则表达式

正则表达式是对字符串的一种逻辑公式，就是用事先定义好的一些特定字符、以及这些特定字符的组合，组成一个“规则字符串”，这个“规则字符串”用来表达对字符串的一种过滤逻辑。

[在线测试正则表达式](http://tool.oschina.net/regex/#)


![image](https://github.com/Einstellung/Crawler_learning/blob/master/Crawler/images/Regular%20Expression/%E5%9B%BE1.png?raw=true)

## re.match
re.match尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就会返回none。

**re.match(pattern, string, flags = 0)**

### 最常规的匹配

```python
import re

content = 'Hello 123 4567 World_This is a Regex Demo'
print(len(content))
result = re.match('^Hello\s\d\d\d\s\d{4}\s\w{10}.*Demo$', content)
print(result)
print(result.group()) #返回匹配结果
print(result.span()) #输出匹配结果的范围
```
### 泛匹配
之前写的匹配过于复杂，如果想要实现同样的效果可以有比较简单的书写格式。可是使用`.*`来匹配所有的字符串。

```python
import re

content = 'Hello 123 4567 World_This is a Regex Demo'
result = re.match('^Hello.*Demo$', content)  #使用.*就可以把中间的所有结果都匹配过去了
print(result)
print(result.group())
print(result.span())
```

### 匹配目标
比如说在下面的字符串里面想要获取1234567，我们可以把1234567的左端点和右端点指定一下。之后用括号括起来，这样就可以把匹配目标找出来了。

1234567的左端点是一个空白字符，所以用\s，之后要匹配的是一个数字，所以用(\d+)，之后用\s来指定右端点，再后面就无所谓了。
```python
import re

content = 'Hello 1234567 World_This is a Regex Demo'
result = re.match('^Hello\s(\d+)\sWorld.*Demo$', content)
print(result)
print(result.group(1))
print(result.span())
```

### 贪婪匹配

```python
import re

content = 'Hello 1234567 World_This is a Regex Demo'
result = re.match('^He.*(\d+).*Demo$', content)
print(result)
print(result.group(1))
```
输出结果：

```
<_sre.SRE_Match object; span=(0, 40), match='Hello 1234567 World_This is a Regex Demo'>
7
```
从图中可以看出，贪婪模式的话，前面`.*`会一直匹配数字，一直到7位置，因为后面有(\d+)，必须输出一个数字，所以一直匹配到最后输出一个7。

### 非贪婪匹配
非贪婪匹配，可以在`.*`后面加一个`?`，这样匹配的结果就是非贪婪匹配。非贪婪匹配会尽可能匹配少的字符。

```python
import re

content = 'Hello 1234567 World_This is a Regex Demo'
result = re.match('^He.*?(\d+).*Demo$', content)
print(result)
print(result.group(1))
```
输出为：

```
<_sre.SRE_Match object; span=(0, 40), match='Hello 1234567 World_This is a Regex Demo'>
1234567

```

？后面有一个\d+，说明要开始匹配数字了，尽可能匹配少的数字，于是就把这些东西全部输出了。

### 匹配模式

```python
import re

content = '''Hello 1234567 World_This
is a Regex Demo
'''
result = re.match('^He.*?(\d+).*?Demo$', content, re.S) #匹配模式就是这里的re.S
print(result.group(1))
```
这里的content是两行，中间隔着一个换行符，如果不加`re.S`那么到换行符位置就停止了，不会匹配后面的内容。加了这个匹配模式之后就可以匹配任意字符了。

### 转义

我们有的时候还想得到一些特殊字符，比如说$5.00，我们想把这个dollar字符匹配出来，但是这个符号在正则表达式里面还是一个特殊字符，不好匹配。这是，可以加转义字符达到匹配的效果。

```python
import re

content = 'price is $5.00'
result = re.match('price is \$5\.00', content)
print(result)
```

**总结：尽量使用泛匹配、使用括号得到匹配目标、尽量使用非贪婪模式、有换行符就用re.S**


## re.search

re.match有一个不太方便的地方是他会从第一个字符开始匹配，即第一个字符如果不匹配，就不检查后面的字符了。

re.search会扫描整个字符串并返回第一个成功的匹配。所以，一般情况下，能用search就不用match。

```python
import re

html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君"><i class="fa fa-user"></i>但愿人长久</a>
        </li>
    </ul>
</div>'''
result = re.search('<li.*?active.*?singer="(.*?)">(.*?)</a>', html, re.S)
if result:
    print(result.group(1), result.group(2)) #group(1)代表第一个小括号匹配的内容，group(2)代表第二个小括号括起来的内容
```
输出结果是：齐秦 往事随风

这里的active表示的是去匹配带有active的标签。

下面看一下如果没有active的情况，HTML还是之前的HTML。

```python
import re

html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君">但愿人长久</a>
        </li>
    </ul>
</div>'''
result = re.search('<li.*?singer="(.*?)">(.*?)</a>', html, re.S)
if result:
    print(result.group(1), result.group(2))
```
输出结果是：任贤齐 沧海一声笑

## re.findall
re.search是找到一个结果，re.findall是返回所有匹配结果。

```python
import re

html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君">但愿人长久</a>
        </li>
    </ul>
</div>'''
results = re.findall('<li.*?href="(.*?)".*?singer="(.*?)">(.*?)</a>', html, re.S)
print(results)
print(type(results))
for result in results:
    print(result)
    print(result[0], result[1], result[2])
```
输出结果：

```
[('/2.mp3', '任贤齐', '沧海一声笑'), ('/3.mp3', '齐秦', '往事随风'), ('/4.mp3', 'beyond', '光辉岁月'), ('/5.mp3', '陈慧琳', '记事本'), ('/6.mp3', '邓丽君', '但愿人长久')]
<class 'list'>
('/2.mp3', '任贤齐', '沧海一声笑')
/2.mp3 任贤齐 沧海一声笑
('/3.mp3', '齐秦', '往事随风')
/3.mp3 齐秦 往事随风
('/4.mp3', 'beyond', '光辉岁月')
/4.mp3 beyond 光辉岁月
('/5.mp3', '陈慧琳', '记事本')
/5.mp3 陈慧琳 记事本
('/6.mp3', '邓丽君', '但愿人长久')
/6.mp3 邓丽君 但愿人长久
```

下面提出更高要求。比如说“一路有你”，没有歌曲位置和唱者。想要实现一个泛化能力更强的代码，可以这样写：

```python
import re

html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君">但愿人长久</a>
        </li>
    </ul>
</div>'''
results = re.findall('<li.*?>\s*?(<a.*?>)?(\w+)(</a>)?\s*?</li>', html, re.S)
print(results)
for result in results:
    print(result[1])
```
输出结果是：

```
[('', '一路上有你', ''), ('<a href="/2.mp3" singer="任贤齐">', '沧海一声笑', '</a>'), ('<a href="/3.mp3" singer="齐秦">', '往事随风', '</a>'), ('<a href="/4.mp3" singer="beyond">', '光辉岁月', '</a>'), ('<a href="/5.mp3" singer="陈慧琳">', '记事本', '</a>'), ('<a href="/6.mp3" singer="邓丽君">', '但愿人长久', '</a>')]
一路上有你
沧海一声笑
往事随风
光辉岁月
记事本
但愿人长久
```
详细解释一下这段正则表达式的代码：`<li.*?>`这一段表示的是li标签的开头和结尾。

这里我们还面临一个换行和不换行的问题，在“齐秦”这个位置，我们的li标签面临一个换行的问题，而在其他位置是不存在换行的事情的。所以我们写`\s*?`其中，\s表示匹配换行符，但是这个换行符可能有也可能没有，所以加上*?

然后匹配一下a标签，但是这个标签也是可能有可能没有的。这里使用`(<a.*?>)?`，小括号还有一个功能是把括起来的内容看做一个整体，括号外面加一个问号，表示匹配了一个或者是0个，表达了有a标签或者没有a标签的意思。

接下面匹配歌名，使用`(\w+)`

`(</a>)?`a标签或者有或者没有作为一个整体

`\s*?`换行空白符或者有或者没有

最后加上一个`</li>`作为结尾

最后说一下小括号的事情，从这个例子可以看出，小括号既能够作为一个整体，对小括号里面的东西进行匹配，同时小括号还可以作为一个分组，将打印结果输出出来。

## re.sub
这个方法是用来做字符串替换的，第一个参数是要传入一个正则表达式，第二个参数是要传入你要替换成的东西，第三个参数是源字符串。


```python
import re

content = 'Extra stings Hello 1234567 World_This is a Regex Demo Extra stings'
content = re.sub('\d+', 'Replacement', content)
print(content)
```
输出结果为：Extra stings Hello Replacement World_This is a Regex Demo Extra stings。

可以看到，123457就被替换为我们想要替换的参数了。

但是有一个问题，如果我们想要的是对原生字符串的**改进**而不是单纯的替换，那么该怎么做。

```python
import re

content = 'Extra stings Hello 1234567 World_This is a Regex Demo Extra stings'
content = re.sub('(\d+)', r'\1 8910', content)
print(content)
```
输出结果是：Extra stings Hello 1234567 8910 World_This is a Regex Demo Extra stings

我们对正则表达式要匹配的内容加一个括号，通过之前的例子可以知道，括号里面的内容可以通过group(1)这样的形式拿过来。为了防止`\`被转义，所以前面加一个r。这里\1就是把第一个括号里面的内容拿过来，做一个替换。

使用sub之后对之前findall方法实施有一个新的思路，就是先把a标签删除，这样拿出歌名就更容易。

```python
import re

html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君">但愿人长久</a>
        </li>
    </ul>
</div>'''
html = re.sub('<a.*?>|</a>', '', html)
print(html)
results = re.findall('<li.*?>(.*?)</li>', html, re.S)
print(results)
for result in results:
    print(result.strip())
```
结果是：

```
<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            沧海一声笑
        </li>
        <li data-view="4" class="active">
            往事随风
        </li>
        <li data-view="6">光辉岁月</li>
        <li data-view="5">记事本</li>
        <li data-view="5">
            但愿人长久
        </li>
    </ul>
</div>
['一路上有你', '\n            沧海一声笑\n        ', '\n            往事随风\n        ', '光辉岁月', '记事本', '\n            但愿人长久\n        ']
一路上有你
沧海一声笑
往事随风
光辉岁月
记事本
但愿人长久
```

## re.compile
这个方法是将正则表达式编译成正则表达式对象。我们可以把匹配模式做成一个正则表达式对象，以后不用重新再写匹配模式，直接调用这个匹配对象就可以了。

```python
import re

content = '''Hello 1234567 World_This
is a Regex Demo'''
pattern = re.compile('Hello.*Demo', re.S)
result = re.match(pattern, content)
#result = re.match('Hello.*Demo', content, re.S)
print(result)
```

### 练习

```python
import requests
import re
content = requests.get('https://book.douban.com/').text
pattern = re.compile('<li.*?cover.*?href="(.*?)".*?title="(.*?)".*?more-meta.*?author">(.*?)</span>.*?year">(.*?)</span>.*?</li>', re.S)
results = re.findall(pattern, content)
for result in results:
    url, name, author, date = result
    author = re.sub('\s', '', author)
    date = re.sub('\s', '', date)
    print(url, name, author, date)
```








