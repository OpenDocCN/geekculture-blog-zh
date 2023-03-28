# 像老板一样修补你的依赖

> 原文：<https://medium.com/geekculture/patch-your-dependencies-like-a-boss-de757367010f?source=collection_archive---------5----------------------->

![](img/d8e08260032b9a64a3d95c5c95ab1fe6.png)

Photo by [Pixabay](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/battle-black-blur-board-game-260024/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 什么是自动化测试&为什么你需要它？

无论何时你在编写一个 python 应用程序，无论是一个 web 应用程序，一个库，一个机器学习算法等等。你最终会测试你的代码，我不是在说手工测试，伙计；我们正在做自动化测试风格哟！

自动化测试使未来的变化更易于管理，就目前为止已经破坏的内容而言。它还使开箱即用的代码文档变得清晰。如果你不满意为什么自动化测试对你有好处，检查一下[这个链接](https://www.testim.io/blog/test-automation-benefits/)和我在本文末尾提供给你的链接。

现在我们在自动化测试是令人敬畏的这一点上达成了一致(我们是，不是吗？)，让我们深入研究更重要的东西。其中之一是如何修补单元测试中的依赖关系。

> [单元测试是你自动化测试的第一步](https://www.testim.io/blog/test-automation-pyramid-a-simple-strategy-for-your-tests/)，有一些关于“我们是否应该跳过它们并跳到集成测试”的争论，但这不是本文的主题，你可以使用本页底部提供的链接自行解决。

![](img/c54a18289619661f61495f9ec130d682.png)

Photo by [Wes Hicks](https://unsplash.com/@sickhews?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 那么什么是“修补”呢？🤔

分享一点个人经验，我不得不说，修补依赖关系是我长期以来的主要关注点，现在我终于找到了一种优雅的方法来处理这个问题，我感到非常兴奋；我在这里开始写它的主要原因。

现在，为了感受一下我们在这里试图解决的问题，假设您有一个服务，它接收来自用户的输入，进行一些外部 API 调用&在进行一些处理之后，返回最终结果。

在这个场景中，为了测试您的应用程序，每当您的测试在本地或者在您的[管道](https://phoenixnap.com/blog/what-is-ci-cd)中运行时，外部 API 调用必须是可用的。

但是如果你的网络连接太慢怎么办？或者，如果运行您的测试的机器不能访问互联网呢？还有一种可能性是，目标网站可能已经关闭[，无法访问](https://www.javatpoint.com/software-engineering-software-failure-mechanisms)。

同样的例子也适用于与数据库的交互。每次你需要测试你的应用程序的时候建立一个数据库服务器可能成本太高。

除非你正在运行[集成测试](https://www.testingxperts.com/blog/what-is-integration-testing)，这超出了本文的范围，现在是借助补丁解决[单元测试的好时机。在这些情况下，我们将模拟一个来自目标的结果，使它看起来像是依赖关系正在返回您想要的结果；这使得用补丁结果测试实际应用程序成为可能。](https://www.geeksforgeeks.org/python-unit-test-objects-patching-set-1/)

> [打补丁](https://docs.python.org/3/library/unittest.mock.html#the-patchers)是模仿外部依赖的行为，比如数据库、互联网上的 API 调用，甚至是库调用，使它看起来像是返回了我们给它的东西。

# 空谈是廉价的，给我看看代码😶

如果上面的介绍没有任何意义，请耐心等待，因为我们将在下面得到几个例子，以确保一切都正确无误。

正如你从上面看到的，我需要模仿一个外部库。这使我能够绕过通过互联网传输的实际请求，结果，我只测试了应用程序；这对我来说非常重要，因为我希望确保我的应用程序正常工作*，如果所有外部库都无缝运行*。

它也使你能够[测量你的覆盖范围](https://coverage.readthedocs.io/)以查看是否存在[死代码](https://en.wikipedia.org/wiki/Dead_code)。

测试方法的参数`monkeypatch`是一个`pytest`内置的夹具，如果你需要的话，你可以在你的每一个测试用例中使用它。它有很多非常有用的方法，我们将在本文中讨论。其中最有用的是`setattr`；它使得对类实例或类方法的任何访问都被重定向到完全可定制的东西。

在上面的代码片段中，我假装替换了`requests.get`来返回一个自定义对象(`MockStatus`)。这个自定义对象有一个我在代码`status_code`中需要的实例属性。因此当我调用`requests.get(URL).status_code`时，它实际上不是真正的库`requests.Response.status_code`，而是`MockStatus.status_code`。很美，不是吗？😻

以下是来自[文档](https://docs.pytest.org/en/stable/monkeypatch.html)本身的可用方法列表:

```
monkeypatch.setattr(obj, name, value, raising=**True**)
monkeypatch.delattr(obj, name, raising=**True**)
monkeypatch.setitem(mapping, name, value)
monkeypatch.delitem(obj, name, raising=**True**)
monkeypatch.setenv(name, value, prepend=**False**)
monkeypatch.delenv(name, raising=**True**)
monkeypatch.syspath_prepend(path)
monkeypatch.chdir(path)
```

**注意** : `setattr`实例方法有两个签名[来源](https://github.com/pytest-dev/pytest/blob/main/src/_pytest/monkeypatch.py#L158:L224):

1.  第一个和我们上面使用的一样，使用点分隔的`modulename.classname.atttibutename`作为第一个参数，在第二个参数中使用自定义值。
2.  第二个是传递一个具体的`object`(记住，[python 中的一切都是对象](/swlh/everything-is-an-object-in-python-learn-to-use-functions-as-objects-ace7f30e283e))作为第一个参数，传递该对象的一个双引号属性名(`str`)作为第二个参数，传递一个自定义值作为最后一个和第三个参数。

我们将在下一个例子中使用这两种方法。所以保持冷静，“愿理解的上帝指引我们”。🎮

# 让我们认真点，做点实际的事情😃

现在我们来看看其他的例子。这一次，让我们认真一点，做一些更有用的事情。我们走吧。

我应该提醒你，下一个例子有点长，你可能会在中间感到厌烦。但是在这里请原谅我，因为有一些宝贵的教训是我花了很长时间才意识到的。

正如你所看到的，这是一个全功能的应用程序，使用了我最喜欢的框架。

但是除了那些没完没了的“我的框架比你的好”之外，让我们吸取教训，继续前进。

我认为源代码非常明显，不需要我说太多，也不需要我把这篇文章写得太长，这样会降低我拥有更多读者的机会😁😉。

但是作为参考，我将更深入地研究其中一个测试用例:`test_create_new_user_successfully`:

*   `client`对象只是 FastAPI 本身的一个[测试应用程序，除了让它成为我在这个和任何未来 python 模块中的所有测试的一个全局可访问的夹具之外，我没有做任何修改。](https://fastapi.tiangolo.com/tutorial/testing/)
*   如前所述，`monkeypatch`是一个除了`pytest`本身之外不需要额外安装的固定装置。查看文档以获得完整的 API 参考。所以在每个测试案例中，你可以看到我在模仿数据库，让它“看起来”数据库已经向应用程序返回了一些东西，并测试源代码是否可以实际处理被模仿的对象。
*   首先，我正在修补 SQLAlchemy `Query`对象的`first`方法；`Query.first`是否等同于[类似于这个](https://docs.sqlalchemy.org/en/14/orm/query.html#sqlalchemy.orm.Query.first) : `SELECT … FROM … LIMIT 1`。通过打这个补丁，我们告诉解释器“假设数据库正在给你这个。*这个*，在这个场景中，其实就是`None`，因为我不想得到这个错误:`Email already registered`。
*   在下一部分中，我将遍历随机生成的用户的每一个键-值对，再次告诉解释器用我们给出的键-值对 ORM 中的所有内容进行伪替换，这样对任何实例属性的每次访问都将返回我们提供的值。
*   `Session.commit`和`Session.refresh`也被模仿，这似乎是一件不必要的事情，因为我们已经在`Query.first`中得到了我们想要的，但是这样做将删除任何实际的数据库交互，否则将需要一个启动并运行的数据库。
*   测试用例的其余部分只是调用端点，获取结果，并确保我们收到的与我们预期的完全一样；我相信你很有能力解决这个问题，但我不想冒昧。

![](img/525d1165b4a9af48852caeff314a8edc.png)

Photo by [cottonbro](https://www.pexels.com/@cottonbro?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/man-in-blue-dress-shirt-beside-woman-in-red-and-white-stripe-shirt-4835467/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 给我看更多，给我看更多，我等不及了😋

好了。你觉得这已经足够了吗？或者我们应该再来一轮？我觉得你懂了，但你知道他们说什么吗？

> 第三次是魅力所在。🤙

这里没什么特别的。只是想看看`setenv`和`delenv`是否在做他们应该做的事情。请记住，在第二个测试用例中做`delenv`实际上是不必要的，除非某些作恶者正在使用下面的命令运行您的测试:

```
DATABASE_URI=i_am_not_empty pytest patch_environment.py
```

如果你没有那个`delenv`，它就会失败。我们还添加了`raising=False`，它可以避免该键的空值引发`KeyError`。

我知道已经够了，但这是最后一轮。我保证🤞。

好吧。适可而止。我知道你已经看够了所有的测试和一切。当我第一次发现有这种东西存在时，我非常兴奋，这篇文章是我对外界的一种欣赏。

![](img/206e6e13d902bc6e078d765cd8ca9a26.png)

Photo by [Ketut Subiyanto](https://www.pexels.com/@ketut-subiyanto?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/cheerful-ethnic-mother-saying-goodbye-to-son-with-food-backpack-4473407/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 再见了，🥲

我是测试的狂热爱好者，所以我做了一点自我宣传，并告诉你们也来看看我的其他相关文章，这不会不合适😁👻：

[](https://python.plainenglish.io/how-to-write-tests-for-your-python-web-app-f6c6993abe36) [## 如何为您的 Python Web 应用程序编写测试

### 向项目添加测试和覆盖率的实用指南

python .平原英语. io](https://python.plainenglish.io/how-to-write-tests-for-your-python-web-app-f6c6993abe36) 

就这样吧。非常感谢你的支持和阅读。希望你喜欢它，并从中获得一些有用的东西。我希望你在发现这个`monkeypatch`的事情后和我一样兴奋(甚至更兴奋),因为我直到现在才知道这件事。

请在下面的评论中留下你的想法或任何疑问。我很乐意就这些有趣的工具进行交流。你也可以提出任何关于你想学的新话题的想法，我也会很乐意花时间为此写一篇文章。别担心，除非我自己先掌握它，否则我不会开始😅。

如果你想更深入地挖掘，这里有你可能需要的链接。

## 自动化测试

[](https://www.testim.io/blog/test-automation-pyramid-a-simple-strategy-for-your-tests/) [## 测试自动化金字塔:一个简单的测试策略——人工智能驱动的 E2E 自动化

### 你的开发团队是否花了太多时间等待他们的测试套件运行？他们是否经常重新进行测试…

www.testim.io](https://www.testim.io/blog/test-automation-pyramid-a-simple-strategy-for-your-tests/) [](https://www.testim.io/blog/test-automation-benefits/) [## 测试自动化的好处:2020 年自动化的 12 个理由

### 每个开发产品的公司都应该有测试。测试是产品开发的重要部分…

www.testim.io](https://www.testim.io/blog/test-automation-benefits/) [](https://www.guru99.com/automation-testing.html) [## 自动化测试教程:什么是自动化测试？

### 自动化测试或测试自动化是一种软件测试技术，它使用特殊的自动化测试…

www.guru99.com](https://www.guru99.com/automation-testing.html) [](https://softwaretestingfundamentals.com/unit-testing/) [## 单元测试-软件测试基础

### 单元测试，也称为组件测试，是软件测试的一个层次，在这个层次上，一个软件的单个单元/组件…

softwaretestingfundamentals.com](https://softwaretestingfundamentals.com/unit-testing/) 

## 模拟和补丁

 [## 模拟对象库- Python 3.9.4 文档

### 源代码:Lib/unittest/mock.py 是一个用于 Python 中测试的库。它允许您更换系统的部件…

docs.python.org](https://docs.python.org/3/library/unittest.mock.html) [](https://www.geeksforgeeks.org/python-unit-test-objects-patching-set-1/) [## Python |单元测试对象修补| Set-1 - GeeksforGeeks

### Python |单元测试对象修补| Set-1 问题是编写单元测试，需要将修补程序应用到选定的…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/python-unit-test-objects-patching-set-1/) 

## Pytest

[](https://docs.pytest.org/) [## 帮助你编写更好的程序

### pytest 和数以千计的其他软件包的维护者正在与 Tidelift 合作，以提供商业支持和…

docs.pytest.org](https://docs.pytest.org/) 

## FastAPI

[](https://fastapi.tiangolo.com) [## FastAPI

### FastAPI 框架，高性能，简单易学，快速编码，准备生产文档…

fastapi.tiangolo.com](https://fastapi.tiangolo.com)  [## pydantic

### 版本文档:1.8.1 使用 python 类型注释的数据验证和设置管理。pydantic…

pydantic-docs.helpmanual.io](https://pydantic-docs.helpmanual.io) 

## 我以前的内容

[](/amerandish/a-tmux-a-beginners-guide-7c129733148) [## Tmux:初学者指南

### 将您的单个终端变成多个终端。

medium.com](/amerandish/a-tmux-a-beginners-guide-7c129733148) [](/skilluped/10-tips-on-writing-a-proper-dockerfile-13956ceb435f) [## 写一份合适的文档的 10 个技巧

### 编写一个合适的 docker 文件并不太难

medium.com](/skilluped/10-tips-on-writing-a-proper-dockerfile-13956ceb435f) [](/skilluped/stop-writing-mediocre-docker-compose-files-26b7b4c9bd14) [## 停止编写平庸的 Docker-Compose 文件

### 带有可操作提示的备忘单

medium.com](/skilluped/stop-writing-mediocre-docker-compose-files-26b7b4c9bd14) [](https://meysam81.medium.com/what-is-haproxy-how-to-get-the-most-from-of-it-a9009b67f618) [## 什么是 HAProxy &如何充分利用它？

### 负载平衡、SSL/TLS、缓存等等。

meysam81.medium.com](https://meysam81.medium.com/what-is-haproxy-how-to-get-the-most-from-of-it-a9009b67f618)