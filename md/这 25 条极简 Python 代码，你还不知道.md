> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/12St8DSgNSTPR9pRlsdTGQ)

作者 | 小 F

来源 | 法纳斯特

头图 | 下载于视觉中国

自从我用 Python 编写第一行代码以来，就被它的简单性、出色的可读性和特别流行的一行代码所吸引。

下面，我将给大家介绍一些 Python 一行程序。

可能有些你还不知道，但对你未来的 Python 项目很有用。

1、交换两个变量

```
# a = 4 b = 5
a,b = b,a
# print(a,b) >> 5,4
```

让我们通过交换两个变量作为一个简单的开始。这是最简单、最直观的方法之一，无需使用临时变量或应用算术操作即可编写。

2、多个变量赋值

```
a,b,c = 4,5.5,'Hello'
#print(a,b,c) >> 4,5.5,hello
```

你可以使用逗号和变量一次性将多个值分配给变量。使用此技术，你可以一次分配多个数据类型。 

你可以使用列表将值分配给变量。下面是将列表中的多个值分配给变量的示例。

```
a,b,*c = [1,2,3,4,5]
print(a,b,c)
> 1 2 [3,4,5]
```

3、列表中偶数的和

有很多方法可以做到这一点，但最好和最简单的方法是使用列表索引和 sum 函数。

```
a = [1,2,3,4,5,6]
s = sum([num for num in a if num%2 == 0])
print(s)
>> 12
```

4、从列表中删除多个元素  

del 是 Python 中用于从列表中删除值的关键字。

```
#### Deleting all even
a = [1,2,3,4,5]
del a[1::2]
print(a)

>[1, 3, 5]

a
```

5、读取文件

```
lst = [line.strip() for line in open('data.txt')]
print(lst)
```

这里我们使用列表来处理。  

首先，我们打开一个文本文件，并使用 for 循环，逐行读取。 最后，使用 strip 删除所有不必要的空间。   

通过使用列表功能，使得代码更简单，更短。

```
list(open('data.txt'))

##Using with will also close the file after use
with open("data.txt") as f:
    lst=[line.strip() for line in f]
print(lst)
```

6、将数据写入文件

```
with open("data.txt",'a',newline='\n') as f: 
    f.write("Python is awsome")
```

上面的代码首先创建一个文件 data.txt(如果没有的话)，然后它会在文件中写 Python is awesome。  

7、创建列表

```
lst = [i for i in range(0,10)]
print(lst)
> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

or

lst = list(range(0,10))
print(lst)
```

我们也可以使用相同的方法创建一个字符串列表。  

```
lst = [("Hello "+i) for i in ['Karl','Abhay','Zen']]
print(lst)
> ['Hello Karl', 'Hello Abhay', 'Hello Zen']
```

8、映射列表或类型转换整个列表  

有时在我们的项目中，我们需要更改列表中所有元素的数据类型。你想到的第一个方法可能是使用循环，然后访问列表中的所有元素，然后一个接一个地更改元素的数据类型。

这个方法是老派的，在 Python 中我们有一个映射函数，可以为我们做这些工作。

```
list(map(int,['1','2','3']))
> [1, 2, 3]

list(map(float,[1,2,3]))
> [1.0, 2.0, 3.0]

[float(i) for i in [1,2,3]]
> [1.0, 2.0, 3.0]
```

9、创建集合  

我们用于创建列表的方法也可以用于创建集合。让我们使用包含范围内所有偶数的平方根方法来创建一个集合。

```
#### Square of all even numbers in an range
{x**2 for x in range(10) if x%2==0}

> {0, 4, 16, 36, 64}
```

10、Fizz Buzz  

在这个测试中，我们需要编写一个程序来打印从 1 到 20 的数字。但如果是 3 的倍数，打印 Fizz，如果是 5 的倍数，打印 Buzz，如果同时是 3 和 5 的倍数，打印 FizzBuzz，否则打印数字。  

看起来我们必须使用循环和多个 if-else 语句。如果你尝试用其他语言来做，你可能需要写 10 行代码，但是使用 Python，我们可以只用一行代码就实现 FizzBuzz。

```
['FizzBuzz' if i%3==0 and i%5==0
    else 'Fizz' if i%3==0 
    else 'Buzz' if i%5==0 
    else i  for i in range(1,20)]
```

在上面的代码中，我们使用列表理解来运行一个从 1 到 20 的循环，然后在循环的每次迭代中，我们检查数字是否能被 3 或 5 整除。如果是，那么我们用 Fizz 或 Buzz 替换数值，或者用 FizzBuzz 数值。  

11、回文  

回文是一个数字或字符串，当它被反转时看起来是一样的。

```
text = 'level'
ispalindrome = text == text[::-1]
ispalindrome

> True
```

12、用空格分隔的整数到一个列表

```
lis = list(map(int, input().split()))
print(lis)

> 1 2 3 4 5 6 7 8
[1, 2, 3, 4, 5, 6, 7, 8]
```

13、Lambda 函数  

lambda 函数是一个小型匿名函数。lambda 函数可以接受任意数量的参数，但只能有一个表达式。

```
# Function that returns square of any number
sqr = lambda x: x * x
sqr(10)

> 100
```

14、检查列表中数字的存在

```
num = 5
if num in [1,2,3,4,5]:
     print('present')

> present  
```

15、打印图案  

在 Python 中，我们只需要一行代码就可以绘制出令人惊叹的图案。

```
n = 5
print('\n'.join('😀' * i for i in range(1, n + 1)))

>
😀
😀😀
😀😀😀
😀😀😀😀
😀😀😀😀😀  
```

16、查找阶乘  

阶乘是一个整数和它下面所有整数的乘积。

```
import math
n = 6
math.factorial(n)

> 720  
```

17、斐波纳契数列

一组数字，其中每个数字 (斐波那契数) 是前面两个数字的和。最简单的斐波那契数列 1，1，2，3，5，8，13 等等。可以使用列表推导式和 for 循环在一个范围内创建斐波那契数列。

```
fibo = [0,1]
[fibo.append(fibo[-2]+fibo[-1]) for i in range(5)]
fibo

> [0, 1, 1, 2, 3, 5, 8]  
```

18、质数

质数是一个只能被自身和 1 整除的数。例如: 2、3、5、7 等。为了在一个范围内生成质数，我们可以使用带有 filter 和 lambda 的 list 函数来生成质数。

```
list(filter(lambda x:all(x % y != 0 for y in range(2, x)), range(2, 13)))

> [2, 3, 5, 7, 11]  
```

19、查找最大数值

```
findmax = lambda x,y: x if x > y else y 
findmax(5,14)

> 14

or 
max(5,14)  
```

在上面的代码中，我们使用 lambda 函数检查比较条件，并根据返回的最大值。

或者使用 max() 内置函数。

20、线性代数

有时候我们需要将列表中的元素缩放 2 到 5 倍。下面的代码解释了如何做到这一点。

```
def scale(lst, x):
    return [i*x for i in lst] 


scale([2,3,4], 2)

> [4,6,8]  
```

21、矩阵转置

您需要将所有行转换为列，反之亦然。在 Python 中，可以使用 zip 函数在一行代码中置换一个矩阵。

```
a=[[1,2,3],
   [4,5,6],
   [7,8,9]] 
transpose = [list(i) for i in zip(*a)] 
transpose

> [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
```

22、计数

当我们需要知道某个值在文本中出现的次数时，这是一个重要而有用的用例。在 Python 中，有 re 库可以来帮你完成这项工作。

```
import re

len(re.findall('python','python is a programming language. python is python.'))

> 3
```

23、用其他文本替换文本

```
"python is a programming language.python is python".replace("python",'Java')

> Java is a programming language. Java is Java
```

24、模拟抛硬币

这可能不是那么重要，但当你需要从一组给定的选择中生成一些随机选择时，它会非常有用。

```
import random

random.choice(['Head',"Tail"])

> Head
```

25、生成组

```
groups = [(a, b) for a in ['a', 'b'] for b in [1, 2, 3]] 
groups

> [('a', 1), ('a', 2), ('a', 3), ('b', 1), ('b', 2), ('b', 3)]
```

```
更多精彩推荐
```

```
☞三种方法，用Python轻松提取PDF中的全部图片
☞科学“干饭”，AI 营养师教你

☞GitHub 标星超 26600，TiDB 社区运营的道与术！
☞告别手敲 SQL ？GPT-3 自动帮你写
```

```
点分享
点收藏
点点赞
点在看
```