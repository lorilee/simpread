> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.zhihu.com](https://www.zhihu.com/question/29978268) ![](https://pic4.zhimg.com/dd7d9b0bb9c84d5397a8d2a5448de8ea_xs.jpg?source=1940ef5c)严昕

现在是 2018 年 7 月，在这里总结这个问题下已有的答案，并补充一些 2017 年以来这方面研究的新进展。

从大类上分，计算句子相似度的方法可以分为两类：

1）无监督的方法，即不使用额外的标注数据，常用的方法有：

（1）对句子中所有词的 word vector 求平均，获得 sentence embedding

（2）以每个词的 tf-idf 为权重，对所有词的 word vector 加权平均，获得 sentence embedding

（3）以 smooth inverse frequency[1]（简称 SIF) 为权重，对所有词的 word vector 加权平均，最后从中减掉 principal component，得到 sentence embedding

（4）通过 Word Mover’s Distance[2]（简称 WMD），直接度量句子之间的相似度

2）有监督的方法，需要额外的标注数据，常见的有监督任务有：

（1）分类任务，例如训练一个 CNN 的文本分类器 [3]，取最后一个 hidden layer 的输出作为 sentence embedding，其实就是取分类器的前几层作为预训练的 encoder

（2）sentence pair 的等价性 / 等义性判定（[4][5]），这种方法的好处是不仅可以得到 sentence embedding，还可以直接学习到距离度量函数里的参数

Yves Peirsman 的这篇博客 [6] 里比较了常见方法在计算句子相似句上的效果：

![](https://pic4.zhimg.com/v2-5c14c164e4c718720b12e5cfdcad6cc6_r.jpg?source=1940ef5c)

从图中可以看到在这几个评估数据上：

1）WMD 方法（WMD-W2V）相对于其他无监督方法并没有明显的优势

2）简单加权的词向量平均（AVG-W2V 和 AVG-W2V-TFIDF）已经可以作为一个较好的 baseline，但是考虑 SIF 加权的词向量平均（SIF-W2V) 通常可以得到更好的效果

3）这里比较了两种预训练的 encoder（InferSent/INF 和 Google's Sentence Encoder/GSE），相对而言 GSE 效果更好一些，但要注意它的性能并不一定在所有的数据集上都稳定

另外，从实践中的经验来看，如果要在无标注的数据上从零开始实现相似度的计算，可以综合几种方法来得到更好的效果。一种可能的步骤如下：

1）使用某种无监督方法，对于句子集合做简单归类和标注

2）通过 1 中得到的标注数据，训练分类模型或者句子对模型，从而得到相似度模型

3）评估模型效果，适当引入新的标注数据，重复步骤 1）2）

Reference：

[1] Sanjeev Arora, et al. 2017. A Simple but Tough-to-Beat Baseline for Sentence Embeddings

[2] Matt J. Kusner, et al. 2015. From Word Embeddings To Document Distances

[3] Yoon Kim. 2014. Convolutional Neural Networks for Sentence Classification

[4] Jonas Mueller, et al. 2016. Siamese Recurrent Architectures for Learning Sentence Similarity

[5] Paul Neculoiu, et al. 2016. Learning Text Similarity with Siamese Recurrent Networks

[6] Yves Peirsman. 2018. Comparing Sentence Similarity Methods

 ![](https://pic4.zhimg.com/498750efe6d6a9af84d422d8be80e9cd_xs.jpg?source=1940ef5c) 鲁灵犀之前在 bat 做过这个，和大家分享一下。  
先说一个还是从词的角度出发考虑的，最后的效果非常好，就是怎么样从词的向量得到句子的向量，首先选出一个词库，比如说 10 万个词，然后用 w2v 跑出所有词的向量，然后对于每一个句子，构造一个 10 万维的向量，向量的每一维是该维对应的词和该句子中每一个词的相似度的最大值。这种方法实际上是 bag of words 的一个扩展，比如说对于 我喜欢用苹果手机 这么一句话，对应的向量，会在三星，诺基亚，小米，电脑等词上也会有比较高的得分。这种做法对于 bag of words 的稀疏性问题效果非常好。  
还做过一个直接训练句子的相似度的一个 query2vec 模型，效果也不错，就不细说了。![](https://pic3.zhimg.com/ef1b5e459_xs.jpg?source=1940ef5c)山同气先回答问题，最简单的方法可以用 doc2vec from gensim  
=============================================  
为啥要用 doc2vec 换句话说，为什么 doc2vec 是性价比最高的选择。注意我一直强调简单或性价比最高，没说这是唯一解决方案。  
我们先看看其他的解决方法：  
1. 用 word2vec 做：如果 word 能表示成向量，最直观的思路，对于 phrase 和 sentence，可以将组成它们的所有 word 词向量加起来（简单相加不做任何处理），作为短语向量，句向量。首先这个方法确实有效，但只对 15 个字以内的短句子比较有效，这种方案常用在搜索时做 query 比对。如果简单向量相加，丢掉了很多词与词相关意思, 也对句子与句子的模式、结构等照成负面影响，无法更精细的表达句子与句子之间的关系。  
2. 用 LSI 或 LSA 做：LSI 是处理相似度的，而基于的是 SVD 分解方式，SVD 分解方式主要用于特征降维，LSI 求解出来的相似度跟 topic 相关性更强，而句子结构等信息较少。顺便说下，句子中词的顺序是不会影响 LSI 相似度结果的，可想而知了。  

而对于 word2vec 和 doc2vec 的模型异同点，以下文章提到了：

引用自：[语义分析的一些方法 (中篇)](https://link.zhihu.com/?target=http%3A//dataunion.org/10760.html)

Le 和 Mikolov 在文章《Distributed Representations of Sentences and Documents》里介绍了 sentence vector。

先看 c-bow 方法，相比于 word2vec 的 c-bow 模型，区别点有：

*   训练过程中新增了 paragraph id，即训练语料中每个句子都有一个唯一的 id。paragraph id 和普通的 word 一样，也是先映射成一个向量，即 paragraph vector。paragraph vector 与 word vector 的维数虽一样，但是来自于两个不同的向量空间。在之后的计算里，paragraph vector 和 word vector 累加或者连接起来，作为输出层 softmax 的输入。在一个句子或者文档的训练过程中，paragraph id 保持不变，共享着同一个 paragraph vector，相当于每次在预测单词的概率时，都利用了整个句子的语义。
*   在预测阶段，给待预测的句子新分配一个 paragraph id，词向量和输出层 softmax 的参数保持训练阶段得到的参数不变，重新利用梯度下降训练待预测的句子。待收敛后，即得到待预测句子的 paragraph vector。

![](https://pic3.zhimg.com/50/7905c8bf09db706d520819bc3a96945a_hd.jpg?source=1940ef5c)  

图 14. sentence2vec cBow 算法

  

sentence2vec 相比于 word2vec 的 skip-gram 模型，区别点为：在 sentence2vec 里，输入都是 paragraph vector，输出是该 paragraph 中随机抽样的词。

  
![](https://pic1.zhimg.com/50/aae15f655a5efd398daecc0ef2690def_hd.jpg?source=1940ef5c)  
引用完毕。  
我们看看 gensim 文档中出现了一个 dbow 参数，而且是这么解释的，  

dbow_words if set to 1 trains word-vectors (in skip-gram fashion) simultaneous with DBOW doc-vector training; default is 0 (faster training of doc-vectors only).

也就是要快还是要效果你是可以控制的，这就是体现性价比的地方。  

  
===================================================================  
以上，所以推荐 doc2vec from gensim ![](https://pic1.zhimg.com/f217f515797db899e0d442152af0b08e_xs.jpg?source=1940ef5c) 董力

今年 ICML-15 上有个文章 From Word Embeddings To Document Distances，是解决题主的问题。文章里面定义了 Word Mover’s Distance，词 - 词 的相似度用 word2vec 结果算欧式距离，句子 - 句子 的相似度通过求解一个 transportation 的优化问题得到。文章里面证明了词向量求平均算欧式距离是 Word Mover’s Distance 的下界，这样在求解最近邻集合的时候，就可以先用词向量平均对候选集快速进行 pruning。  
我实现了一下，结果看着还有些道理，但感觉想计算句子 - 句子 的相似度，还是要做 sentence modeling 才可以，而不是只从 word level 去算。

如果用 word2vec 结果作为一种 soft matching score 的话，其实可以把机器翻译里面的评价指标 BLEU 改造一下，也可以计算句子 - 句子 的相似度。  
======  
如何定义句子的 similarity 其实是比较困难的，往往和具体应用也比较相关，到底需求是 topic 上的相关，还是说 semantic 上的相关，例如：  

*   I like this laptop.  
    
*   I do not like this laptop.

如果用不同的 similarity 定义方法，得出的结果也是不同的。这个和 paraphrase 的研究其实也有些关系，现在大多数工作感觉都是从 similarity 的角度去做，但其实按照严格定义应该是看双向的 entailment。（扯远了）  
======  
只从词去比较的话，还有个比较困难的地方是 similarity 往往需要结合 context 去看，例如：  

*   hot girl（性感）
*   hot water（温度高）
*   hot dog（吃的热狗）

里面都有 hot，但是 similarity 应该是比较低的。![](https://pic1.zhimg.com/da8e974dc_xs.jpg?source=1940ef5c)知乎用户 2016.7.30  
-----------------------------------------------  
看了知友们的答案，我再补充一个结合 word2vec 和 RNN 来构造**无监督式**的句子特征学习模型。  
ps：本科毕业论文的一个子模块（E-LSTM-S），根据文献题目和文献摘要来衡量文本相似度。  
-----------------------------------------------  
在这里，首先给出模型在 CiteUlike(文献分享数据集) 上的结果。  
用 E-LSTM-S 得到每篇文献的文本特征后，可以简单地采用欧式距离计算文本相似度，示例如下：  
![](https://pic3.zhimg.com/cf88d928fc8d6ca444acc62b1c26de3a_r.jpg?source=1940ef5c)

其中 A=>B 表示跟 A 文献最为语义相关的是 B 文献，可以看到：

1. 200=>6091 表明《Learning to cluster web search results》和《Web Document Clustering: A Feasibility Demonstration》两篇文献具有相近的语义，从标题上也可以直观地判断它们都跟 “web clustering” 相关。

2. 6091=>5282 表明《Web Document Clustering: A Feasibility Demonstration》和《A personalized search engine based on web-snippet hierarchical clustering》两篇文献也具有相近的表达，同样跟 “web clustering” 相关。

3. 200=>6091、6091=>5282 这个组合表明，跟 “web clustering” 语义相关的文档将构成一类(类似聚类)。

  
-----------------------------------------------  
那么，实现的过程如何呢？(这里需要先了解 [RNN](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Recurrent_neural_network) 的结构以及如何使用词嵌入 [embedding-layer](https://link.zhihu.com/?target=http%3A//stats.stackexchange.com/questions/182775/what-is-an-embedding-layer-in-a-neural-network))  
模型框架如下：  
![](https://pic1.zhimg.com/44407e6da3aab021fc3dad7340c47c5c_r.jpg?source=1940ef5c)其中，ABCDE... 为 (词序列构成的) 句子，A*B*C*D*E*... 同样为相同句子。**模型的核心就是通过编码－中间表示－解码的形式来学习每个句子的向量表示**，从而可以用距离来衡量句子相似性。  
需要说明的是，有两种方式可以用来表示每个词的词向量 (实验时选 1)：  
1. 采用 one-hot vector，定义 embedding layer，word2vec 作为其初始参数，训练时进行 fine-tune。  
2. 直接采用 word2vec 作为词向量，即在训练过程中不改变每个词的词向量。  
接下来，就涉及到了红色虚线处的 Encoder-Decoder 结构了，细节如下：  
![](https://pic1.zhimg.com/606d37de54709e647b1b05c83b148c16_r.jpg?source=1940ef5c)其中，上半部分是 Embedding，下半部分则是 RNN(LSTM)。具体应用时，将词向量表示依次输入 RNN 的每一时间步，**综合词语义和词顺序的影响**，将其编码成中间表示，再用 Softmax 解码。  
-----------------------------------------------  
当然，最好采用 End-to-End（端到端）的方式直接生成句子的中间特征表示 (E-LSTM-S 是对每一时间步的隐层输出取平均)，可以参考用于机器翻译、邮件回复的 seq2seq 模型。这里我给出几篇文献供大家阅读。  

<1> [Sequence to Sequence Learning with Neural Networks](https://link.zhihu.com/?target=https%3A//www.google.com/url%3Fsa%3Dt%26rct%3Dj%26q%3D%26esrc%3Ds%26source%3Dweb%26cd%3D1%26cad%3Drja%26uact%3D8%26ved%3D0ahUKEwjTwqSXtpzOAhULRo8KHXk0BLAQFgghMAA%26url%3Dhttps%253A%252F%252Fpapers.nips.cc%252Fpaper%252F5346-sequence-to-sequence-learning-with-neural-networks.pdf%26usg%3DAFQjCNHkuuvss8h-_xyrytRBFTKQUb60Sg%26sig2%3DTBZgwXqPsuYYi1BoLJN0sQ%26bvm%3Dbv.128617741%2Cd.dGo)

<2> [Effective Approaches to Attention-based Neural Machine Translation](https://link.zhihu.com/?target=https%3A//www.google.com/url%3Fsa%3Dt%26rct%3Dj%26q%3D%26esrc%3Ds%26source%3Dweb%26cd%3D2%26cad%3Drja%26uact%3D8%26ved%3D0ahUKEwihq6CktpzOAhUGuI8KHRyzA_8QFggqMAE%26url%3Dhttp%253A%252F%252Farxiv.org%252Fabs%252F1508.04025%26usg%3DAFQjCNHtlOQSE2wlbk78-L9jQdtfc1ydcA%26sig2%3DGXDGPh47d_wj5URJJGyJ6A%26bvm%3Dbv.128617741%2Cd.dGo)

<3> [Learning Phrase Representations using RNN Encoder-Decoder for Statistical Machine Translation](https://link.zhihu.com/?target=https%3A//www.google.com/url%3Fsa%3Dt%26rct%3Dj%26q%3D%26esrc%3Ds%26source%3Dweb%26cd%3D2%26cad%3Drja%26uact%3D8%26ved%3D0ahUKEwizi_autpzOAhXJKo8KHadSC1MQFggqMAE%26url%3Dhttps%253A%252F%252Farxiv.org%252Fpdf%252F1406.1078%26usg%3DAFQjCNEItAWUaO4_ZNB8YI0w1jJgwTaedQ%26sig2%3D2XmrPoT3CLP-EehKymAskQ%26bvm%3Dbv.128617741%2Cd.dGo)  

  
此外，也可尝试使用 CNN 替换以上的 RNN，目前 CNN 在文本处理上也有不少研究了。  

<4> [A Convolutional Neural Network for Modelling Sentences](https://link.zhihu.com/?target=https%3A//www.google.com/url%3Fsa%3Dt%26rct%3Dj%26q%3D%26esrc%3Ds%26source%3Dweb%26cd%3D1%26cad%3Drja%26uact%3D8%26ved%3D0ahUKEwiOutvGtpzOAhWKPo8KHbGNDh8QFgghMAA%26url%3Dhttp%253A%252F%252Fwww.aclweb.org%252Fanthology%252FP14-1062%26usg%3DAFQjCNGpQydUVX0JP-bAqqspIMdZmG6TIw%26sig2%3DnAVWvGbM3pK3ppwtLUPk8Q%26bvm%3Dbv.128617741%2Cd.dGo)

<5> [Convolutional Neural Networks for Sentence Classification](https://link.zhihu.com/?target=https%3A//www.google.com/url%3Fsa%3Dt%26rct%3Dj%26q%3D%26esrc%3Ds%26source%3Dweb%26cd%3D1%26cad%3Drja%26uact%3D8%26ved%3D0ahUKEwidndrNtpzOAhVFsY8KHRe5AO4QFgghMAA%26url%3Dhttp%253A%252F%252Femnlp2014.org%252Fpapers%252Fpdf%252FEMNLP2014181.pdf%26usg%3DAFQjCNHa-Vqno2GzBM9P_A-FmFeFFL51aQ%26sig2%3DArOWVUxVyUmNYen2CGJ23w%26bvm%3Dbv.128617741%2Cd.dGo)

<6> [A Sensitivity Analysis of (and Practitioners’ Guide to) Convolutional Neural Networks for Sentence Classification](https://link.zhihu.com/?target=https%3A//www.google.com/url%3Fsa%3Dt%26rct%3Dj%26q%3D%26esrc%3Ds%26source%3Dweb%26cd%3D1%26cad%3Drja%26uact%3D8%26ved%3D0ahUKEwiktvLWtpzOAhXBp48KHVCtAUYQFggfMAA%26url%3Dhttp%253A%252F%252Farxiv.org%252Fabs%252F1510.03820%26usg%3DAFQjCNGSrKvg1Ggx5RX-K--Hq5S3prys3w%26sig2%3DV3A208V6f3JgKF0OrJXljw%26bvm%3Dbv.128617741%2Cd.dGo)

-----------------------------------------------

  
ps：不严谨之处，请 NLP 大牛勿见怪