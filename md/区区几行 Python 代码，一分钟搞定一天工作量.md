
作者 | 陈熹、刘早起  

来源 | 早起 Python

大家好，我是早起。

前几天有一个读者说最近要整理几千份文件，头都要整秃了，不知道能不能用 Python 解决，我们来看一下，你也可以思考一下。

> 由于涉及文件私密所以具体内容已做脱敏处理。

大概是这样，一个文件夹下有多份会议通知信息（本文以 7 份文件为例）

![](https://mmbiz.qpic.cn/mmbiz_png/2GcSFhuAFlAWetCrA14MwicibqNG2EfNyItZwWvocjzXcic8lyUwAYPIPHBqqfefcWvr4uQB3IiaNlUE8d2lWNGedw/640?wx_fmt=png)

每一份通知打开格式基本类似，如下所示👇

![](https://mmbiz.qpic.cn/mmbiz_png/2GcSFhuAFlAWetCrA14MwicibqNG2EfNyISA0SgqZOyjvlm6Zq9mJqQpt3q0cV4zZwgibsq4ATkFxXrg5SwbnP3CQ/640?wx_fmt=png)

现在需要将每份会议文档中的 学习时间、学习内容、学习形式、主持人 四项关键信息提取出来，整理到 Excel 表格中：

![](https://mmbiz.qpic.cn/mmbiz_png/2GcSFhuAFlAWetCrA14MwicibqNG2EfNyIO6UpqmmvsBjJbabBDVwY3l5KZzYYW9sjO5h0nic66hV7t6TTkibOQO2A/640?wx_fmt=png)

在他真实需求中，会议通知四年积累下来有快 1000 份（四年开了这么多次会也是很厉害...），用人力挨个打开文件并录到 Excel 中工作量实在太大。

好家伙，这种**重复的无聊工作**，不就是一份非常适合交给 Python 的自动化工作吗？

下面我们来看看如何用 Python 解决这个问题，主要将涉及：

*   openpyxl 写入 Excel 文件
    
*   python-docx 读取 Word 文件
    
*   glob 批量获取文件路径
    

为了简化上面的需求，本文中需要获取的会议通知文件一共 7 个，分别命名为 会议通知 1.docx、会议通知 2.docx... 会议通知 7.docx，存放在 Notice 文件夹下。输出的目标 Excel 文件命名为 Meeting_temp.xlsx

**基本逻辑**
--------

写代码之前都先**明确完整的问题需要分为几个小步骤实现**。从需求中我们大概可以将代码分为以下几步：

*   获取会议通知 Notice 文件夹下的所有文件；
    
*   解析每一份 Word 文件，获取需要的四个信息，输出到 Excel 中；
    
*   保存 Excel 文件
    

有了逻辑就有了写代码的思路了。第 1 步可以由 glob 库完成，后面两步就是操作 Word 的 python-docx 库和操作 Excel 的 openpyxl 库的交互协作了。

这两个库我们都有说过，如果你不熟悉，一定要先阅读下面的文章！

*   [👉python-docx 操作 Word 详解](https://mp.weixin.qq.com/s?__biz=MzI1MTUyMjc1Mg==&mid=2247503940&idx=1&sn=41e2e99a075f3104402711fdef87c099&scene=21#wechat_redirect)
    
*   [👉openpyxl 操作 Excel 详解](https://mp.weixin.qq.com/s?__biz=MzI1MTUyMjc1Mg==&mid=2247500234&idx=1&sn=5d2a216d1b064102a96123440c95e0fd&scene=21#wechat_redirect)
    

**代码实现**
--------

首先导入需要的库：

```
1from docx import Document
2from openpyxl import load_workbook
3import glob
将模板 Excel 读取进程序：
```

```
1path  = r'C:\Users\xxx' # 路径为会议通知文件夹和 Excel 模板所在的位置，可按实际情况更改
2workbook = load_workbook(path + r'\Meeting_temp.xlsx')
3sheet = workbook.active

写任何批处理的代码之前都建议先写一下单次操作的代码，因此我们先完成对 会议通知 1.docx 文件的解析，确保无误。现在对于文档的结构和关键信息的位置尚不明确，可以先将 Word 以段落 Paragraph 为单位输出观察：
```

```
1wordfile = Document(path + r'\Notice\会议通知 1.docx')
2for paragraph in wordfile.paragraphs:
3    print(paragraph)
```

![](https://mmbiz.qpic.cn/mmbiz_jpg/VeJKXItpwPXWLpENCwyR2GLh1A8ztFHTjCo6nflicA5u2vVQoiaFCtIibe8sHu2uxYP7QurNiazicI61KkH49y5IL5w/640?wx_fmt=jpeg)

文件的文字排布脉络比较清晰，基本是一句话对应一个段落，而需要的信息可以简单通过判断每句话（每段话）前几个字而明确：

```
1for paragraph in wordfile.paragraphs:
2        if paragraph.text[0:5] == '学习时间：':
3            study_time = paragraph.text[5:]
4        if paragraph.text[0:4] == '主持人：':
5            host = paragraph.text[4:]
6        if paragraph.text[0:5] == '学习形式：':
7            study_type = paragraph.text[5:]

对于学习内容的获取比较特殊，不像其他三个信息，都在一句话中，且关键字就为前几个字：
```

![](https://mmbiz.qpic.cn/mmbiz_jpg/2GcSFhuAFlDSCRiblfVbaejgxvicQmRSoVibggmOEfstNNzWf5YWMT3mmSztWTtbp8MJ9rJ3ZjY5RibWce9tQNiahEg/640?wx_fmt=jpeg)

可以看到，“学习内容” 四个字和真正包含的内容分散在不同的句子中. 这里简单用一个策略：

建立一个空列表存放，然后遍历每一段判断，如果一个字符为数字且第二个字符为中文顿号 “、” 就获取存放到列表中。最后把列表中的元素重新组合成一个长字符串即可：

```
1content_lst = []
 2    for paragraph in wordfile.paragraphs:
 3        if paragraph.text[0:5] == '学习时间：':
 4            study_time = paragraph.text[5:]
 5        if paragraph.text[0:4] == '主持人：':
 6            host = paragraph.text[4:]
 7        if paragraph.text[0:5] == '学习形式：':
 8            study_type = paragraph.text[5:]
 9        if len(paragraph.text) >= 2:
10            if paragraph.text[0].isdigit() and paragraph.text[1] == '、':
11                content_lst.append(paragraph.text)
12    content = ' '.join(content_lst)

完成了解析 Word 文件之后，就需要把内容输出的 Excel 文件中了。
```

简单来说，就是将上面代码获取到的几个元素组合成一个列表，通过 sheet.append(list) 的方法写入 Excel 文件中：

```
1number = 0 # 全局中设置一个变量用于计数，做为序号输出
 3wordfile = Document(path + r'\Notice\会议通知 1.docx')
 4content_lst = []
 5for paragraph in wordfile.paragraphs:
 6    if paragraph.text[0:5] == '学习时间：':
 7        study_time = paragraph.text[5:]
 8    if paragraph.text[0:4] == '主持人：':
 9        host = paragraph.text[4:]
10    if paragraph.text[0:5] == '学习形式：':
11        study_type = paragraph.text[5:]
12    if len(paragraph.text) >= 2:
13        if paragraph.text[0].isdigit() and paragraph.text[1] == '、':
14            content_lst.append(paragraph.text)
15content = ' '.join(content_lst)
16number += 1
17sheet.append([number, study_time, content, study_type, host])

单个文件解析完，用 glob 改完获取文件夹下全部文件，建立循环逐个解析就能完成本需求，当然最后记得保存 Excel 文件。
```

完整代码如下👇

```
1from docx import Document
 2from openpyxl import load_workbook
 3import glob
 5path  = r'C:\Users\xxx'
 6workbook = load_workbook(path + r'\Meeting_temp.xlsx')
 7sheet = workbook.active
 8number = 0
10for file in glob.glob(path + r'\Notice\*.docx'):
11    wordfile = Document(file)
12    content_lst = []
13    for paragraph in wordfile.paragraphs:
14        if paragraph.text[0:5] == '学习时间：':
15            study_time = paragraph.text[5:]
16        if paragraph.text[0:4] == '主持人：':
17            host = paragraph.text[4:]
18        if paragraph.text[0:5] == '学习形式：':
19            study_type = paragraph.text[5:]
20        if len(paragraph.text) >= 2:
21            if paragraph.text[0].isdigit() and paragraph.text[1] == '、':
22                content_lst.append(paragraph.text)
23    content = ' '.join(content_lst)
24    number += 1
25    sheet.append([number, study_time, content, study_type, host])
27workbook.save(path + r'\Meeting_notice.xlsx')
```

核心也不过三十行代码，总共不过三秒就搞定了！

如果你也想试试，可以尝试用文中的代码实现，练习数据：

https://pan.baidu.com/s/1H3x16tYAqqaEamw4fdzlrw（提取码：86vc）

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
