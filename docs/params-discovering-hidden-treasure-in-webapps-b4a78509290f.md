# params——发现 WebApps 中隐藏的宝藏

> 原文：<https://medium.com/geekculture/params-discovering-hidden-treasure-in-webapps-b4a78509290f?source=collection_archive---------1----------------------->

![](img/771cbfc8223172464fe7bc8b16bf809e.png)

嘿伙计们！！这是怎么回事？👋我最近在考虑关于 web 应用程序中的参数发现的推文，然而，当我撰写推文时，由于我包括了所有单词列表、工具和方法，线程增长到了 5 条推文。然后我想，为什么不在媒体上发布而不用担心推特的限制呢？所以你有它，享受吧！！😉

# **为什么？**

首先想到的可能是为什么参数发现是必不可少的，对吗？

嗯…！！，如果您是 web 应用程序测试的新手或者已经做了一段时间，在目标应用程序中识别未链接、未知或隐藏的参数可能会导致有趣的信息或漏洞，因为它们没有得到太多关注，并且通常没有正确配置。

当你在 Bug Bounties 中看到一个参数时，每个人都开始发送他们的有效载荷列表，不要担心你这样做是否正确，因为我在开始时也是这样做的，这不是一个错误的方法，但在你尝试了所有其他方法后应该在最后完成，大多数时候当你报告时，他们会重复，对吗？因为有人在做同样的事情或使用同样的技术。

现在我希望你明白为什么检测隐藏参数是至关重要的；它可以帮助您找到漏洞，如 XSS，伊多尔，SSRF，权限提升，LFI，开放重定向，等等。甚至在某些情况下 PII 也会泄密😎很多人都错过了。

# 方法:

让我们从方法论开始吧！很简单“到处猜！! "，开玩笑“不要杀服务器。”

**何时何地做起毛:**

1.  每当你看到一个空白页面，你得到`200 OK`作为回应。
2.  在常见的端点上，例如:`login.php?Fuzz_Here`或`/login?FUZZ_Here`
3.  并且不常用的端点是必须的，例如:`/Thisendpointmakesnosense.php?FUZZ`或`/something?Fuzz`
4.  在 Post body 中也是如此，不是很多人都这样做，是吗？
5.  此外，还依赖于其请求中已经有许多参数的功能。

我通常在 burp 中将端点给 parm-miner 以供猜测，同时在 repeater 中测试它，如果 param-miner 没有发现任何东西，我会使用 intruder 中的一些单词表或另一个工具，如 [x8](https://github.com/sh1yo/x8) 或 [Arjun](https://github.com/s0md3v/Arjun) 。当进行模糊化时，要记住的是限制你使用的工具中的参数和线程的数量，并确保程序允许模糊化。

**总体做什么:**

1.  选择最有吸引力的目标端点。
2.  用一个单词表，把它发给模糊参数的工具。
3.  如果输出中有任何内容，请手动测试 XSS、SSRF、权限提升等等。！！

很简单，不是吗？😜你为什么不在下次圣灵降临节的时候试一试呢？

# 单词列表:

我遵循下面列出的顺序，但它可能会因目标而异。我通常使用 param-miner 和 Arjun 默认单词表找到一些东西，但是如果你有“我确信我会在这里找到一些东西，这看起来很有趣”的感觉，你可能也需要使用其他的当您看到一个端点时，您会想到这一点，但前提是您对 web 应用程序非常熟悉或了解。

**公共:**

1.  【阿琼】所有默认词表:【https://github.com/s0md3v/Arjun/tree/master/arjun/db】T4
2.  *Param-miner " params ":*[https://github . com/ports wigger/Param-miner/blob/master/resources/params](https://github.com/PortSwigger/param-miner/blob/master/resources/params)
3.  *asset note " parameters _ top _ 1m ":*[https://wordlists.assetnote.io/](https://wordlists.assetnote.io/)
4.  *nullenc 0 de " params . txt ":*[https://gist . github . com/nullenc 0 de/9cb 36260207924 f8e 1787279 a 05 EB 773](https://gist.github.com/nullenc0de/9cb36260207924f8e1787279a05eb773)

您可以将所有这些结合起来，形成最终的单词表，但我将把它留给您自己。👍此外，这不是一个详尽的清单；网上还有很多；这些只是我经常使用的一小部分！

**自定义词表:**

可以使用公共单词表，但是偶尔需要定制的目标特定单词表。现在，当我在一个大范围内工作或者在范围内有许多目标要测试时，我通常使用这个。因为某些参数偶尔会在特定的子域中使用，但是因为开发者喜欢复制粘贴程序，所以它们也可能在另一个目标上起作用。😆

下面是我用来生成单词列表的在线工具:

```
cat urls | unfurl format %q | cut -d "=" -f1 | sort -u > params.txt
```

在你运行这个之前，确保你已经使用 [gau](https://github.com/lc/gau) 或 [waybackurls](https://github.com/tomnomnom/waybackurls) 收集了所有子域名的 url，并且已经安装了[展开](https://github.com/tomnomnom/unfurl)

```
cat subdomains.txt | gau > urls
```

现在，这是其中一种方法，但是如果我告诉你有一种更好的方法，那么，我要与你分享的这种方法是使用 Burp 本身，这种方法的唯一问题是它很慢，因为你需要手动抓取网站，但如果你有 Burp Pro，这可以克服。

您将需要 [getAllParams](https://github.com/xnl-h4ck3r/burp-extensions/blob/main/getAllParams.py) 扩展，您可以在 Burp 社区和 Pro 中安装它。

**步骤:**

1.  安装并配置扩展。
2.  手动或自动对目标进行爬网。
3.  转到 Burp Target 选项卡，选择要为其构建单词列表的域，右键单击，然后从列表中选择“获取所有参数”。
    `Target -> Extension -> Get All Params`
4.  现在，导航到“Get All Params”选项卡，复制“Potential parameters found”下显示的所有参数，并将其粘贴到文本文件中。
5.  现在你可以在“工具:”一节中提到的任何工具中使用这个单词表。

这种技术的优点是，它从几个字段和向量中收集参数，而不是像前面的方法那样简单地请求 URL `Get`。

# **工具:**

这些是对我有用的工具；如果你有其他的，你也可以用。👍

1.  *x8*:[https://github.com/Sh1Yo/x8](https://github.com/Sh1Yo/x8)
2.  *阿琼:*[https://github.com/s0md3v/Arjun](https://github.com/s0md3v/Arjun)
3.  *参数挖掘器*:[https://github.com/PortSwigger/param-miner](https://github.com/PortSwigger/param-miner)
4.  【https://github.com/maK-/parameth】帕拉梅思:T23

这里有一个比较前三者的优秀博客:[https://4rt.one/blog/1.html](https://4rt.one/blog/1.html)

您也可以使用其他工具，如 ffuf，甚至是打嗝入侵者本身，但我喜欢使用专门为这项工作设计的工具。😬

# 结论，

参数发现可能非常有用，但是许多人忽略了它或者没有意识到它，并且他们经常忘记或者忘记何时执行它。如果你想找到那些隐藏的错误，这可能是一个很好的开始。我想就此结束，希望你下次狩猎成功。😄👍

还有，别忘了和我分享你的想法和批评。

***你可能会在以下平台找到我:***

*Twitter—*[*https://twitter.com/KathanP19*](https://twitter.com/KathanP19) *LinkedIn—*[*https://www.linkedin.com/in/kathan-patel-01b80516a/*](https://www.linkedin.com/in/kathan-patel-01b80516a/) *Youtube—*[*youtube.com/c/KathanPatel*](https://t.co/0mVYFmpryE?amp=1) *Github—*[https://github.com/KathanP19](https://github.com/KathanP19)

此外，如果你想看到更多这样的帖子，请购买一杯咖啡来支持我。
[https://www.buymeacoffee.com/kathanp19](https://www.buymeacoffee.com/kathanp19)

直到下一次，有一个伟大的一天！！😄