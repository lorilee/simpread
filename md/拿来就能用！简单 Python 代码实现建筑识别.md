> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/JRCLfLUBD81sPSE09ibq5A)

![](https://mmbiz.qpic.cn/mmbiz_gif/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO7vgTGP53icxyLKZKC6yAKvs5s479MoYDVbObrnSuxoXbzNIFMmaX83g/640?wx_fmt=gif)

![](https://mmbiz.qpic.cn/mmbiz_jpg/VeJKXItpwPWFxD6CCHbrdbwqNGiciamJfp1Ddgc0ficM3prKPPoC54O0pyKh47QLu1OJHVib3JgJ1UpVsIDDZafKxQ/640?wx_fmt=jpeg)

作者 | 李秋键

责编 | 伍杏玲

出品 | AI 科技大本营（ID:rgznai100）

在人工智能的发展越来越火热的今天，其中最具有代表性的便是图像识别，其应用比比皆是，如车站的人脸识别系统，交通的智能监控车牌号系统等。卷积神经网络作为图像识别的首选算法，其对于图像的特征提取具有很好的效果，Keras 框架作为卷积神经网络的典型框架，可创建神经网络层，更容易提取图像特征，从而达到区分动物的目的，在生产实践中达到辅助的效果。

这里的建筑物主要是借助神经网络搭建几种常见动物的识别系统，先设定了识别高楼、平房、桥梁和公路四种建筑物。程序分为四个部分：整体网络搭建程序，文件夹整体预测、网络层绘制程序和单个预测可视化程序，分别为 deep learning.py，predict.py、create_graph.py 和 predict-show.py。实现效果如下图：

![](https://mmbiz.qpic.cn/mmbiz_jpg/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPuBaFalMT4JDDvkcWLv7YRCd8twdGapLsp6Q3wicSicQ2ia0O83Ws0RFCg/640?wx_fmt=jpeg)

![](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPicbLA2ybeY4CHKeSS9KAhucoUvEDNG1vzmkarjZvJX67ibKY8fLnRoDA/640?wx_fmt=png)

**![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOc2rXicCsC79VyqrfQywa6E74wdAaexVSMb4RYiacoJ9E71XJcjnkdWwA/640?wx_fmt=jpeg)**

**实验前的准备**
----------

Python 版本是 3.6.5；使用的神经网络是 CNN；所用到的搭建网络层的库是 Keras。

Keras 是一个高层神经网络 API，由纯 Python 编写而成并基 Tensorflow、Theano 以及 CNTK 后端。Keras 为支持快速实验而生，能够把你的 idea 迅速转换为结果，Keras 的基本优点如下：

1、简易和快速的原型设计（keras 具有高度模块化，极简，和可扩充特性）

2、支持 CNN 和 RNN，或二者的结合

3、无缝 CPU 和 GPU 切换

卷积神经网络，是一种前馈神经网络，人工神经元可以响应周围单元，可以进行大型图像处理。卷积神经网络包括卷积层和池化层。

积神经网络 CNN 的结构一般包含这几个层：

输入层：用于数据的输入

卷积层：使用卷积核进行特征提取和特征映射

激励层：由于卷积也是一种线性运算，因此需要增加非线性映射

池化层：进行下采样，对特征图稀疏处理，减少数据运算量。

全连接层：通常在 CNN 的尾部进行重新拟合，减少特征信息的损失

输出层：用于输出结果

当然中间还可以使用一些其他的功能层:

归一化层（Batch Normalization）：在 CNN 中对特征的归一化

切分层：对某些（图片）数据的进行分区域的单独学习

融合层：对独立进行特征学习的分支进行融合

**![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO3ToytjEfeNjJlGPcZqK98xO4MJ2zC1xj07PfS6kAic0LAlMKFBLvWPQ/640?wx_fmt=jpeg)**

**数据集准备**
---------

我们需要准备训练的数据集文件，我找的图片都是来自于百度，将其保存在./data/train 文件夹下，建立四个文件夹，分别保存公路、桥梁、高楼和平房的图片，需要训练的数据集的标签就是文件夹的名称。train 文件夹数据集如下：

![](https://mmbiz.qpic.cn/mmbiz_jpg/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPfY2bdbZibGlMohboek0DPG7sZibrdgDE4WeI24XIcLdibjzWVV7HYliarA/640?wx_fmt=jpeg)

建立./data/test 文件夹，里面放入测试的图片，作为测试集使用。如下图可见：  

![](https://mmbiz.qpic.cn/mmbiz_jpg/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPjsN8l9ibLOkDcWWXfFNIbDfB9oUkHUs80EibeTjI6HniaXgOGsJIPjicTw/640?wx_fmt=jpeg)

**![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOPyiaaIVCePZnia426uNibf0PLTPfS7VRaWql6vFWCwb2Cqgjp06UvU39Q/640?wx_fmt=jpeg)**

**网络层的搭建**
----------

**（1）预处理：**

为了提高模型的精准度，需要在图片中进行旋转角度，平移等图形变化形成新的图形拿来训练从而提高精准度代码如下：

```
train_datagen = ImageDataGenerator(
    rotation_range = 40,     # 随机旋转度数
    width_shift_range = 0.2, # 随机水平平移
    height_shift_range = 0.2,# 随机竖直平移
    rescale = 1/255,         # 数据归一化
    shear_range = 20,       # 随机错切变换
    zoom_range = 0.2,        # 随机放大
    horizontal_flip = True,  # 水平翻转
    fill_mode = 'nearest',   # 填充方式
)
```

接着定义一些用到参数变量，其中包括统一图片的尺寸、分类数量、训练批次和模型大小等等：

```
IMG_W = 224 #定义裁剪的图片宽度
IMG_H = 224 #定义裁剪的图片高度
CLASS = 4 #图片的分类数
EPOCHS = 200 #迭代周期
BATCH_SIZE = 64 #批次大小
TRAIN_PATH = 'data/train' #训练集存放路径
TEST_PATH = 'data/test' #测试集存放路径
SAVE_PATH = 'buildings_selector' #模型保存路径
LEARNING_RATE = 1e-4 #学习率
DROPOUT_RATE = 0 #抗拟合，不工作的神经网络百分比
```

**（2）网络层搭建：**

创建多个卷积层以及池化层，每一层的输入为上一层的输出，其中池化层类似卷积层，主要目的在于降采样，减少训练的数值，可以防止过拟合，代码如下：

```
#再次构建一个卷积层
model.add(Conv2D(filters=32,kernel_size=3,padding='same',activation='relu'))
#构建一个池化层，提取特征，池化层的池化窗口为2*2，步长为2。
model.add(MaxPool2D(pool_size=2,strides=2))
#继续构建卷积层和池化层，区别是卷积核数量为64。
model.add(Conv2D(filters=64,kernel_size=3,padding='same',activation='relu'))
model.add(Conv2D(filters=64,kernel_size=3,padding='same',activation='relu'))
model.add(MaxPool2D(pool_size=2,strides=2))
#继续构建卷积层和池化层，区别是卷积核数量为128。
model.add(Conv2D(filters=128,kernel_size=3,padding='same',activation='relu'))
model.add(Conv2D(filters=128,kernel_size=3,padding='same',activation='relu'))
model.add(MaxPool2D(pool_size=2, strides=2))
model.add(Flatten()) #数据扁平化
model.add(Dense(128,activation='relu')) #构建一个具有128个神经元的全连接层
model.add(Dense(64,activation='relu')) #构建一个具有64个神经元的全连接层
model.add(Dropout(DROPOUT_RATE)) #加入dropout，防止过拟合。
model.add(Dense(CLASS,activation='softmax')) #输出层，一共4个神经元，对应4个分类
```

**（3）模型训练：**

创建优化器和模型。代码如下：

```
train_generator = train_datagen.flow_from_directory( #设置训练集迭代器
    TRAIN_PATH, #训练集存放路径
    target_size=(IMG_W,IMG_H), #训练集图片尺寸
    batch_size=BATCH_SIZE #训练集批次
    )
test_generator = test_datagen.flow_from_directory( #设置测试集迭代器
    TEST_PATH, #测试集存放路径
    target_size=(IMG_W,IMG_H), #测试集图片尺寸
    batch_size=BATCH_SIZE, #测试集批次
    )
print(train_generator.class_indices) #打印迭代器分类，各分类代号为{'du': 0, 'gui': 1,'kang': 2, 'tao': 3}
try:
    model =load_model('{}.h5'.format(SAVE_PATH))  #尝试读取训练好的模型，再次训练
    print('model upload,starttraining!')
except:
    print('not find model,starttraining') #如果没有训练过的模型，则从头开始训练
model.fit_generator( #模型拟合
                   train_generator,  #训练集迭代器
                   steps_per_epoch=len(train_generator), #每个周期需要迭代多少步（图片总量/批次大小=11200/64=175）
                    epochs=EPOCHS, #迭代周期
                    validation_data=test_generator,#测试集迭代器
                   validation_steps=len(test_generator) #测试集迭代多少步
                    )
model.save('{}.h5'.format(SAVE_PATH)) #保存模型
print('finish {} epochs!'.format(EPOCHS))
```

![](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPTiay8x19qqdHEBdBEecxAvYWfG0JzJswVE33GHq7eVCWAf6NdDMF4GQ/640?wx_fmt=png)

```
最终训练的结果200次模型的精准度89%。
```

**![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOPyiaaIVCePZnia426uNibf0PLTPfS7VRaWql6vFWCwb2Cqgjp06UvU39Q/640?wx_fmt=jpeg)**

**数据预测**
--------

```
利用创建好的模型进行试验，建立predict.py文件用来预测图片，将要试验的图片放在./predict文件夹下，首先需要导入库的代码为：
```

```
from keras.models import load_model
from keras.preprocessing.image import img_to_array,load_img
import numpy as np
import os
import matplotlib.pyplot as plt
from matplotlib.font_manager import FontProperties
```

```
接着载入模型，由训练部分的代码可知模型文件为building_selector.h5
```

```
model = load_model('building_selector.h5')
```

```
分类分别为高楼、公路、平房、桥梁，建立一个数组用来保存
```

```
label = np.array(['高楼','公路','平房','桥梁'])
```

```
定义函数，用来把图片转成可以识别的矩阵，由于激活函数值在0~1之间，故需要将其中矩阵值变在0~1之间，故需要除以255以达到目的
```

```
def image_change(image):
 image = image.resize((224, 224))
 image = img_to_array(image)
 image = image / 255
 image = np.expand_dims(image, 0)
 return image
```

```
最终识别效果如下：
```

![](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPn0IiaNUJ1dxe8VdTMadiahoz4ticyQRANHPBZBy8BYTdIWK4mRkMnrkLg/640?wx_fmt=png)

predict 预测结果图  

建立 predict-show.py 程序，里面的整体结构和 predict.py 类似，加上图片标注显示即可。

```
image = load_img("2.jpg")
image = image_change(image)
img=cv2.imread("2.jpg")
img_PIL = Image.fromarray(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
myfont = ImageFont.truetype(r'C:/Windows/Fonts/simfang.ttf', 40)
draw = ImageDraw.Draw(img_PIL)
draw.text((300, 10), label[model.predict_classes(image)][0], font=myfont,fill=(200, 100, 0))
img_OpenCV = cv2.cvtColor(np.asarray(img_PIL), cv2.COLOR_RGB2BGR)
cv2.imshow('frame', img_OpenCV)
cv2.waitKey(0)
```

最终效果如下：

源码地址：https://pan.baidu.com/s/1e5SnM_rL2sxcO_ceC4l99g

提取码：1q5k

作者简介：

李秋键，CSDN 博客专家，CSDN 达人课作者。硕士在读于中国矿业大学，开发有 taptap 竞赛获奖等。

![](https://mmbiz.qpic.cn/mmbiz_jpg/Pn4Sm0RsAuggpZ0lia5ZjmiaqEIGqmY9Ch46r8l3Cv9Lvuqq7ic7xlHVf7zNlwgiaYRMdLib25h7pMEunOCf4IwibDRw/640?wx_fmt=jpeg)

```
更多精彩推荐
```

```
☞科学“干饭”，AI 营养师教你
☞GitHub 标星超 26600，TiDB 社区运营的道与术！
☞区区几行Python代码，一分钟搞定一天工作量
☞读懂深度迁移学习，看这文就够了 
```

```
点分享
点收藏
点点赞
点在看
```