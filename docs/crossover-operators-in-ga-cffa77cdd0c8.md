# 遗传算法中的交叉算子

> 原文：<https://medium.com/geekculture/crossover-operators-in-ga-cffa77cdd0c8?source=collection_archive---------1----------------------->

![](img/01932a09386febece067e3cc359daee7.png)

# 介绍

随着时间的推移，已经开发了无数的交叉算子。在本文中，我将讨论 13 个这样的交叉操作符，它们可以进一步分为 3 类(基于父代的编码)，如下所示:

![](img/80c22eab741a2a93d358b575372800dd.png)

# 单点交叉

**第一步-** 选择两个亲本进行交配。

![](img/13daf5fcd462109df03f21ce9a4ae389.png)

**步骤 2-** 随机选择一个交叉点，在正确的位置交换比特。

![](img/346611f35af502eeb284704ea339c6d8.png)

交叉后，生成的新后代看起来如下:

![](img/b90d9c7b7424c8ce73933ac2eaa90948.png)

# 两点交叉

**第一步-** 选择两个亲本进行交配。

![](img/172942af1f1112df8ddf40272178b2a3.png)

**步骤 2-** 随机选择两个交叉点，在中间位置交换比特。

![](img/5b1a2ac748bb100968ce4006588c5f40.png)

交叉后，生成的新后代看起来如下:

![](img/16d778f4072bac4d2de655ea71104b27.png)

# 多点交叉

**第一步-** 选择两个亲本进行交配。

![](img/172942af1f1112df8ddf40272178b2a3.png)

**步骤 2-** 随机选择多个交叉点，并在备用位置交换位。

![](img/5cd45f29eabbbe4a28f18c7f2a849d9e.png)

交叉后，生成的新后代看起来如下:

![](img/1ddc6019eb3731bc20ad197399daa6be.png)

# 均匀交叉

**步骤 1-** 选择两个亲本进行交配。

![](img/172942af1f1112df8ddf40272178b2a3.png)

**第二步-** 在父母的每个比特位置，抛一枚硬币(设 H=1，T=0)。

![](img/b67f694befa424e9be570fc021e9cae6.png)

**步骤 3-** 按照下面提到的算法产生两个后代:

```
if Toss=1,
 then swap the bits
if Toss=0,
 then don’t swap
```

![](img/0648702611b65ec1c9b61345292eec3f.png)

交叉后，生成的新后代看起来如下:

![](img/a2564df23cc444c121a9bfd395192a9d.png)

# 半均匀交叉

**步骤 1-** 选择两个亲本进行交配。

![](img/172942af1f1112df8ddf40272178b2a3.png)

第二步- 只有当 P1 和 p 2 对应的位不匹配时，才抛硬币(设 H=1，T=0)。

![](img/91c6f5bf82e9d46beaeb8bdf94f48521.png)

**步骤 3-** 按照下面提到的算法产生两个后代:

```
if Toss=1,
 then swap the bits
if Toss=0,
 then don’t swap
```

![](img/fd33d43e3029bc5ec482243bd37185c4.png)

交叉后，生成的新后代看起来如下:

![](img/220827b99673d60483a8cf9a4dd88ca7.png)

# 带交叉遮罩的均匀交叉(CM)

**第一步-** 选择两个亲本进行交配。

![](img/172942af1f1112df8ddf40272178b2a3.png)

**步骤 2-** 定义交叉蒙版(CM)。

![](img/a339130b109e72ba9499547ed0d39359.png)

**步骤 3-** 遵循下述算法:

```
To generate first offspring O1
------------------------------
if CM=0,
 then select P1 bit
if CM=1,
 then select P2 bitTo generate second offspring O2
------------------------------
if CM=0,
 then select P2 bit
if CM=1,
 then select P1 bit
```

交叉后，生成的新后代看起来如下:

![](img/e644e538c6fcb7d32eb18d17091a7ef9.png)

# 洗牌交叉

**第一步-** 选择两个亲本进行交配。

![](img/172942af1f1112df8ddf40272178b2a3.png)

**第二步-** 随机选择一个交叉点，对双亲的基因进行洗牌。

*注:分别对右侧位点和左侧位点进行基因重排。*

![](img/208789c4c5b9d51e89ea05b8caa6723a.png)

**步骤 3-** 执行单点交叉。

![](img/d8d4d33c255f0ef930f3ad105cc7b397.png)

交叉后，生成的新后代看起来如下:

![](img/43108c690dd8e1a6c9a4a29d3ff2db01.png)

# 矩阵交叉

**第一步-** 选择两个亲本进行交配。

![](img/9d7143f6ddc686cb51f77f34deeb4383.png)

**步骤 2-** 用二维表示写出双亲，然后将得到的矩阵分成不重叠的区域。

![](img/b8db8322208a0ced548a80b4aace3516.png)

**步骤 3-** 基于区域执行交叉。

![](img/075777ce83abfd26ce928c6e564f810e.png)

交叉后，生成的新后代看起来如下:

![](img/a9647f3fe0910c869c9e18fa70f500f8.png)

# 三亲杂交

**第一步-** 选择三个亲本进行交配。

![](img/60ef41b738fef04d9a5e9948b7710c9d.png)

**第三步-** 按照下面提到的算法产生一个后代:

```
To generate offspring O1 from (P1,P2,P3) combination
----------------------------------------------------
if P1 bit = P2 bit,
 then select P1 bit
if P1 bit != P2 bit,
 then select P3 bit
```

![](img/e9ab4a54265f6af25f8f22d71600a595.png)

交叉后，生成的新后代看起来如下:

![](img/84c2dc415d6c370aa080932fc2535732.png)

分别使用(P1、P3、P2)和(P2、P3、P1)组合重复相同的步骤来产生子代 O2 和 O3。

*注:有时，第三个父母可以被视为抛硬币或交叉面具(CM)。*

# 线性交叉

**第一步-** 选择两个亲本进行交配。

![](img/767ec664ea0b3ee54ff1aa1f1fed9065.png)

**步骤 2-** 随机选择单个基因(k)。

![](img/df176cef3dac726dce57a823a7be6954.png)

**步骤 3-** 定义α和β参数。

![](img/a78beb6ba54c8507992174b366742596.png)

**步骤 4-** 使用下述公式修饰 P1 的 kth 基因:

![](img/4c4beeb8ae62a8a6374a8ac58702cf06.png)![](img/1bb29f19d49bf6554444be1d3d02053d.png)

交叉后，生成的新后代看起来如下:

![](img/fda08b5566ececdeda3227e0148e6f3e.png)

*注意:通过给定不同的α和β值，我们可以生成任意数量的子代。*

# 单一算术交叉

**步骤 1-** 选择两个亲本进行交配。

![](img/767ec664ea0b3ee54ff1aa1f1fed9065.png)

**步骤 2-** 随机选择单个基因(k)。

![](img/df176cef3dac726dce57a823a7be6954.png)

**步骤 3-** 定义α参数。

![](img/255ce7fb70c36c0492ccacbea8bfeeab.png)

**步骤 4-** 修饰 P1 和 kth 基因产生后代:

![](img/077198f52d8a47bdd002236caecae29a.png)

# 部分映射交叉

**第一步-** 选择两个亲本进行交配。

![](img/ff41edd879cc970e8e65b0f395f71ed5.png)

**步骤 2-** 使用交叉点从父项中随机选择一个子串。

![](img/42692a708103f3088fae2f2467bf64b8.png)

**步骤 3-** 进行两点交叉。

![](img/8435dc1ab111bf7be62f4ce723333b5e.png)![](img/fd01df5e3cf3ad4348b6c3fe5120b2d5.png)

**步骤 4-** 从子串中确定映射关系。

![](img/d28c69dd98d0df5894d93ef6cabc000b.png)

**步骤 5-** 使用映射使未选择的子串中的子串合法化。

交叉后，生成的新后代看起来如下:

![](img/500230a45acbded7c0d77b5291db5f31.png)

# 循环交叉

**第一步-** 选择两个亲本进行交配。

![](img/ff41edd879cc970e8e65b0f395f71ed5.png)

**第二步-** 找到父母定义的循环。

![](img/1e0cb35fb39a2068e0323d0b32aed7ed.png)

**步骤 3-** 按照下面提到的算法产生一个后代:

```
To generate offspring O1
------------------------
if P1 bit in cycle,
 then select P1 bit
if P1 bit not in cycle,
 then select P2 bit
```

![](img/9ddbf94974100452fc9033129d93979e.png)

交叉后，生成的新后代看起来如下:

![](img/048399ed789d096b7570a66a6cabcfd5.png)

为了产生后代 O2，类似地进行，首先用 P2 填充，剩余的用 P1 填充。

这就是本文的全部内容。

不要忘记👏如果你喜欢这篇文章。

如果您想了解更多关于 GA 的知识，请查看我的系列文章:

[](https://apargarg99.medium.com/genetic-algorithm-ga-series-9cf533b292f) [## 遗传算法(GA)系列

### 我发表了不少关于遗传算法的文章。虽然这些文章站在自己的立场，它将更多…

apargarg99.medium.com](https://apargarg99.medium.com/genetic-algorithm-ga-series-9cf533b292f) 

如果你有任何问题或者想要澄清什么，你可以在 LinkedIn 上找到我。

~快乐学习。