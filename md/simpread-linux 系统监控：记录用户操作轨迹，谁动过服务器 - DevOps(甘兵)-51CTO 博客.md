> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://blog.51cto.com/ganbing/2053636

**1、前言**

      我们在实际工作当中，都碰到过误操作、误删除、误修改过配置文件等等事件。对于没有堡垒机的公司来说，要在 linux 系统上深究到底谁做过配置文件的修改、做过误删除是很头疼的事情，特别是遇到删库跑路的事件，更头大了。当然你可以通过 history 来查看历史命令记录，如果把 history 记录涂抹掉了，是不是啥也看不到了，如果你想查看在某个时间段到底是谁通过 vim 编辑过某个文件呢？

      那么，有什么办法可以看见这些操作呢，答案是一定有的，具体怎么实现呢，linux script 命令正有如此强大的功能，可以满足我们的需求，script 可以记录终端会话，只要是 linux6.3 以上的系统，都会自带 script 命令，下面我用 centos 7 系统来测试一下。  

**2、配置**

**2.1 验证 script 命令（我这里是有的）**

**2.2 配置 profile 文件，在末尾添加如下内容：**

```
[root@localhost ~]# which script
/usr/bin/script
```

```
[root@localhost ~]# vim /etc/profile

if [ $UID -ge 0 ]; then
        exec /usr/bin/script -t 2>/var/log/script/$USER-$UID-`date +%Y%m%d%H%M`.date -a -f -q /var/log/script/$USER-$UID-`date +%Y%m%d%H%M`.log
fi
```

```
    -f     如果需要在输出到日志文件的同时，也可以查看日志文件的内容，可以使用 -f 参数。PS:可以用于教学,两个命令行接-f可以实时演示

```

```
    -a     输出录制的文件，在现有内容上追加新的内容

```

```
    -q     可以使script命令以静默模式运行

```

如下图所示：  

![](https://s1.51cto.com/images/20171222/1513928185346518.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

说明：

用户登录执行的操作都会记录到 / var/log/script/*.log  里（保存日志的目录根据你自己定义），我们可以通过 more、vi 等命令查看目录里的日志。  

注意：

*   我这里把用户 ID 大于 0 的都记录下来了，你可以重新登录用户，随便操作一些命令，查看生成的文件。
    
*   root 用户的 ID 为 0，新建普通用户的 UID 是从 500 开始的 (通过 cat /etc/password 可以查看用户的 UID)，如果你不想记录 root 用户的操作，你把 if 里面的值改成 500：  if [ $UID - ge 500 ];
    

**2.3 创建目录、赋予权限**

    你是不是以为写了这条 if 语句在 / etc/profile 文件中就完事了，目录都没创建呢：  

**2.4 使环境生效**

3、验证

    好了，你可以退出 linux 终端，在重新登录一下，然后随便敲几个命令来看看。  

从上图可以看到，在 / var/log/script 目录中，已经产生了 log 和 data 为后缀的文件，并且还看到了 root 用户和 UID 号 0。

    .log：记录了操作

    .data：可以回放操作

我们用 scriptreplay 来回放一下操作，看下效果如何：

**注意：**先指定 “时间文件 .data”，然后是 “命令文件 .log”，不要颠倒了。  

以上就完也了记录用户的所有操作，并且还可以随时查看，相当于有回放功能，像录像一样，以后定位是谁的问题就好找原因了。

**> 喜欢我的文章，请点击最上方右角处的《关注》支持一下！**