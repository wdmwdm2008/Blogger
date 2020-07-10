### Beam Search 介绍
beam search尝试在广度优先基础上进行进行搜索空间的优化（类似于剪枝）达到减少内存消耗的目的。

算法过程
定义词表大小是V，beam size是 B，序列长度是L。

假设V=100，B=3：

1. 生成第1个词时，选择概率最大的3个词（假设是a，b，c），即从100个中选了前3个；

2. 生成第2个词时，将当前序列a/b/c分别与词表中的 100个词组合，得到 3*100个序列，从中选 3个概率最大的，作为当前序列（假设现在是am，bq，as）；

3. 持续上述过程，直到结束。最终输出3个得分最高的。

算法复杂度[公式]
在第2步，要计算 [公式] 次。序列长度是L，生成长度为L的序列，计算 [公式] 次。

### 算法评价
优点
(1) 减少计算开销。相对于广度优先搜索，广搜每次都要保留所有可能的结果，复杂度是 [公式] ，指数级。

缺点（第3部分详细讲）
(1) 数据下溢

(2) 倾向于生成短的序列

(3) 单一性问题

Beam size 设置
(1) B越大

优点：可考虑的选择越多，能找到的句子越好

缺点：计算代价更大，速度越慢，内存消耗越大

(2) B越小

优点：计算代价小，速度快，内存占用越小

缺点：可考虑的选择变少，结果没那么好

### 问题解决
##### 数据下溢

解决方法：概率取log值
解释：
求序列概率的时候，序列概率是多个条件概率的乘积，每个概率都小于1甚至远远小于1，很多概率相乘起来，会得到很小很小的数字，会造成数据下溢，即数值太小，计算机的浮点表示不能精确储存。

因此我们不会最大化这个乘积，而是取log值，取log不会影响排序结果，但是在数值上会更稳定，不容易出现数据下溢。

##### 倾向于生成短的序列

解决方法：归一化对数似然函数
最简单的方法，对于概率值相乘（或者对数概率相加）的结果，除以序列长度 [公式] 。

实践中，通常采用更柔和的方法，在[公式]上加上指数[公式]，即 [公式] ，例如[公式]。如果[公式]， [公式] 就相当于完全用长度来归一化；如果[公式]， [公式] 就相当于完全没有归一化，[公式]就是在完全归一化和没有归一化之间。

解释：
生成的句子序列越长，概率值相乘（或者对数概率相加）的结果就越小，所以倾向于生成短序列。对序列长度进行惩罚，降低生成短序列的倾向。

##### 单一性问题

解释：
beam search 有一个大问题是输出的 [公式] 个句子的差异性很小，无法体现语言的多样性（比如文本摘要、机器翻译的生成文本，往往有不止一种表述方式）。

解决方法：分组 加入相似性惩罚。diverse beam search 来自论文
具体如下：选择 Beam size 为 [公式] ，然后将其分为 [公式] 组，每一组就有 [公式] 个beam。每个单独的组内跟 beam search很像，不断延展序列。同时通过引入一个dissimilarity 项来保证组与组之间有差异。

##### 如上图所示，B = 6, G=3，每一组的beam width为2。

组内与 beam search 很像：从t-1到 t 时刻，不断的减少搜索空间（如同beam search一样）。

组间差异：对于t=4时刻，我们先对第一组输出y（t=4），然后我们开始对第二组输出y（t=4），但是第二组y（t=4）的score不仅取决于第二组之前的y（t=3），也取决于其与第一组的相似程度。以此类推，在t=4时刻对于第三组的输出，我们从上图可以看到其score的打分标准。这儿对于其 dissimilarity 项的计算采用的办法是 hamming diversity，这个理解起来很简单，比如这个时刻可能输出的词在上面的组出现过，我们就对这个词的分数-1，如果这个时刻可能输出的词在上面组没有出现过，我们就对这个词的分数不惩罚。

### 其他相关问题：
##### 训练的时候需要 Beam Search 吗？

不需要。因为训练的时候知道每一步的正确答案，没必要进行这样的搜索。

##### 为什么不用贪心搜索？

贪心搜索相当于 Beam Search 中 B=1的情况，每次只选择概率最大的词，容易陷入局部最优，但我们真正需要的是一个序列，我们希望整个序列的概率最大。

##### 维特比算法

维特比算法是用动态规划的思想。简单来说就是：从开始状态之后每走一步，就记录下到达该状态的所有路径的概率最大值，然后以此最大值为基准继续向后推进。显然，如果这个最大值都不能使该状态成为最大似然状态路径上的结点的话，那些小于它的概率值（以及对应的路径）就更没有可能了。

Beam Search与Viterbi算法虽然都是解空间的剪枝算法，但它们的思路是不同的。Beam Search是对状态迁移的路径进行剪枝，而 Viterbi 算法是合并不同路径到达同一状态的概率值，用最大值作为对该状态的充分估计值，从而在后续计算中，忽略历史信息（这种以偏概全也就是所谓的Markov性），以达到剪枝的目的。
从状态转移图的角度来说，Beam Search是空间剪枝，而Viterbi算法是时间剪枝。

### Referece 
https://zhuanlan.zhihu.com/p/43703136