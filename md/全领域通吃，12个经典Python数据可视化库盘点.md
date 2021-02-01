> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/Hci6ehO1DdvlLT8Be4U-Lw)

![Image](https://mmbiz.qpic.cn/mmbiz_gif/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO7vgTGP53icxyLKZKC6yAKvs5s479MoYDVbObrnSuxoXbzNIFMmaX83g/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPXmquHteiajpmib19cYhRfKUMbqI93CfN0aSSk6m3JShN6yw5IbqqD5eVzGJw4KWzNqSomvUibNhhyxQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

责编 | 寇雪芹  

头图 | 下载于视觉中国

来源 | 博文视点Broadview

  

  

> Python有很多数据可视化库，这些数据可视化库主要分为**探索式可视化库**和**交互式可视化库**。前者透过简单直接的视觉图形，更方便用户看懂原数据，后者主要用于与业务结合过程中展现总体分析结果。

在数据可视化的研究热潮中，如何让数据生动呈现，成了一个具有挑战性的任务，随之也出现了大量的可视化软件。相对于其他商业可视化软件，Python是开源且免费的，而且具有易上手、效果好的优点。

大家普遍第一次接触到的Python数据可视化库基本上都是Matplotlib。Python还有很多数据可视化库，本文盘点了12款常用的Python数据可视化库，挑选适合自己业务的那一款吧！

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

探索式可视化库  

  

探索式分析最大的优势在于，可以让业务人员在海量数据中“自由发挥”，不受数据模型的限制。通过探索式分析和可视化，业务人员可以快速发现业务中存在的问题。

Python探索式可视化库主要包括如下几个。

  **1.Matplotlib**

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "1.png")

Matplotlib是Python数据可视化库的元老，尽管它已有十多年的历史，但仍然是Python社区中使用最广泛的绘图库，编写几行代码即可生成线图、直方图、功率谱密度图、条形图、错误图、散点图等。

Python的可视化库众多，各有各的特点，但是Matplotlib是一个非常基础的Python可视化库，如果需要学习Python数据可视化，那么Matplotlib是非学不可的，之后再学习其他库就比较简单了。Matplotlib的中文学习资料比较丰富，其中最好的学习资料是其官方网站的帮助文档，大家可以在上面查阅自己感兴趣的内容。

  2.Seaborn

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "2.png")

Seaborn利用Matplotlib的强大功能，只用几行代码就能创建出漂亮的图表。它们的关键区别在于，Seaborn的默认款式和调色板设计更加美观和现代。由于Seaborn是在Matplotlib基础上构建的，因此用户还需要了解Matplotlib以便调整Seaborn的默认值。

Seaborn同Matplotlib一样，也是Python进行数据可视化分析的重要第三方包。

Seaborn在Matplotlib的基础上进行了更高级的API封装，使用户绘图更加容易，所绘图形更加漂亮。Seaborn是基于Matplotlib的一个模块，专用于统计可视化，可以和Pandas进行无缝连接，使可视化的初学者更容易上手。

相对于Matplotlib，Seaborn语法更简洁，两者的关系类似于Numpy和Pandas的关系。但是需要注意的是，应该把Seaborn视为Matplotlib的补充，而不是替代物。

Seaborn库旨在以数据可视化为中心来挖掘与理解数据，它提供的面向数据集制图函数主要是对行列索引和数组的操作，包含对整个数据集进行内部的语义映射与统计整合，以此生成信息丰富的图表。

 **3.Pyecharts**

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "3.png")

Pyecharts是我国开发人员开发的，相比较Matplotlib、Seaborn等可视化库，Pyecharts十分符合国内用户的使用习惯。

Pyecharts的目的是实现Echarts与Python的对接，以便在Python中使用Echarts生成图表。

Echarts是Apache软件基金会的一个数据可视化JavaScript库，生成的图的可视化效果非常好，其凭借良好的交互性，精巧的图表设计，得到了众多开发者的认可

 **4.Missingno**

处理缺失的数据是一件让人痛苦的事，Missingno通过使用视觉摘要来快速评估数据集的完整性，而不是通过大篇幅的表格。它可以根据热力图或树状图的完成度或点的相关度对数据进行过滤和排序。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

交互式可视化库  

  

数据可视化可以是静态的也可以是交互的，交互式的数据可视化是指人们使用计算机和移动设备深入图表和图形的具体细节，然后用交互的方式改变他们看到的数据。

Python交互式可视化库主要包括如下几个。

 **1.Bokeh**

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "11.png")

Bokeh基于JavaScript实现交互式可视化，它是原生Python语法，它可以在Web浏览器中实现美观的视觉效果。

它的优势在于能够创建交互式的网站图，可以很容易地将数据输出为JSON对象、HTML文档或交互式Web应用程序。Bokeh还支持流媒体和实时数据。

但是它也有明显的缺点：一是版本时常更新，最重要的是有时语法不向下兼容，这对于开发者来说是噩梦；二是语法晦涩，与Matplotlib相比，可以说是有过之而无不及。

 **2.HoloViews**

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "22.png")

HoloViews是一个开源的Python库，旨在使数据分析和可视化更加简便，可以用非常少的代码行完成数据分析和可视化。

除了默认的Matplotlib后端，它还添加了一个Bokeh后端。结合Bokeh提供的交互式小部件，可以使用HTML5和WebGL快速生成交互式视图，以及进行高维数据的可视化探索。

 **3.Plotly**

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg== "33.png")

Plotly是一个数据可视化的在线平台，与Bokeh一样，Plotly的强项在于制作交互式视图，但它提供了一些在大多数库中没有的图表，如等高线图、树状图和3D图表。可以在线绘制条形图、散点图、饼图、直方图等多种图形，可以画出很多媲美Tableau的高质量图。

Plotly支持在线编辑图形，支持Python、JavaScript、MATLAB和R等多种语言的API。Plotly生成的所有图表实际上都是由JavaScript产生的，无论是在浏览器还是在Jupyter中，所有的可视化、交互都是基于plotly.js的，它是一个高级的声明性图表库，提供了20多种图表类型，包含3D图表、统计图和SVG地图等。

  **4.pygal**

与Bokeh和Plotly一样，pygal提供了可以嵌入Web浏览器的交互式视图。区别在于，它能够将图表输出为SVG格式。如果用户使用较小的数据集，则输出位SVG格式的图像就可以了，但是如果用户制作的图表包含数十万个数据点，那么它们就会很难被渲染并变得反应迟钝。

Pygal绘制线图的方法很简单，可以将图表渲染为一个SVG文件，用户使用浏览器打开SVG文件就可以查看生成的图表。

  **5.plotnine**

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

plotnine是Python中图形语法的一种实现，它基于ggplot2包，语法绘图功能强大，可以轻松将数据映射到构成图的可视对象，然后创建自定义的图形。plotnine提供各种不同的可视化视图，易于适应定制化输出，安装十分简单，用户可以通过pip install plotnine命令直接安装。

plotnine的优点为代码简洁，易学；绘制出的图流畅大方；不需要很多的代码就可以绘制出很不错的图。在使用plotnine绘图之前，首先需要理解绘图的基本概念。

  **6.Altair**

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

Altair是一个专为Python编写，它可以让数据科学家更多地关注数据本身和其内在的联系。

因为是基于Vega-Lite（交互式图形语法）的声明性统计可视化库，Altair API具有简单、友好、一致等特点。

声明意味着用户只需要提供数据列与编码通道之间的链接，例如，x轴、y轴、颜色等，其余的绘图细节它会自动处理。声明使Altair变得简单、友好和一致，用户使用Altair可以轻松设计出有效且美观的可视化代码。

  **7.ggplot**

ggplot是基于R语言的ggplot2包和Python的绘图系统。ggplot的运行方式与Matplotlib不同，它允许用户对组件进行分层以创建完整的绘图。例如，用户可以从轴开始画，然后添加点，接着添加线、趋势线等。虽然图形语法被认为是绘图的“直观”方法，但经验丰富的Matplotlib用户可能需要时间来适应这个新的方式。

  **8.Gleam**

Gleam的灵感来自R语言的Shiny包。它允许用户仅使用Python脚本就可将分析结果转换为交互式Web应用程序，因此用户不必了解任何其他语言，如HTML、CSS或JavaScript。Gleam适用于任何Python数据可视化库。在创建绘图后，用户可以在它的上面添加字段，以便对数据进行筛选和排序。

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

```
更多精彩推荐

```

```
☞科学“干饭”，AI 营养师教你
☞GitHub 标星超 26600，TiDB 社区运营的道与术！
☞区区几行Python代码，一分钟搞定一天工作量
☞告别手敲 SQL ？GPT-3 自动帮你写


点分享
点收藏
点点赞
点在看

```