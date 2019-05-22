# Natural Language Processing

### 自然语言处理--NLP



## 自然语言处理面向的问题

- **主题识别**——这个文本讨论了哪些主题、人物、公司或地点？
- **情感分析**——这个文本传达的对于一个实体或主题的感觉是积极、消极还是中立的？
- **机器翻译**——将输入从一种语言转换为另一种语言，例如，从英语到法语
- **文字转语音**——将口语输入转换为书面形式
- **理解和解释**——哪些信息是对特定问题的回答？



## 自然语言处理的主要范畴 

[wiki]([https://zh.wikipedia.org/wiki/%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E5%A4%84%E7%90%86](https://zh.wikipedia.org/wiki/自然语言处理))

- [文本朗读](https://zh.wikipedia.org/w/index.php?title=文本朗讀&action=edit&redlink=1)（Text to speech）/[语音合成](https://zh.wikipedia.org/wiki/語音合成)（Speech synthesis）
- [语音识别](https://zh.wikipedia.org/wiki/語音識別)（Speech recognition）
- [中文自动分词](https://zh.wikipedia.org/wiki/中文自动分词)（Chinese word segmentation）
- [词性标注](https://zh.wikipedia.org/w/index.php?title=词性标注&action=edit&redlink=1)（Part-of-speech tagging）
- [句法分析](https://zh.wikipedia.org/w/index.php?title=句法分析&action=edit&redlink=1)（Parsing）
- [自然语言生成](https://zh.wikipedia.org/wiki/自然語言生成)（Natural language generation）
- [文本分类](https://zh.wikipedia.org/w/index.php?title=文本分类&action=edit&redlink=1)（Text categorization）
- [信息检索](https://zh.wikipedia.org/wiki/信息检索)（Information retrieval）
- [信息抽取](https://zh.wikipedia.org/wiki/信息抽取)（Information extraction）
- [文字校对](https://zh.wikipedia.org/wiki/校對)（Text-proofing）
- [问答系统](https://zh.wikipedia.org/wiki/問答系統)（Question answering）
- [机器翻译](https://zh.wikipedia.org/wiki/機器翻譯)（Machine translation）
- [自动摘要](https://zh.wikipedia.org/w/index.php?title=自動摘要&action=edit&redlink=1)（Automatic summarization）
- [文字蕴涵](https://zh.wikipedia.org/wiki/文字蘊涵)（Textual entailment）
- [命名实体识别](https://zh.wikipedia.org/wiki/命名实体识别)（Named entity recognition）



## 数据表征

​		NLP的一个关键问题是 *如何将数据表征为一个算法*。

> [表征]([https://baike.baidu.com/item/%E8%A1%A8%E5%BE%81](https://baike.baidu.com/item/表征)) 是指信息记载或表达的方式，能把某些实体或某类信息表达清楚的形式化系统以及说明该系统如何行使其职能的若干规则。因此，我们可以这样理解，表征是指可以指代某种东西的符号或信号，即某一事物缺席时，它代表该事物。

### 1. 词袋表征

​		**词袋**（Bow， Bag of Word）表征，指用离散计数或者二进制的独热编码表示语料库的词特征。

> **语料库**：单个文本或文档的集合。
>
> 如：corpus = ['The cat sat on the mat', 'The dog sat on the mat', 'The goat sat on the mat']
>
> **词汇表**：语料库中不重复的词的集合
>
> **离散计数**：每个词在语料库中出现的次数。
>
> | corpus[i] | cat  | dog  | goat | mat  | sat  | on   |
> | --------- | ---- | ---- | ---- | ---- | ---- | ---- |
> | i = 0     | 1    | 0    | 0    | 1    | 1    | 2    |
> | i = 1     | 0    | 1    | 0    | 1    | 1    | 2    |
> | i = 2     | 0    | 0    | 1    | 1    | 1    | 2    |
>
> **独热编码**：记录预料库中每个词是否出现。
>
> | corpus[i] | cat  | dog  | goat | mat  | sat  | on   |
> | --------- | ---- | ---- | ---- | ---- | ---- | ---- |
> | i = 0     | 1    | 0    | 0    | 1    | 1    | 1    |
> | i = 1     | 0    | 1    | 0    | 1    | 1    | 1    |
> | i = 2     | 0    | 0    | 1    | 1    | 1    | 1    |



​		词袋表征缺点：

1. 词序不变性
2. 缺乏语义泛化
3. 独热编码导致填充稀疏



### 2. 分布式表征--词嵌入（词向量）

​		某个词可由 d 个特征来描述，则这 d 个特征组成的向量就是词嵌入（词向量）。 而 d 是作为超参数存在，我们可以自行设定需要的个数，并称其为 嵌入维度。

​		若词汇表大小是 V，那么将会有 Vxd 大小的 **查找表** 用来表示某个语料库中词汇表的词特征。

> 假设 V = 5， d = 2，并选取两个特征 Gender(Man = -1, Woman = 1)，Edible（no=-1, yes=1）
>
> |        | King  | Queen | Apple | Pen   | Shoes |
> | ------ | ----- | ----- | ----- | ----- | ----- |
> | Gender | -0.93 | 0.96  | 0.02  | 0.04  | -0.03 |
> | Edible | -0.88 | -0.92 | 0.95  | -0.82 | -0.56 |
>
> 实际情况是，我们不会设定也不会知道具体有哪些特征，就像卷积神经网络会学习到我们所不能理解的特征一样。最终我们的目的是通过学习某种联系来学得词嵌入，某种联系可见后面 `Skip-Gram` 和 `CBoW`。



​		查找表中词的定位使用独热编码实现。

> 词的独热编码是 1xV 的向量，与 Vxd 的词嵌入表做矩阵乘法就能取得所需词的词嵌入。



### Word2Vec

​		Word2Vec 指代用来产生词嵌入的相关模型，由神经网络构成。主要依赖两个设计方案：Skip-Gram 和 CBoW。

#### 1. Skip-Gram

![](/home/brandewijn/Document/GitKraken/Footprint/Deep_Learning/Natural_Language_Processing/Images/skip-gram.png)

> w(t) 指文本中，第 t 个位置的词的词嵌入。

​		在 Skip-Gram 的架构中，选择的关系是目标词与上下文的相关性，以此来习得词嵌入。

> 目标词指的是需要优化的词，在文本中会进行轮换。
>
> **窗口** 大小决定了对于目标词而言的上下文范围。 如果窗口是 2,则选取目标词左边两个和右边两个词作为上下文，分别与目标词组成向量组。
>
> 非上下文则从非上下文范围中随机选取。



​		作为输入，目标词与上下文、非上下文分别组成向量组（目标词，上下文）、（目标词，非上下文）。通过它们各自的词嵌入做点积求得它们的相关性。对于（目标词，上下文）向量组，相关性尽可能高; 对于（目标词，非上下文），相关性尽可能低。

**简单网络构造**

```python
# Input is a word's one-hot, here is 1 x vocabulary_size
target_word = Input((1,))
context_word = Input((1,))

# An embedding layer is just a lookup table - a matrix of size vocabulary_size x 
# EMBEDDING_SIZE
# We select input_length rows from this matrix

embedding_layer = Embedding(vocabulary_size+1, EMBEDDING_SIZE, input_length=1, name='embedding_layer‘)

# Expect an output of similarity score between 0 and 1
output_layer = Dense(1, activation='sigmoid')

# Select the row indexed by target_word, reshape it for convenience
target_embedding = embedding_layer(target_word)
target_embedding = Reshape((EMBEDDING_SIZE,))(target_embedding)

# Select the row indexed by context_word, reshape it for convenience
context_embedding = embedding_layer(context_word)
context_embedding = Reshape((EMBEDDING_SIZE,))(context_embedding)

# Perform the dot product on the two embeddings, and run through the output sigmoid 
output = Dot(axes=1)([target_embedding, context_embedding])
output = output_layer(output)

# Setup a model for training
model = Model(inputs=[target_word, context_word], outputs=output)

optimizer = RMSprop(lr=0.0001, rho=0.99)
model.compile(loss='binary_crossentropy', optimizer=optimizer)

model.summary()
```



#### 2. CBoW

![](/home/brandewijn/Document/GitKraken/Footprint/Deep_Learning/Natural_Language_Processing/Images/CBOW.png)

​		CBoW，连续词袋模型，与 Skip-Gram 不同的地方在于同时考虑目标词前后所有上下文，取它们的平均词嵌入作为自己的词嵌入。

> 也可以选择 sum 而非 average 的形式。

#### 简单网络构造

```python
inputs = Input((4,))
# num_classes : number of words in vocabulary
embedding_layer = Embedding(num_classes, EMBEDDING_SIZE, input_length=2*window_size, name='embedding_layer')
# compute average
mean_layer = Lambda(lambda x: K.mean(x, axis=1))
output_layer = Dense(num_classes, activation='softmax')

output = embedding_layer(inputs)
output = mean_layer(output)
output = output_layer(output)

model = Model(inputs=[inputs], outputs=output)

optimizer = RMSprop(lr=0.1, rho=0.99)
model.compile(loss='categorical_crossentropy', optimizer=optimizer)
```

