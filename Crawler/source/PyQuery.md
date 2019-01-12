# PyQuery
强大又灵活的网页解析库，如果觉得正则表达式写起来太复杂，BeautifulSoup语法太难记得话。如果你熟悉jQuery的语法，那么PyQuery是你绝佳的选择。

安装：pip3 install pyquery

## 初始化

### 字符串初始化

```python
html = '''
<div>
    <ul>
         <li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
print(doc('li'))
```
输出为：

```
<li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
```
这个选择器是一个CSS选择器，实现效果和Beautiful Soup里面的CSS选择器效果是一样的。选id的话前面加`#`，如果选class，前面加`.`，选标签的话，前面什么也不加。

### URL初始化
如果传入一个链接的话，会自动请求链接，并把链接得到的HTML当做参数，传给doc。

```python
from pyquery import PyQuery as pq
doc = pq(url='http://www.baidu.com')
print(doc('head'))
```


### 文件初始化
可以指定filename，读取文件路径的HTML。

```python
from pyquery import PyQuery as pq
doc = pq(filename='demo.html')
print(doc('li'))
```

## 基本CSS选择器

```python
html = '''
<div id="container">
    <ul class="list">
         <li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
print(doc('#container .list li'))
```
输出结果是：

```
<li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
```
这个选择方式层层推进的方式进行选择。这里是查找id为container，class为list的li标签。注意中间需要用空格隔开。

## 查找元素

### 子元素

find会查找当前元素里面满足CSS选择器的所有元素。

```python
html = '''
<div id="container">
    <ul class="list">
         <li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
items = doc('.list')
print(type(items))
print(items)
lis = items.find('li')
print(type(lis))
print(lis)
```
输出结果是：

```
<class 'pyquery.pyquery.PyQuery'>
<ul class="list">
         <li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
 
<class 'pyquery.pyquery.PyQuery'>
<li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
```

这样就可以实现层层嵌套的选择了。

下面的用法是查找直接子元素。

```python
lis = items.children()
print(type(lis))
print(lis)
```
也可以对子元素实现CSS选择器。

```python
lis = items.children('.active')
print(lis)
```

### 父元素

每一个标签外面只可能有一个父元素，可以通过`parent`来获取父元素。

```python
html = '''
<div id="container">
    <ul class="list">
         <li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
items = doc('.list')
container = items.parent()
print(type(container))
print(container)
```
输出为：

```
<class 'pyquery.pyquery.PyQuery'>
<div id="container">
    <ul class="list">
         <li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
 </div>
```
parents返回所有祖先节点。

```python
from pyquery import PyQuery as pq
doc = pq(html)
items = doc('.list')
parents = items.parents()
print(type(parents))
print(parents)
```
当然也可以对祖先节点加入CSS选择器。

```python
parent = items.parents('.wrap')
print(parent)
```

### 兄弟元素

```python
html = '''
<div class="wrap">
    <div id="container">
        <ul class="list">
             <li class="item-0">first item</li>
             <li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         </ul>
     </div>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
li = doc('.list .item-0.active')
print(li.siblings())
```
输出为：

```
<li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0">first item</li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
```
这里选择器有一点不同的地方在于`.item-0`后面没有加空格，直接就是`.active`不加空格表示并列的意思，表示同时包含`.item-0`和`.active`的class。

同样的，我们也可以加入CSS选择器。

```python
from pyquery import PyQuery as pq
doc = pq(html)
li = doc('.list .item-0.active')
print(li.siblings('.active'))
```

## 遍历
这里介绍一下如何对查找的元素进行遍历。

### 单个元素

```python
html = '''
<div class="wrap">
    <div id="container">
        <ul class="list">
             <li class="item-0">first item</li>
             <li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         </ul>
     </div>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
li = doc('.item-0.active')
print(li)
```
输出：

```
<li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
```

### 多个元素
但是这样只会匹配一个单个元素，如果你要查找多个元素，这样就需要用到`item`来遍历了。

```python
html = '''
<div class="wrap">
    <div id="container">
        <ul class="list">
             <li class="item-0">first item</li>
             <li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         </ul>
     </div>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
lis = doc('li').items()
print(type(lis))
for li in lis:
    print(li)
```
输出结果：

```
<class 'generator'>
<li class="item-0">first item</li>
             
<li class="item-1"><a href="link2.html">second item</a></li>
             
<li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             
<li class="item-1 active"><a href="link4.html">fourth item</a></li>
             
<li class="item-0"><a href="link5.html">fifth item</a></li>
```

## 获取信息

### 获取属性

```python
html = '''
<div class="wrap">
    <div id="container">
        <ul class="list">
             <li class="item-0">first item</li>
             <li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         </ul>
     </div>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
a = doc('.item-0.active a')  #空格表示选择里面的内容，就是a里面的内容
print(a)
print(a.attr('href')) #获取属性
print(a.attr.href)   #这两种方法是一样的，都可以用来获取属性信息
```
输出为：

```
<a href="link3.html"><span class="bold">third item</span></a>
link3.html
link3.html
```

### 获取文本

```python
html = '''
<div class="wrap">
    <div id="container">
        <ul class="list">
             <li class="item-0">first item</li>
             <li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         </ul>
     </div>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
a = doc('.item-0.active a')
print(a)
print(a.text())
```
输出为：

```
<a href="link3.html"><span class="bold">third item</span></a>
third item
```

### 获取HTML

```python
html = '''
<div class="wrap">
    <div id="container">
        <ul class="list">
             <li class="item-0">first item</li>
             <li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         </ul>
     </div>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
li = doc('.item-0.active')
print(li)
print(li.html())
```
输出为：

```
<li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             
<a href="link3.html"><span class="bold">third item</span></a>
```

## DOM操作

### addClass、removeClass

```python
html = '''
<div class="wrap">
    <div id="container">
        <ul class="list">
             <li class="item-0">first item</li>
             <li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         </ul>
     </div>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
li = doc('.item-0.active')
print(li)
li.removeClass('active')
print(li)
li.addClass('active')
print(li)
```
输出为：

```
<li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             
<li class="item-0"><a href="link3.html"><span class="bold">third item</span></a></li>
             
<li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
```
### 修改attrs、css

```python
html = '''
<div class="wrap">
    <div id="container">
        <ul class="list">
             <li class="item-0">first item</li>
             <li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         </ul>
     </div>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
li = doc('.item-0.active')
print(li)
li.attr('name', 'link') #如果没有name属性，会添加name=link，如果本来有name属性，会修改为name=link
print(li)
li.css('font-size', '14px')
print(li)
```
输出为：

```python
<li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             
<li class="item-0 active" name="link"><a href="link3.html"><span class="bold">third item</span></a></li>
             
<li class="item-0 active" name="link" style="font-size: 14px"><a href="link3.html"><span class="bold">third item</span></a></li>
```

### remove

```python
html = '''
<div class="wrap">
    Hello, World
    <p>This is a paragraph.</p>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
wrap = doc('.wrap')
print(wrap.text())
wrap.find('p').remove() #先用find方法找到p标签，之后用remove移除p标签
print(wrap.text())
```
输出为：

```
Hello, World This is a paragraph.
Hello, World
```

### 其他DOM方法

http://pyquery.readthedocs.io/en/latest/api.html

## 伪类选择器

可以用CSS3的一些伪类选项器，选择一些特定的元素。

```python
html = '''
<div class="wrap">
    <div id="container">
        <ul class="list">
             <li class="item-0">first item</li>
             <li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         </ul>
     </div>
 </div>
'''
from pyquery import PyQuery as pq
doc = pq(html)
li = doc('li:first-child')
print(li)
li = doc('li:last-child')
print(li)
li = doc('li:nth-child(2)') #指定索引顺序，获取第二个元素标签。
print(li)
li = doc('li:gt(2)') #greater than的缩写，获取序号比2大的标签
print(li)
li = doc('li:nth-child(2n)') #获取偶数标签，如果是奇数可以写成2n+1
print(li)
li = doc('li:contains(second)') #获取指定内容的标签
print(li)
```
输出为：

```
<li class="item-0">first item</li>
             
<li class="item-0"><a href="link5.html">fifth item</a></li>
         
<li class="item-1"><a href="link2.html">second item</a></li>
             
<li class="item-1 active"><a href="link4.html">fourth item</a></li>
             <li class="item-0"><a href="link5.html">fifth item</a></li>
         
<li class="item-1"><a href="link2.html">second item</a></li>
             <li class="item-1 active"><a href="link4.html">fourth item</a></li>
             
<li class="item-1"><a href="link2.html">second item</a></li>
```
更多CSS选择器可以查看 http://www.w3school.com.cn/css/index.asp

## 官方文档

http://pyquery.readthedocs.io/