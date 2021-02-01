> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/OV_tvo5Hw3ktFcIvzZ-ZEA)

![](https://mmbiz.qpic.cn/mmbiz_gif/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO7vgTGP53icxyLKZKC6yAKvs5s479MoYDVbObrnSuxoXbzNIFMmaX83g/640?wx_fmt=gif)

【编者按】芯片推动了人类社会数字化、信息化、智能化的发展。从某种程度上来说，芯片技术的发展也影响着行业未来的走向。你觉得未来的芯片世界将会如何？

![](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPVFKe7TPBFO4G5uxcibeIWUCo7D6YHsYt3PjRhCArPmelrBOsz7nKELia0FfA8ygic7RNlvmXY8ZhARQ/640?wx_fmt=png)

编译 | 虎说八道  责编 | 张文

头图 | CSDN 下载自视觉中国

出品 | CSDN（ID：CSDNnews）

芯片是信息技术的引擎，推动着人类社会的数字化、信息化与智能化。随着摩尔定律濒临终结，维持芯片技术创新面临挑战。开源芯片设计将是应对挑战的新思路。如今芯片设计动辄需要上亿研发费用、投入上百人，只有少数企业才能承担。反观互联网领域通过开源软件降低开发门槛，创造了繁荣的互联网产业。如果开源芯片设计能将芯片设计门槛降低几个数量级——3-5 人的小团队在 3-4 个月内，只需几万元便能研制出一款有市场竞争力的芯片，必将吸引大量人员投入芯片产业，重塑其繁荣。

十年前，一个这样的想法就诞生于加利福尼亚大学伯克利分校的一个实验室中，他们创造了一种通用的计算机芯片语言，按照他们的设想，这套指令将被能所有芯片制造商所使用，而不属于任何公司。它的出现并不是要成为一种令人印象深刻的新技术，它只是想要那些进入芯片行业的创新者们在同一个起跑线上，以此来降低进入芯片行业的门槛，最早推动其发展。

![](https://mmbiz.qpic.cn/mmbiz_png/Pn4Sm0RsAug9icpGY46HtR8C2bRF5SAFicMGEwm3LscibyFJ2HT5jx4MpAQgvSMfh2Kh4MNz7p366CbL6IWnZicFSw/640?wx_fmt=png)

David Patterson(RISC-V 的开放标准芯片指令的创建者)

正如伯克利所做的努力一样，RISC-V 在迈向全球芯片标准的过程中正在影响越来越多的人参与到建设 RISC-V 生态当中。由于这些人的参与，RISC-V 已经开始在芯片设计方面产生一些技术突破。

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOc2rXicCsC79VyqrfQywa6E74wdAaexVSMb4RYiacoJ9E71XJcjnkdWwA/640?wx_fmt=jpeg)

**RISC-V 开放了处理器设计**  

比如说：最近使用 RISC-V 的微处理器设计的时钟速度已经达到 5GHz，**远高于最新的运行于 3.2 GHz 频率的英特尔最强服务器芯片 E7**。并且，新型 RISC-V 芯片在 1.1V 时仅消耗 1 瓦功率，还**不到英特尔所消耗功率的百分之一。**

RISC-V 的速度和功率效率也超过了 Exynos4 的规格，Exynos 4 是三星电子为其智能手机生产的顶级处理器，它基于 ARM Holdings Plc 提供的计算核心。  

加州大学伯克利分校的教授 David Patterson 在接受 ZDNet 采访时回应到：“当我看到基于 RISC-V 设计的芯片演示结果时，我觉得太不可思议了。我认为 IBM 大型机要想达到 5GHZ 频率的芯片，应该需要配备液冷技术，并且每小时大约需要耗电 100 瓦。另外我还听 FPGA 的开发人员说，软核可以达到 600MHz。”

这些成就，在他和伯克利大学的同伴 KrsteAsanović 于 2011 年首次为 RISC-V 撰写宣言时，他们并没有想到。

Patterson 感慨到：技术创新正在兴起，创新的潜力永远存在。

Patterson 认为有一件事情是一定会发生的，因为 RISC-V 是开放的，所有的这些竞争的都将是可见的，也正是因为有了这些竞争，我们才能在芯片设计领域看到一些真正有趣的观点。

比如说：这个新型的 5 GHz 处理器原型并不是由初创公司的创建。它是由硅谷知识产权设计公司 Micro Magic Inc. 制造的，该公司已经与大型硅谷公司进行了 25 年的合作。这个处理器原型由少量但经验丰富的芯片设计人员完成足以证明，设计复兴可能即将到来。 

嵌入式基准微处理器联盟（Embedded BenchmarkMicroprocessor Consortium）记录，该芯片不仅在功耗更低的情况下速度更快，而且在原始 CPU 性能的基准得分（称为 CoreMark）上的得分也高于英特尔和三星。RISC-V 芯片的得分为 13000，是基于 ARM 的 Exynos 的每核性能得分的两倍以上。

尽管 Intel Xeon 的每个内核名义上要高一些为 26009，但 Xeon 部件需要同时 120 个执行线程才能达到该性能。长期担任芯片行业高管的 Andy Huang 博士在接受电话采访时向 ZDNet 解释说，**它突破在于 CPU 和内存交互的方式。**

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO3ToytjEfeNjJlGPcZqK98xO4MJ2zC1xj07PfS6kAic0LAlMKFBLvWPQ/640?wx_fmt=jpeg)

**RISC-V 原型消除了快速内存和慢速芯片可能存在的瓶颈**  

“如果内存运行速度为 5 GHz，逻辑运行速度为 1 GHz，那瓶颈在哪里呢？” Andy Huang 笑了笑说，关键在于，由于 RISC-V 是开放的，与英特尔芯片的复杂指令集架构甚至 ARM 芯片中的 RISC 版本不同，RISC-V 可以通过芯片设计来解决这一瓶颈。并且，他用 Android 与 iOS 进行了类比，Android 相较于 IOS 目前最大的优势就是开放，任何厂商都可以定制化自己的操作系统。Huang 说：“这就是为什么我们将所有成功归功于 Patterson 博士的原因，目前为止，他创建了最高效，最优雅的 RISC 体系结构。”

Huang 博士强调，与 CISC 或 ARM 的指令集各有 1000 多个指令不同，RISC-V 的指令集少于一百个。由于 RISC-V 指令集的简单性，Micro Magic 才能够使用标准硅晶圆生产芯片，而无需进行特殊调整。这样就可以使用所谓的往复运行，在制造过程中，芯片与其他人的芯片在同一晶圆上分组在一起，这样造价会便宜很多。

Patterson 指出：“人们谈论要花费 1 亿美元来定制 ASIC，实际上他们并没有花一亿美元来做到这些。”Patterson 在评价 Micro Magic 时说。

尽管没有得到 Patterson 和 Huang 的强调，但还有第二个因素在起作用。如果你没有支付 ARM 许可证的费用，那么使用往复运行就容易得多，但必须将其分摊到许多部分。

![](https://mmbiz.qpic.cn/mmbiz_jpg/VeJKXItpwPVFKe7TPBFO4G5uxcibeIWUCCmGEnrtuZq2pI6FDiawn4TfP0QHQt2Hov2eygF9cHyHFQiczHJXzaRkg/640?wx_fmt=jpeg)

全球最大的芯片制造商之一的英伟达（Nvidia）正在以 400 亿美元的价格收购 ARM，这一交易颇具争议。此次出售将使 Nvidia 能够从 ARM 的知识产权中获得专利使用费，并为世界上使用最广泛的芯片指令制定了发展路线图。

Nvidia 首席执行官黄仁勋（JensenHuang）描述了他对 ARM 的雄心勃勃的计划，并且他向投资者保证，ARM 的管理层们，不会反对这场收购案。很显然这场收购是为了 Nvidia 寻找替代品提供新的可能，当媒体就此事问 Patterson 时，他往往表现的很保守，因为英伟达是 RISC-V 生态系统的成员，并非常积极的支持 RISC-V 生态的发展。

Micro Magic 的 Huang 表示，自从 Micro Magic 成功发布其芯片公告以来，他已经吸引了技术巨头的兴趣。目前已经有两家巨头上市公司给他抛出了橄榄枝。

Huang 提供了一种假想的方案，在这种方案中，Apple 或 Google 可以使用该芯片，并且该芯片可以在能耗方面取得突破。Huang 对 ZDNet 表示说：“谷歌已经拥有移动开源软件 Android，想想如果所有移动客户也拥有最省电，性能最高的开源 RISC 核心，将会为他们带来什么好处，让我们再想象一下最新的 Apple Watch 不必隔夜充电，那又将是何种体验。” 

Huang 说，不管有没有这样的大规模的市场，Micro Magic 都希望将其 RISC-V 知识产权纳入越来越多的设计中，以便对全球用电量产生实质性影响。“我们使用此 IP 的意图是帮助世界，帮助 PC、笔记本电脑、平板电脑、手机，可穿戴设备、游戏设备、电动汽车和 IoT 减少电量的使用，我们目标是将世界的碳排放量减少一半。”

一个原型 CPU 并不是革命性的，与英特尔和其他公司的实际售卖的产品进行比较，可以得出一个事实，即完成芯片设计还需要更多的零件。这就是 RISC-V 周围公司的生态系统变得越来越丰富的原因，可喜的是使用 RISC-V 的公司的数量虽少，但仍在增长。

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOPyiaaIVCePZnia426uNibf0PLTPfS7VRaWql6vFWCwb2Cqgjp06UvU39Q/640?wx_fmt=jpeg)

**RISC-V 真正的使用范围是无法全部可见的**  

“你可以想到的所有产品，一直到数据中心，现在人们都在认真考虑 RISC-V，从某种意义上说，目前已经出现了这样的思考：从几年前我为什么会使用 RISC-V，到为什么我不使用 RISC-V？”Patterson 激动地说。

在 RISC-V 生态系统中突出的是 SiFive，这是一家位于硅谷的初创公司，多年来一直专门基于 RISC-V 开发芯片知识产权。

今年 8 月，该公司成立了一个业务部门，致力于为各种应用（包括 AI 和边缘计算）生产定制芯片，称为 OpenFive。Patterson 的合作者 Asanović 教授是 SiFive 的首席架构师。另一个生态突出的贡献者是嵌入式处理器制造商台湾晶心科技（Andes Technology of Taiwan），该公司多年来已向电子产品制造商出售了数十亿个 CPU 设计。

上个月，SiFive 和 Andes Technology 都在著名的芯片技术会议 Linley FallProcessor Conference 上展示了使用 RISC-V 的 AI 新芯片设计。SiFive 告诉 ZDNet，它现在已经赢得了 80 多家公司的 200 多个设计订单，其中包括前十大半导体制造商中的六个。SiFive 对 ZDNet 表示：“SiFive 目前已发货了数千万个内核。” 晶心在本月的季度报告中告诉投资者，今年其收入的大约三分之一来自基于 RISC-V 的零件。 

希捷科技公司和大型磁盘驱动器制造商西部数据公司都是下个月的 RISC-V 峰会的赞助商，该峰会是 RISC-V 生态系统的第三届年度技术大会。该活动是由 RISC-V 国际协会赞助的，该协会是一家非营利性公司，目前代表 750 多个致力于推进该标准的组织，其中包括中国智能手机厂商华为、芯片制造商 Xilinx、高通以及 IBM，Asanović 是该小组的主席，而 Patterson 是副主席。

但是，无论 RISC-V 多么成功，世界可能永远都不知道其用法的全部范围。因为 ARM 和其他商业技术提供商要求使用者必须要签署相关文件的，而 RISC-V 的使用人是可以选择不签署相关文件来进行披露使用情况。RISC-V  International 要求供应商自愿披露使用情况。

这样一来，**RISC-V 真正的使用范围是无法全部可见的**。

尽管如此，诸如 Micro Magic 产品的技术进步证据向 Patterson 和其他人证明了 RISC-V 的影响最终可能是很大的。最近，Patterson 通过视频与合作者进行了一系列的一对一访谈，以在线的方式庆祝 RISC-V 十周年。

Patterson 告诉 ZDNet，一位合作者告诉他说：“在五到十年内，RISC-V 可能是世界上最重要的指令集”。Patterson 认为这听起来有些疯狂，但这并非不可能。

参考链接：https://www.zdnet.com/article/risc-v-the-linux-of-the-chip-world-is-starting-to-produce-technological-breakthroughs/

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