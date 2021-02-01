> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/yaVhM13JscQtYW9i114lUA)

![](https://mmbiz.qpic.cn/mmbiz_gif/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO7vgTGP53icxyLKZKC6yAKvs5s479MoYDVbObrnSuxoXbzNIFMmaX83g/640?wx_fmt=gif)

![](https://mmbiz.qpic.cn/mmbiz_jpg/VeJKXItpwPWFxD6CCHbrdbwqNGiciamJfp1Ddgc0ficM3prKPPoC54O0pyKh47QLu1OJHVib3JgJ1UpVsIDDZafKxQ/640?wx_fmt=jpeg)

作者 | 李秋键

责编 | 夕颜

出品 | AI 科技大本营（ID:rgznai100）

> 引言：近几年来，谈起发展最火热的几个关键词必然是人工智能、大数据以及物联网的万物互联、边缘计算等等了。而今天，我们就将利用 Python 实现物联网下的数据传输功能。主要的内容包括：本地视频传输到服务器、视频传输到手机实时显示、以及文本传输等方式。

物联网是新一代信息技术的重要组成部分，也是 "信息化" 时代的重要发展阶段。顾名思义，物联网就是物物相连的互联网。这有两层意思：其一，物联网的核心和基础仍然是互联网，是在互联网基础上的延伸和扩展的网络; 其二，其用户端延伸和扩展到了任何物品与物品之间，进行信息交换和通信，也就是物物相息。物联网通过智能感知、识别技术与普适计算等通信感知技术，广泛应用于网络的融合中，也因此被称为继计算机、互联网之后世界信息产业发展的第三次浪潮。而物联网最为核心的功能便是数据传输功能，利用互联网实现数据在任何可以接受数据的设备平台上达到传输效果，其中设备可以包括：本地 PC、服务器、树莓派、手机、手环等等。

其中实现的效果如下可见：

![](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPXWLpENCwyR2GLh1A8ztFHTN5LicSHqOIQNo7KXTlWd7Ubv3xaZvyFv5hLlj4Cz3NCEaAEibSXmzDpw/640?wx_fmt=png)

视频传输到服务器效果图

![](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPXWLpENCwyR2GLh1A8ztFHT5GNzQ7Rt92nw3oRkr8FIfXZtcF9qswKxLWicqCvKpQAaKKcTWruOdiaw/640?wx_fmt=png)

视频传输到手机效果图

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOc2rXicCsC79VyqrfQywa6E74wdAaexVSMb4RYiacoJ9E71XJcjnkdWwA/640?wx_fmt=jpeg)

**实验前的准备**
----------

首先我们使用的 Python 版本是 3.6.5 所用到的模块如下：

**Opencv 模块：**在这里我们用来读取视频流数据，以及图片或者是视频的编码解码和数据视频的显示；

**Numpy 模块：**在这里用来和图片解码结合使用进行数据运算；

**Socket 模块：**Socket 又称 "套接字"，应用程序通常通过 "套接字" 向网络发出请求或者应答网络请求，使主机间或者一台计算机上的进程间可以通讯。

**Flask 框架：**Flask 是一个 Python 编写的 Web 微框架，让我们可以使用 Python 语言快速实现一个网站或 Web 服务。

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO3ToytjEfeNjJlGPcZqK98xO4MJ2zC1xj07PfS6kAic0LAlMKFBLvWPQ/640?wx_fmt=jpeg)

**视频传输**
--------

### **2.1 视频传输服务器**

客户端通过 opencv 读取本地摄像头数据，然后编码成数据流格式，利用 socket 实现向服务端的发送，客户端代码如下：

```
#客户端代码import socketimport threadingimport cv2import numpy as np#接受服务器返回的数据的函数def recvlink(client):    while True:        msg=client.recv(1024)        print('Ubuntu say: '+msg.decode('utf-8'))def main():    #创建ipv4的socket对象，使用TCP协议(SOCK_STREAM)    client=socket.socket(socket.AF_INET,socket.SOCK_STREAM)    #设置服务器ip地址，注意应该是服务器的公网ip    host='ip地址'    #设置要发送到的服务器端口,需要在云服务器管理界面打开对应端口的防火墙    port=端口    #建立TCP协议连接,这时候服务器就会监听到到连接请求，并开始等待接受client发送的数据    client.connect((host,port))    #建立连接后，服务器端会返回连接成功消息    start_msg=client.recv(1024)    print(start_msg.decode('utf-8'))    #开启一个线程用来接受服务器发来的消息    t=threading.Thread(target=recvlink,args=(client,))    t.start()    cap = cv2.VideoCapture(0)    quality = 25  # 图像的质量    encode_param = [int(cv2.IMWRITE_JPEG_QUALITY), quality]    while (cap.isOpened()):        ret, frame = cap.read()        if ret == True:            img_encode = cv2.imencode(".jpg", frame, encode_param)[1]            data_encode = np.array(img_encode)            str_encode = data_encode.tostring()            print(str_encode)            print(len(str_encode))            #输入要发送的信息            sendmsg="kehu"            #向服务器发送消息            client.send(str_encode)            if sendmsg=='quit':                break    #结束时关闭客户端    client.close()if __name__ == '__main__':    main()
```

服务器端通过设置 bufSize 防止出现粘包，利用 socket 接收数据流，然后解码成为图片，并实时显示：

```
#服务器端import socketimport threadingimport numpy as npimport cv2#接受客户端消息函数def recv_msg(clientsocket):    global temp    while True:        # 接受客户端消息,设置一次最多接受1024字节的数据        recv_msg = clientsocket.recv(10240)        # 把接收到的东西解码        msg = np.fromstring(recv_msg, np.uint8)        img_decode = cv2.imdecode(msg, cv2.IMREAD_COLOR)        try:            s=img_decode.shape            img_decode=img_decode            temp=img_decode        except:            img_decode=temp            pass        cv2.imshow('SERVER', img_decode)        if cv2.waitKey(1) & 0xFF == ord('q'):            breakdef main():    #创建服务器端socket对象 ipv4 + TCP协议，和客户端一样    socket_server=socket.socket(socket.AF_INET,socket.SOCK_STREAM)    # 注意注意注意，我们要绑定监听的地址和端口。服务器可能有多块网卡，可以绑定到某一块网卡的IP地址上，也可以用0.0.0.0绑定到所有的网络地址    # 还可以用127.0.0.1绑定到本机地址。127.0.0.1是一个特殊的IP地址，表示本机地址，如果绑定到这个地址，客户端必须同时在本机运行才能连接，也就是说，外部的计算机无法连接进来。    # 这个程序中host使用'0.0.0.0'或服务器内网ip地址都可以，我这里就使用了内网ip地址    #host='0.0.0.0'    host=''    #设置被监听的端口号,小于1024的端口号不能使用，因为他们是Internet标准服务的端口号    port=    #绑定地址    socket_server.bind((host,port))    #设置最大监听数，也就是最多可以同时响应几个客户端请求，一般配合多线程使用    socket_server.listen(5)    #等待客户端连接,一旦有了连接就立刻向下执行，否则等待    #accept()函数会返回一个元组，第一个元素是客户端socket对象，第二个元素是客户端地址（ip地址+端口号）    clientsocket,addr=socket_server.accept()    # 有了客户端连接后之后才能执行以下代码，我们先向客户端发送连接成功消息    clientsocket.send('你现在已经连接上了服务器啦，我们来聊天吧！'.encode('utf-8'))    # 和客户端一样开启一个线程接受客户端的信息    t=threading.Thread(target=recv_msg,args=(clientsocket,))    t.start()'''    # 发送消息    while True:        reply="cer"        clientsocket.send(reply.encode('utf-8'))    clientsocket.close()'''if __name__=='__main__':    main()
```

![](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPXWLpENCwyR2GLh1A8ztFHTCUgLDMUib5z3onhvZTChQqzGqAEsvkHnicf1vyNPya3QJGico1RXlatQg/640?wx_fmt=png)

### **2.2 视频传输到手机**

可以利用 opencv 读取视频或是摄像头，进行编解码后传输。代码如下：

```
from flask import Flask, render_template, Responseimport cv2import timeclass VideoCamera(object):    def __init__(self):        # 通过opencv获取实时视频流        self.video = cv2.VideoCapture(0)    def __del__(self):        self.video.release()    def get_frame(self):        try:            image=cv2.imread("1.jpg")            ceshi = image.shape            global temp            temp=image            # 因为opencv读取的图片并非jpeg格式，因此要用motion JPEG模式需要先将图片转码成jpg格式图片            ret, jpeg = cv2.imencode('.jpg', image)        except:            image = temp            # 因为opencv读取的图片并非jpeg格式，因此要用motion JPEG模式需要先将图片转码成jpg格式图片            ret, jpeg = cv2.imencode('.jpg', image)        return jpeg.tobytes()app = Flask(__name__)@app.route('/')  # 主页def index():    # jinja2模板，具体格式保存在index.html文件中    return render_template('index.html')def gen(camera):    while True:        #time.sleep(0.5)        frame = camera.get_frame()        # 使用generator函数输出视频流， 每次请求输出的content类型是image/jpeg        yield (b'--frame\r\n'                   b'Content-Type: image/jpeg\r\n\r\n' + frame + b'\r\n\r\n')@app.route('/video_feed')  # 这个地址返回视频流响应def video_feed():    return Response(gen(VideoCamera()),                    mimetype='multipart/x-mixed-replace; boundary=frame')
```

需要搭配的 html 代码：

```
<html>  <head>    <title>Video Streaming Demonstration</title>  </head>  <body>    <h1>Video Streaming Demonstration</h1>    <img src="{{ url_for('video_feed') }}">  </body></html>
```

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOPyiaaIVCePZnia426uNibf0PLTPfS7VRaWql6vFWCwb2Cqgjp06UvU39Q/640?wx_fmt=jpeg)

**图片形成视频流传输 flask**
-------------------

由于手机端不方便运行 Python 程序，我们可以利用 flask 搭建视频传输网页，再利用 opencv 保存图片更新图片，以及 flask 更新图片实现视频传输效果。

```
#!/usr/bin/env pythonfrom importlib import import_moduleimport osfrom flask import Flask, render_template, Response# import camera driverif os.environ.get('CAMERA'):    Camera = import_module('camera_' + os.environ['CAMERA']).Cameraelse:    from camera import Camera# Raspberry Pi camera module (requires picamera package)# from camera_pi import Cameraapp = Flask(__name__)@app.route('/')def index():    """Video streaming home page."""    return render_template('index.html')def gen(camera):    """Video streaming generator function."""    while True:        frame = camera.get_frame()        yield (b'--frame\r\n'               b'Content-Type: image/jpeg\r\n\r\n' + frame + b'\r\n')@app.route('/video_feed')def video_feed():    """Video streaming route. Put this in the src attribute of an img tag."""    return Response(gen(Camera()),                    mimetype='multipart/x-mixed-replace; boundary=frame')if __name__ == '__main__':app.run(host='0.0.0.0', threaded=True)
```

![](https://mmbiz.qpic.cn/mmbiz_png/VeJKXItpwPXWLpENCwyR2GLh1A8ztFHT5GNzQ7Rt92nw3oRkr8FIfXZtcF9qswKxLWicqCvKpQAaKKcTWruOdiaw/640?wx_fmt=png)

源码地址：

链接：https://pan.baidu.com/s/1AMftWOZXnT-gNu-fuhMAaQ

提取码：dx7z

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