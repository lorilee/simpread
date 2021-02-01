> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/5soDdKgFigR3J6xjwpmHKg)

![](https://mmbiz.qpic.cn/mmbiz_gif/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO7vgTGP53icxyLKZKC6yAKvs5s479MoYDVbObrnSuxoXbzNIFMmaX83g/640?wx_fmt=gif)

![](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPVFKe7TPBFO4G5uxcibeIWUC9k80nsPCibTWeLqYcfibzqImImWZI3ZII82LLJmEx7DLuxic6pae7Dgjg/640?wx_fmt=png)

作者丨神经小兮

来源丨超神经 

合理膳食、营养均衡的重要性已不必多说，但具体如何落实，却不简单。为了得到搭配更合理、更健康、更符合人们口味的食谱，AI 也加入了营养师的队伍。

在健康饮食方面，现代人对养生、专家推荐、最全食谱这些关键词，有着超乎想象的热忱。

健身人群追求增肌，执着于低碳水、高生酮的饮食；爱美人群追求速效减肥，轻断食、哥本哈根减肥法最得人心；术后病人、孕产妇被口口相传的小米粥、猪蹄汤洗脑；向往绿色生活的年轻人、学佛人群崇尚素食，在有限的选择中，尽量保持身心健康。

![](https://mmbiz.qpic.cn/mmbiz_jpg/QkCvnz083AiaRvwmsh5icicvKMWwicN76UU8GejO8HuLHMCtWibicUkAiaLfLw7qRDAwyyIUianA4gvysVj0ZdfXkEPhrQ/640?wx_fmt=jpeg)

![](https://mmbiz.qpic.cn/mmbiz_jpg/QkCvnz083AiaRvwmsh5icicvKMWwicN76UU8jbElz3t5FlXDlFq58WlQuoFZdjY8PibLzN3z5o540wIN3icQrlgo0uvQ/640?wx_fmt=jpeg)

![](https://mmbiz.qpic.cn/mmbiz_jpg/QkCvnz083AiaRvwmsh5icicvKMWwicN76UU8fcxth3HQxuRKI4K1jGWyHIkFMiaQL2T4G9phbXE5Ix2o5JM6FfbYx3Q/640?wx_fmt=jpeg)

![](https://mmbiz.qpic.cn/mmbiz_jpg/QkCvnz083AiaRvwmsh5icicvKMWwicN76UU8U4kkcOOzfttO8xiaE3IstKaJjCIhMaQJjg08vRHibTtdSQMicbGHQY5Yg/640?wx_fmt=jpeg)

  

**（滑动查看）****丰富又专业的食谱，并不****适合每一个人**

面对丰富的选择，到底怎样搭配才是科学合理的？一些研究者正在借助算法和大数据的优势，为每个人推荐更健康、更营养的食谱。

   

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOc2rXicCsC79VyqrfQywa6E74wdAaexVSMb4RYiacoJ9E71XJcjnkdWwA/640?wx_fmt=jpeg)

AI 学会「看人下菜碟」

  

上文提到的通用型食谱最大的问题就是，每个人的体质不同、口味不同、地域资源不同，另外消费水平不同。

一些博主的高质量食谱里动辄就是三文鱼、牛油果这些价格昂贵、部分地区难以购买的食材，也让很多人望而却步。

为了实现千人千面的饮食推荐，很多研究者也引入了人工智能技术。

美国伦斯勒理工大学联合 IBM Research 的研究人员，最近开发了一个个性化饮食推荐系统 pFoodReQ，可以**根据个人用户的喜好和饮食需求，推荐量身定制的食谱。**

![](https://mmbiz.qpic.cn/mmbiz_png/QkCvnz083AgLTwDIicym3maLCv4R9oCiaatJADvdKUFiclxlibEpicnSfJrBH1L133cEicpCr9SKzibK7P4qOEALOrcmA/640?wx_fmt=png)

**基于大规模食物知识图谱上受限问题回答的**个性化食物推荐****  

**论文地址：https://arxiv.org/pdf/2101.01775.pdf**

作者在论文中指出，现有的饮食推荐方法，普遍存在三个主要缺点：

1.  不理解用户的准确要求；
    
2.  不考虑过敏和营养需求的关键因素；
    
3.  没有基于丰富的食物选择定制食谱。
    

在本项研究中，研究团队则提出个性化食物推荐，并将其视为对食物知识图谱（Knowledge Graph，简称 KG）的受限问题的回答，从而设法用统一的方式解决上述问题。

团队提出了一种基于知识库问答 KBQA（Knowledge Base Question Answering）的个性化食品推荐框架，即**通过问答的方式进行个性化食品推荐。**

**![](https://mmbiz.qpic.cn/mmbiz_jpg/QkCvnz083AiaRvwmsh5icicvKMWwicN76UU871QZAZhicdZqNTzybyZJsrRtUcaBRaynresuo0eTD2nzUGSGlkjWouA/640?wx_fmt=jpeg)**

**对于不同用户，系统会进行不同的需求分析**

具体来说，pFoodReQ 系统会按照用户的问题，比如**「一顿包含面包的好早餐是什么？」**，然后从 KG 中检索满足这一查询条件的所有食谱。再对这些食谱中的成分进行适用度评分，**最后推荐评分排名最高的几份食谱**。

![](https://mmbiz.qpic.cn/mmbiz_png/QkCvnz083AgLTwDIicym3maLCv4R9oCiaakwWm9CFHMZOMSZEu5lzDT7iahNxOpcxkJI17FTNRbCicLnTFz7dnTaqQ/640?wx_fmt=png)

**基于问答机制的个性化食品推荐体系结构示意**

最后验证实验结果表明，他们提出的方法明显优于非个性化的方法，能够推荐更相关、更健康的食谱。

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO3ToytjEfeNjJlGPcZqK98xO4MJ2zC1xj07PfS6kAic0LAlMKFBLvWPQ/640?wx_fmt=jpeg)

食谱千千万，AI 只推荐适合你的菜  

  

总的来说，团队建立该系统，**共经历了创建数据集、生成基准问题、编译健康指南、系统训练四个步骤。**

 **第一步：食****谱数据集创建** 

团队基于广泛的食物知识图谱 FoodKG（FoodKG 集成了食谱、食品和营养数据），创建了一个基准的 QA 数据集（暂未公开）**，其中包含超过 100 万份食谱，770 万份营养记录，和 730 万种食物，同时还包含相应的配料和营养成分，**并参考了 ADA 美国糖尿病学会推荐的生活方式指南。

![](https://mmbiz.qpic.cn/mmbiz_png/QkCvnz083AiaRvwmsh5icicvKMWwicN76UU8mH1brpSBBnw2nDCapcu3ia5PcQAXMkqpt16v2JMwkz1BGK5fyybAV3Q/640?wx_fmt=png)

**（推荐）Open food facts 食品营养成分数据集  
**

**收集全球各类食品的成分、过敏信息、添加剂等数据  
**

**_数据集下载：https://hyper.ai/datasets/5615_**

据作者介绍，这是第一个与配料、营养和食谱相关的、支持问答系统的个性化食物推荐数据集。

数据集中的每个示例都包含一个用户查询、饮食偏好、与用户相关的健康指南和基本答案 (即食谱推荐)。

 **第二步：生成****基准问题** 

为了得到反映人们真实饮食情况的问题，**作者在社交媒体 Reddit 上收集了 200 多个食谱、糖尿病相关的问题，**一共发现了 156 篇网友求食谱的帖子，大家主要集中在四类问题上：

*   哪些配料可以食用;
    
*   哪些成分不能食用;
    
*   「低碳水化合物」或「高蛋白」，有哪些推荐；
    
*   「意大利风味」或「地中海风味」，有哪些推荐。
    

![](https://mmbiz.qpic.cn/mmbiz_png/QkCvnz083AgLTwDIicym3maLCv4R9oCiaafhkyK6GffSLr7nj5LKYRiaEKiagXRmVficd2dmTkM8rQsobRdP1ZLaUiaw/640?wx_fmt=png)

**团队对 Reddit 上的问题进行信息分析**  

**界定其中提到的食谱、食物、成分等**

根据这些帖子的提问方式，团队总结了 56 个不同的模板，并基于此生成了一些基准问题。

 **第三步：编译健康指南** 

前期准备工作完成后，就可以进行健康饮食推荐了。

团队从 ADA 生活方式指南中，选择了一些与食物相关的指南，这些指南涉及到营养和微量元素，并将其作为额外的食物推荐要求。因此其系统的推荐，都是符合健康指南的健康食谱。

由于这些准则是自然语言的，所以将它们转换为结构化表示 (例如，存储键值对的哈希表)。

![](https://mmbiz.qpic.cn/mmbiz_png/QkCvnz083AgLTwDIicym3maLCv4R9oCiaaDeVehiaqicLxmtKW7a3x2TplVFPHu19WiaEib4FZEzfGDUEwCSQhUEQQFg/640?wx_fmt=png)

**将自然语言的饮食建议，转换为结构化数据进行表示**

 **第四步：训练个性化系统**  

为了实现个性化，团队还分别解决了查询扩展、过敏查询的问题。作者认为，**一个有效的食物推荐系统，应该尊重饮食偏好和健康指南中的个性化需求。**因此，对于用户查询，会进行进一步的扩展。

比如用户向系统提问**「请推荐一份包含面包的早餐」**，系统则会根据用户此前的查询、膳食偏好、饮食历史日志，了解其饮食偏好，自动将单一的查询进行扩展，转换成附加个性化需求的查询。

经过扩展后的查询就变成了：**「推荐一份包含面包、不含花生、含 5g 至 30g 碳水化合物的优质早餐」。**

正因如此，面对不同用户提出的相同问题，系统能够给出不同的食谱建议。

 **实验结果：优于其它模型** 

团队对食物推荐进行了人类评估，方法是向 8 个评估者，提供随机测试的 50 个问题，以及用户角色，包括配料偏好 (喜欢和不喜欢) 和适用的营养指南。

对于每个问题，以随机顺序输入给 BAMnet、P-BOW、P-MatchNN 和 pFoodReQ 四种模型，并得出答案。每个答案都包含前三个菜谱 (如果检索到的菜谱超过三个的话)、成分列表和营养价值。

![](https://mmbiz.qpic.cn/mmbiz_png/QkCvnz083AgLTwDIicym3maLCv4R9oCiaa37NO3QNNhyRJvmzRkl4PicCvx87b7arWCSHGYsXShAyMh8NGxic6npEQ/640?wx_fmt=png)

**评分范围为 1-10 分 (得分越高，结果越好）**

**pFoodReQ 取得最高分**

不过，目前这一个性化的饮食推荐系统，还只是团队研究的第一步。作者表示，「未来还存在很多挑战，我们需要更复杂的回答基准，处理隐含的用户意图和各种特殊情况。」

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOPyiaaIVCePZnia426uNibf0PLTPfS7VRaWql6vFWCwb2Cqgjp06UvU39Q/640?wx_fmt=jpeg)

  

 AI 营养师，国人更需要

  

  

合理膳食的重要性已不用多说，但目前我国饮食习惯仍然非常不健康，**据《柳叶刀》发布的研究报告，在全球 195 个国家中，中国排名 140。**

要从根本上改变不健康的饮食习惯，远远不止于推荐食谱那么简单。而 AI 的介入，能否为我们带来更多可能？以下是我们总结的 AI 营养师所具有的一些优势：

 **面对机器推荐，卸下心理防备** 

面对人类营养师、健身教练，很多人也许碍于面子或隐私问题，并不会透露自己最真实的饮食习惯和消费水平。

但换成 AI 营养师就不一样了，你大可以放下心理包袱，把自己更真实的需求告诉 AI，然后让它从万千食谱中搜出你感兴趣、并兼具营养的那一款。

 **包容 cheat day，动态调整菜谱** 

如今各种营养搭配、均衡饮食科普文充斥于网络，但对于读者来说，执行难度太高，依从性低。

我们总会遇到不能完全执行菜谱、或者外出应酬的情况。AI 能够针对这些变化，对后续的菜谱做出及时调整，适配各种变化。

 **补充营养师缺口，提升健康认知** 

虽然人们的健康意识正在崛起，但我国营养师这一职业却面临极大的缺口。

据调查，在日本，每 300 人就配备一个营养师；在美国，每 4000 人配备一个营养师；**而在中国，每 40 多万人才有一个营养师。**

如果 AI 具备了完善了营养健康学知识，那么人人都可以拥有 24 小时伴随的私人营养师，可以随时随地进行膳食搭配指导。

届时，「今天怎么吃」这个难题，就只管交给 AI 吧！

![](https://mmbiz.qpic.cn/mmbiz_gif/QkCvnz083AiaRvwmsh5icicvKMWwicN76UU8mxvwURib9lb0bDic9riaB0pJJtA3TibJt4etC5jy0NG8MNP0fx9IV0qEhA/640?wx_fmt=gif)

![](https://mmbiz.qpic.cn/mmbiz_jpg/Pn4Sm0RsAuggpZ0lia5ZjmiaqEIGqmY9Ch46r8l3Cv9Lvuqq7ic7xlHVf7zNlwgiaYRMdLib25h7pMEunOCf4IwibDRw/640?wx_fmt=jpeg)

```
更多精彩推荐
```

```
☞读懂深度迁移学习，看这文就够了 | 赠书
☞『带你学AI』一文带你搞懂OCR识别算法CRNN：解析+源码
☞龙泉寺贤超法师：用 AI 为古籍经书识别、断句、翻译
☞微信十周年，腾讯晒成绩单了！
```

```
点分享
点收藏
点点赞
点在看
```