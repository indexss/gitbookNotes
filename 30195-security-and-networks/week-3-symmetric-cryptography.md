---
description: 介绍了编码，简单的加密，着重讲解了对称加密的知识。
---

# Week 3 - Symmetric Cryptography

## Code versus Ciphers

编码是一种表示数据的方式，如ASCII, Hex, Base64, etc.

cipher是一种不知道规则就难以从code到data的模式。

* 几乎都用到了key
* 编码前叫plain text, 编码后叫cipher text
* 加密；encryption，解密：decryption

### 编码举例：

<figure><img src="../.gitbook/assets/image (6).png" alt="" width="375"><figcaption></figcaption></figure>

### Hex：

<figure><img src="../.gitbook/assets/image (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

### ASCII 不赘述

### Base64: 可打印字符最短的编码方式，使用Hex

<figure><img src="../.gitbook/assets/image (2) (1).png" alt="" width="375"><figcaption></figcaption></figure>

### Caesar Cipher

统一向前或后推n个字母。n rotations

<mark style="color:red;">Kerckhoffs's principle: A cipher should be secure even if the attacker knows everything about it apart from the key.</mark>

但最多就26个key.

更好的方案是指定某个字母替换另一个字母，这样就有26! 种key。

但其实这种也不行，因为字母语言有频率分析，对应这张表就可以大差不差猜出来：

<figure><img src="../.gitbook/assets/image (3) (1).png" alt="" width="375"><figcaption></figcaption></figure>

## Symmetric Cryptography

### Pre Knowledge

* Modular Arithmetric
* XOR
*   One Time Pads 一次性(例子用的加法)

    对于任意长度的密文，如果不知道密钥，则该密文是与同一长度的明文加密相同概率的所有相同长度明文。

    问题：密钥太长，且只能用一次

    <figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>



## Block Ciphers

现代加密都以明文块加密，由一系列permutations, substitutions重复在每个块上组成，permutations和substitutions由key控制。

### AES

AES是最先进的块加密，每块长128bits。

会生成10个round keys，由128位的主密钥生成。

使用一种Permutation: ShiftRows

使用三种Substitution: SubBytes，MixColumns，AddRoundKeys

AES过程：

1.  每块128bits，由4\*4矩阵表示，每个元素一个byte

    <figure><img src="../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>


2.  SubBytes: S-box

    <figure><img src="../.gitbook/assets/image (2).png" alt="" width="563"><figcaption></figcaption></figure>

    S-box是一个16\*16的表，每个元素都是1个byte，使用加密字节的高4位和低四位分别定位S-box的行和列，从而定位替换元素，进而进行替换。0x53就定位到5行3列。
3.  ShiftRows

    <figure><img src="../.gitbook/assets/image (3).png" alt="" width="563"><figcaption></figcaption></figure>

    第0行shift 0，第1行shift 1，第i行shift i
4.  MixColumn

    <figure><img src="../.gitbook/assets/image (4).png" alt="" width="563"><figcaption></figcaption></figure>

    这里c(x) = 3x^3 + x^2 + x +2
5.  AddRoundKey

    <figure><img src="../.gitbook/assets/image (5).png" alt="" width="563"><figcaption></figcaption></figure>

#### AES安全性来源：

没有数学证明是不可逆的，但是已经很好了。

有一种side channel attack，通过通测量功耗、执行时间等盘外招来破译，后面会将到。

安全保障来源：

1. 行和列微小的shuffle会带来输出的级大变化。
2. 需要至少一个非线性操作，在AES中由SubByte提供。

