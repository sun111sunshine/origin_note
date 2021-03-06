# TCP UDP的区别

## TCP/IP五层网络模型

- 物理层：物理层建立在物理间通信的基础之上，作为系统和通信介质的接口，用来实现链路实体间透明的（bit）流传输。只有该层为真实的物理通信，其他层为虚拟通信。
- 数据链路层：在物理层提供比特流（bit）服务的基础上，建立相邻节点之间的数据链路，通过差错控制提供数据帧在信道上无差错的传输，并进行各电路的动作系列，数据单位称为帧。
- 网络层:选择合适的路由，使用数据分组将数据交付到目的主机
- 传输层:负责主机中进程间的通信
- 应用层:直接为用户的应用程序提供服务

## UDP详解

> 用户数据报协议，又称为用户使用报协定，是一个简单的面向数据报的传输层协议正式规范为RFC 768;
> 在TCP/IP的模型中，UDP为网络层以上和应用层以下提供了一个简单的接口。UDP只提供数据的`不可靠传输`,它一旦把应用程序发给网络层的数据发送出去，就不保留数据备份（所以UDP有时候就被认为是不可靠的数据报协议）。UDP在IP数据报的头部仅仅加入了复用和数据校校验（字段）。

![UDP报头](https://lc-api-gold-cdn.xitu.io/30ff395f6ec190cff277.png?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

UDP的头部是由源端口号、目标端口号、报长和校验4个部分组成，其中两个是可选的。各16bit的来源端口和目的端口用来标记发送和接受的应用进程。因为UDP不需要应答，所以来源端口是可选的，如果来源端口不用，那么置为零。在目的端口后面是长度固定的以字节为单位的长度域，用来指定UDP数据报包括数据部分的长度，长度最小值为8byte。首部剩下地16bit是用来对首部和数据部分一起做校验和（Checksum）的，checksum主要是用来检测UDP段在传输中是否发生了错误。还有就是，校验和计算中也需要计算UDP伪头部，伪头部包含IP头部的一些字段。我们刚才介绍了识别一个通信需要5项信息，而UDP头部只有端口号，余下的三项在IP头部，所以引入了伪头部的概念。（IPv6的IP头部没有校验和字段）

### UDP的优点

- 无需建立连接（减少延迟）
- 实现简单：无需维护连接状态
- 头部开销比较小，最少值为8bit.
- 没有阻塞控制;应用可以更好的控制发送时间和发送速率。

### 基于UDP的协议有

- 域名系统（DNS）
- 简单网络管理的协议(SNMP)
- 动态主机配置协议（DHCP）
- 路由信息协议（RIP）
- 自举协议（BOOTP）
- 简单文件传输协议（TFTP）

## TCP详解

> 传输控制协议，是一种面向连接的，可靠的，基于字节流的传输层协议。
> 在因特网协议族中，tcp是位于ip层之上应用层之下的中间层，不同主机的应用层之间需要可靠的，像管道一样的连接，但是IP层不提供这样的流机制，而是提供不可靠的包交换。
> 应用层向TCP层发送用于网间传输的、用8位字节表示的数据流，然后TCP把数据流分区成适当长度的报文段（通常受该计算机连接的网络的数据链路层的最大传输单元（MTU）的限制）。之后TCP把结果包传给IP层，由它来通过网络将包传送给接收端实体的TCP层。TCP为了保证不发生丢包，就给每个包一个序号，同时序号也保证了传送到接收端实体的包的按序接收。然后接收端实体对已成功收到的包发回一个相应的确认（ACK）；如果发送端实体在合理的往返时延（RTT）内未收到确认，那么对应的数据包就被假设为已丢失将会被进行重传。TCP用一个校验和函数来检验数据是否有错误；在发送和接收时都要计算校验和

## TCP和UDP的区别

`主要从连接性、可靠性、有序性、有界性、拥塞控制、传输速率、传输速度、量级头部大小等方便`

- TCP是面向连接的协议，UDP是无连接的协议，tcp在发送数据之前需要进行三次握手，UDP发送数据之前不需要建立连接。
- TCP可靠，UDP不可靠，TCP丢包之后会重传，UDP不会。
- TCP有序，UDP无序，消息在传输过程中可能会产生乱序，后发送的消息可能会先到达，TCP需要对其进行排序，UDP不会。
- TCP无界，UDP有界；TCP通过字节流进行传输，UDP中每一个包都是独立的。
- TCP有流量控制，但是UDP没有
- TCP传输慢，UDP传输快，主要是因为需要建立三次握手
- TCP是重量级的，UDP是轻量级的，TCP要建立连接、保证可靠性和有序性，就会传输更多的信息，如TCP的包头比较大。
- TCP头部比UDP大,TCP头部需要20字节，UDP头部只要8个字节。