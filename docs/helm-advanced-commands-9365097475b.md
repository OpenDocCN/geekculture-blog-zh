# Helm —高级命令

> 原文：<https://medium.com/geekculture/helm-advanced-commands-9365097475b?source=collection_archive---------3----------------------->

## 舵的一些高级命令

![](img/ef6ea51e3acac0795795b7822a88465c.png)

Photo by [Matt Artz](https://unsplash.com/@mattartz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**上一篇:** [掌舵——在行动](https://faun.pub/helm-in-action-652abc10aa1c)

## 头盔-生成-名称

我们可以使用 **—生成名称**标志在舵图安装过程中自动生成发布名称

```
**>>** helm install bitnami/tomcat **--generate-name**
```

我们还可以提供一个名称模板，用于在安装过程中生成发布名称

```
**>>** helm install bitnami/tomcat --generate-name \
  ** --name-template=**"myserver-{{randNumeric 3}}"
```

**演示:**

## 舵-演习

在使用 helm chart 创建 Kubernetes 对象之前，我们可以使用 **—试运行**命令来检查是否一切正常。

```
**>>** helm install tomcat bitnami/tomcat \
 --values values_tomcat.yaml **--dry-run**
```

执行上述命令后，它将返回发行说明和 helm 将发送到 kube-API 服务器用于创建 k8s 对象的所有清单文件。在此 查看 [**的输出。**](https://gist.github.com/shamimice03/2cd5d60db59a28c0fa0e277ddc43a180)

## 头盔—模板

当我们使用— **模拟运行**命令时，我们必须能够访问 k8s 集群。— **预演**命令将在与 k8s 集群通信后生成发行说明和模板文件。并且生成的安装和升级的清单文件会完全不同。**试运行**命令的主要目的是在执行之前验证所有必要的配置。但是，如果我们无法访问 k8s 集群，我们仍然希望生成 k8s 对象的所有模板文件，这些模板文件将作为 helm 版本的一部分创建。在那种情况下，我们可以使用**舵模板**命令。

```
 **helm template [NAME]   [CHART]      [flags]**
**>>** helm template tomcat bitnami/tomcat --values values_tomcat.yaml
```

执行上述命令后，它将生成 k8s 对象的所有模板文件，这些文件将作为 helm 版本的一部分创建。在 处查看 [**的输出。**](https://gist.github.com/shamimice03/db074769324de0735efcd289c65b8607)

## 头盔—拿

**helm get** 命令由多个子命令组成，可用于获取发布的扩展信息，包括:

-用于生成发布的值
-生成的清单文件
-发布期间图表生成的注释
-与发布相关联的挂钩

## 赫尔姆—历史

**helm history** 命令将打印给定版本的所有**历史版本**。

```
 **helm history [RELEASE_NAME]**
**>>** helm history  tomcat
```

## 舵-回滚

helm 的主要优势之一是能够**回滚**到以前的版本。

> 要查看给定版本的所有历史修订，我们可以使用 **helm history** 命令。

```
 **helm rollback <RELEASE> [REVISION]
>>** helm rollback  tomcat    2
--------------------------------------------------------------------
  Rollback was a success! Happy Helming!
```

rollback 命令的第一个参数是发布版本的名称，第二个参数是修订(版本)号。如果省略 revision 参数，它将回滚到以前的版本。例如，如果我们有 4 个版本，我们忽略了版本号。然后发布将**回滚**到版本 3。

为了更好地理解，请参见下面的演示。

## 头盔—安装或升级

我们可以使用**头盔升级**命令以及**安装**选项。如果给定的版本已经存在于系统中，那么**升级**命令将发生。相反，如果给定的版本不存在，helm **install** 命令将安装该版本。

## 头盔—等待和超时

在舵图的安装过程中，我们可以使用**等待**选项。如果我们在安装/升级过程中指定了 **wait** 选项，helm 将在将发布标记为成功之前等待，直到部署、状态集或复制集的所有 pod、PVC、服务和最小数量的 pod 都处于**就绪状态**。Helm 将等待—超时(默认为 5 分钟)

```
**>>** helm install myserver bitnami/tomcat **--wait**
```

除此之外，如果我们想覆盖默认值**(5min)**；我们可以使用**超时**选项和**等待**选项。

```
**>>** helm install myserver bitnami/tomcat **--wait** **--timeout 3m30s**
```

## Helm —原子和超时

之前，我们已经讨论过**等待**选项，如果我们设置**等待**选项，helm 将**等待**k8s 对象在定义的时间内准备就绪。如果所有相关 k8s 对象在规定时间内**未处于就绪状态**，安装将被声明为**失败**。为了避免这种情况，我们可以使用— **原子**选项。

如果我们在安装时设置了**原子**选项，安装过程会在失败时删除安装。如果使用 **—原子**，将自动设置 **—等待**标志。

```
**>>** helm **install** mysvr bitnami/tomcat -f values_tomcat.yaml **-- atomic**
```

如果我们在升级时设置了— **原子**选项，那么在**升级失败**的情况下，升级过程将**回滚**所做的更改。

```
**>>** helm **upgrade** mysvr bitnami/tomcat -f values_tomcat.yaml **--atomic**
```

除此之外，我们可以通过使用— **超时**选项来覆盖默认的**超时**(5 分钟)。

```
**>>** helm install mysvr bitnami/tomcat -f values_tomcat.yaml \
  **--atomic** **--timeout 3m30s**
```

## 舵-失败时清理

**失败时清理**允许在**升级失败**时删除相应升级中创建的新资源。

```
**>>** helm **upgrade** mysvr bitnami/tomcat -f values_tomcat.yaml \
 **--cleanup-on-fail**
```

## 舵力

当我们发送一个请求给**升级**一个特定舵图的 Kubernetes 对象时。Kubernetes 将重新启动那些值已更改的 pod，而不是所有的 pod。但是如果我们想要强制重启某个图表的所有窗格，那么我们可以使用 **— force** 命令。Helm 将删除当前部署，而不是修改部署，并且将重新创建部署。因此，Kubernetes 将删除旧的 pod 并创建新的 pod。

> ***注意:*** *使用****—force****标志连同* ***升级*** *命令会有一些停机时间。*

```
**>>** helm **upgrade** mysvr bitnami/tomcat -f values_tomcat.yaml **--force**
```

## 接下来—

[](https://faun.pub/helm-create-your-first-chart-f3aa304c3544) [## Helm —创建您的第一个图表

### 创建新图表

faun.pub](https://faun.pub/helm-create-your-first-chart-f3aa304c3544) 

> 如果你觉得这篇文章有帮助，请不要忘记点击**按钮**和**按钮**来帮助我写更多这样的文章。
> ***谢谢🖤***

## **参考文献**

[](https://helm.sh/docs/helm/helm/) [## 舵

### Kubernetes 的头盔包装经理。Kubernetes 软件包管理器对头盔的常见操作:头盔搜索:搜索…

helm.sh](https://helm.sh/docs/helm/helm/)