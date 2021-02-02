> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://juejin.im/post/5adf018b6fb9a07aa541f426

**通过安装 Anaconda3，使用 matplotlib 库**

**0. 基本绘画设置**  
**(1). Figure 和 Subplot**  
matplotlib 的图像都位于 Figure 对象中，你可以用 plt.figure 创建一个新的 Figure.

```
#这时会弹出一个空窗
fig = plt.figure()12复制代码
```

但是不能通过空 Figure 绘图，必须用 add_subplot 创建一个或多个 subplot 才行

```
#这条代码的意思是：图像应该是2X2的，且当前选中的是第一个
ax1 = fig.add_subplot(2,2,1)

#再绘制两个
ax2 = fig.add_subplot(2,2,2)
ax3 = fig.add_subplot(2,2,3)123456复制代码
```

此时绘图，就会在 subplot 上进行绘制  
例如：

```
from numpy.random import randn
plt.plot(randn(50).cumsum(), 'k--')
#'k--'是一个线型选项，用于告诉matplotlib绘制黑色虚线图。123复制代码
```

别一种更为方便的方法，可以创建一个新的 Figure，并返回一个含有已创建 subplot 对象的 NumPy 数组：

```
#创建了一个两行，三列的区域
fig, axes = plt.subplots(2,3)12复制代码
```

![](https://user-gold-cdn.xitu.io/2018/4/24/162f71e027ae54d5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**(2). 调整 subplot 周围的间距**  
默认情况下，matplotlib 会在 subplot 外围留下一定的边距，并在 subplot 之间留下一定的间距。  
利用 subplots_adjust 方法可以调整间距

```
fig, axes = plt.subplots(2,2, sharex=True, sharey=True)
for i in range(2):
    for j in range(2):
        axes[i,j].hist(randn(500), bins=50, color='k', alpha=0.5)

plt.subplots_adjust(wspace=0, hspace=0)123456复制代码
```

![](https://user-gold-cdn.xitu.io/2018/4/24/162f71e028052732?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**(3). 颜色、标记和线型**  
根据 x,y 绘制绿色虚线

```
ax.plot(x, y, 'g--')
#与下面设置是一样的
ax.plot(x, y, linestyle='--', color='g')123复制代码
```

线型图可以加上一些标记，来强调实际的点

```
plt.plot(randn(30).cumsum(), 'ko--')
#等价于
plt.plot(randn(30).cumsum(), color='k', linestyle='dashed', marker='o')123复制代码
```

**(4). 刻度、标签和图例**  
**添加刻度和标签**

```
fig = plt.figure()
ax = fig.add_subplot(1,1,1)
ax.plot(randn(1000).cumsum())

#修改X轴的刻度
#刻度放在哪些位置
ticks = ax.set_xticks([0,250,500,750,1000])
#也可以将其他值用作标签
labels = ax.set_xticklabels(['one','two','three','four','five'], rotation=30, fontsize='small')

#为X轴设置一个名称
ax.set_title('My first matplotlib plot')
#设置一个标题
ax.set_xlabel('Stages')1234567891011121314复制代码
```

![](https://user-gold-cdn.xitu.io/2018/4/24/162f71e02406a392?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**添加图例：**

```
fig = plt.figure()
ax = fig.add_subplot(1,1,1)
ax.plot(randn(1000).cumsum(), 'k', label='one')
ax.plot(randn(1000).cumsum(), 'k--', label='two')
ax.plot(randn(1000).cumsum(), 'k.', label='three')

ax.legend(loc='best')1234567复制代码
```

![](https://user-gold-cdn.xitu.io/2018/4/24/162f71e028c117d8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**1. 绘制简单的折线图**

**(1). 函数 plot()**  
列表传递给这个函数, 这个函数将尝试根据这些数字绘制出有意义的图形。

```
import matplotlib.pyplot as plt
squares = [1, 4, 9, 16, 25]
plt.plot(squares)
plt.show()1234复制代码
```

**(2). 修改标签文字和线条粗细**

```
import matplotlib.pyplot as plt
squares = [1, 4, 9, 16, 25]

#参数linewidth 决定了plot() 绘制的线条的粗细
plt.plot(squares, linewidth=5)


# 设置图表标题， 并给坐标轴加上标签
plt.title("Square Numbers", fontsize=24)
plt.xlabel("Value", fontsize=14)
plt.ylabel("Square of Value", fontsize=14)

# 设置刻度标记的大小
plt.tick_params(axis='both', labelsize=14)
plt.show()123456789101112131415复制代码
```

**(3). 校正图形**

*   当你向 plot() 提供一系列数字时， 它假设第一个数据点对应的 x 坐标值为 0， 但我们的第一个点对应的 x 值为 1。
*   为改变这种默认行为， 我们可以给 plot() 同时提供输入值和输出值：

```
input_values = [1, 2, 3, 4, 5]
squares = [1, 4, 9, 16, 25]
plt.plot(input_values, squares, linewidth=5)123复制代码
```

**2. 绘制绘制散点图**  
**(1). 使用 scatter() 绘制散点图并设置其样式**

```
import matplotlib.pyplot as plt
plt.scatter(2, 4)
plt.show()123复制代码
```

**(2). 使用 scatter() 绘制一系列点**

```
import matplotlib.pyplot as plt

x_values = [1, 2, 3, 4, 5]
y_values = [1, 4, 9, 16, 25]
#并使用实参s 设置了绘制图形时使用的点的尺寸
plt.scatter(x_values, y_values, s=100)
plt.show()1234567复制代码
```

**(3). 删除数据点的轮廓**  
可在调用 scatter() 时传递实参 edgecolor=’none’

```
plt.scatter(x_values, y_values, edgecolor='none', s=40)1复制代码
```

**(4). 自定义颜色**  
要修改数据点的颜色， 可向 scatter() 传递参数 c ， 并将其设置为要使用的颜色的名称，

```
plt.scatter(x_values, y_values, c='red', edgecolor='none', s=40)1复制代码
```

**(5). 使用颜色映射**  
颜色映射 （ colormap） 是一系列颜色， 它们从起始颜色渐变到结束颜色。

```
import matplotlib.pyplot as plt

x_values = list(range(1001))
y_values = [x**2 for x in x_values]
plt.scatter(x_values, y_values, c=y_values, cmap=plt.cm.Blues, edgecolor='none', s=40)


# 设置每个坐标轴的取值范围
plt.axis([0, 1100, 0, 1100000])
plt.show()12345678910复制代码
```

*   我们将参数 c 设置成了一个 y 值列表， 并使用参数 cmap 告诉 pyplot 使用哪个颜色映射。
*   这些代码将 y 值较小的点显示为浅蓝色， 并将 y 值较大的点显示为深蓝色.

**(6). 自动保存图表**  
要让程序自动将图表保存到文件中， 可将对 plt.show() 的调用替换为对 plt.savefig() 的调用

```
plt.savefig('squares_plot.png', bbox_inches='tight')1复制代码
```

*   第一个实参指定要以什么样的文件名保存图表， 这个文件将存储到 scatter_squares.py 所在的目录中；
*   第二个实参指定将图表多余的空白区域裁剪掉。 如果要保留图表周围多余的空白区域， 可省略这个实参。

**(7). 隐藏坐标轴**

plt.axes().get_xaxis().set_visible(False)  
plt.axes().get_yaxis().set_visible(False)

**(8). 调整尺寸以适合屏幕**  
函数 figure() 用于指定图表的宽度、 高度、 分辨率和背景色。  
你需要给形参 figsize 指定一个元组， 向 matplotlib 指出绘图窗口的尺寸， 单位为英寸。  
plt.figure(figsize=(10, 6))

**3. 随机漫步**  
**choice() 函数**

```
#choice([0, 1, 2, 3, 4]) 随机地选择一个0~4之间的整数
x_distance = choice([0, 1, 2, 3, 4])12复制代码
```

**range() 函数**  
返回一系列连续增加的整数

**randint() 函数**  
返回一个 1 和面数之间的随机数