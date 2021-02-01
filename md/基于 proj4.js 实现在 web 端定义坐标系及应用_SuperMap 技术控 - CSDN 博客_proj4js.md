> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/supermapsupport/article/details/88967390)

一、背景
----

        proj4 是一个 JavaScript 库，它专注于地图投影的表达以及转换。采用一种非常简单明了的投影表达－－PROJ4，比其它的投影定义简单，明显。很容易就能看到各种地理坐标系和地图投影的参数，同时它有强大的坐标转换功能。  
**目前 proj4 支持的坐标参考系有**：  
![](https://img-blog.csdnimg.cn/20190402100127297.png)

二、 下载 proj4 的包
--------------

[http://trac.osgeo.org/proj4js/wiki/Download](http://trac.osgeo.org/proj4js/wiki/Download)

三、 引用 proj4 的包
--------------

**三种方式：**  
1、npm install proj4  
2、从 proj4 的下载地址下载对应版本，然后从下载的包中的 dist / 文件夹中手动获取 proj4.js 文件。  
3、如果不想下载，可以引用在线的 proj4js 包，托管在 cdn 上：[https://cdnjs.com/libraries/proj4js。](https://cdnjs.com/libraries/proj4js%E3%80%82)

四、proj4 参数详解
------------

        有关各种投影的参数定义，可参考 [https://epsg.io/](https://epsg.io/) 或者 [http://spatialreference.org](http://spatialreference.org)；如果已有定义，可搜索并查看其投影参数。如果没有定义，需要用户自定义投影，可通过 defs 定义 EPSGCODE 来创建投影，defs 内容为参考投影参数：分别包括名称、投影、转换到 WGS84 坐标系（三参数、七参数）、椭球长半轴、扁率、原点纬线、中央经线、两条标准纬线、东偏移量、北偏移量和单位等。  
比如从 https://epsg.io 网站中获得 EPSG:3395 的 WKT 描述：  
![](https://img-blog.csdnimg.cn/20190402101726129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVybWFwc3VwcG9ydA==,size_16,color_FFFFFF,t_70)  
或者从 http://spatialreference.org/ref/epsg/3395/proj4 / 网站可以获得 EPSG:3395 的投影参数：  
+proj=merc +lon_0=0 +k=1 +x_0=0 +y_0=0 +ellps=WGS84 +datum=WGS84 +units=m +no_defs

**proj4 常用定义参数：**  
+proj 投影坐标系统名称  
+a 椭球体长半轴长度  
+alpha 用于斜墨卡托和其它几个可能的投影  
+axis 轴方向  
+b 椭球体短半轴长度  
+datum 基准面名  
+ellps 椭球体名  
+k 比例因子 (old name)  
+k_0 比例因子 (new name)  
+lat_0 维度起点  
+lat_1 标准平行纬线第一条  
+lat_2 标准平行纬线第二条  
+lat_ts 有效纬度范围  
+lon_0 中央经线  
+lonc 经度用于斜墨卡托和其它几个可能的投影  
+pm 备用本初子午线  
+proj 投影名  
+south 表示南半球 UTM 区域  
+to_meter 乘数，转换地图单位为 1.0m  
+towgs84 3 或 7 参数基准面转换  
+units meters(米), US survey feet(美国测量英尺), 等.  
+vto_meter 垂直变换为米.  
+vunits 垂直单位.  
+x_0 东伪偏移  
+y_0 北伪偏移  
+zone UTM 区域

五、 proj4 在 web 端的应用，以 iClient for Openlayer 为例
----------------------------------------------

        OpenLayers 默认只支持 EPSG:4326 和 EPSG:3857。但是在实际的 SuperMap iClient Openlayer 开发中，必然会遇到很多默认不支持的坐标系，对于默认不支持的坐标系 SuperMap iClient Openlayer 集成了 proj4.js 库，可以直接通过 proj4.defs() 来自定义坐标系。

**开发代码：**  
1、 加载 proj4 库文件

```
<script type="text/javascript" include='proj4' src="../../dist/openlayers/include-openlayers.js"></script>
```

2、对接自定义坐标系地图

```
var map, url ="http://localhost:8090/iserver/services/map-ugcv5-3395map/rest/maps/3395map";
	
	//定义3395坐标系
	 proj4.defs("EPSG:3395","+proj=merc +lon_0=0 +k=1 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs");
	  
	// 计算resolutions
	  getResolutions = function (zoom, scale, targetMinZoom, targetMaxZoom) {
            var res = scaleToResolution(scale);
            var minRes = res * Math.pow(2, zoom - targetMinZoom);
            var resolutions = [];
            for (var index = 0; index < targetMaxZoom - targetMinZoom + 1; index++) {
                resolutions.push(minRes / Math.pow(2, index));
            }
            return resolutions;

        }
        scaleToResolution = function (scale) {
            var inchPerMeter = 1 / 0.0254;
            return 1 / (scale * 96 * inchPerMeter);
        };
        //第0级比例尺是0.000000015625
    var resolutions = getResolutions(0,0.000000015625,0,6);
    var projection =ol.proj.get('EPSG:3395');
        //新的投影必设地图范围
        projection.setExtent([8374565.292377971,1866182.1613285837,1.4947981223720774E7,7070412.849097984]);  
    //对接3395地图 
    map = new ol.Map({
        target: 'map',
        controls: ol.control.defaults({attributionOptions: {collapsed: false}})
            .extend([new ol.supermap.control.Logo()]),
        view: new ol.View({
            center: [11661273.258049373,4468297.505213284],
            zoom: 0,
            projection: projection,
            resolutions: resolutions
        })
    });
    var layer = new ol.layer.Tile({
       source: new ol.source.TileSuperMapRest({
            url: url,
            //手动构建自定义目标的TileGrid
			tileGrid: new ol.tilegrid.TileGrid({
						extent: [8374565.292377971,1866182.1613285837,1.4947981223720774E7,7070412.849097984],
						resolutions: resolutions
						}),
            wrapX: true,
			
        }),
        projection:projection
    });
    map.addLayer(layer);
```

3、坐标系转换

```
//将经纬度4326的坐标系转换成3395
var pointCoord=ol.proj.transform([100.33,32.280000018770551],'EPSG:4326','EPSG:3395');
 console.log(pointCoord);//转换后的结果[11168684.511289138, 3777303.8282244676]

//用转换后的坐标构造矢量要素
var pointFeature = {
  "type": "Feature",
  "geometry": {
    "type": "Point",
    "coordinates": [pointCoord[0],pointCoord[1]]
  },
  "properties": {
    "name": "Dinagat Islands"
  }
}
//创建矢量图层，并将要素添加到矢量图层上
 var vectorSource = new ol.source.Vector({
                features: (new ol.format.GeoJSON()).readFeatures(pointFeature),
                wrapX: false
            });
  var resultLayer = new ol.layer.Vector({
                source: vectorSource
            });
 map.addLayer(resultLayer);
```

六、转换的结果及代码
----------

地图信息：![](https://img-blog.csdnimg.cn/2019040210460252.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVybWFwc3VwcG9ydA==,size_16,color_FFFFFF,t_70)  
web 端对接的地图以及转换后的点位置：  
![](https://img-blog.csdnimg.cn/20190402113334792.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVybWFwc3VwcG9ydA==,size_16,color_FFFFFF,t_70)  
范例代码链接：  
[https://download.csdn.net/download/supermapsupport/11081781](https://download.csdn.net/download/supermapsupport/11081781)