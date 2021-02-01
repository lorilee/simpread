> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/KYJL888/article/details/100675671)

1、[高斯投影](https://www.baidu.com/s?wd=%E9%AB%98%E6%96%AF%E6%8A%95%E5%BD%B1&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)
=========================================================================================================================

高斯投影又称横轴椭圆柱[等角投影](https://www.baidu.com/s?wd=%E7%AD%89%E8%A7%92%E6%8A%95%E5%BD%B1&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)，是德国测量学家高斯于 1825～1830 年首先提出的，1912 年，德国测量学家克吕格推导出实用的坐标投影公式，又称为：[高斯 - 克吕格投影](https://www.baidu.com/s?wd=%E9%AB%98%E6%96%AF-%E5%85%8B%E5%90%95%E6%A0%BC%E6%8A%95%E5%BD%B1&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)；  
它属于正形投影，特点是[中央子午线](https://www.baidu.com/s?wd=%E4%B8%AD%E5%A4%AE%E5%AD%90%E5%8D%88%E7%BA%BF&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)不变形；但是除了[中央子午线](https://www.baidu.com/s?wd=%E4%B8%AD%E5%A4%AE%E5%AD%90%E5%8D%88%E7%BA%BF&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)，其他子午线的测段连线投影后都是要变形的，而且离[中央子午线](https://www.baidu.com/s?wd=%E4%B8%AD%E5%A4%AE%E5%AD%90%E5%8D%88%E7%BA%BF&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)经度越远，变形越大。

3 度带或 6 度带
==========

为了控制投影后的长度变形，通常我们使用采用分带投影的方法。常用 3 度带或 6 度带分带，城市或工程控制网坐标可采用 1.5 度带或任意带分带。  
**2、6° 带划分：**  
投影带宽度：相邻子午面间的经度差来划分  
1）自中央子午面起，自西向东每 6° 为一带，全球共分 60 带。  
2）带号 n 与其中央子午线的经度 (L0）有下列关系：  
L0 = n×6°-3 °  
3）已知经度 L 求带号 n：  
n = int(L/6) + 1  
4）我国境内有 11 个 6° 带（13 带到 23 带）  
**3、3° 带：**  
1）自东经 1°30′开始每隔经差 3° 划分，全球共分 120 带。  
2）带号 n 与其中央子午线的经度 (L0）有下列关系：  
L0 =n×3°  
3）已知经度 L 求带号 n：  
n = int((L-1.5)/3) + 1  
4）我国境内 21 个 3° 带（25 带到 45 带）  
4、举个例子来看：  
1）已知某地经度是 117 度 32 分 36.456431 秒（先换算为 117.54346012°）  
其 6 度带划分之对应带号为  
n=int(117.54346012°/6) + 1=20 带  
其 3 度带划分之对应带号为  
n = int(117.54346012°-1.5°)/3) + 1= 39 带  
2）中国地区，若说是某地区是 20 带，则其属于 6 度带划分的，其中央子午线经度为  
L0=20*6°-3°=117°  
若说是某地区是 39 带，则其属于 3 度带划分的，其中央子午线经度为  
L0=39*3°=117°

`3``度带高斯坐标转``6``度带高斯坐标`
-----------------------

1. 大地坐标 (高斯坐标) 转换经纬度

`北京``54``椭球下：``41°59′03.658448896″ 83°43′03.308870994″``，`  
`[西安](https://www.baidu.com/s?wd=%E8%A5%BF%E5%AE%8980&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)`[`80`](https://www.baidu.com/s?wd=%E8%A5%BF%E5%AE%8980&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)`椭球下：``41°59′06.245742324″ 83°43′03.577911478″`  
`中国``2000``椭球下：``41°59′06.315922696″ 83°43′03.585510656″`  
`总结：无论哪个椭球，其实偏差仅是秒级小数点后一位。`

`3``度带大地坐标``39``483503.76 4222837.592  (39反映3度带的号` L0=39*3°=117`)`

`转换为经纬度``,``经度``116:48:42.595529`

![](https://img-blog.csdnimg.cn/20190909210913785.png)  
`3``度带大地坐标` `483503.76 4222837.592` `(6``位，``7``位) ---HAD (6位数，7为数)`

`6``度带``大地坐标``(``高斯坐标``)`

`1.jpg 119597830` `20433926.27707332 4438953.56324711 35.622500 337.65405702 2.95040068 -1.42715489  （`20 属于 6 度带划分的，其中央子午线经度为 L0=20*6°-3°=117°`）`

`2.jpg 119597850` `20433926.27622186 4438953.56325571 35.622500` `337.65461753 2.94034159 -1.44442386`

`经纬度与大地``(``高斯``)``坐标轨迹相同`

WGS84 经纬度坐标 6 度分带高斯投影正算
=======================

WGS84 坐标系下的经纬度坐标经过投影可以转换为平面坐标，本文采用高斯投影正算，6 度带投影。

中央子午线的概念：  
由于控制投影变形的大小，所以引入中央子午线的概念控制投影变形。

分带：投影带分为 3 度带与 6 度带

带号：  
3 度分带：以本初子午线东偏 1.5 度开始，3 度为 1 带。  
6 度分带：本初子午线起，东偏 6 度为 1 带。

带号和中央子午线的关系：  
若已知某点的经度为λ, 则该点的 6º 带的带号 N 由下式计算：  
N=int（λ/6）+1 (int 为取整的意思)  
中央子午线经度 Ln=6°N-3°  
若已知某点的经度为 L, 则该点所在 3º 带的带号按下式计算：  
n=L/3 （四舍五入）  
中央子午线经度 Ln=3°n

补充参考

[_PROJ4_ 初探 (转并整理格式)-4](https://blog.csdn.net/KYJL888/article/details/83896570) [ - CSDN 博客](https://blog.csdn.net/KYJL888/article/details/83896570)

作者： [KYJL888](http://blog.csdn.net/KYJL888) 日期：2019-04-12 [https://blog.csdn.net/KYJL888/article/details/83896570](https://blog.csdn.net/KYJL888/article/details/83896570) 499 次阅读

[matlab 经纬度坐标与高斯坐标的转换 (_proj4_)-5](https://blog.csdn.net/KYJL888/article/details/84874351) [ - CSDN 博客](https://blog.csdn.net/KYJL888/article/details/84874351)

作者： [KYJL888](http://blog.csdn.net/KYJL888) 日期：2019-04-12 [https://blog.csdn.net/KYJL888/article/details/84874351](https://blog.csdn.net/KYJL888/article/details/84874351) 880 次阅读

[_Proj_._4_ 坐标系统创建参数 - 2](https://blog.csdn.net/KYJL888/article/details/89251293) [ - CSDN 博客](https://blog.csdn.net/KYJL888/article/details/89251293)

作者： [KYJL888](http://blog.csdn.net/KYJL888) 日期：2019-04-12 [https://blog.csdn.net/KYJL888/article/details/89251293](https://blog.csdn.net/KYJL888/article/details/89251293) 332 次阅读

[_Proj_._4_ 库的编译及使用 - 1](https://blog.csdn.net/KYJL888/article/details/89250826) [ - CSDN 博客](https://blog.csdn.net/KYJL888/article/details/89250826)

作者： [KYJL888](http://blog.csdn.net/KYJL888) 日期：2019-04-12 [https://blog.csdn.net/KYJL888/article/details/89250826](https://blog.csdn.net/KYJL888/article/details/89250826)