> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/46922291)

﻿最近项目需要，实现文本的词频分析，折腾了几天才完成任务，有点成就感，最后整理总结一下这部分的内容，希望更多同僚受益。

一. 使用前准备
--------

**环境**：Python3.6

*   安装结巴：`pip install jiaba`
*   下载停用词词典[哈工大停用词词典](https://link.zhihu.com/?target=https%3A//download.csdn.net/my/uploads)
*   构建补充词典`userdict`，后文详解
*   运行文章最后面的完整代码

二. jieba 官网
-----------

[参考​gitee.com](https://link.zhihu.com/?target=https%3A//gitee.com/fxsjy/jieba/)

三. 功能介绍
-------

"结巴" 中文分词：做最好的 Python 中文分词组件

*   支持三种分词模式：

*   **精确模式**，试图将句子最精确地切开，适合文本分析；
*   **全模式**，把句子中所有的可以成词的词语都扫描出来, 速度非常快，但是不能解决歧义；
*   **搜索引擎模式**，在精确模式的基础上，对长词再次切分，提高召回率，适合用于搜索引擎分词。

四. 算法思路
-------

*   基于 Trie 树结构实现高效的词图扫描，生成句子中汉字所有可能成词情况所构成的有向无环图（DAG)
*   采用了动态规划查找最大概率路径, 找出基于词频的最大切分组合
*   对于未登录词，采用了基于汉字成词能力的 HMM 模型，使用了 Viterbi 算法

五. 使用方法
-------

1、分词
----

*   `jieba.cut`方法接受两个输入参数: 1) 第一个参数为需要分词的字符串 2）cut_all 参数用来控制是否采用全模式
*   `jieba.cut_for_search`方法接受一个参数：需要分词的字符串, 该方法适合用于搜索引擎构建倒排索引的分词，粒度比较细 注意：待分词的字符串可以是 gbk 字符串、utf-8 字符串或者 unicode
*   `jieba.cut`以及`jieba.cut_for_search`返回的结构都是一个可迭代的 generator，可以使用 for 循环来获得分词后得到的每一个词语 (unicode)，也可以用 list(jieba.cut(...)) 转化为 list

代码

```
#encoding=utf-8
import jieba

seg_list = jieba.cut("我来到北京清华大学", cut_all=True)
print "Full Mode:", "/ ".join(seg_list)  # 全模式

seg_list = jieba.cut("我来到北京清华大学", cut_all=False)
print "Default Mode:", "/ ".join(seg_list)  # 精确模式

seg_list = jieba.cut("他来到了网易杭研大厦")  # 默认是精确模式
print ", ".join(seg_list)

seg_list = jieba.cut_for_search("小明硕士毕业于中国科学院计算所，后在日本京都大学深造")  # 搜索引擎模式
print ", ".join(seg_list)
```

输出：

```
【全模式】: 我/ 来到/ 北京/ 清华/ 清华大学/ 华大/ 大学

【精确模式】: 我/ 来到/ 北京/ 清华大学

【新词识别】：他, 来到, 了, 网易, 杭研, 大厦    (此处，“杭研”并没有在词典中，但是也被Viterbi算法识别出来了)

【搜索引擎模式】： 小明, 硕士, 毕业, 于, 中国, 科学, 学院, 科学院, 中国科学院, 计算, 计算所, 后, 在, 日本, 京都, 大学, 日本京都大学, 深造
```

2、自定义词典补充
---------

*   开发者可以指定自己自定义的词典，以便包含 jieba 词库里没有的词。虽然 jieba 有新词识别能力，但是自行添加新词可以保证更高的正确率  
    
*   用法： `jieba.load_userdict(file_name)` # file_name 为自定义词典的路径  
    
*   词典格式和`dict.txt`一样，一个词占一行；每一行分三部分，一部分为词语，另一部分为词频，最后为词性（可省略），用空格隔开  
    
*   `userdict.txt`即补充词库示例

```
极速模式 20
北京清华大学 5
李小福 2 nr
创新办 3 i
easy_install 3 eng
好用 300
韩玉赏鉴 3 nz
八一双鹿 3 nz
台中
凱特琳 nz
Edu Trust认证 2000
```

*   用法示例：

```
#encoding=utf-8
from __future__ import print_function, unicode_literals
import sys
sys.path.append("../")
import jieba
jieba.load_userdict("userdict.txt")
import jieba.posseg as pseg

jieba.add_word('石墨烯')
jieba.add_word('凱特琳')
jieba.del_word('自定义词')

test_sent = (
"李小福是创新办主任也是云计算方面的专家; 什么是八一双鹿\n"
"例如我输入一个带“韩玉赏鉴”的标题，在自定义词库中也增加了此词为N类\n"
"「台中」正確應該不會被切開。mac上可分出「石墨烯」；此時又可以分出來凱特琳了。"
)
words = jieba.cut(test_sent)
print('/'.join(words))

print("="*40)

result = pseg.cut(test_sent)

for w in result:
    print(w.word, "/", w.flag, ", ", end=' ')

print("\n" + "="*40)

terms = jieba.cut('easy_install is great')
print('/'.join(terms))
terms = jieba.cut('python 的正则表达式是好用的')
print('/'.join(terms))

print("="*40)
# test frequency tune
testlist = [
('今天天气不错', ('今天', '天气')),
('如果放到post中将出错。', ('中', '将')),
('我们中出了一个叛徒', ('中', '出')),
]

for sent, seg in testlist:
    print('/'.join(jieba.cut(sent, HMM=False)))
    word = ''.join(seg)
    print('%s Before: %s, After: %s' % (word, jieba.get_FREQ(word), jieba.suggest_freq(seg, True)))
    print('/'.join(jieba.cut(sent, HMM=False)))
print("-"*40)
```

之前： 李小福 / 是 / 创新 / 办 / 主任 / 也 / 是 / 云 / 计算 / 方面 / 的 / 专家 /

加载自定义词库后：　李小福 / 是 / **创新办** / 主任 / 也 / 是 / **云计算** / 方面 / 的 / 专家 /

3、停用词词库补充
---------

*   停用词：停用词是指在信息检索中，为节省存储空间和提高搜索效率，在处理自然语言数据（或文本）之前或之后会自动过滤掉某些字或词，这些字或词即被称为 Stop Words（停用词）。这些停用词都是人工输入、非自动化生成的，生成后的停用词会形成一个停用词表。但是，并没有一个明确的停用词表能够适用于所有的工具。甚至有一些工具是明确地避免使用停用词来支持短语搜索的。  
    
*   使用哈工大停用词词库：

[下载地址​download.csdn.net](https://link.zhihu.com/?target=https%3A//download.csdn.net/my/uploads)

4、词频统计
------

[PYTHON3.6 对中文文本分词、去停用词以及词频统计_WhiteRiver 的博客 - CSDN 博客​blog.csdn.net![图标](https://pic1.zhimg.com/v2-1df35caab48f4baf0947c6929a398504_180x120.jpg)](https://link.zhihu.com/?target=https%3A//blog.csdn.net/qq_27882113/article/details/78126952)

完整代码：

```
from collections import Counter
import jieba
jieba.load_userdict('userdict.txt')

# 创建停用词list
def stopwordslist(filepath):
    stopwords = [line.strip() for line in open(filepath, 'r').readlines()]
    return stopwords

# 对句子进行分词
def seg_sentence(sentence):
    sentence_seged = jieba.cut(sentence.strip())
    stopwords = stopwordslist('G:\\哈工大停用词表.txt')  # 这里加载停用词的路径
    outstr = ''
    for word in sentence_seged:
        if word not in stopwords:
            if word != '\t':
                outstr += word
                outstr += " "
    return outstr


inputs = open('hebing_wenben\\wenben.txt', 'r') #加载要处理的文件的路径
outputs = open('output.txt', 'w') #加载处理后的文件路径
for line in inputs:
    line_seg = seg_sentence(line)  # 这里的返回值是字符串
    outputs.write(line_seg)
outputs.close()
inputs.close()
# WordCount
with open('output.txt', 'r') as fr: #读入已经去除停用词的文件
    data = jieba.cut(fr.read())
data = dict(Counter(data))

with open('cipin.txt', 'w') as fw: #读入存储wordcount的文件路径
    for k, v in data.items():
        fw.write('%s,%d\n' % (k, v))
```

相关文章
----

[手把手教你制作 中英文 词云 | python demo​blog.csdn.net![图标](https://pic1.zhimg.com/v2-d60b392fcfb79d0c60b4a510c7106798_120x160.jpg)](https://link.zhihu.com/?target=https%3A//blog.csdn.net/qq_30262201/article/details/78801367)