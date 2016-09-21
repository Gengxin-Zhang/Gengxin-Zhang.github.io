---
layout: post
title: 在服务姬上搞事情
tag:
- project
date: 2016-09-19
excerpt: "今天教练批下来一台闲置电脑给Gengxin，于是我用它搭起了服务器~~姬~~"
image: http://images2015.cnblogs.com/blog/831801/201609/831801-20160919213506340-1828196577.png
feature: http://images2015.cnblogs.com/blog/831801/201609/831801-20160919213329590-1081767775.png
---

昨天，教练为了迎接新二队的到来，把原本只有三圈一体机的奥赛室，强行改装成了一个小机房……于是乎我终于盼到了不小心多搬来的那台闲置电脑\\(0_o)/.  
~~终于可以尽情调教服务姬了~~  

拿到我亲爱的服务姬，首先，格掉瘟毒死……嗯，没有硬还原的学校电脑真是好。接着，拿出我的小U盘，请出了我们的Ubuntu桑~~ 教练批给我的这台电脑还是不错的，是台原本就放在奥赛室的一体机，配置见此文封面图…… ~~好叭在这里也放一下~~  
<figure>
  <a href="http://images2015.cnblogs.com/blog/831801/201609/831801-20160919213506340-1828196577.png"><img src="http://images2015.cnblogs.com/blog/831801/201609/831801-20160919213506340-1828196577.png"></a>
</figure>

4G内存+Pentium双核CPU…… 起码应付点日常~~能让我免费随便搞~~还是够了……  

## One · LierOJ  
系统装好后第一件事就是sudo apt update，这个不用说……  
装好了系统，第一件事就是部署我的LierOJ~  
因为OJ还在开发中，所以暂时就先不用apache和wsgi啥啥的了，直接python manege.py runserver就好了~  
于是我很自信的git clone下来了刚刚上传的代码，runserver 0.0.0.0:8000  
等等，好像出错了！  
Gengxin一拍脑袋…… 哦对！我还用了一大堆库呢！！赶紧pip install开起来  
让我想想，当时都用过什么呢？？哦~ 处理头像PILLOW不能少，用户分析用了Wordcloud做标签云，异步+定时任务用了django-celery，支持Markdown用了django-makedown-deux……嗯……对pip install……

好了，服务姬终于不再报错了~  

## Two · Samba

很久以前Gengxin在奥赛室用树莓派做服务器的时候，用树莓派搭过一个NAS，还给它3D打印了一个壳子，历二奥赛队得以在装着还原的机房里愉快地生存下去……可是……因为Gengxin的不小心，树莓派进水坏掉了……于是曾经那个可怜的服务姬就再也没出现过。现在终于有了新的服务姬，单单运行一个LierOJ实属浪费，于是Gengxin做的第二件事，就是再搭一个文件共享系统。  
首先第一步，
~~~
sudo apt update
~~~
~~不用说也是这个~~  
接着，安装我们的Samba。
~~~
sudo apt install samba
~~~
