# 设置地理复制

> 原文：<https://medium.com/geekculture/set-up-geo-replication-f7b9cb79bca4?source=collection_archive---------7----------------------->

![](img/efaa655d3c7d55ca3a73416e83afeb72.png)

Geo-replication

在上一篇博客中，我们介绍了地理复制的基础知识及其不同的部署服务。在这篇博客中，我们将一步一步地进行设置。🚶

# 先决条件👷

您至少有 2 台虚拟机。其中一个将容纳主音量💾而另一个将持有**二级卷💾**。我们将只在这两个卷上设置地理复制。

让我们现在开始设置😄

# 设置🔧

1.  创建 2 个卷。如果您想知道如何设置卷，请参考本文

[](/dev-genius/replica-volume-in-glusterfs-b05324ce1b6f) [## glusterfs 中的副本卷

### 这是建立 glusterfs 工作环境故事的延续。

medium.com](/dev-genius/replica-volume-in-glusterfs-b05324ce1b6f) 

这是副本卷。只有 2 台服务器时，您可以使用 2 个分布式卷，每个卷有 1 台服务器。可以参考之后的[。](https://docs.gluster.org/en/latest/Administrator-Guide/Setting-Up-Volumes/#creating-distributed-volumes)

2.开始🚲第 2 卷。在这里，我将这两卷看作是 **geom** (主卷)和 **geos** (副卷)

```
# gluster vol start geom
# gluster vol start geos
```

以下两个命令必须成功，否则卷不会启动。

3.如果您的主群集:

*   至少有 2 个节点的**然后执行以下命令并直接转到**步骤 8****

```
# gluster vol set all cluster.enable-shared-storage enable
```

*   从**步骤 4** 开始，集群中有**个节点**。

4.您需要在主服务器上创建一个共享存储卷。(您可以用 IP 地址替换主机名)

```
# gluster vol create gluster_shared_storage replica 3 servera:/data/glusterfs/avol/brick1/b11 servera:/data/glusterfs/avol/brick1/b12 servera:/data/glusterfs/avol/brick1/b13 force
```

为什么**逼**？😈

这是因为它会给出警告😷因为您尝试在同一台服务器上使用三块砖创建复制副本卷，因此会失败。如果你不希望这种情况发生，最终有必要增加力量。

5.开始🚲您现在创建的共享卷

```
# gluster vol start gluster_shared_storage
```

6.现在您需要创建一个共享存储目录。💾这将用作共享存储的装载点。

```
# mkdir -p /var/run/gluster/shared_storage
```

> 在 **mkdir** - **p** 命令的帮助下，你可以创建一个目录的子目录。如果父目录不存在，它将首先创建父目录。但是，如果它已经存在，那么它将不会打印错误消息，并将进一步创建子目录。

7.挂载共享卷💾在刚刚创建的目录中。

```
# mount -t glusterfs servera:gluster_shared_storage /var/run/gluster/shared_storage
```

> 当您“挂载”某个东西时，您将对其中包含的文件系统的访问权放在了您的根文件系统结构上。有效地给出文件的位置。

要了解更多关于坐骑的信息，请参考[本](https://askubuntu.com/questions/20680/what-does-it-mean-to-mount-something)。

8.**无密码 ssh 设置:**

在任一台主服务器上生成 ssh-key🔑

```
# ssh-keygen
```

将 ssh-key 复制到任何一个辅助服务器上

```
# ssh-copy-id root@serverb 
```

您可以用 IP 地址替换主机名。

9.创建 ssh 密钥

```
# gluster system:: execute gsec_create 
```

该命令执行以下操作:

*   它创建了两对 ssh 密钥(私有和公共)，一对用于集群中每个节点上的“tar over ssg ”,另一对用于“gsyncd shell”。
*   它在 ssh 公钥前面分别加上 COMMAND = " tar $ { SSH _ ORIGINAL _ COMMAND # * } "和' COMMAND = "/usr/lib exec/glusterfs/gsyncd " '作为' tar '和 gsyncd ssh 公钥的前缀。
*   它将来自集群所有节点的上述带前缀的 ssh 公钥聚合到位于“/var/lib/glusterd/geo-replication/common-secret . PEM . pub”的单个文件中

10.创建会话并将 ssh 密钥推送到所有辅助集群节点。

```
# gluster vol geo-rep geom serverb::geos create push-pem
```

11.您需要为地理报告启用`gluster_shared_storage`。尽管如果主卷不是复制卷/ EC 卷，可以忽略此步骤。

```
# gluster vol geo-rep geom serverb::geos config use_meta_volume true
```

地理代表使用共享存储卷来决定哪个工作者必须是活动的。它为每个副本创建公共文件，并且工人获得其上的 fcntl 锁，无论谁赢都将成为活动的。

12.通过执行以下命令检查地理代表的状态。

```
# gluster vol geo-rep geom serverb::geos status
```

您将看到一个表格，其中显示了地理代表工作人员的状态。它应该显示**创建了**。

13.您需要启动地理代表并再次检查状态。

```
# gluster vol geo-rep geom serverb::geos start
# gluster vol geo-rep geom serverb::geos status
```

所有 geo-rep worker 的状态都应该转到**初始化**，然后转到**历史抓取**再转到**变更日志抓取**。

## 和**完成😃🎊**

# 参考

 [## Gluster 文档

### GlusterFS 是一个可扩展的网络文件系统，适用于数据密集型任务，如云存储和媒体流…

docs.gluster.org](https://docs.gluster.org/en/latest/) 

# 另外，阅读

[](https://ujjwal-msrit.medium.com/gluster-directory-quotas-ebdf95bbfef6) [## Gluster —目录配额

### 什么是配额？

ujjwal-msrit.medium.com](https://ujjwal-msrit.medium.com/gluster-directory-quotas-ebdf95bbfef6) [](/geekculture/compiling-rpms-36ac24970f0e) [## 编译 rpm

### 内容📑

medium.com](/geekculture/compiling-rpms-36ac24970f0e)