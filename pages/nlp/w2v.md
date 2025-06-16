# One-Hot Embeddings
Before the rise of deep learning and modern NLP, One-Hot Encoding was one of the most common and direct methods for representing text data, especially vocabulary. It represents each word as a sparse vector, where the dimension of the vector is equal to the size of the vocabulary.

## Core Idea
Suppose we have a vocabulary containing V unique words. For each word in the vocabulary, One-Hot Encoding creates a vector of length V. In this vector:

- Only one position has a value of 1. This position corresponds to the word's index in the vocabulary.
- All other positions have a value of 0.

## Construction Process
- **Collect Corpus:** Obtain all the text data you want to process.
- **Build Vocabulary:** Extract all unique words from the corpus and assign a unique integer index to each word. Typically, low-frequency words are filtered out or grouped as "unknown words" (UNK).
- **Create One-Hot Vectors:** Generate the corresponding One-Hot vector for each word based on its index in the vocabulary.

## Disadvantages
1. **Curse of Dimensionality:** This is the primary drawback. If the vocabulary size is V (e.g., a medium-sized corpus might contain 50,000 to 1,000,000 different words), then the dimension of each word's One-Hot vector will be V. Such high-dimensional vectors lead to significant memory consumption and computational overhead.
2. **Inability to Represent Semantic Relationships between Words:** One-Hot vectors are orthogonal. This means the dot product between the One-Hot vectors of any two different words is 0, implying they are mutually perpendicular in the vector space. This indicates no measure of similarity between them. For example, "cat" and "dog" are semantically related as animals, but in One-Hot encoding, their vector representations show no similarity, failing to reflect this connection. This prevents models from capturing semantic relationships between words (like synonyms, antonyms, hyponymy, hypernymy, etc.).
3. **Lack of Contextual Information:** One-Hot encoding only represents the existence and uniqueness of a word itself, without considering its contextual information within a sentence. The same word might have different meanings in different contexts (polysemy), but the One-Hot vector cannot distinguish these subtle differences.

---

# Word2Vec
Word2Vec is a group of models and techniques proposed by Google in 2013 for generating word vectors (Word Embeddings). It aims to solve the problem of traditional One-Hot encoding being unable to capture semantic relationships between words by mapping each word into a low-dimensional, dense real-number vector space, so that semantically similar words are closer in the vector space.

## Core Idea: Distributional Hypothesis
Word2Vec is based on a core linguistic hypothesis, the "Distributional Hypothesis": **words that appear in similar contexts often have similar meanings.** In other words, the meaning of a word can be inferred from the words around it. Word2Vec models learn word vector representations by learning how to predict a target word based on its context, or predict the context based on a target word.

## Two Main Word2Vec Models
1. **CBOW (Continuous Bag-of-Words)**
    - **Objective:** To predict a target word (output) given its context words (input).
    - **Working Principle:** The CBOW model averages or sums the vectors of context words (within a given window size) and then uses this averaged/summed vector to predict the center word. It assumes word order is not important, focusing only on the set of words in the context.
    - **Input Layer:** One-Hot vectors of context words.
    - **Output Layer:** One-Hot vector of the target word (predicted via Softmax).
    - **Advantage:** Relatively faster training speed and performs better for high-frequency words.

2. **Skip-gram (SG)**
    - **Objective:** To predict context words (output) given a target word (input).
    - **Working Principle:** Opposite to CBOW, the Skip-gram model takes a One-Hot vector of a target word as input and tries to predict the words around it (within the context window). For each target word, it generates multiple prediction tasks for its context words.
    - **Input Layer:** One-Hot vector of the target word.
    - **Output Layer:** One-Hot vectors of context words (predicted via Softmax).
    - **Advantage:** Performs better for low-frequency words and large corpora, as it can learn from a broader range of contextual information.

## Word2Vec Output and Applications
After training, the Word2Vec model generates a fixed-dimension vector for each word in the vocabulary. These vectors have the following important properties:
1. **Semantic Similarity:** Semantically similar words (like "king" and "queen") are closer in the vector space (e.g., measured by cosine similarity).
2. **Analogy Reasoning:** Word vectors can capture complex relationships between words, such as the famous "King - Man + Woman = Queen" vector operation. This shows that the vectors not only encode the meaning of words but also their relationships.

## Disadvantages
1. **Inability to Handle Polysemy (Multiple Meanings):** This is Word2Vec's primary limitation. Regardless of whether "bank" refers to a financial institution or a river's edge, it will have only one fixed vector representation, unable to distinguish its meaning in different contexts.
2. **Inability to Handle OOV (Out-Of-Vocabulary) Words:** For words not encountered in the training corpus, Word2Vec cannot generate a vector for them. Typically, a special token is used or a random vector is assigned, which leads to loss of information. FastText addresses this to some extent.


# 独热编码
在深度学习和现代 NLP 发展起来之前，One-Hot 编码（独热编码）是表示文本数据，尤其是词汇，最常用且最直接的方法之一。它将每个词表示为一个稀疏向量，这个向量的维度与词汇表的大小相同。

## 核心思想
假设我们有一个词汇表（Vocabulary），包含 V 个唯一的词。对于词汇表中的每一个词，One-Hot 编码会创建一个长度为 V 的向量。在这个向量中：

- 只有一个位置的值为 1。 这个位置对应着该词在词汇表中的索引。
- 其余所有位置的值都为 0。

## 构建过程
- 收集语料库： 获得你想要处理的所有文本数据。
- 构建词汇表： 从语料库中提取所有唯一的词，并为每个词分配一个唯一的整数索引。通常，会将词频较低的词过滤掉，或者将它们归为“未知词”（UNK）。
- 创建 One-Hot 向量： 根据词汇表中的索引，为每个词生成其对应的 One-Hot 向量。

## 缺点
1. 维度灾难 (Curse of Dimensionality)：这是最主要的缺点。如果词汇表大小为 V（例如，一个中等规模的语料库可能包含 50,000 到 1,000,000 个不同的词），那么每个词的 One-Hot 向量的维度就是 V。如此高维的向量会带来巨大的内存消耗和计算开销。
2. 无法表示词语之间的语义关系：One-Hot 向量是正交的。这意味着任意两个不同词的 One-Hot 向量之间的点积为 0，它们在向量空间中是相互垂直的。这表示它们之间没有任何相似性度量。例如，“猫”和“狗”在语义上是动物，是相关的，但在 One-Hot 编码中，它们的向量表示没有任何相似性，无法体现这种关联。这导致模型无法捕捉到词语之间的语义关联性（如同义词、反义词、上下位关系等）。
3. 缺乏上下文信息：One-Hot 编码只表示了单词本身的存在和唯一性，而没有考虑单词在句子中的上下文信息。同一个单词在不同语境下可能有不同的含义（一词多义），但 One-Hot 向量无法区分这些细微差别。

# Word2Vec
Word2Vec 是一组由 Google 在 2013 年提出的用于生成词向量（Word Embeddings）的模型和技术。它旨在解决传统 One-Hot 编码无法捕捉词语之间语义关系的问题，将每个单词映射到一个低维、稠密的实数向量空间中，使得语义相似的词在向量空间中的距离更近。

## 核心思想：分布式假设
Word2Vec 基于一个核心的语言学假设，即“分布式假设”：出现在相似上下文中的词，其语义也往往相似。 换句话说，一个词的含义可以通过其周围的词来推断。Word2Vec 模型就是通过学习如何根据上下文预测目标词，或者根据目标词预测上下文，来学习词的向量表示。

## Word2Vec 的两种主要模型
1. CBOW (Continuous Bag-of-Words) - 连续词袋模型
    - 目标： 根据上下文词（输入）来预测目标词（输出）。
    - 工作原理： CBOW 模型将一个词的上下文词的向量（在给定窗口大小内）进行平均或求和，然后用这个平均/求和的向量来预测中心词。它假设词的顺序不重要，只关注上下文中的词集合。
    - 输入层： 上下文词的 One-Hot 向量。
    - 输出层： 目标词的 One-Hot 向量（通过 Softmax 预测）。
    - 优点： 训练速度相对较快，对于高频词效果较好。

2. Skip-gram (SG) - 跳字模型
    - 目标： 根据目标词（输入）来预测上下文词（输出）。
    - 工作原理： Skip-gram 模型与 CBOW 相反，它输入一个目标词的 One-Hot 向量，然后尝试预测其周围（上下文窗口内）的词。对于每个目标词，它会生成多个上下文词的预测任务。
    - 输入层： 目标词的 One-Hot 向量。
    - 输出层： 上下文词的 One-Hot 向量（通过 Softmax 预测）。
    - 优点： 对于低频词和大型语料库效果更好，因为它可以从更广泛的上下文信息中学习。

## Word2Vec 的输出和应用
训练完成后，Word2Vec 模型会为词汇表中的每个词生成一个固定维度的向量。这些向量具有以下重要特性：
1. 语义相似性： 语义相似的词（如“国王”和“女王”）在向量空间中的距离更近（例如，通过余弦相似度衡量）。
2. 类比推理： 词向量可以捕捉词语之间的复杂关系，例如著名的“King - Man + Woman = Queen”的向量运算。这表明向量不仅编码了词的含义，还编码了它们之间的关系。

## 缺点
1. 无法处理一词多义： 这是 Word2Vec 最主要的局限性。无论“bank”是“银行”还是“河岸”，它都只有一个固定的向量表示，无法区分其在不同上下文中的含义。
2. 无法处理 OOV (Out-Of-Vocabulary) 词： 对于在训练语料中未出现的词，Word2Vec 无法为其生成向量。通常需要使用特殊标记或分配随机向量，这会丢失信息。FastText 在一定程度上解决了这个问题。