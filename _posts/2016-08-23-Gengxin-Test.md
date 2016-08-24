---
layout: post
title: "Gengxin's First Test"
date:   2016-08-23
excerpt: "Go on."
tag:
- test
comments: true
mine: true
image: "http://zhangone.top/image/post.png"
---

![Test Image](http://zhangone.top/image/post.png)

<button class="btn shake shake-slow"> Test </button>

# This is a test  

## This is a test  

### This is a test  
this is a test.  
just a test

~~~   
This is a test   
~~~   
<div markdown="0"><a href="#" class="btn">This is a Test</a></div>
{% highlight python %}
# -*- coding: UTF-8 -*-
import sys  
from os import path
import jieba  
import jieba.analyse  
from PIL import Image
import numpy as np
from optparse import OptionParser  
import matplotlib.pyplot as plt
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
USAGE = "usage:    python extract_tags.py [file name] -k [top k]"  
d = path.dirname(__file__)
parser = OptionParser(USAGE)  
parser.add_option("-k", dest="topK")  
opt, args = parser.parse_args()  
if len(args) < 1:  
    print USAGE
    sys.exit(1)

file_name = args[0]                          
if opt.topK is None:  
    topK = 10  
else:  
    topK = int(opt.topK)                                     
content = open(file_name, 'rb').read()                              
tags = jieba.analyse.extract_tags(content, topK=topK, withWeight=True)

words = []

for tag in tags:
    words.append( (tag[0],tag[1]*1000))

wordcloud = WordCloud(font_path="方正喵呜体.ttf").fit_words(words)
alice_coloring = np.array(Image.open("xhr.jpeg"))

wc = WordCloud(background_color="white",max_words=2000, mask=alice_coloring,
        #stopwords = STOPWORDS.add("said"),
        max_font_size = 70,
        random_state = 43,
        prefer_horizontal = 0.7,
        font_path = "方正喵呜体.ttf",
        scale = 15)

wc.fit_words(words)

image_colors = ImageColorGenerator(alice_coloring)
plt.imshow(wc.recolor(color_func=image_colors))
plt.axis("off")
plt.figure()
plt.imshow(wordcloud)
plt.axis("off")
'''
plt.figure()
plt.imshow(alice_coloring,cmap=plt.cm.gray)
plt.axis("off")
'''
plt.show()
wc.recolor(color_func=image_colors).to_image().save("3.jpg")
wordcloud.to_image().save("4.jpg")

{% highlight %}

<kbd>T</kbd><kbd>h</kbd><kbd>i</kbd><kbd>s</kbd><kbd>-</kbd><kbd>a</kbd><kbd>-</kbd><kbd>t</kbd><kbd>e</kbd><kbd>s</kbd><kbd>t</kbd>  
**This is a test**  
`This is a test`
