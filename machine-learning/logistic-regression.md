# Logistic Regression

## Hypothesis Set

首先回顾一下监督学习的定义：

在给定训练集T的情况下，学习一个mapping，由输入X映射到输出y。\


<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

那么下图就说明了一个有监督学习的所有组件：

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

别的都好说，这个Hypothesis Set是什么呢？其实就是所选定的函数型，比如线形之类的。为什么有这玩意呢？因为传统的机器学习方法基本都是给定一个函数型，然后去学习里面每个w的大小，组合出一个完整的映射，来看性能。如果类比到深度学习里面，其实hypothesis set就是网络，比如说CNN，Transformer，RNN之类的，你先挑一个网络，然后进行梯度下降，最后比较精度。

### 有监督学习解决什么问题呢？

给定一个训练集，训练集怎么来的呢？从所有样本中，以一种未知但固定的概率从中独立同分布地抽出来的。目标是学习一个可泛化在未看见过的样本上的映射。

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

## 逻辑回归

尽管名字是回归，逻辑回归用来处理分类问题。

在逻辑回归模型中，我们把一个样本x，以及其属于某个类别的概率(实际上是logit)表示为一个线性组合。

从深度学习的角度看，逻辑回归就是一个以sigmoid函数为激活函数的单层线形神经网络，输出的内容为0-1的一个概率。

在下面，我们聚焦在2分类问题。

那么sigmoid是怎么来的呢？就是从logit里来的。怎么来的？

我们想要用线形模型WX建模概率p，那么就有一个问题，WX的范围是-∞,+∞，我们想要建模一个函数h,使得wx的范围和h(p)范围一样，也就是wx = h(p), h-1(wx) = p

h是什么呢？h就是ln(p/1-p), 这玩意范围在-无穷，+无穷，对上了，就能用。

而我们就把h，也就是logit的反函数就叫做sigmoid，1/(1+e-wx)



那么我们总结一下逻辑回归到现在的内容。wx越大，离的决策边界越远，置信度越高。正越大，p1越大，越负越p2.          &#x20;

### 怎么学W？

逻辑回归：极大似然法。

> 思想：我们进行一次采样，得出一个结果。我们认为，由于我们一次采样就采样到了这个结果，所以我们认为代表这个结果的概率密度就应当是最大的，所以我们求导=0就可以得到限制条件，从而得到想要的参数。
>
> 二项分布又称Bernoulli distribution
>
> 似然函数如下：
>
> <img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240408181103020.png" alt="" data-size="original">
>
> 在一般情况下
>
> * 如果X是离散的，那么就是所有情况f(x)的乘积
> * 如果X是连续的，那么就是求f(x;θ)对x的边缘
>
> 根据极大似然的思想，可以得到估计结果：
>
> <img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240408181314683.png" alt="" data-size="original">
>
> 这个函数怎么求最大值？先对齐log再求导，因为不改变其单调性。求导后这个玩意叫cost function

我们一般用负ln形式求导。一方面可以快速把乘转化为+，另一方面可以迎合loss function的形式，使loss最小即可。

这玩意如果进行一下数学推导，其实就是交叉熵loss, Cross Entropy Loss.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

