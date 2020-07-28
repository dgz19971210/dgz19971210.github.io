---
title: TCP的三次握手和四次挥手
date: 2019-03-07 20:50:55
tags: 
    - 网络
categories: 网络
encrypt:
description:
---

{% cq %} 前言 {% endcq %}

{% note info %}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;简述TCP的三次握手和四次挥手。一些个人的简单理解
{% endnote %}

<!-- more -->



# TCP 的六种标志位

## SYN 同步标志

同步序列编号（**Synchronize Sequence Numbers**）是TCP/IP建立连接时使用的握手信号。在客户机和服务器之间建立正常的TCP网络连接时，客户机首先发出一个SYN消息，服务器使用SYN+ACK应答表示接收到了这个消息，最后客户机再以 ACK 消息响应。这样在客户机和服务器之间才能建立起可靠的TCP连接，数据才可以在客户机和服务器之间传递。

## ACK：确认标志

确认编号（**Acknowledgement Number**）在数据通信中，接收站发给发送站的一种传输类[控制字符](https://baike.baidu.com/item/%E6%8E%A7%E5%88%B6%E5%AD%97%E7%AC%A6/6913704)。表示发来的数据已确认接收无误。

## PSH 推标志

该标志置位时，接收端不将该[数据](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE)进行队列处理，而是尽可能快将数据转由应用处理。在处理 [telnet](https://baike.baidu.com/item/telnet) 或 rlogin 等交互模式的连接时，该标志总是置位的。

## FIN 结束标志

带有该标志置位的数据包用来结束一个TCP回话，但对应端口仍处于开放状态，准备接收后续数据。

## URG 紧急标志

指镇优先（**Urgent Pointer**）

## RST 复位标志

用于复位相应的TCP连接。





# TCP 三次握手

<!-- ![](https://ws1.sinaimg.cn/large/006iOFs0gy1g0ukssf46mj30ed08gwei.jpg) -->



> SYN ( Synchronize Sequence Numbers )：同步序列号，用于建立连接

> ACK ( Acknowledgement )：确认序列号有效 

> seq ( Sequence number )： 顺序号码 

> ack ( Acknowledge number )：确认号码



* 第一次握手：Client将标志位SYN置为1，随机产生一个值seq=J，并将该数据包发送给Server，Client进入SYN_SENT状态，等待Server确认。
* 第二次握手：Server收到数据包后由标志位SYN=1知道Client请求建立连接，Server将标志位SYN和ACK都置为1，ack=J+1，随机产生一个值seq=K，并将该数据包发送给Client以确认连接请求，Server进入SYN_RCVD状态。
* 第三次握手：Client收到确认后，检查ack是否为J+1，ACK是否为1，如果正确则将标志位ACK置为1，ack=K+1，并将该数据包发送给Server，Server检查ack是否为K+1，ACK是否为1，如果正确则连接建立成功，Client和Server进入ESTABLISHED状态，完成三次握手，随后Client与Server之间可以开始传输数据了。

## 为什么要三次握手

在《计算机网络》一书中其中有提到，三次握手的目的是“为了防止已经失效的连接请求报文段突然又传到服务端，因而产生错误”，这种情况是：一端(client)A发出去的第一个连接请求报文并没有丢失，而是因为某些未知的原因在某个网络节点上发生滞留，导致延迟到连接释放以后的某个时间才到达另一端(server)B。本来这是一个早已失效的报文段，但是B收到此失效的报文之后，会误认为是A再次发出的一个新的连接请求，于是B端就向A又发出确认报文，表示同意建立连接。如果不采用“三次握手”，那么只要B端发出确认报文就会认为新的连接已经建立了，但是A端并没有发出建立连接的请求，因此不会去向B端发送数据，B端没有收到数据就会一直等待，这样B端就会白白浪费掉很多资源。如果采用“三次握手”的话就不会出现这种情况，B端收到一个过时失效的报文段之后，向A端发出确认，此时A并没有要求建立连接，所以就不会向B端发送确认，这个时候B端也能够知道连接没有建立。

问题的本质是，信道是不可靠的，但是我们要建立可靠的连接发送可靠的数据，也就是数据传输是需要可靠的。在这个时候三次握手是一个理论上的最小值，并不是说是tcp协议要求的，而是为了满足在不可靠的信道上传输可靠的数据所要求的。

我们再来考虑，如果不是三次握手会出现什么情况呢：

假设有A和B两端要进行通信，

1. 第一次：首先A发送一个(SYN)到B，意思是A要和B建立连接进行通信；

   > 如果是只有一次握手的话，这样肯定是不行的，A压根都不知道B是不是收到了这个请求。TCP协议变的与面向无连接的UDP 协议无异

   

2. 第二次：B收到A要建立连接的请求之后，发送一个确认(SYN+ACK)给A，意思是收到A的消息了，B这里也是通的，表示可以建立连接；

   > 如果只有两次通信的话，这时候B不确定A是否收到了确认消息，有可能这个确认消息由于某些原因丢了。也可能 B 收到的是 A 发送的失效了的报文（A发送后，由于某种原因没收到回应，报文被作废），B 建立了连接，A 却一直没有发送数据，占用着 B 的连接资源

   

3. 第三次：A如果收到了B的确认消息之后，再发出一个确认(ACK)消息，意思是告诉B，这边是通的，然后A和B就可以建立连接相互通信了；

   > 这个时候经过了三次握手，A和B双方确认了两边都是通的，可以相互通信了，已经可以建立一个可靠的连接，并且可以相互发送数据。

   

4. 第四次：这个时候已经不需要B再发送一个确认消息了，两边已经通过前三次建立了一个可靠的连接，如果再发送第四次确认消息的话，就浪费资源了。

   > 如果第二个报文段B发出的(SYN+ACK)分别发送的话，也是可以理解为四次，但是被优化了，一起发送了。



# TCP 四次挥手

所谓四次挥手（Four-Way Wavehand）即终止TCP连接，就是指断开一个TCP连接时，需要客户端和服务端总共发送4个包以确认连接的断开。在socket编程中，这一过程由客户端或服务端任一方执行close来触发，整个流程如下图所示：

<!-- ![](https://ws1.sinaimg.cn/large/006iOFs0gy1g0ukuxutooj30e308zjrf.jpg) -->



 由于TCP连接时全双工的，因此，每个方向都必须要单独进行关闭，这一原则是当一方完成数据发送任务后，发送一个FIN来终止这一方向的连接，收到一个FIN只是意味着这一方向上没有数据流动了，即不会再收到数据了，但是在这个TCP连接上仍然能够发送数据，直到这一方向也发送了FIN。首先进行关闭的一方将执行主动关闭，而另一方则执行被动关闭，上图描述的即是如此。

* 第一次挥手：Client发送一个FIN，用来关闭Client到Server的数据传送，Client进入FIN_WAIT_1状态。
* 第二次挥手：Server收到FIN后，发送一个ACK给Client，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号），Server进入CLOSE_WAIT状态。
* 第三次挥手：Server发送一个FIN，用来关闭Server到Client的数据传送，Server进入LAST_ACK状态。
* 第四次挥手：Client收到FIN后，Client进入TIME_WAIT状态，接着发送一个ACK给Server，确认序号为收到序号+1，Server进入CLOSED状态，完成四次挥手。



被动关闭具体流程：

<!-- ![](https://ws1.sinaimg.cn/large/006iOFs0gy1g0unpy9lz2j30dz05b0sp.jpg) -->







# 为什么要四次挥手

本质的原因是tcp是全双工的，要实现可靠的连接关闭，A发出结束报文FIN，收到B确认后A知道自己没有数据需要发送了，B知道A不再发送数据了，自己也不会接收数据了，但是此时A还是可以接收数据，B也可以发送数据；当B发出FIN报文的时候此时两边才会真正的断开连接，读写分开。



因为TCP有个半关闭状态，假设A.B要释放连接，那么A发送一个释放连接报文给B，B收到后发送确认，这个时候A不发数据，但是B如果发数据A还是要接受，这叫半关闭。然后B还要发给A连接释放报文，然后A发确认，所以是4次。

在tcp连接握手时为何ACK是和SYN一起发送，这里ACK却没有和FIN一起发送呢。原因是因为tcp是**全双工模式**，**接收到FIN时意味将没有数据再发来，但是还是可以继续发送数据。**



**四次挥手牵扯到的状态装换：**

**FIN_WAIT_1 ** 表示在等待另一方的FIN报文，和FIN_WAIT_2的区别是，FIN_WAIT_1表示socket现在要主动关闭连接，在发送完FIN报文后socket进入FIN_WAIT_1状态，当收到另一方发送FIN的ACK之后立即进入FIN_WAIT_2状态；

**FIN_WAIT_2 ** 同上，此时需要做的事情是可能还会接收数据，然后等待另一方的FIN；

**TIME_WAIT ** 存在主动关闭的一方，表示收到了对方的FIN报文，并发送出了ACK报文，就等2MSL(Max Segment Lifetime))后即可回到CLOSED可用状态了，需要等一段时间时原因是网络是不可靠的，不能保证这个ACK发送成功了，如果失败了，对端会超时重传FIN；

**CLOSING ** 表示在发送FIN之后，没有收到对方的ACK，而是收到了对方的FIN，这中情况很少见，只有在两端几乎同时关闭同一个socket的时候才会出现CLOSING状态；

**CLOSE_WAIT ** 表示收到对方的FIN之后，回给对方ACK，此时处于CLOSE_WAIT状态，等待关闭，要看自己是否还有数据要发送；

**LAST_ACK ** 表示收到对方的FIN之后，回给对方ACK，然后自己也要关闭发送FIN，等待另一方的ACK时候的状态；

**CLOSED ** 这个状态表示连接已经断开。





<!-- ![](https://ws1.sinaimg.cn/large/006iOFs0gy1g0uowyfpy6j30k00mbq3x.jpg) -->



# 参考：

* [CSDN](https://blog.csdn.net/qq_34386891/article/details/80515912)
* [简书](https://www.jianshu.com/p/e7f45779008a)
* [CSDN](https://blog.csdn.net/sdgihshdv/article/details/79503274)
* [CSDN](https://blog.csdn.net/zzhongcy/article/details/38851271)


