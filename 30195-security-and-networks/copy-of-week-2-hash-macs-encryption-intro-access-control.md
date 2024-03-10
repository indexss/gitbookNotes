---
description: 本周内容比较多，但多数为intro类型
---

# Copy of Week 2 - Hash, MACs, Encryption Intro, Access Control

{% hint style="info" %}
目前，通过密钥分发协议和对称加密可以保证密钥是安全的，但是我们仍需要检测ciphertext manipulation。这时候，Hash，MACs和Authenticated Encryption就可以解决问题。
{% endhint %}

## Hash

### 定义

A hash of any message is a short string generated from that message.

### 特性

1. 同一个字符串Hash后结果总是相同
2. 任何对原文小的修改都会对Hash后的结果影响很大，完全不同
3. 找出Hash函数的逆函数很困难
4. 找到两个不同的原文，但拥有相同的哈希值很难。

### 对Hash的攻击

1. Preimage attack: 前像攻击，碰运气找一个M，使得Hash(M) == 被Hash的原文。几乎不可能实现
2. Collision attack: 找两个不同的信息，拥有相同Hash值
3.  Prefix collision attack:&#x20;

    攻击者可以选择两个不同的前缀p1和p2,然后附在不同的字符串m1,m2前面，那么有：

    ```java
     hash(p1 ∥ m1) = hash(p2 ∥ m2)
    ```

### Birthday Paradox 生日悖论

至少要多少个人，使得其中有两人生日相同的概率为50%？

答案是23。23人，就有C 23 2 = 23\*22/2 = 253对生日。而两个人生日不同的概率是364/365，那么都不同的概率是364/365 ^ 253 == 0.4995。

这个例子说明了很容易碰撞。

### Hash函数举例

#### SHA Family

1993年 NIST发明了SHA-0。1995年改进修复为SHA-1。

1.  SHA1

    生日攻击应该需要2^80次哈希test，但2005年有人发现了2^63次的攻击，所以没人相信SHA1了。
2.  SHA2

    用了更长的Hash，256bits or 512bits。但由于基于SHA1，和SHA1有同样的弱点，密码学家不满意。
3.  SHA3比赛

    2008年10月31号开始，在2014年被用作NIST标准。

#### MD Family

MD4和MD5都不安全，但如果只关心preimage attack的话，或者只关心完整性的话，还可以 MD6就是Ron Rivest对SHA3做出的修改。很好用。



## Message Authentication Codes

### 介绍

MACs和MD5 Hash校验的思想比较相似，但是不太一样。MACs可以校验信息来源和信息完整性，而MD5只能验证信息完整性。

MACs思想：use a key to ensure that message has not been changed

例子：

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

可以看到，MACs实际上就是数字签名的对称加密版本。

### 对MACs的攻击

Length extension attack：Add data to a MAC withoud knowing the key.

原理：很多MAC是通过MD Hash实现的，而MD有个特点，如果攻击者能够获得一个消息$$M$$的散列值$$H(M)$$和该消息的长度，即使不知道消息内容，也能够计算$$M$$加上某个附加数据$$D$$后的散列值$$H(M∥D)$$。一些MAC是这样实现的：$$MACk​(M)=H(k∥M)$$。

### 分块加密 Block Cipher Modes

#### CBC

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>CBC mode</p></figcaption></figure>

{% hint style="info" %}
IV与块同长，第一个IV随机选择，不需要保密，后面的IV均为前一个的密文。

CBC解密基于这个式子：对于 A1 xor A2 = B1, 那么有B1 xor A2 = A1

流程为，先通过key解密，再和IV（前一个的密文异或），得到块明文，最后再拼接。
{% endhint %}

CBC MAC：

<figure><img src="../.gitbook/assets/image (6).png" alt="" width="563"><figcaption></figcaption></figure>

Hash MAC:

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="375"><figcaption></figcaption></figure>

### Broken Hash to MAC 用CBC MAC替换垃圾的Hash MAC

<figure><img src="../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>

一般用AES加密。块加密模式还有CBC，ECB和CTR，后面会提到。

**注意**：如果我知道了原文的话，我可以替换CTR加密的密文，从而攻击。这个在assignment 2里有用到。

## Authenticated Encryption Modes

前面提到的原文攻击，可以通过Authenticated  Encryption Modes解决。

原理是：当只使用AES加密后，你无法知道一段文字是密文还是被篡改的密文。认证加密的一般做法是把MAC加到密文中，这样，你就可以通过MAC（类似签名）来判断密文有没有被篡改了。

### CCM mode

CCM全称为CTR with CBC-MAC，CCM不复杂，但被证实是安全的。过程如下：

1. 用ARS CBC生成MAC
2. 原文拼接MAC，使用与MAC相同的key进行CTR加密。

## Access Control

Access Control就是Linux中的文件权限管理，分别管理创建者，同用户组，其他人的读，写执行权限。

### Access Control Matrix

这是一个有关所有实例权限的矩阵，问题是维持这一个矩阵十分困难，且矩阵被破坏后，所有的控制权就都丧失了，代价太大，应当分布式存储。

<figure><img src="../.gitbook/assets/image (8).png" alt="" width="563"><figcaption></figcaption></figure>

### Access Control Lists (ACLs)

我们不想存一个巨大的矩阵，所以用ACL。思想：把文件和他的权限信息存在一起。

UNIX中的ACL：

<figure><img src="../.gitbook/assets/image (9).png" alt="" width="563"><figcaption></figcaption></figure>

权限指示符含义：\


<figure><img src="../.gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure>

