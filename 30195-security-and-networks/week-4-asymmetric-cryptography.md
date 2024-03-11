---
description: 介绍了非对称加密，以及公钥加密。
---

# Week 4 - Asymmetric Cryptography

## Introducino to Public-Key Cryptography

### Cryptography: Four Directions

* Confidentiality : 保密性
* Message Integrity : 消息完整性
* Sender Authentication : 发送者身份验证
* (soft) Sender Undeniability (non-repudiation) : 发送者不可否认，不可抵赖

接下来的课程将围绕这四个方向介绍其实现方法。

{% hint style="info" %}
前面说过Kerckhoffs' Principle, 这里详细描述一遍：

* A cryptographic system should be secure even if everything about the system, except the key, is public knowledge.&#x20;
* Modern Applications demand even Tamper-Resistance.
{% endhint %}

### Problems about Symmetric Encryption

加密和解密用同一个key，就是对称加密，但是，像下图的情况，就需要很多个key，具体来说，每个人需要维护n-1把key，总共需要n(n-1)/2把：

<figure><img src="../.gitbook/assets/image.png" alt="" width="375"><figcaption></figcaption></figure>

### Asymmetric Encryption: Public Key Encryption

我们想要每个人只维护两个key: one is Public and one is Private

这两个key是 asymmetric的: 相关但不相同

公钥所有人都知道，私钥只有自己知道

公钥加密流程：

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

现在，n个人的交流，只需要维持n个公钥和n个私钥

2n < n(n-1)/2  得出n>5的时候，使用公私钥就更加划算。

由于公钥私钥可以互相加解密，所以可以利用这一特性签名：

<figure><img src="../.gitbook/assets/image (2).png" alt="" width="375"><figcaption></figcaption></figure>

签名者用私钥签名，校对者用公钥检查看文档是否被修改，从而判定是否接受这份文件。

#### Public Key Infrastructure (PKI)

由于公钥是公开的，所以需要一个第三方权威机构来维护公钥的分发查询，这个叫作PKI。

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="563"><figcaption></figcaption></figure>

1、接收者A向注册权威处注册 2、注册权威告诉服务器生成一个证书，其中包含公私钥  3、公私钥发回A 4、将A的公钥放在数据库中 5、发送者B在数据库中拿到A的公钥 6、用A的公钥加密对A的信息

## Secure Key Exchange







