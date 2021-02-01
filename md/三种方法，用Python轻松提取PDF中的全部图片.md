> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/Y7temTf4fcXUtzV9ZnXmrg)

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

作者 | 陈熹、刘早起  

来源 | 早起Python

头图 | 下载于视觉中国

  

有时我们需要**将一份或者多份PDF文件中的图片提取出来**，如果采取在线的网站实现的话又担心图片泄漏，手动操作又觉得麻烦，其实用Python也可以轻松搞定！  

今天就跟大家系统分享几种**Python提取 PDF 图片的方法。**其实没有非常完美的方法，每种方法提取效率都不是百分之百，因此可以考虑用多种方法进行互补，主要将涉及：

*   基于 fitz 库和正则搜索提取图片
    
*   基于 pdf2image 库的两种方法提取图片
    

  

---

![Image](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOc2rXicCsC79VyqrfQywa6E74wdAaexVSMb4RYiacoJ9E71XJcjnkdWwA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

基于 fitz 库和正则搜索  

-----------------

  

fitz 是 pymupdf 的子模块，需要先用命令行安装 pymupdf：

```
1pip install pymupdf

```

但注意导入时使用 import fitz 导入模块!  

下面的代码就利用 fitz 库提取图片需要通过正则匹配图片元素，将模板元素转化为像素后再以图片形式写出

```
 1import fitz
 2import re
 3import os
 4
 5file_path = r'C:\xxx\xxx.pdf' # PDF 文件路径
 6dir_path = r'C:\xxx' # 存放图片的文件夹
 7
 8def pdf2image1(path, pic_path):
 9    checkIM = r"/Subtype(?= */Image)"
10    pdf = fitz.open(path)
11    lenXREF = pdf._getXrefLength()
12    count = 1
13    for i in range(1, lenXREF):
14        text = pdf._getXrefString(i)
15        isImage = re.search(checkIM, text)
16        if not isImage:
17            continue
18        pix = fitz.Pixmap(pdf, i)
19        new_name = f"img_{count}.png"
20        pix.writePNG(os.path.join(pic_path, new_name))
21        count += 1
22        pix = None
23
24pdf2image1(file_path, dir_path)

```

运行提取示例文件后结果如下：

![Image](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPXmquHteiajpmib19cYhRfKUMZK69FL68HTPjFQoH1FPNxmqGIWd4QsicBNeEBVddezdSUgXWeaw4kxQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

可以看到，**有一些很小的色块也被提取成图片**，那么怎么过滤掉它们呢？  

有一个简单的方法是**通过大小过滤**，pix 像素在 fitz 库中存在一个重要的方法 pix.size 可以反映像素多少，简单的色素块该值较低，可以通过设置一个阈值过滤。以阈值 10000 为例过滤：

```
 1import fitz
 2import re
 3import os
 4
 5file_path = r'C:\xxx\xxx.pdf' # PDF 文件路径
 6dir_path = r'C:\xxx' # 存放图片的文件夹
 7
 8def pdf2image1(path, pic_path):
 9    checkIM = r"/Subtype(?= */Image)"
10    pdf = fitz.open(path)
11    lenXREF = pdf._getXrefLength()
12    count = 1
13    for i in range(1, lenXREF):
14        text = pdf._getXrefString(i)
15        isImage = re.search(checkIM, text)
16        if not isImage:
17            continue
18        pix = fitz.Pixmap(pdf, i)
19        if pix.size < 10000: # 在这里添加一处判断一个循环
20            continue # 不符合阈值则跳过至下
21        new_name = f"img_{count}.png"
22        pix.writePNG(os.path.join(pic_path, new_name))
23        count += 1
24        pix = None
25
26pdf2image1(file_path, dir_path)



```

可以看到，全部图片都被正常提取！  

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

基于 pdf2image 库的两种方法

  

看名字就知道这个库的用处，官方文档：

https://www.cnpython.com/pypi/pdf2image

可以简单通过 pip install pdf2image 安装，但poppler才是真正起做用的转换器，因此需要额外安装和配置：

*   windows用户必须安装poppler for Windows，然后将bin/文件夹添加到PATH
    
*   Mac用户必须安装poppler for Mac
    

具体发挥作用的代码官方文档也给出了详细的说明：

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

那么我们就分别尝试这两种方法：  

```
 1from pdf2image import convert_from_path,convert_from_bytes
 2import tempfile
 3from pdf2image.exceptions import PDFInfoNotInstalledError, PDFPageCountError, PDFSyntaxError
 4import os
 5
 6file_path = r'C:\xxx\xxx.pdf' # PDF 文件路径
 7dir_path = r'C:\xxx' # 存放图片的文件夹
 8
 9def pdf2image2(file_path, dir_path):
10    images = convert_from_path(file_path, dpi=200)
11    for image in images:
12        if not os.path.exists(dir_path):
13            os.makedirs(dir_path)
14        image.save(file_path + f'\img_{images.index(image)}.png', 'PNG')
15
16pdf2image2(file_path, dir_path)

```

可以成功提取图片。再试试第二种方法：

```
 1from pdf2image import convert_from_path,convert_from_bytes
 2import tempfile
 3from pdf2image.exceptions import PDFInfoNotInstalledError, PDFPageCountError, PDFSyntaxError
 4import os
 5
 6file_path = r'C:\xxx\xxx.pdf' # PDF 文件路径
 7dir_path = r'C:\xxx' # 存放图片的文件夹
 8
 9def pdf2image3(file_path, dir_path):
10    images = convert_from_bytes(open(file_path, 'rb').read())
11    for image in images:
12        if not os.path.exists(dir_path):
13            os.makedirs(dir_path)
14        image.save(file_path + f'\img_{images.index(image)}.png', 'PNG')
15
16pdf2image3(file_path, dir_path)




```

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

可以看到结果和之前一致，PDF中全部图片都被提取出来！  

再补充一下。核心方法covert_from_bytes包含大量参数，可以自行修改。几个常用参数总结如下：

| 

参数

 | 

意义

 |
| --- | --- |
| 

pdf_path

 | 

PDF 文档路径

 |
| 

dpi

 | 

图像质量（如果是学术期刊杂志常见 300dpi）

 |
| 

output_folder

 | 

将生成的图像写入文件夹（而不是直接写入内存）

 |
| 

first_page

 | 

起始转换页数

 |
| 

last_page

 | 

转换至哪一页

 |
| 

fmt

 | 

图像格式，可以指定为 png，默认为 ppm

 |
| 

thread_count

 | 

允许参与转换的线程数

 |
| 

userpw

 | 

PDF 的密码

 |
| 

output_file

 | 

输出文件名

 |
| 

poppler_path

 | 

指定 poppler 的安装路径，一开始配置好就无需指定

 |

值得一提的是thread_count 参数，可以启动多线程会大大加快转换速度，尤其是 PDF 页面较多时。有兴趣的读者可以做尝试。  

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