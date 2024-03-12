---
description: 讲解了HTTPS协议和Tor协议
---

# Week 6 - TLS & Tor

## SSL/TLS Protocol

### Basic Information

SSL (Secure Sockets Layer) 被更名为 TLS (Transport Layer Security)。

基于非对称加密提供加密的信息交流和身份认证

协议栈可以变化，例：RSA(非对称) + DES(对称) + DH(密钥交换)，是在运行前商量好的。

TLS跑在应用层和传输层（TCP/UDP）之间，加密解密操作对应用层透明。

<figure><img src="../.gitbook/assets/image.png" alt="" width="188"><figcaption></figcaption></figure>

### X.509 Standard for Certificates

包含了主体，主题的公钥，颁发者姓名等等

颁发者对Certificates的哈希签了名

如果你相信这个颁发者，并通过了签名认证，则你可以相信这个公钥

### TLS流程

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>

### TLS-DHE流程

引入DHE来获得Forward Secrecy。S知道x，C知道y

<figure><img src="../.gitbook/assets/image (2).png" alt="" width="563"><figcaption></figcaption></figure>

### TLS Cipher Suites

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### TLS Handshake

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="563"><figcaption></figcaption></figure>

### TLS Weeknesses

* Configuration Weekness
  *   Cipher downgrading

      选择Cipher suite是Client和Server取交集。攻击者可以迫使你选低级suite，从而攻击。
  *   Self-signed certificates

      公司如果服务器很多，维护证书的工作量就很大，有人就开始自签证书。自签的问题就是，public key可能会被替换，就很容易遭受MitM。
* Direct Attack against implementations
  * Apple's goto fail bug ： goto fail; goto fail;
  * [LogJam](https://zhuanlan.zhihu.com/p/32697829) ：将DF中的g^a g^b换成短的t\_a, t\_b，对DHE\_EXPORT有效。
  * [HeartBleed](https://zhuanlan.zhihu.com/p/32697829)

### TLS 1.3

2018新标准，删除了太弱的suites，简化握手提高效率，强制使用前向保密，除非实施MitM，否则无法被拦截。



## Tor

