# 使用 Python 的欧几里德算法

> 原文：<https://medium.com/geekculture/euclidean-algorithm-using-python-dc7785bb674a?source=collection_archive---------13----------------------->

![](img/e5d9c392c4d0dd1873a14e51418d7419.png)

欧几里德算法，数论中最重要的算法之一，即将用 python 编写。这篇文章直截了当，让你不必等待一段时间的真实背景。祝你好运。

我们走吧。

如果你搜索过这个算法，那么你应该知道 gcd(最大公约数)是什么。

## **该算法的原理使用:**

a.就是求两个非负整数的 gcd 和
b .后来一直延伸到求这两个数的 gcd 的线性组合。

现在，在知道了它的用途之后，让我们开始研究算法。
为了更好的理解，我们将分别编写欧几里德算法和扩展欧几里德算法。

## 1.欧几里德算法

![](img/76b38f1417211a958231ffa0d9073425.png)

**用途:**
该算法只求两个非负整数的 gcd。

**代码:**

```
print(“Input two non-negative integers a and b such that a>=b”)
a,b= map(int, input().split())
x,y=a,b # Store temporarily while b>0:
 r=a%b
 a=b
 b=rprint(f”gcd({x},{y})={a}”)
```

您按照给定的条件提供输入，并获得任意两个非负整数之间的期望 gcd。

## **2。扩展欧几里德算法**

![](img/075b82e35d8e54dfb02a74c4a9966558.png)

现在最重要的算法是从前面的算法扩展而来的，如下所示:

**用途:** a .用于求两个非负整数之间的 gcd。
b .求 gcd(a，b)为 a 和 b 的线性组合即
gcd(a，b)=sa+tb 其中，
a 和 b 为非负整数
s 和 t 为任意

注:gcd(a，b)=sa+tb 也称为 bezout 恒等式。s 和 t 是 bezout 系数。

## **代码:**

```
def extended_euclidean(a, b): if b == 0: gcd, s, t = a, 1, 0 return (gcd, s, t) else: s2, t2, s1, t1 = 1, 0, 0, 1 while b > 0: q= a // b r, s, t = (a - b * q),(s2 - q * s1),( t2 - q * t1) a,b,s2,t2,s1,t1=b,r,s1,t1,s,t gcd,s,t=a,s2,t2 return (gcd,s,t)#Take inputprint("Input two non-negative integers a and b such that a>=b")a, b = map(int, input().split())x, y = a, b  # Store temporarilyresult=extended_euclidean(a,b) #Print the resultprint(f"gcd({x},{y})={result[0]}")print(f"Linear combination gcd(a,b)=sa+tb:\n {result[0]}={result[1]}*{x}+{result[2]}*{y}")
```

各位，这都是关于欧几里德和扩展欧几里德算法的，它们是数论中非常重要的主题。

更多话题敬请关注。不断学习，不断激励。

现在再见。开心点。❤😀