# iOS程序员如何使用Python写网路爬虫

标签（空格分隔）： 未分类

---

https://github.com/zangqilong198812

链接：http://www.jianshu.com/p/b87413a9307e

写网络爬虫，除了c/c++,第二门语言最好的选择就是python.

**原因就是**

1.语法简单

2.库太多,随便想要什么功能的库都找得到,简直编程界的哆啦A梦.

3.语法优美,不信?你去看看python超过两千行的代码再回头看看用oc写的超过两千行的代码,oc写的简直丑到极致(没命名空间,点语法调用和括号调用混用).

**为什么要会写爬虫?**

春节前有一件活无人认领,我就自告奋勇认领了,具体如下:

> 自己写程序在豆瓣读书上抓取人
> 
> 熊节觉得一个好的程序员应该读过那20本好书
> ——《重构》《精益创业》《敏捷软件开发》《测试驱动开发》等等。他在为ThoughtWorks组建成都分公司团队的时候，发愁正统招聘方法太慢了。于是，他花了几个晚上用自己高中自学的水货代码水平写了一个程序，去抓取豆瓣上读过这些技术书籍的人。然后不断递归，再抓到这些人都读过其它什么书，再继续抓读过那些书的人。抓了几万人之后，他再用Hadoop来分析，筛选出了几十个技术大牛。
> 
> 他把这些大牛的豆瓣账号扔给了公司女HR，让HR去一个个发豆邮勾搭。


春节期间断断续续边看边学写了个爬豆瓣上优秀iOS开发人员的爬虫.所以感觉iOS开发人员有必要掌握这项技术.

再举个例子,你如果想自己弄个app,例如每日精选美女之类的app,你服务端总得有图吧,怎么弄?自己用爬虫爬啊,爬到链接了塞到数据库里,传个json,app直接sdwebimage就好了.多爽!

废话不多说.开始写.

我先假设你用的是mac,然后mac都预装了python2.x,然后呢,你有了python没用,你得有库.没库怎么干活?怎么安装库呢?python界也有个类似于我们iOS开发里cocoapods的东西,这个东西叫做pip.

pip和cocoapods用起来的命令都极其类似,我们只需要两个库,一个叫做urllib2,一个叫做beautifulsoup.

urllib2是干什么的呢?它的作用就是把网页down下来,然后你就可以分析网页了.

beautifulsoup干什么的呢?你用urllib2把网页down下来了之后,里面都是html+css什么的,你想要从乱七八糟的一堆html里面找到正确的图片链接那可不是件简单的事,据我这几天的学习,做法无非两个,一个是自己写正则表达式然后用一个叫re的python库,另一个是使用lxml解析xpath.这两个说实话都不太好用,一个正则就够你吃一壶的.后来我搜索了很久,发现了一个库叫做beautifulsoup,用这个库解析html超级好用.

然后你们打开terminal敲入下面这个命令.
```
pip install BeautifulSoup
```
然后就会自动帮你安装BeautifulSoup这个东西了.urllib2因为是自带的,所以不用你下载了.

好的我们打www.dbmeizi.com,这个邪恶的网站,首页都是软妹子.直接右键打开源文件.

你看到的是这些东西.
![ ](http://www.huochai.mobi/p/refer_image/?image=http://mmbiz.qpic.cn/mmbiz/NVvB3l3e9aFVDkor3IblVD6AnPefcAnZeav2mWT19h2NEzviaaVMibK8f33a07qfc40Sh7cVj7cZndicGyyDSHM5A/0?wx_fmt=jpeg)

你看到的和乱码没什么区别,但是我们需要仔细观察.终于找到了图片的链接.
![ ](http://www.huochai.mobi/p/refer_image/?image=http://mmbiz.qpic.cn/mmbiz/NVvB3l3e9aFVDkor3IblVD6AnPefcAnZrzGswYeDr4JprhZ5IoHJPrC1G6u0grY99F2Uq9YuBUyuaNUxTNIJCg/0?wx_fmt=jpeg)

图片链接就在li这个标签下地img标签里.现在我们需要做的就是尝试着把这种类型的li从所有html中分离出来.我们可以看到li这个标签有个属性叫做class,这个属性的值是class="span3",我们把这段话li class="span3"

搜索一下,我们发现有20个结果.恰巧,我们这个页面的图片也只有20个,那么可以确定的是我们找到了区别于其他标签的唯一性.

再仔细分析下,img这个标签在li这个标签里有且只有一个.那么,也就是说,我们先搜索出所有符合条件的li标签,然后找到里面的img标签就可以找到所有的图片链接了.

然后看代码.
```
#!/usr/bin/python

#-*- coding: utf-8 -*-

#encoding=utf-8


import urllib

import os

from BeautifulSoup import BeautifulSoup

def getAllImageLink():

html = urllib2.urlopen('http://www.dbmeizi.com').read()

soup = BeautifulSoup(html)


liResult = soup.findAll('li',attrs={"class":"span3"})


for li in liResult:

imageEntityArray = li.findAll('img')

for image in imageEntityArray:

link = image.get('data-src')

imageName = image.get('data-id')

filesavepath = '/Users/weihua0618/Desktop/meizipicture/%s.jpg' % imageName

urllib.urlretrieve(link,filesavepath)

print filesavepath


if __name__ == '__main__':

getAllImageLink()
```
我们来一句一句分析下.其实python的语法超级简单.

凡是#打头的就是python里面的注释语句类似于oc里的//.

分别说明我们的环境是python,编码是utf-8

然后import了四个库,分别是urllib2,urllib,os,和beautifulsoup库.

导入beautifulsoup库的方式和其他三个不太一样.我暂时也不清楚为什么python用这种导入方式,不过照猫画虎就行了.

然后def打头的就是定义一个函数,python里面是不用分号做句与句的分隔符的.他用缩进来表示.与def缩进一个tab的都是函数体.
```
html = urllib2.urlopen('http://www.dbmeizi.com').read()
```
这句很简单,就是读取网页的html.然后把值赋给html这个变量.python里声明变量前面不用加任何东西,不用加声明语句和变量类型,就连javascript声明变量还要加个var呢.

我们获取了网页的html之后呢,声明了一个beautifulsoup变量soup,用来准备解析html.
```
liResult = soup.findAll('li',attrs={"class":"span3"})
```
这句话的意思就是,寻找html中所有li标签,并且这个li标签有个属性class,class的值是span3.

注意这个findAll函数,有点常识的话你应该清楚,凡是带all的函数基本上返回的都是一个数组,所以我们liResult这个变量实际上是一个数组.
```
for li in liResult:
```
这句话基本和oc里的遍历数组语法完全一样.就是遍历liResult里的每一个变量.那么每一个变量就是一个\

●标签.
```
imageEntityArray = li.findAll('img')
```
获得了li标签,我们再找出所有的img标签.

一样的道理,遍历所有img标签(实际上只有一个).
```
link = image.get('data-src')

imageName = image.get('data-id')
```
这两句的意思就是,获取img标签里的'data-src'属性和'data-id'属性,data-src就是我们最想要的图片链接了.data-id我们会用来当做下载图片之后的名字.
```
filesavepath = '/Users/weihua0618/Desktop/meizipicture/%s.jpg' % imageName

urllib.urlretrieve(link,filesavepath)
```
这两句,第一句是设置一个文件存放地址,第二句用urllib这个库的urlretrieve这个方法下载我们的图片,并且把图片放到刚才的路径里.

![ ](http://www.huochai.mobi/p/refer_image/?image=http://mmbiz.qpic.cn/mmbiz/NVvB3l3e9aFVDkor3IblVD6AnPefcAnZbnJYBJzic5Uyp5ktmeV1eoIPvQWDF1wLk8kdPozNr8dc4KnvC0ZVkaA/0?wx_fmt=gif)

好了,我们的图片就下载完了.

说说我是怎么爬虫所有豆瓣ios开发的,我先找到所有标签为ios开发的书籍,然后把所有书的id抓到,然后用id找到所有阅读过书的用户id,把所有用户id抓下来之后用hadoop分析,哪些用户id读过的书最多,列出前一百个.然后,你们懂得...(昨天我的ip还是mac地址已经被豆瓣封了)

我感觉,我可以在简历上郑重的写下"精通python和大数据分析" -_-!




