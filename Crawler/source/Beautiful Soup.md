# Beautiful Soup
这是一个灵活又方便的网页解析库，处理高效，支持多种解析器。利用它不同编写正则表达式就可以方便的实现网页信息提取。

安装：pip install beautifulsoup4

## 解析库

![image](https://github.com/Einstellung/Crawler_learning/blob/master/Crawler/images/Beautiful%20Soup/1.png?raw=true)

## 基本使用

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.prettify())  #格式化代码，自动进行代码补全，容错处理
print(soup.title.string) #将title内容以string形式输出
```
输出效果如下：

```
<html>
 <head>
  <title>
   The Dormouse's story
  </title>
 </head>
 <body>
  <p class="title" name="dromouse">
   <b>
    The Dormouse's story
   </b>
  </p>
  <p class="story">
   Once upon a time there were three little sisters; and their names were
   <a class="sister" href="http://example.com/elsie" id="link1">
    <!-- Elsie -->
   </a>
   ,
   <a class="sister" href="http://example.com/lacie" id="link2">
    Lacie
   </a>
   and
   <a class="sister" href="http://example.com/tillie" id="link3">
    Tillie
   </a>
   ;
and they lived at the bottom of a well.
  </p>
  <p class="story">
   ...
  </p>
 </body>
</html>
The Dormouse's story
```

## 标签选择器

### 选择元素

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.title)  #选择title标签
print(type(soup.title))
print(soup.head) #选择head标签
print(soup.p)   #选择p标签
```
输出结果：

```
<title>The Dormouse's story</title>
<class 'bs4.element.Tag'>
<head><title>The Dormouse's story</title></head>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
```
可以看到标签选择器，对于如果有多个标签的话，只会输出访问到匹配的第一个标签。

### 获取名称
获取名称指的是获取标签的名称。

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.title.name)
```
输出：title

这是把最外层标签的名称打印输出出来。

### 获取属性

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.p.attrs['name'])
print(soup.p['name'])
```
输出结果：

```
dromouse
dromouse
```
两种获取标签属性的效果是一样的。（获取name属性的值）

### 获取内容

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.p.string)
```
输出结果是：The Dormouse's story

实现输出第一个p标签结果的内容。

### 嵌套选择
还可以用层层递进的方式进行嵌套选择。

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.head.title.string)
```
输出结果：The Dormouse's story

### 子节点和子孙节点

```python
html = """
<html>
    <head>
        <title>The Dormouse's story</title>
    </head>
    <body>
        <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a href="http://example.com/elsie" class="sister" id="link1">
                <span>Elsie</span>
            </a>
            <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> 
            and
            <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
        <p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.p.contents) #将所有子节点内容以列表的形式进行返回
```
输出结果：

```
['\n            Once upon a time there were three little sisters; and their names were\n            ', <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>, '\n', <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, ' \n            and\n            ', <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>, '\n            and they lived at the bottom of a well.\n        ']
```
与`contents`方法不同的是`children`是一个迭代器，用print输出的话，不会输出一个列表，而是迭代器形式的格式，需要通过迭代打印的办法才可以输出我们看起来想要的结果。

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.p.children)
for i, child in enumerate(soup.p.children):
    print(i, child)
```
输出：

```
<list_iterator object at 0x1064f7dd8>
0 
            Once upon a time there were three little sisters; and their names were
            
1 <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
2 

3 <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
4  
            and
            
5 <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
6 
            and they lived at the bottom of a well.
```

还有一个`decendands`的属性，返回的结果也是迭代器类型的。用于获取所有子孙节点。

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.p.descendants)
for i, child in enumerate(soup.p.descendants):
    print(i, child)
```
输出结果是：

```
<generator object descendants at 0x10650e678>
0 
            Once upon a time there were three little sisters; and their names were
            
1 <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
2 

3 <span>Elsie</span>
4 Elsie
5 

6 

7 <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
8 Lacie
9  
            and
            
10 <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
11 Tillie
12 
            and they lived at the bottom of a well.
```
之前我们只是获取a标签里面的内容，这里我们对于a标签的字标签`<span>`的内容也可以获取到了。

### 父节点和祖先节点

```python
html = """
<html>
    <head>
        <title>The Dormouse's story</title>
    </head>
    <body>
        <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a href="http://example.com/elsie" class="sister" id="link1">
                <span>Elsie</span>
            </a>
            <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> 
            and
            <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
        <p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.a.parent) #a标签的父节点
```
输出结果：

```
<p class="story">
            Once upon a time there were three little sisters; and their names were
            <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
            and
            <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
```
这是获取第一个a标签对应的父节点内容。

下面展示获取祖先节点，父节点以及其父亲的父亲节点：

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(list(enumerate(soup.a.parents)))
```
输出结果：

```
[(0, <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
            and
            <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>), (1, <body>
<p class="story">
            Once upon a time there were three little sisters; and their names were
            <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
            and
            <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
<p class="story">...</p>
</body>), (2, <html>
<head>
<title>The Dormouse's story</title>
</head>
<body>
<p class="story">
            Once upon a time there were three little sisters; and their names were
            <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
            and
            <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
<p class="story">...</p>
</body></html>), (3, <html>
<head>
<title>The Dormouse's story</title>
</head>
<body>
<p class="story">
            Once upon a time there were three little sisters; and their names were
            <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
            and
            <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
<p class="story">...</p>
</body></html>)]
```

### 兄弟节点

```python
html = """
<html>
    <head>
        <title>The Dormouse's story</title>
    </head>
    <body>
        <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a href="http://example.com/elsie" class="sister" id="link1">
                <span>Elsie</span>
            </a>
            <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> 
            and
            <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
        <p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(list(enumerate(soup.a.next_siblings)))
print(list(enumerate(soup.a.previous_siblings)))
```
输出结果

```
[(0, '\n'), (1, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>), (2, ' \n            and\n            '), (3, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>), (4, '\n            and they lived at the bottom of a well.\n        ')]
[(0, '\n            Once upon a time there were three little sisters; and their names were\n            ')]
```

## 标准选择器

### find_all( name , attrs , recursive , text , **kwargs )

**可根据标签名、属性、内容查找文档**

#### name

```python
html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.find_all('ul'))
print(type(soup.find_all('ul')[0]))
```
输出结果：

```
[<ul class="list" id="list-1">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>, <ul class="list list-small" id="list-2">
<li class="element">Foo</li>
<li class="element">Bar</li>
</ul>]
<class 'bs4.element.Tag'>
```
可以看到输出的是列表样式。

还可以通过遍历的方式来提取元素：

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
for ul in soup.find_all('ul'):
    print(ul.find_all('li'))
```
输出：

```
[<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>]
[<li class="element">Foo</li>, <li class="element">Bar</li>]
```
这样，来进行层层嵌套的查找。

#### attrs
要求传输参数的类型应该是一个字典形式，按照键值对的规则。

```python
html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1" name="elements">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.find_all(attrs={'id': 'list-1'}))
print(soup.find_all(attrs={'name': 'elements'}))
```
输出结果：

```
[<ul class="list" id="list-1" name="elements">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>]
[<ul class="list" id="list-1" name="elements">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>]
```
实现同样的功能还有一个更简单的办法，不需要使用attrs：

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.find_all(id='list-1'))
print(soup.find_all(class_='element'))
```
结果是：

```
[<ul class="list" id="list-1">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>]
[<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>, <li class="element">Foo</li>, <li class="element">Bar</li>]
```
这里，有一个需要注意的地方时`class`是python里面的一个关键字，所以不能直接作为参数。所以要写成`class_`。

#### text
text就是根据文本内容进行选择。需要注意的是，这里的查找只是对标签里面的文本内容进行查找，不会找到标签。

```python
html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.find_all(text='Foo'))
```
输出结果是：['Foo', 'Foo']

### find( name , attrs , recursive , text , **kwargs )

**find返回单个元素，find_all返回所有元素**

find返回到的是匹配的第一个值。其他用法和find_all是完全一样的。

### find_parents() find_parent()

**find_parents()返回所有祖先节点，find_parent()返回直接父节点**

### find_next_siblings() find_next_sibling()

**find_next_siblings()返回后面所有兄弟节点，find_next_sibling()返回后面第一个兄弟节点**

### find_previous_siblings() find_previous_sibling()

**find_previous_siblings()返回前面所有兄弟节点，find_previous_sibling()返回前面第一个兄弟节点**

### find_all_next() find_next()

**find_all_next()返回节点后所有符合条件的节点, find_next()返回第一个符合条件的节点**

### find_all_previous() 和 find_previous()

**find_all_previous()返回节点后所有符合条件的节点, find_previous()返回第一个符合条件的节点**

## CSS选择器

CSS是层叠样式表(英文全称：Cascading Style Sheets)，是一种用来表现HTML（标准通用标记语言的一个应用）或XML（标准通用标记语言的一个子集）等文件样式的计算机语言。CSS不仅可以静态地修饰网页，还可以配合各种脚本语言动态地对网页各元素进行格式化。

通过select()直接传入CSS选择器即可完成选择。


```python
html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.select('.panel .panel-heading'))
print(soup.select('ul li'))
print(soup.select('#list-2 .element'))
print(type(soup.select('ul')[0]))
```
输出结果是：

```
[<div class="panel-heading">
<h4>Hello</h4>
</div>]
[<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>, <li class="element">Foo</li>, <li class="element">Bar</li>]
[<li class="element">Foo</li>, <li class="element">Bar</li>]
<class 'bs4.element.Tag'>
```
这里介绍一下几种select的选择方式。

**第一种**：选择`panel`里面的`panel-heading`标签。通过符号`.`和空格的方式进行选择。

**第二种**：选择所有ul标签里面的li标签。

**第三种**：通过id进行选择，独有的标志是#。之后加空格和点符号，就可以选择想要选的内容了。

层层迭代的方式还可以这样写：

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
for ul in soup.select('ul'):
    print(ul.select('li'))
```
输出结果是：

```
[<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>]
[<li class="element">Foo</li>, <li class="element">Bar</li>]
```

### 获取属性

```python
html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
for ul in soup.select('ul'):
    print(ul['id'])   #两种获取属性的方法是一样的
    print(ul.attrs['id'])
```
结果是：

```
list-1
list-1
list-2
list-2
```

### 获取内容
获取标签里面的文本内容。

```python
html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
for li in soup.select('li'):
    print(li.get_text())
```
输出结果是：

```
Foo
Bar
Jay
Foo
Bar
```

## 总结

* 推荐使用lxml解析库，解决不了，或者标签混乱时用html.parser
* 标签选择筛选功能弱但是速度快
* 建议使用find()、find_all() 查询匹配单个结果或者多个结果
* 如果对CSS选择器熟悉建议使用select()
* 记住常用的获取属性和文本值的方法






























