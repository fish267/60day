Beautiful Soup

Beautiful Soup 是 html/xml 的 python 解析器, 可以定位 DOM 元素信息.

该工具在[爬虫阶段](http://www.atatech.org/articles/48415) 中, 处于<b>解析网页</b>阶段, 可以简化正则表达式, 简单示例:

```shell
>>> from bs4 import BeautifulSoup as BS
>>> from urllib.request import urlopen
>>> soup = BS(urlopen('https://www.alipay.com/'))
>>> soup.title
<title>支付宝 知托付！</title>
>>> soup.find_all('meta')
[<meta charset="utf-8"/>, <meta content="IE=edge" http-equiv="X-UA-Compatible"/>, <meta content="webkit" name="renderer"/>, <meta content="支付宝,电子支付/网上支付/安全支付/手机支付,安全购物/网络购物付款/付款/收款,水电煤缴费/信用卡还款/AA收款,支付宝网站" name="keywords"/>, <meta content="支付宝，全球领先的独立第三方支付平台，致力于为广大用户提供安全快速的电子支付/网上支付/安全支付/手机支付体验，及转账收款/水电煤缴费/信用卡还款/AA收款等生活服务应用。" name="description"/>]

```

## 安装

1. pip 安装

```shell
pip install beautifulsoup4
```

2. 源码安装

如果没有 <code>easy_install</code> 和 <code>pip</code>, 需要[下载源码](http://www.crummy.com/software/BeautifulSoup/bs4/download/4.0/), 使用 <code>setup.py</code> 来安装.


```python
python3 setup.py install
```
## 使用

将网页源码传入 <code>BeautifulSoup</code>的构造函数, 就能使用该工具的各类方法, 如下:
```python
from bs4 import BeautifulSoup as BS

soup = BS('<html><p>Source Code</p></html>')
print(soup.p.text)
#Source Code
```
### Tag

BeautifulSoup 将 html 代码解析成树状结构, 每个节点都是 Tag 对象, 具有 name 以及 class 等属性.
```python
soup = BS("<div id='soup'><p class='super'>Hello Google!</p></div>")

# 尖括号围起来的就是 Tag, 比如 div a p b meta head body
print(type(soup.p))
# <class 'bs4.element.Tag'>
print(soup.p)
# <p class="super">Hello Google!</p>
print(soup.p.name)
# 'p'
print(soup.div)
# <div id="soup"><p class="super">Hello Google!</p></div>

# 每个 Tag 有部分属性, 比如 class stype type 等, tag['attribute']来调用
print(soup.p['class'])
# ['super']
print(soup.div['id'])
# 'soup'
```
### find || find_all

<code>soup.a</code>只能获取到文档树第一个超链接 tag, 如果需要检索出同类型的 tag, 使用 <code>find_all</code> 方法搞定.

<code>find_all</code> 作为 BeautifulSoup 的过滤器, 是使用最为频繁的方法, 以至于该方法都不用写了, <code>soup.find_all("div")</code>  和 <code>soup("div")</code> 作用一样一样的. 下面仔细说明一下该方法的用法:
```python

soup = BS('''<html><a href='alipay.com'>alipay</a> <div> <a href='google.com'>google</a></div></html>''', 'html.parser')
print(soup.find_all('a'))

# [<a href="alipay.com">alipay</a>, <a href="google.com">google</a>]
```

#### 过滤 Tag

    find_all( name , attrs , recursive , text , **kwargs )

传入字符串或者字符串数组, 作为 Tag 的名字来搜索该 Tag, 比如

```python
soup = BS('''<html><a href='alipay.com'>alipay</a>
                <div> <a href='google.com'>google</a></div>
                <b class='xx'>yahoo</b>
                <h2 id='h2'>shadowsocks</h2>
             </html>''',
          'html.parser')

# 1. 入参为 tag.name
soup.find_all('a')
# [<a href="alipay.com">alipay</a>, <a href="google.com">google</a>]

# 2. 入参为字符串数组 [tag1.name, tag2.name]
print(soup.find_all(['a', 'div']))
# [<a href="alipay.com">alipay</a>, <div> <a href="google.com">google</a></div>, <a href="google.com">google</a>]

# 3. (tag.name, class)
# 查找 class 为 xx, 类型为 <b> 的 Tag
print(soup.find_all('b', 'xx')
# [<b class="xx">yahoo</b>]

# 4. 根据 id 查找, id 可以替换成 href, style,  text, class_(注意有下划线, 防止和关键字冲突)
print(soup.find(id='hh'))
# [<h2 id='h2'>shadowsocks</h2>]

```

#### 支持正则过滤

如果传入正则表达式作为参数,Beautiful Soup会通过正则表达式的 match() 来匹配内容, 简直爽爆了

```python
import re

for tag in soup.find_all(re.compile("^b")):
    print(tag.name)
# body
# b

for tag in soup.find_all(re.compile("t")):
    print(tag.name)
# html
# title
```

再高级一点, 入参可以直接传入个函数, 不深究了.

### select

BeautifulSoup 还支持 [CSS 选择器](http://www.w3school.com.cn/css/css_selector_type.asp), 对于 CSS 玩的溜的人很容易掌握元素定位.

简单举几个例子, 具体使用, 还是以 <code>find_all</code> 为主.

```python

html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""

soup = BS(html_doc, 'html.parser')
print(soup.select('title'))
# [<title>The Dormouse's story</title>]

```

根据子标签嵌套查找, 使用 tag.name > tag1.name > tag2.name, 注意留着空格

```python
print(soup.select('body > p > b'))
# [<b>The Dormouse's story</b>]
```

根据 CSS 类名查找

```python

print(soup.select('sister'))
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

根据 id 来过滤

```python

print(soup.select('#link1'))
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

## 举个例子

比如将 [阿里味儿](https://www.aliway.com/csyy.php), 首页的帖子信息爬下来

### 1. 分析页面代码

点击标题所在的 <tr> , 发现对应的代码如下


```html
<tr align="center" class="tr3  t_one">
        <td><a title="" href="read.php?fid=38&amp;tid=313209" target="_blank"><img src="images/aliway/thread/topicnew.gif" border="0" align="absmiddle" title="开放主题"></a></td>
        <td class="tal" id="td_313209">
            <a href="thread.php?fid=38" class="f12" target="_blank">[业务交流]</a>

            <a href="search.php?step=2&amp;sch_type=21" target="_blank"><img src="images/aliway/file/headtopic_3.gif" align="absmiddle" title="全局置顶主题"></a>

            <a href="read.php?fid=38&amp;tid=313209" id="a_ajax_313209" class="subject" target="_blank">但求天下无假药，人人能买到平价药，良心药！</a>&nbsp;  <span class="gray tpage">( +105 )</span>
            <span class="w">  <img src="images/aliway/file/multipage.gif" border="0" align="absmiddle"> <span style="font-family:verdana;"> <a href="read.php?fid=38&amp;tid=313209&amp;page=1">1</a> <a href="read.php?fid=38&amp;tid=313209&amp;page=2">2</a> <a href="read.php?fid=38&amp;tid=313209&amp;page=3">3</a></span> </span>
    </td>
    <td class="tal y-style"><a href="u.php?action=show&amp;uid=864" class="bl" target="_blank">王磊
(昆阳)
</a>
        <div class="f10 gray">2016-02-25</div></td>
    <td class="tal y-style f10 gray"><span class="s3">52</span>/1570</td>
    <td class="tal y-style">
        <a href="u.php?action=show&amp;uid=81147" target="_blank">王森
    (火梵)
</a><br>
        <span class="f10 gray">2016-02-25 21:54</span>
    </td>
    </tr>
```

通过观察, 发现帖子标题表现格式比较特殊, 有 id 可以作为切入点, 它的父节点是 td, 再上一层是 tr, <code>tag.parent.parent</code>可以获取到 tag 的爷爷节点


    <a href="read.php?fid=38&amp;tid=313209" id="a_ajax_313209" class="subject" target="_blank">但求天下无假药，人人能买到平价药，良心药！</a>&nbsp;  <span class="gray tpage">( +105 )</span>


通过

```python

soup(id=re.compile('a_ajax_'))

```
得到标题的链接和地址, 从上上一层节点中, 读取作者和发布时间信息等

```python
p = tag.parent.parent
p(class_='bl')
# [<a class="bl" href="u.php?action=show&amp;uid=864" target="_blank">王磊(昆阳)</a>]

p(class_='f10 gray'))
# <div class="f10 gray">2016-02-25</div>

### 2. 登陆 BUC

要通过脚本获取阿里味儿的首页代码, 需要经过 BUC 统一登陆, 具体登陆可以看 [Python 使用 Selenium 登陆 BUC](http://www.atatech.org/articles/48446)

### 3. 代码

简单 Demo 如下:
```python
