> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/superboy_liang/article/details/76546458)

对于了解深度学习、自然语言处理 NLP 的读者来说，Word2Vec 可以说是家喻户晓的工具，尽管不是每一个人都用到了它，但应该大家都会听说过它——Google 出品的高效率的获取词向量的工具。  
说到提取关键词，一般会想到 TF-IDF 和 TextRank，大家是否想过，Word2Vec 还可以用来提取关键词？而且，用 Word2Vec 提取关键词，已经初步含有了语义上的理解，而不仅仅是简单的统计了，而且还是无监督的！  
链接: [https://pan.baidu.com/s/1dF7oTH3](https://pan.baidu.com/s/1dF7oTH3) 密码: uw4r  
包含文件：word2vec_wx, word2vec_wx.syn1neg.npy, word2vec_wx.syn1.npy, word2vec_wx.wv.syn0.npy，4 个文件都是 Gensim 加载模型所必需的。具体每个文件的含义我也没弄清楚，word2vec_wx 大概是模型声明，word2vec_wx.wv.syn0.npy 应该就是我们所说的词向量表，word2vec_wx.syn1.npy 是隐层到输出层的参数（Huffman 树的参数），word2vec_wx.syn1neg.npy 就不大清楚了～

```
import sys
try:
    reload(sys)
    sys.setdefaultencoding('utf-8')
except:
    pass

import codecs
from textrank4zh import TextRank4Keyword, TextRank4Sentence
from gensim import models as gmodel
import pandas as pd

model = gmodel.Word2Vec.load("./word2vec/word2vec_wx")




#textrank算法查找text文本中num个关键词
def GetKeyWords(text,num):

    tr4w = TextRank4Keyword()
    tr4w.analyze(text=text, lower=True, window=2)   # py2中text必须是utf8编码的str或者unicode对象，py3中必须是utf8编码的bytes或者str对象
    return tr4w.get_keywords(num, word_min_len=1)

#textrank算法查找text文本中num个关键词中存在的关键短语
def GetKeyPhrases(text,num):

    tr4w = TextRank4Keyword()
    tr4w.analyze(text=text, lower=True, window=2)   # py2中text必须是utf8编码的str或者unicode对象，py3中必须是utf8编码的bytes或者str对象
    return tr4w.get_keyphrases(keywords_num=num, min_occur_num= 2)
#textrank算法查找text文本中num个关键句子
def GetKeySentences(text,num):
    sentence = TextRank4Sentence()
    sentence.analyze(text=text,lower=True)
    s_list = sentence.get_key_sentences(num=num,sentence_min_len=5)  
    return s_list
#word2vec计算两个词的相似度        
def ComputeWordSimilar(word1,word2):
    sim = model.similarity(word1, word2)
    return sim
#word2vec计算两个词列表的相似度
def ComputeWordListSimilar(list1, list2):
    list_sim =  model.n_similarity(list1, list2)
    return list_sim
##word2vec查找最相近的词
def findMostSimilarWord(word):
    result = model.most_similar(word)
    return result



if __name__ == '__main__':     

    text1 = codecs.open('01.txt', 'r', 'utf-8').read()
    text2 = codecs.open('02.txt', 'r', 'utf-8').read()
    #得到关键词
    a=GetKeyWords(text1,10)
    print( '关键词：' )

    '''
    lista、listb 分别表示两个文本提取关键词的到的关键词集合
    '''    

    listb=[]
    lista=[]
    for item in a:
        lista.append(item.word)
        print(item.word, item.weight)

    b=GetKeyWords(text2,10)
    print( '关键词2：' ) 
    for item in b:
        listb.append(item.word)
        print(item.word, item.weight)


    #ComputeWordListSimilar()

    #得到关键词语
    c=GetKeyPhrases(text1,20)
    d=GetKeyPhrases(text2,20)
    print('关键短语：' )
    for phrase in c,d:
        print(phrase)


    # 导入模型

    e=findMostSimilarWord([u'海贼王'])
    print(pd.Series(e))

    f=ComputeWordSimilar(u'活动',u'掌握')
    print(f)
```