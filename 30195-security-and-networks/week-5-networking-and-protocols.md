---
description: 加密的部分基本结束，开始网络与协议部分
---

# Week 5 - Networking and Protocols

## The Internet and Sockets（科普，不重要）

这部分很笼统地讲述了一下网络，就结束了，并没有从模型入手。

### 网络发展史与常用工具

* 1969年，网络起源于DARPA，希望把各个大学的计算机连起来
*   如果采用租赁线路，而不是专线，就会存在多个用户在使用帧碰撞的问题，于是采用了packet of data来避免碰撞，复用线路。

    <figure><img src="../.gitbook/assets/image (21).png" alt="" width="375"><figcaption></figcaption></figure>

    <figure><img src="../.gitbook/assets/image (22).png" alt="" width="563"><figcaption></figcaption></figure>
* Traceroute命令可以看到路由路径。traceroute domainName/ip
* 1974年，使用量增加，大家发现packets经常丢失，所以在ip层上开发了TCP协议，用来重发丢失帧。TCP/IP成为实际标准网络栈。
* DNS服务将domain name转化为IP。
* Ports端口，允许ip间进行多重连接，定义连接规格的东西叫作Socket，规格为：(destination IP, destination port, source IP, source port)。WWW:80, ssh:22, dns:53
* Netcat nc是网络界的瑞士军刀，可以利用socket规格来简历聊天连接。nc -l port为在port上开启侦听，nc ip port为对ip:port发送信息。
* Nmap用来扫描端口。nmap 127.0.0.1访问最常用的1000个端口是否开放，nmap -A 127.0.0.1用来发送给这些端口信息去得知这些服务是什么。nmap -p- 127.0.0.1扫描所有端口。
*   实际网络模型：

    <figure><img src="../.gitbook/assets/image (23).png" alt="" width="375"><figcaption></figcaption></figure>
* DHCP用来给新机器动态分配IP，不长期存储
* ARP用来得知MAC和IP的对应关系，前提是使用了NAT服务。
* Wireshark可以进行抓包分析网络
*   模型间传递信息：

    <figure><img src="../.gitbook/assets/image (24).png" alt="" width="375"><figcaption></figcaption></figure>
* Java程序在应用层，与传输层间使用socket通信
* Internet在设计的时候就没考虑安全问题，所有信息都可以被攻击者嗅探，我们要自行保护安全。

## Cryptographic Protocols

### 符号约定

发送信息, 攻击者E假装为A

### &#x20;![](<../.gitbook/assets/image (25).png>) ![](<../.gitbook/assets/image (26).png>)

### 简单协议例子

<figure><img src="../.gitbook/assets/image (27).png" alt="" width="563"><figcaption></figcaption></figure>

攻击这可以拦截并重放这个被加密的信息。这样，在Alice并没有想给Bob自我介绍的时候，Elvis可以假装Alice给Bob自我介绍。

### Nonce

Nonce是只用一次的随机数

<figure><img src="../.gitbook/assets/image (28).png" alt="" width="563"><figcaption></figcaption></figure>

Nonce应该与明文被一同加密，不然可能会被替换重放：

![](<../.gitbook/assets/image (29).png>)<img src="../.gitbook/assets/image (30).png" alt="" data-size="original">

<figure><img src="../.gitbook/assets/image (31).png" alt="" width="563"><figcaption></figcaption></figure>

### Session Key Establishment Protocol

前面例子的前提是AB已经共享了一个session key，而在现实中，双方应当使用Key Establishment protocol来确定双方的session key。更进一步，双方都希望对方是真实的对方，所以使用对方的publickey来确定对方身份，public key可以由Trusted Third Party提供

#### Needham-Schroeder Public Key Protocol

前提：AB双方知道对方的public key, Na Nb可以用来被生成对称的session key

<figure><img src="../.gitbook/assets/image (32).png" alt="" width="349"><figcaption></figcaption></figure>

**对NS协议的攻击 - MitM攻击**

这里我不用课件的图了，表示形式没有我找到的这张图清楚。这个MitM攻击的结果就是，B被迷惑了，B以为自己在和A对话，实际上是在和E对话。对于A来说，他其实一开始就是想和E对话，只不过E利用了无辜的A，把A的消息转发给了B。对于A为什么给E发{Nb}\_PKe，你可能会觉得，A明明知道了是Nb，应该发现不对劲了，为什么还是发给了E呢？哈哈，实际上Nb实际情况下并不是Nb，而是一串数字，A怎么知道是Nb还是Ne呢？所以这种攻击是有效的。

<figure><img src="../.gitbook/assets/image (38).png" alt="" width="375"><figcaption></figcaption></figure>

