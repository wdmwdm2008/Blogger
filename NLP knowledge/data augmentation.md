### NLP中文本数据增强方法
1. EDA  
2. Back Translation  
3. 生成对抗网络  
4. 实例交叉增强
5. 随机噪声注入 - 这些方法的思想是在文本中加入噪声，使所训练的模型对扰动具有鲁棒性。
6. 语法树操作 - 一个不改变句子意思的转换是句子从主动语态到被动语态的转换，反之亦然。


#### EDA
1) 随机插入(RI: Randomly Insert)  
2) 同义词替换（SR: Synonyms Replace） -》基于词典的替换和基于-》基于词向量的替换-》基于mask language model和TF-IDF的替换
3) 随机交换(RS: Randomly Swap)  
4) 随机删除(RD: Randomly Delete)  

#### 生成对抗式网络
生成对抗网络模型（GAN）以及它的各种变形，通过生成器和判别器的相互博弈，以达到一个纳什平衡，不断迭代增强训练数据达到以假乱真的效果，
最后用生成器大量生成同分布的数据，以达到数据增强的效果。但是GAN模型比较难训练，所以需要对GAN模型训练的足够好，才能更加有效的生成高质量的数据。  

#### 实例交叉增强

这项技术是由Luque在他的关于TASS 2019情绪分析的论文中提出的。这项技术的灵感来自于遗传学中发生的染色体交叉操作。
该方法将tweets分为两部分，两个具有相同极性的随机推文(即正面/负面)进行交换。这个方法的假设是，即使结果是不符合语法和语义的，新文本仍将保留情感的极性。

#### 随机噪声注入
1) 拼写错误注入  
2) QWERTY键盘错误注入  
3) Unigram噪声  
4) Blank Noising
5) 句子打乱

### 实现
要使用上述所有方法，可以使用名为nlpaug的python库：https://github.com/makcedward/nlpaug。它提供了一个简单且一致的API来应用这些技术。

### Reference
https://mp.weixin.qq.com/s/an1ewfO6pWyR6QmQyFv3Rw
https://mp.weixin.qq.com/s/aIydEKcDYWNaczMUi07tIQ
https://mp.weixin.qq.com/s/8pmL2mtanyZ0NqTH_cY8tQ
