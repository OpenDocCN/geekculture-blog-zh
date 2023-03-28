# 文本特征提取(1/3):单词袋模型

> 原文：<https://medium.com/geekculture/text-feature-extraction-1-3-bag-of-words-model-649dbeeade79?source=collection_archive---------7----------------------->

![](img/61cab80d457efab17cda09216647c416.png)

[Source](https://unsplash.com/s/photos/cafe-study)

在任何机器学习模型中，特征都起着主要作用。只有当特征是数字时，训练模型才是可能的。因此，我们有各种技术将分类编码成数字，如标签编码、一键编码、散列等。同样，在 NLP 中，基于文本的模型也需要使用各种特征提取技术(如单词袋(BOW)、TF-IDF 或单词嵌入)来获得数字特征。

这个博客系列将解释如何使用 BOW、TFIDF 和单词嵌入获得基于文本的数字特征。我们将在这个博客中讨论弓模型。BOW 技术是最基本的特征提取和最早的方法。最容易理解和实现。那么，我们开始吧。

## 直觉

在自然语言处理中，文档的集合称为语料库，而标记(词)的集合称为文档。这意味着具有相似单词的文档应该是相似的。

## 理论与概念

*   单词包是一种将**文本数据转换成数字向量**作为特征的特征提取方法
*   这些数字是文档中每个单词(标记)的计数
*   生成类型为 *scipy.sparse.csr.matrix* 的**稀疏矩阵**(大部分为 0)
*   [下面的 CountVectorizer()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) 提供了某些参数，这些参数能够执行数据预处理，如停止字、令牌模式、lower 等。它在令牌大小上是灵活的，因为默认的 ngram_range 表示 1 个单词，但它可以根据使用情况进行更改。

> *class* `sklearn.feature_extraction.text.**CountVectorizer**` ( *** ， *input='content'* ， *encoding='utf-8'* ， *decode_error='strict'* ， *strip_accents=None* ， *lowercase=True* ，*预处理程序=None* ， *tokenizer=None* ， *stop_wordsu)\b\w\w+\b'* ， *ngram_range=(1* ， *1)* ， *analyzer='word'* ， *max_df=1.0* ， *min_df=1* ， *max_features=None* ， *vocabulary=None* ， *binary=False* ，

## *过程*

*让我们切入正题，研究一下建立单词袋模型的步骤。
***正文:*** 我是沙池，我是学习者*

1.  *文本被**标记化**
    “我”、“我”、“我”、“沙池”、“和”、“我”、“我”、“我”、“leaner”*
2.  *找出独特的单词，构建一个**词汇**
    【我】【am】【沙池】【和】【学习者】*
3.  *通过一键编码将记号转换成数字向量，并**计数每个词汇单词** {“I”:2，“am”:2，“沙池”:1，“and”:1，“学习者”:1}*

## *履行*

*为了理解 BOW 模型，让我们先来看看如何手动实现，然后 will for Sklearn 实现。*

1.  ***手动***

*   *让我们创建我们的句子语料库，并将它们转换成小写，以便不区分“this”和“This”。*

```
*doc = ["This is a good city",
      "You are good human",
      "This is worth fight for your own worth"]
#Convert into lowercase
doc = list(map(str.lower, doc))*
```

*   *标记并创建一个识别独特单词的词汇表。
    为词汇设置索引*

```
*unique_words = set((doc[0] + ' '+ doc[1] +' '+ doc[2]).split())
index_dict = {}
for ind, i in enumerate(sorted(unique_words)):
    index_dict[i] = ind
"""
{'a': 0,
 'are': 1,
 'city': 2,
 'fight': 3,
 'for': 4,
 'good': 5,
 'human': 6,
 'is': 7,
 'own': 8,
 'this': 9,
 'worth': 10,
 'you': 11,
 'your': 12}
"""*
```

*   *创建一个稀疏矩阵
    ——遍历语料库中的每个文档(句子)。获取它的字数。
    -创建一个稀疏矩阵，为行、列和值创建变量。
    —获取与语料库相关的“行”数据
    —获取“列”数据作为与 index_dict 相关的索引
    —获取“值”数据作为与 count_dict 相关的计数*

```
*from scipy.sparse import csr_matrixrow,col,val = [],[],[]
for idx, text in enumerate(doc):
    count_dict = {}
    tokens = text.split()
    # Get count of each word in sentence
    for word in tokens:        
        count_dict[word] = tokens.count(word)   

    for word, count in count_dict.items():
        ind = index_dict[word]        
        row.append(idx)
        col.append(ind)
        val.append(count)
print((csr_matrix((val, (row, col)),shape = (len(doc),len(index_dict)))).toarray())"""
[[1 0 1 0 0 1 0 1 0 1 0 0 0]
 [0 1 0 0 0 1 1 0 0 0 0 1 0]
 [0 0 0 1 1 0 0 1 1 1 2 0 1]]
"""*
```

***2。上面的步骤只用两行代码就可以完成。Scikit-learn 提供了一个名为 Count Vectorizer 的库，需要在语料库上进行拟合和转换。***

```
*cv = CountVectorizer(token_pattern=r"(?u)\b\w+\b")
count_occurrences = cv.fit_transform(doc)
count_occurrences.toarray()
"""
array([[1, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0],
       [0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0],
       [0, 0, 0, 1, 1, 0, 0, 1, 1, 1, 2, 0, 1]], dtype=int64)
"""*
```

## *缺点*

*   ***语义**:因为只考虑字数，所以有语义、结构或语法意义*
*   ***稀疏矩阵** : BOW 产生一个稀疏矩阵(大部分是 0)。任何稀疏模型都很难建模，因为它需要大量的内存和计算资源。*

*为了克服上述缺点，引入了 TF-IDF 特征提取方法，这是文本特征提取系列的下一部分。*

***看看我的**[**GitHub repo**](https://github.com/shachi01/NLP/blob/main/BagOfWords_Scratch_Sklearn.ipynb)**总结了这里演示的所有代码。***

***此外，您还可以通过本** [**GitHub 资源库**](https://github.com/shachi01/NLP/blob/main/BagOfWords_MovieReviews.ipynb) **了解更多关于如何开发一个词袋模型来预测电影评论情绪的信息。***

*如果你喜欢这位作者的博客，请随意关注，因为这位作者向你保证会带来更多有趣的人工智能相关的东西。
谢谢，
学习愉快！😄*

****可以通过***[***LinkedIn***](https://www.linkedin.com/in/kaul-shachi)***取得联系。****