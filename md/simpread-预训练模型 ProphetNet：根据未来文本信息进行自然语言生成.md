> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://mp.weixin.qq.com/s?__biz=MzI0ODcxODk5OA==&mid=2247513552&idx=3&sn=a6346ced0df31300cd691c2d3e40c802&chksm=e99e8629dee90f3f5d1ce74552f9941d86c76bc090ca926f0949f797baa5d1beb7aa80cc68d3&scene=126&sessionid=1583802242&key=0d82e3955cf2d37e17387e71e378b9d657f240e5fa047bf0b66145efc8a0f883622fc70c9a4f7dae001e1fa7fa64d2f192dd81f58e63797b57af10678fc1794c3b006bb9c0b97f8dc982e4b5d3cb3dee&ascene=1&uin=MTU2NDkyMjU2MA%3D%3D&devicetype=Windows+10&version=62080079&lang=zh_CN&exportkey=A2jDRfmnPHOCIzga2eiOpHo%3D&pass_ticket=0gwhg0EQiE%2Bc38PXy1KvCidL%2FpSPPQTYhvaDkPiop0bqVrEF9DrsC3jOP%2BZvlwXi

![](https://mmbiz.qpic.cn/mmbiz_gif/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO7vgTGP53icxyLKZKC6yAKvs5s479MoYDVbObrnSuxoXbzNIFMmaX83g/640?wx_fmt=gif)

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAaeSicXwd0tCY7I6R0OAxaJ4a5MptfVzJuwib4eRZdibIjWc66yBXxgZkfM5mXj6j0xBIqhG1E9dHyqQ/640?wx_fmt=jpeg)

作者 | 刘大一恒、齐炜祯、晏宇、宫叶云、段楠、周明

来源 | 微软研究院 AI 头条（ID:MSRAsia）

编者按：微软亚洲研究院提出新的预训练模型 ProphetNet，提出了一种新的自监督学习目标——同时预测多个未来字符，在序列到序列的多个自然语言生成任务都取得了优异性能。

大规模预训练语言模型在自然语言理解（NLU）和自然语言生成（NLG）中都取得了突破性成果。这些模型通常使用特殊的自监督学习目标先在大规模无标记语料中进行预训练，然后在下游任务上微调。

传统自回归语言模型通过估计文本语料概率分布被广泛用于文本建模，序列到序列的建模（seq2seq），以及预训练语言模型中（如 GPT 等）。这类模型通常使用 teacher-forcing 的方法训练，即每一时刻通过给定之前时刻的所有字符以预测下一个时刻的字符。然而，**这种方式可能会让模型偏向于依赖最近的字符，而非通过捕捉长依赖（long-term dependency）的信息去预测下一个字符。**有如以下原因：（1）局部的关系，如两元字符的组合，往往比长依赖更强烈；（2）Teacher-forcing 每一时刻只考虑对下一个字符的预测，并未显式地让模型学习对其他未来字符的建模和规划。最终可能导致模型对局部字符组合的学习过拟合，而对全局的一致性和长依赖欠拟合。尤其是当模型通过贪心解码（greedy decoding）的方式生成序列时，序列往往倾向于维持局部的一致性而忽略有意义的全局结构。

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOc2rXicCsC79VyqrfQywa6E74wdAaexVSMb4RYiacoJ9E71XJcjnkdWwA/640?wx_fmt=jpeg)

**ProphetNet（先知网络）**

针对上述问题，我们提出了一个新的 seq2seq 预训练模型， 我们称之为 ProphetNet（先知网络）。该模型带有一个新颖的自监督学习目标函数，即预测未来的 N 元组（Predicting Future N-gram）。与传统 seq2seq 的 Teacher-forcing 每一时刻只预测下一个字符不同，**ProphetNet 每一时刻将学习去同时预测未来的 N 个字符。**如图 1 所示：

![](https://mmbiz.qpic.cn/mmbiz_png/HkPvwCuFwNPbiawpBUjPDCsazHa4h8qezSO1DTdf7icxdagOsQpiaWCNzxk9Tl9K0yXmnvXiauUluQzaMK8OUd9gFA/640?wx_fmt=png)

图 1：左边是传统的语言模型，每一时刻预测下一时刻的字符。右边是 Bigram 形式下的 ProphetNet，每一时刻同时预测未来的两个字符。

预测未来 N 元组这一自监督学习目标在训练过程中显式地鼓励模型在预测下一个字符时考虑未来更远的字符，做到对未来字符的规划，以防止模型对强局部相关（strong local correlation）过拟合。

ProphetNet 基于 Transformer 的 seq2seq 架构，其设计有两个目标：1. 模型能够以高效的方式在训练过程中完成每时刻同时预测未来的 N 个字符；2. 模型可以灵活地转换为传统的 seq2seq 架构（每时刻只预测一下个字符），以在推理或微调阶段兼容现有的方法和任务。为此，我们受 XLNet 中 Two-stream self attention 的启发，提出了用于模型 decoder 端的 N-stream self-attention 机制。图 2 展示了 bigram 形式下的 N-stream self-attention 样例。

除了原始的 multi-head self-attention 之外（我们称为 main stream self-attention，如图 2a），N-stream self-attention 包含了额外的 N 个 predicting stream self-attention（如图 2b 和图 2c），用于分别预测第 n 个未来时刻的字符（如图 2d）所示。每一个 predicting stream 与 main stream 共享参数，我们可以随时关闭 predicting stream 以让模型转换回传统 seq2seq 的模式。  

![](https://mmbiz.qpic.cn/mmbiz_png/HkPvwCuFwNPbiawpBUjPDCsazHa4h8qez5mbmj9PQB8S9Vf0ujdcbI0Id3ics0h42OK14eT3ibz4oXCW1Z4hRQI8Q/640?wx_fmt=png)

图 2：(a) 为 main stream self-attention；(b) 为 1-st predicting stream self-attention；(c) 为 2-nd predicting stream self-attention；(d) 展示了 n-stream self-attention 的输入输出及流程。

由于难以获取到大量带标记的序列对数据，我们用去噪的自编码任务通过大量无标记文本预训练 ProphetNet。去噪的自编码任务旨在输入被噪音函数破坏后的序列，让模型学习去复原原始序列。该任务被广泛应于 seq2seq 模型的预训练中，如 MASS、BART、T5 等。本文中使用 MASS 的预训练方式，通过引入提出的 predicting n-stream 自监督学习目标函数预训练 ProphetNet。我们以 bigram 形式的 ProphetNet 为例，整个流程如图 3 所示：

![](https://mmbiz.qpic.cn/mmbiz_png/HkPvwCuFwNPbiawpBUjPDCsazHa4h8qezo7HEzJEicQib2c49BRfkELqC2l4azcrXedEA5KwIbMbqCLGYO8YJI04g/640?wx_fmt=png)

图 3：二元形式下的 Prophet 整体框架图

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO3ToytjEfeNjJlGPcZqK98xO4MJ2zC1xj07PfS6kAic0LAlMKFBLvWPQ/640?wx_fmt=jpeg)

**实验结果**

  

我们使用两个规模的语料数据训练 ProphetNet。ProphetNet 包含 12 层的 encoder 和 12 层的 decoder，隐层大小为 1024。先在 BERT 所使用的 BookCorpus+Wikipedia（16GB）的数据上预训练模型，将模型在 Text summarization 和 Question generation 两个 NLG 任务上的三个数据集微调并评估模型性能。与使用同等规模数据（16GB）的预训练模型相比，ProphetNet 在 CNN/DailyMail、Gigaword 和 SQuAD 1.1 question generation 数据集上都取得了最高的性能，如表 1-3 所示。

![](https://mmbiz.qpic.cn/mmbiz_png/HkPvwCuFwNPbiawpBUjPDCsazHa4h8qezoTBHPeb9sNFibEnbp52LhzglKNDqibT2JtT06Pxia5o9I6dIOS4Lw7YFQ/640?wx_fmt=png)

表 1：CNN/DailyMail 测试集结果

![](https://mmbiz.qpic.cn/mmbiz_png/HkPvwCuFwNPbiawpBUjPDCsazHa4h8qezLCBiamzo80GOru5Js7JjCQzicoibh1OGA6zMtfkBPeXBbOuXNWNP88mQQ/640?wx_fmt=png)

表 2：Gigaword 测试集结果

![](https://mmbiz.qpic.cn/mmbiz_png/HkPvwCuFwNPbiawpBUjPDCsazHa4h8qezUGyw6DqbHf4fGQnicUibmz1A9ZoQEkEqsIPh37sJWOTZrVIsEQ6qxpew/640?wx_fmt=png)

表 3：SQuAD 1.1 测试集结果（上半部分）SQuAD 1.1 交换验证测试集结果（下半部分）

除了使用 16GB 的语料训练模型，我们也进行了更大规模的预训练实验。该实验中，我们使用了 160GB 的语料（该语料被 BART、RoBETRa 等模型预训练中所使用）预训练 ProphetNet。我们展示了预训练 14 个 epoch 后的 ProphetNet 在 CNN/DailyMail 和 Gigaword 两个任务上微调和测试的结果。如表 4 所示。需要注意的是，在相同大小的训练数据下，我们模型的预训练 epoch 仅约为 BART 的三分之一。我们模型的训练数据使用量仅约为 T5 和 PEGASUSLARGE（C4）的五分之一，约为 PEGASUSLARGE（HugeNews） 的二十分之一。尽管如此，我们的模型仍然在 CNN/DailyMail 上取得了最高的 ROUGE-1 和 ROUGE-L F1 scores。并在 Gigaword 上实现了新的 state-of-the-art 性能。  

![](https://mmbiz.qpic.cn/mmbiz_png/HkPvwCuFwNPbiawpBUjPDCsazHa4h8qez5DOcJfXicOkVjYTXQ4dslzt3buGtyV14baw8oKicJXQ3EIIib72QPPocQ/640?wx_fmt=png)

表 4：模型经大规模语料预训练后在 CNN/DailyMail 和 Gigaword 测试集的结果

为了进一步探索 ProphetNet 的性能，我们在不预训练的情况下比较了 ProphetNet 和 Transformer 在 CNN/DailyMail 上的性能。实验结果如表 5 所示，ProphetNet 在该任务上超越了同等参数量的 Transformer。

![](https://mmbiz.qpic.cn/mmbiz_png/HkPvwCuFwNPbiawpBUjPDCsazHa4h8qez5Ij6WqynFAcQKgJCibfQBLmmicEMuFf3lNYIiblBTiaVicicBsZb3NdANmdA/640?wx_fmt=png)

表 5：模型不经过预训练在 CNN/DailyMail 验证集结果

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOPyiaaIVCePZnia426uNibf0PLTPfS7VRaWql6vFWCwb2Cqgjp06UvU39Q/640?wx_fmt=jpeg)

  

**总结**

  

本文介绍了微软亚洲研究院在序列到序列模型预训练的一个工作：ProphetNet，该模型提出了一种新的自监督学习目标，在同一时刻同时预测多个未来字符。并通过提出的 N-stream self-attention 机制高效地实现了模型在该目标下的训练。实验表明，该模型在序列到序列的多个自然语言生成任务都取得了不错的性能。我们将在之后尝试使用更大规模的模型架构和语料进行预训练，并进一步深入地探索该机制。

论文链接：https://arxiv.org/pdf/2001.04063.pdf

【end】

  

◆

****原力计划****

◆

  

《原力计划【第二季】- 学习力挑战》正式开始！即日起至 3 月 21 日，千万流量支持原创作者！更有专属【勋章】等你来挑战

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAab2fvibn5jf6Oz6ccSWLtwzLrYLZULcXjia1yibEDgqHWSQGMyNdcBucbo9jflebT1ickMuhItllsmHQ/640?wx_fmt=jpeg)

**推荐阅读**

*   [Python 数据清理终极指南（2020 版）](http://mp.weixin.qq.com/s?__biz=MzI0ODcxODk5OA==&mid=2247513530&idx=1&sn=c7bb57dbf65ff3d872f990e358fa32b5&chksm=e99e8643dee90f552db61dcfccc1ae8256b9e8b03f459990e4e2c084d12d55f7d660a98940e6&scene=21#wechat_redirect)
    
*   [口罩检测识别率惊人，这个 Python 项目开源了](http://mp.weixin.qq.com/s?__biz=MzI0ODcxODk5OA==&mid=2247513530&idx=2&sn=f6272dcea448dc31a5afe6f4a8c78650&chksm=e99e8643dee90f55ba464fc0fd36dde26670d68a37d4189a448a13571a705cdd117f7c79c44f&scene=21#wechat_redirect)
    
*   [谈论新型冠状病毒、比特币、苹果公司…… 沃伦 • 巴菲特受访中的 18 个金句，值得一看！](https://mp.weixin.qq.com/s?__biz=MzU2MTE1NDk2Mg==&mid=2247500186&idx=1&sn=03a67143a6e467328fe0f8b5182dcb83&scene=21#wechat_redirect)
    
*   [天猫超市回应大数据杀熟；华为 Mate Xs 被热炒至 6 万元；Elasticsearch7.6.1 发布](https://mp.weixin.qq.com/s?__biz=MjM5MjAwODM4MA==&mid=2650740074&idx=1&sn=5b13b3cbc3189c30def533ef6c62d9fa&scene=21#wechat_redirect)
    
*   [一张图对比阿里、腾讯复工的区别](https://mp.weixin.qq.com/s?__biz=MzA5MzY4NTQwMA==&mid=2651016765&idx=2&sn=28848f9a70e2cff62812adeedbb34675&scene=21#wechat_redirect)
    
*   [不看就亏系列！这里有完整的 Hadoop 集群搭建教程，和最易懂的 Hadoop 概念！| 附代码](https://mp.weixin.qq.com/s?__biz=MzA3MjY1MTQwNQ==&mid=2649833925&idx=1&sn=c6dd877a0d190b5b20d5ab2a94f71ac1&scene=21#wechat_redirect)
    

![](https://mmbiz.qpic.cn/mmbiz_png/Pn4Sm0RsAuhpplm16ibb8iaib7RoGQ5iaHEdvAd0o9e1LlUGA2k0Yib222agOxzweXhahA9GuzJcGBg0dA4DzlibxRqw/640?wx_fmt=png)

你点的每个 “在看”，我都认真当成了 AI