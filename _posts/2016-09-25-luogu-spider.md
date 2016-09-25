---
layout: post
title: Gengxin的第一次“大数据分析”
tag:
- python
date: 2016-09-25
excerpt: "有点无聊的爬取了洛谷题库所有题面，学知乎上大哥哥们数据分析一把~"
image: http://images2015.cnblogs.com/blog/831801/201609/831801-20160925192053469-2080465632.jpg
feature: http://images2015.cnblogs.com/blog/831801/201609/831801-20160925192053469-2080465632.jpg
---

这是Gengxin拖了好几天的计划  

在知乎，dalao们的专栏里各种吊炸天的高逼格文章，看得我是心潮澎湃。从一开始看到<a href="https://zhuanlan.zhihu.com/666666">段小草</a>专栏的某篇文章开始了解大数据以来，一直在想自己什么时候也要试着分析点东西~~毕竟装逼是多么的快乐~~。 前几天，像往常一样打开知乎，忽然看到了这么一个标题————<a href="https://zhuanlan.zhihu.com/p/22541207">爬取了bilibili站644w视频信息之后的故事。</a> 噫！b站！嗯，很好，你成功引起了我的注意~~（霸道总裁脸）~~。 点开链接，哇！好有逼格的总结，分析的很对嘛！一脸崇拜。里面提到的视频，正是我这几天刚刚看到的，比如说坂本大佬啊，咬人猫的极乐净土啊（逃……等等等等。   

真是神奇呢……  

为什么我不试试呢？？  

好~试试就试试。那我要爬什么呢？  

以前我因为无聊写的Python小爬虫也不少，不过都是写玩玩的东西，像爬取之家新闻啦、爬取作业帮搜索结果啦、博客园闪存自动刷星星啦什么的，没什么技术含量，都是明摆在那里的数据，直接正则表达式匹配代替人手工<kbd>Ctrl+C</kbd>、<kbd>Ctrl+V</kbd>而已。既然自己水平不够，那就来个简单点的好了~以前想些OJ助手的时候曾经试着爬过国内的几个OJ，发现他们居然都没有防爬虫机制~~（或者是说被我不小心绕过了）~~……好，就从<a href="http://www.luogu.org/">洛谷</a>下手了。  

首先，直接`urllib.urlopen()`试试。果然，洛谷不但可以不登陆查看题目，而且可以直接获取到页面html。好像没有任何的防爬虫机制~~（应该仅仅是好像）~~。
先写下这么一段代码：  

{% highlight python %}
#-*- coding:utf-8 -*
import time,sys
import urllib
import urllib2
import random
import re


def get_header(pid):
    headers=[
    "Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; rv:11.0) like Gecko",
    "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36",
    "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:48.0) Gecko/20100101 Firefox/48.0",
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.79 Safari/537.36 Edge/14.14393"
    ]
    Referer='http://www.luogu.org/problem/lists?name=&orderitem=&order=&tag=&page='+str((pid-1000)/50+1)
    return {'User-Agent':headers[random.randint(0,3)],'Referer':Referer}

def get_html(url,pid):
    req = urllib2.Request(url=url, headers=get_header(pid))
    response = urllib2.urlopen(req)
    return response.read()

def main():
    pidlist = range(1001,3392)
    num = len(pidlist)
    for i in range(num):
        pid = random.choice(pidlist)
        pidlist.remove(pid)
        url = "http://www.luogu.org/problem/show?pid="+str(pid)
        html = get_html(url,i)

if __name__ == '__main__':
    main()

{% endhighlight %}

这就是Gengxin的小爬虫的基本框架了。因为是新手嘛，也不知道太多，比较习惯把获取html啊什么的放到单独的函数里。因为之前在洛谷群里见过有人因为作死被kkk封ip的事件，即使没有发现有防爬虫机制，我也小心地加上了headers，请求里带上`User-Agent` 和 `Referer`。
运行程序，看了输出，不错，题目信息都包含在了html。
其他的，都是些小爬虫基本的东西，Gengxin就先不再详细的说了，代码放在Gengxin的Github上了，欢迎各位大神看看~~（临幸）~~，提点意见~  

经过好长时间的改啊改，终于，爬虫算是写好了。所有数据都能准确获取，而且对异常情况做出了必要的处理。  
好了，加上数据库相关的代码，运行。  
经过四个多小时的努力，Gengxin的小爬虫，终于爬到了洛谷的大部分题目。这里之所以说是大部分，是因为在前几次试着爬的时候，发现洛谷上居然有几个特殊的题目，没有描述、输入输出样例，或者直接是NOIP初赛题目……这些题目都被Gengxin的小爬虫pass掉了。

<figure>
  <a href="http://images2015.cnblogs.com/blog/831801/201609/831801-20160925213913553-1874832292.png"><img src="http://images2015.cnblogs.com/blog/831801/201609/831801-20160925213913553-1874832292.png"></a>
</figure>  

很好，总共爬取了2356道题目(好像计数是从0开始的)。
我们的数据库，从2kb变成了3.97M
<figure>
  <a href="http://images2015.cnblogs.com/blog/831801/201609/831801-20160925214244646-2017303201.png"><img src="http://images2015.cnblogs.com/blog/831801/201609/831801-20160925214244646-2017303201.png"></a>
</figure>  

好了，爬取的过程算是结束了。接着就要看能从这些数据里得到些什么了。
