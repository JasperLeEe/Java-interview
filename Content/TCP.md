OSI七层模型

应用层

表示层

会话层

传输层

网络层

数据链路层

物理层

（一）TCP\UDP的区别

TCP和UDP是OSI模型中的传输层协议。TCP提供可靠的通信传输，而UDP则常被用于让广播和细节控制交给应用的通信传输。

大致区别如下：

+ TCP面向连接，UDP面向非连接即发送数据前不需要建立连接
+ TCP提供可靠的服务（数据传输），UDP无法保证
+ TCP面向字节流，UDP面向报文
+ TCP数据传输慢，UDP数据传输快



（二）TCP三次握手

![jellythinkTCP4](/Users/jasperleee/Downloads/jellythinkTCP4.jpg)



1. 第一次握手：建立连接。客户端发送连接请求报文段，将SYN位置为1，Sequence Number为x；然后，客户端进入SYN_SEND状态，等待服务器确认；
2. 第二次握手：服务器收到SYN报文段。服务器收到客户端的SYN报文段，需要对这个报文段进行确认，设置Acknowledgment Number为x+1；同时，自己还要发送SYN请求信息，将SYN位置为1，Sequence Number为y；将报文发送给客户端，此时服务器进入SYN_RECV状态；
3. 第三次握手，客户端收到SYN+ACK报文段。然后将Acknowledgment Number设置为y+1，向服务器发送ACK报文段，这个报文段发送完毕后，客户端和服务端都进入ESTABLISHED状态，完成TCP三次握手