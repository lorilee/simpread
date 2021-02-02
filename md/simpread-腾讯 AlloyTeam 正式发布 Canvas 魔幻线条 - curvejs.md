> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://juejin.im/post/58f9752fb123db411954825c ![](https://lc-gold-cdn.xitu.io/e972ea898709cd0a4de0.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

写在前面
----

[curvejs](https://github.com/AlloyTeam/curvejs) 中文读 ["克 js"]，是腾讯 AlloyTeam 打造的一款魔幻线条框架，让线条成为一名优秀的舞者，让线条们成为优秀的舞团，HTML5 Canvas 就是舞台。

官网：[alloyteam.github.io/curvejs/](https://alloyteam.github.io/curvejs/)

你还记得 window 经典的屏幕保护程序《变幻线》吗？

![](https://user-gold-cdn.xitu.io/2017/4/21/df6bbd694c83c9660d0c3d1bf56dba95?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

其原理就是使用 Perlin-Noise + Particle System + Bézier curve + Color Transition 制作而成。

使用 curvejs 实现类似变幻线功能只需要不到 10 行代码：

```
const  { Stage, Curve, motion } = curvejs

let stage = new Stage(document.getElementById('myCanvas'))

stage.add(new Curve({
    color: '#00FF00',
    data: {value: 0, step: 0.008, width: 600, height: 400},
    motion: motion.noise
}))复制代码
```

[【体验地址】](https://alloyteam.github.io/curvejs/pg/rd.html?type=noise)

当然，curvejs 的能力不仅仅是变换线，这完全取决于你的想象力。比如：

*   [Points-To](https://alloyteam.github.io/curvejs/pg/rd.html?type=points-to)
*   [Rotate](https://alloyteam.github.io/curvejs/pg/rd.html?type=rotate)
*   [Word](https://alloyteam.github.io/curvejs/pg/rd.html?type=word)
*   [Perlin-Noise](https://alloyteam.github.io/curvejs/pg/rd.html?type=noise)
*   [Simple](https://alloyteam.github.io/curvejs/pg/rd.html?type=simple)
*   [Simple-ES5](https://alloyteam.github.io/curvejs/pg/rd.html?type=simple-es5)
*   [Curves](https://alloyteam.github.io/curvejs/pg/rd.html?type=curves)
*   [Line](https://alloyteam.github.io/curvejs/pg/rd.html?type=line)
*   [Close](https://alloyteam.github.io/curvejs/pg/rd.html?type=close)

使用指南
----

```
$ npm install curvejs复制代码
```

```
import curvejs from 'curvejs'复制代码
```

也可以直接插入 script 到你的 HTML 页面:

```
<script src="https://unpkg.com/curvejs@0.2.0/dist/curve.min.js"></script>复制代码
```

开始跳舞:

```
var Stage = curvejs.Stage,
    Curve = curvejs.Curve,
    canvas = document.getElementById('myCanvas'),
    stage = new Stage(canvas),
    rd = function() {
     return -2 + Math.random() * 2
    }

var curve = new Curve({
  color: '#00FF00',
  points: [277, 327, 230, 314, 236, 326, 257, 326],
  data: [rd(), rd(), rd(), rd(), rd(), rd(), rd(), rd()],
  motion: function motion(points, data) {
      points.forEach(function (item, index) {
          points[index] += data[index]
      })
  }
})

stage.add(curve)

function tick(){
  stage.update()
  requestAnimationFrame(tick)
}

tick()复制代码
```

上面的 points 代表了三次贝塞尔曲线的 4 个点。motion 代表运动方式，motion 可以拿去到 points 和 data。motion 里函数的 this 指向 Curve 是实例 curve。

使用内置 motion
-----------

```
var curve = new Curve({
  points: [277, 327, 230, 314, 236, 326, 257, 326],
  data: {angle: 0, r:5 ,step:Math.PI / 50 }
  motion: curvejs.motion.dance
})复制代码
```

基本原理
----

![](https://user-gold-cdn.xitu.io/2017/4/21/332c5ce4b8983fef0efcc439072ba9c2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

*   每次创建 Curve 可以传入八个数字，其实就代表上面的 4 个点的坐标
*   motion 里可以拿到 points 进行自定义变幻
*   幻影不需要开发者考虑，curvejs 会自动生成幻影

这里需要特别强调，curvejs 的幻影不是利用 canvas 的黑色底，然后 fillRect 填充半透而产生，而是 Particle System。所以 curvejs 制作出的效果不用一定是黑色背景，而且 canvas 也可以是透明，这就大大增加了适用场景。

提交你的 motion
-----------

在 [motion 目录](https://github.com/AlloyTeam/curvejs/tree/master/src/motion), 有许多内置的 motion 提供给开发者使用，但是你也可以提交你的 motion 到这个项目，我会第一时间 review 并合入主干。

基本 motion 格式规则:

```
/**
 * motion description.
 *
 * @param {points}
 * @param {data}
 *      data rule example:
 *      [1, 0.2, -3, 0.7, 0.5, 0.3, -1, 1]
 */
export default function (points, data) {
    //你的motion逻辑
}复制代码
```

curvejs 相关
----------

*   官网：[alloyteam.github.io/curvejs/](https://alloyteam.github.io/curvejs/)
*   Github: [github.com/AlloyTeam/c…](https://github.com/AlloyTeam/curvejs)
*   更加方便的交流关于 curvejs 的一切可以加入 QQ 的 curvejs 交流群 (179181560)