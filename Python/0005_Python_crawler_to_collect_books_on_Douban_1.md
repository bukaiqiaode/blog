# Python爬取douban图书信息与趟坑笔记(一)

> 本系列记录了使用Python爬取douban图书信息的方式方法，以及个中的趟坑经验

## Python的版本
```
  Python 2.7.13, under Windows 10 64bit.
```

## 安装需要的类库
```shell
  pip install urllib
  pip install beautifulsoup4
```

## 选择一个起手的页面
爬哪些页面呢？我找来找去，觉得就从这个250的列表开始了: https://book.douban.com/top250?start=0

## 写一个起手的代码
新建一个(.py)文件，就叫 ```db_script.py``` 了。
代码如下:
```python
    # -*- coding: UTF-8 -*-    

    if __name__ == "__main__":
        print 'done'
```
运行一下，先证明我们没有起步就带bug出门。
- Python_0005_0001.png

## 把刚才安装的库引入进来
```python
    # -*- coding: UTF-8 -*-

    from bs4 import BeautifulSoup
    import urllib

    if __name__ == "__main__":
        print 'done'
```

## 写一个函数，读取一个网页回来
写一个函数，接受一个url作为参数，请求这个url并返回请求的结果。在这个函数中，我们使用urllib库来发送网络请求。
```python
    # -*- coding: UTF-8 -*-

    from bs4 import BeautifulSoup
    import urllib


    def get_html_text(url):
        '''
        Given url, query the page-content using this url
        '''
        try:
            resp = urllib.urlopen(url)
            html_data = resp.read().decode('utf-8')
            return html_data
        except:
            #just return empty-string if any failure.
            return ""

    if __name__ == "__main__":
        print 'done'
```

## 尝试请求目标页面
简单粗暴，请求目标页面并打印出它的内容
```python
    # -*- coding: UTF-8 -*-

    from bs4 import BeautifulSoup
    import urllib


    def get_html_text(url):
        '''
        Given url, query the page-content using this url
        '''
        try:
            resp = urllib.urlopen(url)
            html_data = resp.read().decode('utf-8')
            return html_data
        except:
            #just return empty-string if any failure.
            return ""

    if __name__ == "__main__":
        url = "https://book.douban.com/top250?start=0"

        html_data = get_html_text(url)
        print html_data

        print 'done'
```
运行一把，看看效果。
- Python_0005_0002.png

华丽丽的失败啊！！！
为什么？怎么解决？请看本系列的第二篇~~
