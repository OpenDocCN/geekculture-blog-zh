# 使用 python 解决数学问题(快速代码-Python)

> 原文：<https://medium.com/geekculture/solving-math-problems-using-python-quick-code-python-52b1b37a79d5?source=collection_archive---------1----------------------->

![](img/71d2dd83f310262250cbcd685f1e4589.png)

(credit: Quanta Magazine)

这是**快速代码-python** 系列的**第二章**，回顾上一篇博客，我们已经看到了如何给变量赋值，操作优先级，BODMAS 如何帮助 Python 计算方程，我们可以和不可以使用的变量名，以及 Python 中可用的不同数据类型。[点击此处](/@Shreedharvellay/python-introduction-and-variable-assignment-quick-code-python-4611cc1df224)查看详情。

> 注意:我已经以线性的方式编写了**快速代码-Python** 系列，这样即使是新手也可以按照这个系列来理解 Python 实际能做什么。对于擅长基础知识的人来说，你可以跟随这个系列来更新你的概念，或者选择这个系列中你想了解更多的章节。

在这篇博客中，我们将把所有的理论付诸实践。使用 python 我们可以轻松解决一些有趣的问题。这个博客的主要目标是能够传达使用 python 获得问题的结果是多么容易和简单。

**A .计算矩形的面积和周长**

> 矩形的面积:a*b
> 
> 矩形的周长:2*(a +b)

```
a = 2
b = 3
area = a*b
perimeter = 2 * (a+b)print(area) #printing the area
**O/P : 6**print(perimeter)#printing peri
**O/P : 10**
```

**B .用 BODMAS 解一个简单方程**

**B** 球拍 **O** 订单 **D** 分割 **M** 乘法 **A** 加法 **S** 减法

```
x = 14
y = 23
z = 6
f = 10answer = (x — y) * z / f 
print (“Result =”, answer)**O/P : Result = -5.4**e = ((x - y) * z) / f    
print("Result =",  answer)**O/P : Result = -5.4**e = (x - y) * (z / f);    
print("Result =",  answer)**O/P : Result = -5.4**
```

**C .测试一个数是偶数还是奇数**

很容易找到一个数字是偶数还是奇数。只需应用模数运算符，即可得到除法的余数。

> 如果提醒是一个，那么它是奇怪的
> 
> 如果提醒为零，则为偶数

![](img/8297754a279e9cef30cbdd4282965e5c.png)

another way to find even or odd 😂 (credit: me.me)

```
a = 6
b = 2
c = a%b
print(c)**O/P : 0 # number is even**a = 7
b = 2
c = a%b
print(c)**O/P : 1 # number is odd** 
```

当然，我们可以在 print 语句中添加一些行来表示数字是偶数还是奇数，但是现在，为了便于理解，我将把它们放在一个注释行中(#…。)

**D .(任务 1)取一个数，求平方。(任务 2)计算任务(1)所得结果的平方根**

```
**#(task 1)take a number and square it**x = 6
y = x**2
print(y)**O/P : 36****#(task 2) take square root of the result you get from task(1)**import math #math library
z = math.sqrt(y) #using sqrt()
print(z)**O/P : 6**
```

我们使用一个名为 math 的库，这是一个内置的 python 库，可以进行快速的数学运算。函数`math.sqrt`取我们从**(任务 1)** 得到的结果`y`的平方根

**E .计算立方体的面积和体积。**

> 立方体的面积:6*a*a
> 
> 立方体的体积:a*a*a

```
a = 30
area = 6 * a * a
volume = a * a * aprint(area)
**O/P : 5400**print(volume)
**O/P : 27000**
```

**F .将一个数字增加 5 倍**

> 取一个数，如 6，加 5 次+1(增量)

有两种方法可以做到这一点，

```
**# method number one:** x = 6
x+=1
x+=1
x+=1
x+=1
x+=1
print(x)**O/P : 11****#method number 2** x = 6
x+=5 #increment 5 times
print(x)**O/P : 11**
```

作为一名程序员，我们总是寻找最少的代码行来解决问题，这样可以改善我们的代码表示，帮助我们更容易地调试。我建议用方法二来递增数字。

**G .写代码求解方程 z = | x y | *(x+y)**

```
x = 4
y = 3
z = abs(x-y) * (x+y)
print(z)**O/P : 7**
```

**H .将摄氏温度转换为华氏温度**

> 将摄氏温度转换成华氏温度的公式是，
> 
> F = 9/5* C +32

```
Celsius = 45
Fahrenheit = 9.0/5.0 * Celsius + 32
print(Fahrenheit)**O/P : 113.0**
```

I .将 KMPH 换算成 MPH

> 将 KMPH 换算成英里/小时的公式是:
> 
> 英里/小时= 0.6214 * KMPH

```
kmph = 100
mph =  0.6214 * kmph
print(mph)**O/P : 62.13999999999999**
```

这篇博客到此为止，希望你已经学会了使用 python 解决问题的简单方法。查看**快速代码-Python** 系列的前一章[这里](/geekculture/python-introduction-and-variable-assignment-quick-code-python-4611cc1df224)。

敬请期待:)