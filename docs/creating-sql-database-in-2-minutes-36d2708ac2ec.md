# 在 2 分钟内创建 SQL 数据库

> 原文：<https://medium.com/geekculture/creating-sql-database-in-2-minutes-36d2708ac2ec?source=collection_archive---------36----------------------->

![](img/724e74379cfcb5b7a59e31f8fe364536.png)

Photo by [Mika Baumeister](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

数据库是包含一些特定类型的结构化数据的表的集合。这些数据库可以在 SQL 的帮助下轻松创建。然后，SQL 命令(也称为 SQL 查询)用于从中检索所需的数据或信息。

现在，让我们看看如何使用 SQL 在数据库中创建、删除备份和创建表。

# 1.创建数据库:

创建新的 SQL 数据库是最简单的任务之一。只需编写一条 CREATE 语句，我们就大功告成了。

**语法:**

> *创建数据库*数据库 _ 名称；

# 2.删除数据库:

我们可以使用 drop 语句删除现有的数据库，其语法如下:

**语法:**

> *删除数据库*DATABASE _ name；

# 3.备份数据库:

要创建数据库的备份，我们必须使用 backup 语句，并使用如下所示的语法为它提供一个存储位置，您需要在该位置存储数据库的备份:

**语法:**

> *备份数据库*数据库名称
> 
> *到磁盘*= ' file path '*；*

这里的文件路径指的是 PC 上存储数据库备份的位置。

# 4.在数据库中创建表:

创建数据库后，我们可以通过在 CREATE TABLE 语句中添加数据库名称来在特定数据库中创建表，如下所示:

**语法:**

> *创建表*database _ name . TABLE _ name*(*column _ name 1 datatype _ 1，column_name2 datatype2，… *)*

# 结论:

在此，我们学习了如何使用 SQL 在数据库中创建、删除、备份和创建表。更多关于 Python、机器学习、数据科学和前端开发的内容请关注 [Chirag Rathi](https://medium.com/u/7a0563da8b9d?source=post_page-----36d2708ac2ec--------------------------------) ！最后提供了一些最好的 SQL 在线资源的链接。

快乐学习！😊

# 找到我🤙：

*   [**领英**](https://www.linkedin.com/in/chiragrathi12/)
*   [**推特**](https://twitter.com/ChiragRathi8)
*   访问我的作品集网站[**chiragathi . me**](http://www.chiragrathi.me/)

[](https://www.w3schools.com/sql/) [## SQL 教程

### SQL 是一种在数据库中存储、操作和检索数据的标准语言。我们的 SQL 教程将教你…

www.w3schools.com](https://www.w3schools.com/sql/) [](https://www.hackerrank.com/domains/sql) [## 解决 SQL 代码挑战

### 加入 700 多万开发人员在 HackerRank 上解决代码挑战的行列，这是为…

www.hackerrank.com](https://www.hackerrank.com/domains/sql)