> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.jianshu.com/p/c2c638a1c113

[![](https://upload.jianshu.io/users/upload_avatars/665439/0a3c9099e529.png?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/b7757a81a98f)

2015.11.26 13:07:28 字数 672 阅读 4,431

`bezierCurveTo()`方法
-------------------

绘制三次贝塞尔曲线代码如下。

```
context.bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y);
```

这个方法可谓是绘制波浪线的神器。根据之前的结论，n 阶贝塞尔曲线就有 n-1 个控制点，所以三次贝塞尔曲线有 1 个起始点、1 个终止点、2 个控制点。因此传入的 6 个参数分别为控制点 cp1 (cp1x, cp1y)，控制点 cp2 (cp2x, cp2y)，与终止点 (x, y)。

这个方法也是不用大家去掌握参数具体是怎么填的，只要知道参数的意义就行。和`quadraticCurveTo()`方法一样，`bezierCurveTo()`的三次贝塞尔曲线网上也能找到互动的网页工具。这里提供一个网页：[Canvas Bézier Curve Example](https://link.jianshu.com/?t=http://tinyurl.com/html5bezier)，大家可以动手试一下。

![](http://upload-images.jianshu.io/upload_images/665439-328bc31e6163dac3.jpg)

三次贝塞尔曲线交互工具

绘制 XP 壁纸
--------

这里我们拿 XP 的壁纸开刀，来练习一下我们之前学习过的绘制方法。

```
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <title>XP壁纸</title>
    <style>
        body { background: url("./images/bg3.jpg") repeat; }
        #canvas { border: 1px solid #aaaaaa; display: block; margin: 50px auto; }
    </style>
</head>
<body>
<div id="canvas-warp">
    <canvas id="canvas">
        你的浏览器居然不支持Canvas？！赶快换一个吧！！
    </canvas>
</div>

<script>
    window.onload = function(){
        var canvas = document.getElementById("canvas");
        canvas.width = 800;
        canvas.height = 600;
        var context = canvas.getContext("2d");
        context.fillStyle = "#FFF";
        context.fillRect(0,0,800,600);

        drawPrairie(context);
        drawSky(context);
        for(var i=0; i <5; i++){
            var x0 = 500 * Math.random() + 50;
            var y0 = 200 * Math.random() + 50;
            var c0 = 100 * Math.random() + 50;
            drawCloud(context, x0, y0, c0);

        }

    };

    function drawSky(cxt){
        cxt.save();

        cxt.beginPath();
        cxt.moveTo(0, 420);
        cxt.bezierCurveTo(250, 300, 350, 550, 800, 400);
        cxt.lineTo(800,0);
        cxt.lineTo(0,0);
        cxt.closePath();

        var lineStyle = cxt.createRadialGradient(400, 0, 50, 400, 0, 200);
        lineStyle .addColorStop(0, "#42A9AA");
        lineStyle .addColorStop(1, "#2491AA");

        cxt.fillStyle = lineStyle;

        cxt.fill();

        cxt.restore();
    }

    function drawPrairie(cxt){
        cxt.save();

        cxt.beginPath();
        cxt.moveTo(0, 420);
        cxt.bezierCurveTo(250, 300, 350, 550, 800, 400);
        cxt.lineTo(800,600);
        cxt.lineTo(0,600);
        cxt.closePath();

        var lineStyle = cxt.createLinearGradient(0, 600, 600, 0);
        lineStyle .addColorStop(0, "#00AA58");
        lineStyle .addColorStop(0.3, "#63AA7B");
        lineStyle .addColorStop(1, "#04AA00");

        cxt.fillStyle = lineStyle;
        cxt.fill();

        cxt.restore();
    }



    /*渲染单个云朵
     context:  canvas.getContext("2d")对象
     cx: 云朵X轴位置
     cy: 云朵Y轴位置
     cw: 云朵宽度
     */
    function drawCloud(cxt, cx, cy, cw) {
        //云朵移动范围即画布宽度
        var maxWidth = 800;
        //如果超过边界从头开始绘制
        cx = cx % maxWidth;
        //云朵高度为宽度的60%
        var ch = cw * 0.6;
        //开始绘制云朵

        cxt.beginPath();
        cxt.fillStyle = "white";
        //创建渐变
        var grd = cxt.createLinearGradient(0, 0, 0, cy);
        grd.addColorStop(0, 'rgba(255,255,255,0.8)');
        grd.addColorStop(1, 'rgba(255,255,255,0.5)');
        cxt.fillStyle = grd;

        //在不同位置创建5个圆拼接成云朵现状
        cxt.arc(cx, cy, cw * 0.19, 0, 360, false);
        cxt.arc(cx + cw * 0.08, cy - ch * 0.3, cw * 0.11, 0, 360, false);
        cxt.arc(cx + cw * 0.3, cy - ch * 0.25, cw * 0.25, 0, 360, false);
        cxt.arc(cx + cw * 0.6, cy, cw * 0.21, 0, 360, false);
        cxt.arc(cx + cw * 0.3, cy - ch * 0.1, cw * 0.28, 0, 360, false);
        cxt.closePath();

        cxt.fill();
    }
</script>
</body>
</html>
```

[演示 12-1](https://link.jianshu.com/?t=http://airingursb.github.io/canvas/Canvas/12/12-1.html)

运行结果：

![](http://upload-images.jianshu.io/upload_images/665439-1eb0ff1bcec4fb9b.jpg)

仿 XP 壁纸

是不是很萌？是不是非常的酷！这个案例几乎用到了之前所传授给你们的所有武功——三次贝塞尔曲线，径向渐变，线性渐变，绘制圆弧等等。分开写了三个函数，一个绘制草原、一个绘制蓝天、一个绘制白云…… 大家尝试自己实现一下，当做一次阶段性复习~

### 保存和恢复 Canvas 状态

这里还使用到了两个新方法`save()`和`restore()`。之前说过了 canvas 是基于状态的绘制（说了好多次，感觉自己好啰嗦）。保存（推送）当前状态到堆栈，调用以下函数。

```
context.save();
```

调出最后存储的堆栈恢复画布，使用以下函数。

```
context.restore();
```

* * *

不知道大家壁纸绘制的如何，肯定非常的酷有没有？到此为止路径的知识和填充样式我们已经全部讲完了，大家也画出了很多或优美、或抽象的艺术作品。不管怎么样，这是属于我们的艺术，我们继续前进！😋

* * *

如果您喜欢本书，请使用支付宝扫描下面的二维码捐助作者。

![](http://upload-images.jianshu.io/upload_images/665439-ab0bb5badad2eeea.png)

二维码

谢谢您的支持！也欢迎您订阅本书。

如果书中有疏漏或错误之处，敬请您指出，期待您的 pull request。如果有任何疑问也可以发送 issue。

本人邮箱：[airing@ursb.me](https://link.jianshu.com/?t=mailto:airing@ursb.me)

本人博客：[http://ursb.me](https://link.jianshu.com/?t=http://ursb.me)

本书地址：[http://airingursb.gitbooks.io/canvas](https://link.jianshu.com/?t=http://airingursb.gitbooks.io/canvas)

本书 github：[http://github.com/airingursb/canvas](https://link.jianshu.com/?t=http://github.com/airingursb/canvas)

![](http://upload-images.jianshu.io/upload_images/665439-bd2771eaa0e57189.jpg)

Canvas--Draw on the Web

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[![](https://upload.jianshu.io/users/upload_avatars/665439/0a3c9099e529.png?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/webp)](https://www.jianshu.com/u/b7757a81a98f)

[Airing](https://www.jianshu.com/u/b7757a81a98f "Airing") 我是一只小小小小熊 个人主页：http://me.ursb.me

总资产 153 (约 15.99 元) 共写了 11.0W 字获得 873 个赞共 349 个粉丝

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

*   贝塞尔曲线 Bézier curve(贝塞尔曲线) 是应用于二维图形应用程序的数学曲线。 曲线定义：起始点、终止点、...
    
    [![](https://upload-images.jianshu.io/upload_images/665439-a609067d0a1205e4.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)](https://www.jianshu.com/p/9349002be138)
*   看这篇文章之前，请确保你有基本的前端知识，知道 Canvas 最基本的使用方法, 知道贝塞尔曲线在 CSS3 中的使用。本文...
    
    [![](https://upload-images.jianshu.io/upload_images/221109-3e3b00ea62638105.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)](https://www.jianshu.com/p/1ddaae528bac)
*   一：canvas 简介 1.1 什么是 canvas？ ①：canvas 是 HTML5 提供的一种新标签 ②：HTML5 ...
    
*   转载请标明出处：http://www.jianshu.com/p/c0d7ad796cee 前言： 贝塞尔曲线又称...
    
    [![](https://upload.jianshu.io/users/upload_avatars/1903970/0398d1b7d2a9.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/48/h/48/format/webp)许方镇](https://www.jianshu.com/u/971ab742e7fc)阅读 76
    
    [![](https://upload-images.jianshu.io/upload_images/1903970-1eb3302629d7a0ca.gif?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)](https://www.jianshu.com/p/c0d7ad796cee)
*   书中代码示例效果展示：Core HTML5 Canvas Examples 基础知识 canvas 元素 canva...
    
    [![](https://upload-images.jianshu.io/upload_images/2255197-d75fe959d32eb772.png?imageMogr2/auto-orient/strip|imageView2/1/w/300/h/240/format/webp)](https://www.jianshu.com/p/3de074f739a8)