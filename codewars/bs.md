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
