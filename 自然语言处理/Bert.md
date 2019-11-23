# Bert的双向体现在什么地方？
mask+attention，mask的word结合全部其他encoder word的信息

# Bert的是怎样预训练的？
- MLM：将完整句子中的部分字mask，预测该mask词
- NSP：为每个训练前的例子选择句子 A 和 B 时，50% 的情况下 B 是真的在 A 后面的下一个句子， 50% 的情况下是来自语料库的随机句子，进行二分预测是否为真实下一句

# 在数据中随机选择 15% 的标记，其中80%被换位\[mask]，10%不变、10%随机替换其他单词，原因是什么？
- mask只会出现在构造句子中，当真实场景下是不会出现mask的，全mask不match句型了
- 随机替换也帮助训练修正了\[unused]和\[UNK]

# elmo、GPT、bert三者之间有什么区别？
- 特征提取器：elmo采用LSTM进行提取，GPT和bert则采用Transformer进行提取。很多任务表明Transformer特征提取能力强于LSTM，elmo采用1层静态向量+2层LSTM，多层提取能力有限，而GPT和bert中的Transformer可采用多层，并行计算能力强。
- 单/双向语言模型：GPT采用单向语言模型，elmo和bert采用双向语言模型。但是elmo实际上是两个单向语言模型（方向相反）的拼接，这种融合特征的能力比bert一体化融合特征方式弱。
- GPT和bert都采用Transformer，Transformer是encoder-decoder结构，GPT的单向语言模型采用decoder部分，decoder的部分见到的都是不完整的句子；bert的双向语言模型则采用encoder部分，采用了完整句子

# 长文本预测如何构造Tokens？
- head-only：保存前 510 个 token （留两个位置给 \[CLS] 和 \[SEP] ）
- tail-only：保存最后 510 个token
- head + tail ：选择前128个 token 和最后382个 token（文本在800以内）或者前256个token+后254个token（文本大于800tokens）

# 