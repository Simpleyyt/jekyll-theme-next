## FastText文本分类算法

### 一、算法简述

FastText是一个快速文本分类算法，在使用标准多核CPU的情况下，在10分钟内可以对超过10亿个单词进行训练，并且在不到一分钟的时间内对312K类中的50万个句子进行分类。 与基于神经网络的文本分类算法相比它主要由两个优点**首先FastText在保持高精度的同时极大地加快了训练速度和测试速度。再有就是不需要使用预先训练好的词向量，因为FastText会自己训练词向量 。** 

***

### 二、算法的应用场景

**1.文本分类**

![1532170982124](C:\Users\lilia\AppData\Local\Temp\1532170982124.png)

**2.情感分析** 

![1532171015201](C:\Users\lilia\AppData\Local\Temp\1532171015201.png)

**3.Web搜索、信息检索、网页排名等**  

****

### 三、预备知识及模型结构介绍

FastText的模型结构与Word2Vec中的CBOW模型基本一致。下面先介绍一些预备知识。

#### 3.1词向量

词向量，顾名思义，就是指单词或者汉字的向量化表示。

词向量一般有两种表示方式，分别是稀疏向量和密集向量。

##### 3.1.1 稀疏向量(one-hot representation） 

稀疏向量，就是用一个很长的向量来表示一个词，向量的长度为词典的大小N，向量的分量只有一个1，其他全为0，1的位置对应该词在词典中的索引。 假设一段文本有1000个词，如果用一个矩阵来表示这个文本，那么这个矩阵的维度为1000*1000。假设文本中有‘’方便面‘’，‘’面条‘’，‘’狮子’‘这三个词，用one-hot向量表示 的话，可以表示为下面这种形式。

![1532171684606](C:\Users\lilia\AppData\Local\Temp\1532171684606.png)

但是这种表示方法存在一些缺点：

**1.当语料长度过长的时候(容易引发维数灾难)。**举个例子，如果一个文本有10万个词，那么用稀疏向量来表示这个文本矩阵的话，需要用一个10万×10万的矩阵来表示它。

**2.无法体现出近义词之间的关系。**例如上面的‘’方便面‘’，‘’面条‘’，在中文中两个词是有一定关系的，但是如果用稀疏向量就无法表示出他们之间的关系，如果采用余弦相似度计算的话，它们之间的相似度为0。

![1532172097463](C:\Users\lilia\AppData\Local\Temp\1532172097463.png)

**3.不支持可微分优化(即不能使用经典的反向传播算法) 。**



##### 3.1.2 密集向量(distributed representation)  

也可以称之为word embedding，基本思路是通过训练将每个词映射成一个固定长度的短向量，所有这些向量就构成一个词向量空间，每一个向量可视为该空间上的一个点。**这种表示方法可以克服上述方法的缺点。** 

![1532172270287](C:\Users\lilia\AppData\Local\Temp\1532172270287.png)

通过one-hot矩阵乘以一个embedding层就可以得到一个词的向量表示，同时也达到了降维的效果。

此时向量长度可以自由选择，与词典规模无关。 

这种表示方式能更精准的表现出近义词之间的关系。

****

#### 3.2 n-gram feature 

FastText使用了n-gram来给文本添加额外的特征来得到关于局部词顺序的部分信息 。对于句子：“我 喜欢 喝 咖啡”, 如果不考虑顺序，那么就是每个词，“我”，“喜欢”，“喝”，“咖啡”这五个单词的word embedding求平均。如果考虑2-gram, 那么除了以上五个词，还有“我喜欢”，“喜欢喝”，“喝咖啡”等词。“我喜欢”，“喜欢喝”，“喝咖啡”这三个词就作为这个句子的文本特征。 

但为了提高效率，实际中会过滤掉低频的 N-gram。否则将会严重影响速度的话。

下面举个英文单词的例子：

对于单词向量apple，假设们采用的是3-gram特征，那么apple可以表示为图中五个3-gram特征，这五个特征都有各自的词向量，五个特征的词向量之和即为apple这个词的向量。

![1532172564359](C:\Users\lilia\AppData\Local\Temp\1532172564359.png)

使用n-gran feature有如下**优点:**

**1.为罕见的单词生成更好的单词嵌入**

即使单词很少，它们的字符仍然与其他单词共享，因此嵌入仍然可以很好

**2.在词汇单词中，即使单词没有出现在训练语料库中，它们也可以从字符级n-gram中构造单词的向量。**

**3.采用n-gram额外特征来得到关于局部词顺序的部分信息** 。

但是也存在一些**缺点:**

**随着语料库大小的增长，内存需求也会增长 。** 

论文中作者提出的解决方案是：

**1.过滤掉出现次数过少的词 。**

**2.使用hash存储。**

**3.由采用字粒度变为采用词粒度**

还是使用上面那个句子**“我喜欢喝咖啡”**，如果采用**字粒度**的2-gram的话，产生的特征如下：

“我喜”，“喜欢”，“欢喝”，“喝咖”，“咖啡”。

如果采用词粒度的2-gram的话，产生的特征如下:

“我喜欢”，“喜欢喝”，“喝咖啡”。

****

#### 3.Hierarchical softmax

FastText采用了Hierarchical softmax来对大数据集进行加速。FastText根据语料中每个标签出现的次数构造一颗Huffman树，Huffman树的结构如下：

![1532173706618](C:\Users\lilia\AppData\Local\Temp\1532173706618.png)



下面我们来看下FastText的模型结构

![1532173750289](C:\Users\lilia\AppData\Local\Temp\1532173750289.png)

模型总共分为三层，输入层输入的是一个已经分词后短文本。

如果用普通softmax训练，每个标签都需要计算，而采用霍夫曼树，标签数量多，权重就高，霍夫曼编码自然就越短，这样按照霍夫曼编码路径计算标签，就可以极大减少计算量。

短文本中每个词的词向量是由该短文本的ont-hot矩阵乘以一个初始化的矩阵w得到的。

下面分两部分来介绍这个模型，首先是从输入层到隐藏层：

![1532178555146](C:\Users\lilia\AppData\Local\Temp\1532178555146.png)

X1、X2…Xn是单词以及字符的n-gram特征的one-hot表示。

V1、V2…Vn是单词以及字符的n-gram特征的密集向量表示 。

Z为V1、V2…Vn的累加和。

W是一个矩阵其长度与one-hot向量的长度一致，但是其宽度为50~300之间的一个数。开始的W中的值是随机初始化的。

下面介绍隐藏层到输出层：

![1532178900039](C:\Users\lilia\AppData\Local\Temp\1532178900039.png)

$p^w$:从根节点出发到达w对应叶子节点的路径。

$l^w$: 路径$p^w$中包含节点的个数 。

$p_1^w$,$ p_2^w$, ···, $p_w^{l^w}$:路径上的节点, 表示词 $p_w^{l^w}$w对应的节点

$d_2^w$ . $d_3^w$ , ···, $d_w^ {l^w}$∈{0, 1}:词w的Huffman编码，有$l^w$-1个 e

$Θ_1^w$ , $ Θ_2^w $, ···, $ Θ_w^ {l^w} $∈$R^m$:路径上非叶子节点对应的向量 

这颗Huffman树的叶子节点代表的是标签，根据语料中每个标签出现的次数构成的。

下面是公式推导部分，其过程和word2vec的一致，**大家可以去看《word2vec中的数学原理》这本书**，上面的推到很详细，我们这里简要推导下：

![1532179607334](C:\Users\lilia\AppData\Local\Temp\1532179607334.png)

由上面这个式子，我们规定将一个节点进行分类时，**分到左边就是负类，分到右边就是正类。** 

由sigmoid公式，一个结点被分为正类的概率是: 

$σ(z^T θ)$  =$ \frac 1{1+ⅇ ^ {-z^T θ} } $

被分为负类的概率是：

1 - $σ(z^T θ)$

假设我们输入了一篇短文本，其标签为label 4,那么到达label 4节点要进行3次二分类，3次二分类的结果如下：

第一次： $P(d_2^w |z,θ_1^w )$ = 1 - σ(z^T θ) 

第二次： $P(d_3^w |z,θ_2^w )$ = σ(z^T θ) 

第三次： $P(d_4^w |z,θ_3^w )$ = σ(z^T θ) 

最后我们要求的概率表示如下：

![1532180226005](C:\Users\lilia\AppData\Local\Temp\1532180226005.png)

我们可以将条件概率$P( label|content(w_i))$的一般公式写为： 设我们输入

![1532180265958](C:\Users\lilia\AppData\Local\Temp\1532180265958.png)

或者写为下面的整体表达式形式: 

![1532180292994](C:\Users\lilia\AppData\Local\Temp\1532180292994.png)

将公式一代入对数似然函数得：

![1532180314963](C:\Users\lilia\AppData\Local\Temp\1532180314963.png)

为了方便计算将上式简记为： 

![1532180343871](C:\Users\lilia\AppData\Local\Temp\1532180343871.png)

观察目标参数L(w, j)可知，该函数中的参数包括向量Z，$ θ_w^{j-1}$， w∈C，j = 2, … , $l^w$. 

考虑L(w, j)关于$θ_w^ {j-1}$ 的梯度计算 

![1532180436554](C:\Users\lilia\AppData\Local\Temp\1532180436554.png)

$θ_w^{j-1}$的更新公式可写为:

![1532180476809](C:\Users\lilia\AppData\Local\Temp\1532180476809.png)

观察L(w, j)发现，Z和 θ_(j-1)^w是对称的，所以L(w, j)关于Z的梯度可写为:

![1532180496417](C:\Users\lilia\AppData\Local\Temp\1532180496417.png)



最终我们要求的是corpus中每个n-gram字符的向量，我们可以通过下式更新v(w): 

![1532180514862](C:\Users\lilia\AppData\Local\Temp\1532180514862.png)

公式推导到此结束，下面举个来举个例子。

### 四、实例

我们使用的语料是清华大学整理的THUCNews中文语料，里面包含14个类别的新闻，最少的一个类有3000+条新闻，最多的80000+条新闻 下面采用的分词工具是jieba，也可采用其他分词工具 。

下面是代码：

首先第一步是对语料进行分词并且打上相关标签:

```python
import jieba
import os

basedir = "THUCNews/" #这是我的文件地址，需跟据文件夹位置进行更改
dir_list = ['affairs','constellation','economic','edu','ent',
            'social', 'fashion','game','home','house','lottery','science','sports','stock']
##生成fastext的训练和测试数据集

ftrain = open("news_fasttext_train.txt","w")
ftest = open("news_fasttext_test.txt","w")

num = -1
for e in dir_list:
    num += 1
    indir = basedir + e + '/'
    files = os.listdir(indir)
    count = 0
    for fileName in files:
        count += 1            
        filepath = indir + fileName
        with open(filepath,'r') as fr:
            text = fr.read()
        seg_text= jieba.cut(text.replace("\t","").replace("\n"," "))
        outline = " ".join(seg_text) +"\t__label__" + e + "\n"
        # print(outline)
         # break

        if count < 10000:
            ftrain.write(outline)
            ftrain.flush()
            continue
        elif count  < 20000:
            ftest.write(outline)
            ftest.flush()
            continue
        else:
            break
print("success!")

ftrain.close()
ftest.close()
```

每个语料选取了20000篇的新闻，分词大概用了3h。有两个语料数据不超过10000，因此会被自动过滤掉。

下面我们用训练数据集生成模型

```python
# _*_coding:utf-8 _*_
import logging
import time
logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.INFO)
import fasttext
#训练模型
start=time.clock()
classifier = fasttext.supervised("news_fasttext_train_2.txt","news_fasttext.model",label_prefix="__label__")
end=time.clock()
total_time=end-start
print("总耗时:"+str(total_time))
#load训练好的模型
#classifier = fasttext.load_model('news_fasttext.model.bin', label_prefix='__label__')
```

使用测试数据集测试模型的精度

```python
#测试模型
#load训练好的模型
#import fasttext
result = classifier.test("news_fasttext_test_2.txt")
print(result.precision)
print(result.recall)
```

计算模型对每个类别的准确率、召回率、F值

```python
# -*- coding: utf-8 -*-
import logging
logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.INFO)
import fasttext


classifier = fasttext.load_model('news_fasttext.model.bin', label_prefix='__label__')
labels_right = []
texts = []
with open("news_fasttext_test.txt") as fr:
    for line in fr:
        line = line.rstrip()
        labels_right.append(line.split("\t")[1].replace("__label__",""))
        texts.append(line.split("\t")[0])
#        print(labels_right[0])
#        print(texts[0])
#     break
labels_predict = [e[0] for e in classifier.predict(texts)] #预测输出结果为二维形式
#print(labels_predict[0])
    
text_labels = list(set(labels_right))
text_predict_labels = list(set(labels_predict))
print(text_predict_labels)
print(text_labels)

A = dict.fromkeys(text_labels,0)  #预测正确的各个类的数目
B = dict.fromkeys(text_labels,0)   #测试数据集中各个类的数目
C = dict.fromkeys(text_predict_labels,0) #预测结果中各个类的数目
for i in range(0,len(labels_right)):
    B[labels_right[i]] += 1
    C[labels_predict[i]] += 1
    if labels_right[i] == labels_predict[i]:
        A[labels_right[i]] += 1

print(A) 
print(B)
print(C)
#计算准确率，召回率，F值
for key in B:
    try:
        r = float(A[key]) / float(B[key])
        p = float(A[key]) / float(C[key])
        f = p * r * 2 / (p + r)
        print("%s:\t p:%f\t r:%f\t f:%f" % (key,p,r,f))
    except:
        print("error:", key, "right:", A.get(key,0), "real:", B.get(key,0), "predict:",C.get(key,0))
```

下面我们随便找一篇文档来测试下这个模型

```python
test_corpus = ['李克强总理近日就电影《我不是药神》引发舆论热议作出批示，要求有关部门加快落实抗癌药降价保供等相关措施。癌症等重病患者关于进口‘救命药’买不起、拖不起、买不到等诉求，突出反映了推进解决药品降价保供问题的紧迫性。”总理在批示中指出，“国务院常务会确定的相关措施要抓紧落实，能加快的要尽可能加快。”今年4月和6月，李克强两次主持召开国务院常务会议，决定对进口抗癌药实施零关税并鼓励创新药进口，加快已在境外上市新药审批、落实抗癌药降价措施、强化短缺药供应保障。会议决定，较大幅度降低抗癌药生产、进口环节增值税税负，采取政府集中采购、将进口创新药特别是急需的抗癌药及时纳入医保报销目录等方式，并研究利用跨境电商渠道，多措并举消除流通环节各种不合理加价，对创新化学药加强知识产权保护，强化质量监管。“抗癌药是救命药，不能税降了价不降。”总理说，“必须多措并举打通中间环节，督促推动抗癌药加快降价，让群众有切实获得感。”在今年4月的一次基层考察中，李克强还专程来到一家外资药企，以将药品纳入医保、实施政府采购等方式，希望该药企生产的抗癌药等重大疾病药品价格能够更加优惠公道。“现在谁家里一旦有个癌症病人，全家都会倾其所有，甚至整个家族都需施以援手。癌症已经成为威胁人民群众生命健康的‘头号杀手’。”总理说，“要尽最大力量，救治患者并减轻患者家庭负担。”李克强明确要求这项工作要进一步“提速”：“对癌症病人来说，时间就是生命！”在影片《我不是药神》讲述患病群体用药难题，引发舆论广泛关注讨论后，李克强特别批示有关部门，要“急群众所急”，推动相关措施加快落到实处。']
```

```python
# 使用jieba对语料进行分词
import jieba
predict_list=[]
predict_list.append(' '.join(jieba.lcut(test_corpus[0])))
```

```python
# 输出分词后的语料
predict_list
```

```python
# 输出预测结果
classifier_1.predict(predict_list)
```

```python
# 输出预测结果以及准确度
classifier_1.predict_proba(predict_list)
```

### 五、fastText和word2vec的区别

**相似处：**

> 1.图模型结构很像，都是采用embedding向量的形式，得到word的隐向量表达。

> 2.都采用很多相似的优化方法，比如使用Hierarchical softmax优化训练和预测中的打分速度。

**不同处：**

> 1.模型的输出层：word2vec的输出层，对应的是每一个term，计算某term的概率最大；而fasttext的输出层对应的是 分类的label。不过不管输出层对应的是什么内容，起对应的vector都不会被保留和使用；

> 2.模型的输入层：word2vec的输出层，是 context window 内的term；而fasttext 对应的整个sentence的内容，包括term，也包括 n-gram的内容；

两者本质的不同，体现在 h-softmax的使用：

> Wordvec的目的是得到词向量，该词向量 最终是在输入层得到，输出层对应的 h-softmax 也会生成一系列的向量，但最终都被抛弃，不会使用。

> fasttext则充分利用了h-softmax的分类功能，遍历分类树的所有叶节点，找到概率最大的label（一个或者N个）