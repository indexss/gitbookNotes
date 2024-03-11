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

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="375"><figcaption></figcaption></figure>

### Asymmetric Encryption: Public Key Encryption

我们想要每个人只维护两个key: one is Public and one is Private

这两个key是 asymmetric的: 相关但不相同

公钥所有人都知道，私钥只有自己知道

公钥加密流程：

<figure><img src="../.gitbook/assets/image (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

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

问题定义：我们已经有了很好的对称加密算法（如AES-CBC PKCS7）来让我们的信息在不可靠信道上传输，但问题是，如果双方没有事先约定好symmetric key，那么对称加密是行不通的，因为key不应该直接在不可靠信道上传输。所以我们需要一种方法，来在不可靠信道上安全地确立symmetric key。

### 真实世界问题解决建模

从真实世界视角上建模，我们可以想象一个双面锁箱，左右都有锁，左边的锁可以用Alice的钥匙打开，右边的可以用Bob的锁打开，锁箱就在公共区域内，也就可以类比成非可靠信道。Alice把左边的门打开，放入秘密信息（symmetric key）然后上锁，Bob从右边把门打开把信息拿走。这样，在没有事先约定好的情况下，Alice和Bob就确定了symmetric key。**Diffie Hellman Key Exchange**就用了这样的思想。

### Diffie-Hellman Key Exchange

<figure><img src="../.gitbook/assets/image.png" alt="" width="563"><figcaption></figcaption></figure>

流程描述：

1. Alice和Bob先在不可靠信道上共同确定一个素数p和一个g   s.t. g < p && gcd(g, p-1) = 1
2. Alice选择一个a <- {2 .. p-2}，然后计算 $$A= g^a\ mod\ p$$，Bob选择一个b <- {2 .. p-2}，计算 $$B = g^b\ mod\ p$$
3. Alice把A发给Bob，Bob收到后把B发回给Alice
4. 对Alice来说，Alice知道a B p g，则 $$key=B^a\ mod\ p=g^{ab}\ mod\ p$$，对Bob来说，Bob知道b A p g，那么 $$key=A^b\ mod\ p=g^{ab}\ mod\ p$$

{% hint style="info" %}
**安全性说明**

窃听者无法得知g^ab，因为窃听者能看到g^a，g^b，g，p，但是由于对数计算十分困难，而g和p一般取的都很大，实际上无法从g^a 和 g反推出a的值，b也是一样，所以窃听者无法得到g^ab
{% endhint %}

