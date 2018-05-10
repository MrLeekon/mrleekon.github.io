# <Python>[用BeautifulSoup库抓取信息时去掉字符串首尾空白的几种方法](http://woodenrobot.me/2016/09/22/用BeautifulSoup库抓取信息时去掉字符串首尾空白的几种方法/)
## 前言
在抓取网页信息时经常遇到很多头尾加了空格的字符串，在此介绍几种处理的小技巧。

### 例子
1.

```
<p>     woodenrobot       </p>
```

2.

```
<p>   woodenrobot1<em>  woodenrobot2  </em>  </p>
```

## 方法

### 对于例1

如果遇到例1这种情况下面几种方法可以通用。

```python
from bs4 import BeautifulSoup
html = '<p>     woodenrobot       </p>'
soup = BeautifulSoup(html)
a = soup.get_text()
b = soup.get_text().strip()
c = soup.get_text(strip=True)
d = soup.strings
e = soup.stripped_strings
print('a: %s\nb: %s\nc: %s\nd: %s\ne: %s' % (a, b, c, list(d), list(e)))
```

输出结果如下：

```python
a:    woodenrobot  
b: woodenrobot
c: woodenrobot
d: ['   woodenrobot  ']
e: ['woodenrobot']
```

其中a与d未处理去掉首尾空格，d, e是遍历整个子孙节点得到一个生成器。

### 对于例2

```python
from bs4 import BeautifulSoup
html = '<p>   woodenrobot1<em>   woodenrobot2  </em>  </p>'
soup = BeautifulSoup(html)
a = soup.get_text()
b = soup.get_text().strip()
c = soup.get_text(strip=True)
d = soup.strings
e = soup.stripped_strings
print('a: %s\nb: %s\nc: %s\nd: %s\ne: %s' % (a, b, c, list(d), list(e)))
```

输出结果：

```python
a:    woodenrobot1  woodenrobot2   
b: woodenrobot1  woodenrobot2
c: woodenrobot1woodenrobot2
d: ['   woodenrobot1', '  woodenrobot2  ', ' ']
e: ['woodenrobot1', 'woodenrobot2']
```

通过结果我们知道对于复杂一点的特殊结构这个三种方法还是有一些差异存在，所以我们需要根据不同的需求选择不同的方法。

