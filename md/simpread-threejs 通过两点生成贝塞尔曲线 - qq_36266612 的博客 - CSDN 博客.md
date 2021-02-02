> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://blog.csdn.net/qq_36266612/article/details/88709029 [](http://creativecommons.org/licenses/by-sa/4.0/)版权声明：本文为博主原创文章，遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。 本文链接：[https://blog.csdn.net/qq_36266612/article/details/88709029](https://blog.csdn.net/qq_36266612/article/details/88709029)

三阶贝塞尔曲线

```
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
function addLines(v0, v3) {
    // 计算向量夹角
    let angle = v0.angleTo(v3) * 270 / Math.PI / 10; // 0 ~ Math.PI
    let aLen = angle * 50,
        hLen = angle * angle * 120;
    let p0 = new THREE.Vector3(0, 0, 0);

    // 开始，结束点
    // let v0 = groupDots.children[0].position;
    // let v3 = groupDots.children[1].position;

    // 法线向量
    let rayLine = new THREE.Ray(p0, getVCenter(v0.clone(), v3.clone()));

    // 顶点坐标
    let vtop = rayLine.at(hLen / rayLine.at(1).distanceTo(p0));

    // 控制点坐标
    let v1 = getLenVcetor(v0.clone(), vtop, aLen);
    let v2 = getLenVcetor(v3.clone(), vtop, aLen);

    // 绘制贝塞尔曲线
    let curve = new THREE.CubicBezierCurve3(v0, v1, v2, v3);
    let geo = new THREE.Geometry();
    geo.vertices = curve.getPoints(50);
    let mat = new THREE.LineBasicMaterial({color: 0xff0000});

    return {
        curve: curve,
        lineMesh: new THREE.Line(geo, mat)
    };
}

// 计算v1,v2 的中点
function getVCenter(v1, v2) {
    let v = v1.add(v2);
    return v.divideScalar(2);
}

// 计算V1，V2向量固定长度的点
function getLenVcetor(v1, v2, len) {
    let v1v2Len = v1.distanceTo(v2);
    return v1.lerp(v2, len / v1v2Len);
}


```

直接调用 addlines 方法可通过两点生成贝塞尔曲线