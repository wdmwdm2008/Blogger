NP-Hard和NP-Complete的区别
对NP-Hard问题和NP-Complete问题的一个直观的理解就是指那些很难（很可能是不可能）找到多项式时间算法【MengMa注：多项式时间就是用一个多项式表示的时间】的问题. 因此一般初学算法的人都会问这样一个问题: NP-Hard和NP-Complete有什么不同? 简单的回答是根据定义, 如果所有NP问题都可以多项式归约【MengMa: 归约是使用解决其它问题的"黑盒"来解决另一个问题. 这个技术我们刷LeetCode会用到。
】到问题A, 那么问题A就是NP-Hard; 如果问题A既是NP-Hard又是NP, 那么它就是NP-Complete. 从定义我们很容易看出, NP-Hard问题类包含了NP-Complete类. 但进一步的我们会问, 是否有属于NP-Hard但不属于NP-Complete的问题呢? 答案是肯定的. 例如停机问题, 也即给出一个程序和输入, 判定它的运行是否会终止. 停机问题是不可判的, 那它当然也不是NP问题. 但对于SAT【MengMa: 下文有写啥是SAT】这样的NP-Complete问题, 却可以多项式归约到停机问题. 因为我们可以构造程序A, 该程序对输入的公式穷举其变量的所有赋值, 如果存在赋值使其为真, 则停机, 否则进入无限循环. 这样, 判断公式是否可满足便转化为判断以公式为输入的程序A是否停机. 所以, 停机问题是NP-Hard而不是NP-Complete.
NP:全称Non-deterministic polynomial time，指可以在多项式时间内可验证问题。经常有人误把NP理解为non-polynomial time。
NP 是 Non-deterministic Polynomial 的缩写,NP 问题通俗来说是其解的正确性能够被很容易检查的问题, 这里"很容易检查"指的是存在一个多项式检查算法.

例如, 著名的推销员旅行问题(Travel Saleman Problem or TSP):假设一个推销员需要从香港出发, 经过广州, 北京, 上海,....., 等 n 个城市， 最后返回香港。 任意两个城市之间都有飞机直达， 但票价不等。现在假设公司只给报销 $C 块钱, 问是否存在一个行程安排，使得他能遍历所有城市，而且总的路费小于 $C ?

推销员旅行问题显然是 NP 的. 因为如果你任意给出一个行程安排, 可以很容易算出旅行总开销. 但是, 要想知道一条总路费小于 $C 的行程是否存在, 在最坏情况下, 必须检查所有可能的旅行安排! 这将是个天文数字.

NP-complete 问题是所有 NP 问题中最难的问题. 它的定义是, 如果你可以找到一个解决某个 NP-complete 问题的多项式算法, 那么所有的 NP 问题都将可以很容易地解决. 【类似于：擒贼先擒王？？】

通常证明一个问题 A 是 NP-complete 需要两步, 第一先证明 A 是 NP 的, 即满足容易被检查这个性质; 第二步是构造一个从某个已知的 NP-complete 问题 B 到 A 的多项式变换, 使得如果 B 能够被容易地求解, A 也能被容易地 解决. 这样一来, 我们至少需要知道一个 NP-complete 问题.

第一个 NP complete 问题是 SAT 问题, 由 COOK 在 1971 年证明. SAT 问题指的是, 给定一个包含 n 个布尔变量(只能为真或假) X1, X2, .., Xn 的逻辑析取范式【析取范式见baidu】, 是否存在它们的一个取值组合, 使得该析取范式被满足? 可以用一个具体例子来说明这一问题, 假设你要安排一个 1000 人的晚宴, 每桌 10 人, 共 100 桌. 主人给了你一张纸, 上面写明其中哪些 人因为 江湖恩怨不能坐在同一张桌子上, 问是否存在一个满足所有这些约束条件的晚宴安排? 这个问题显然是 NP 的, 因为如果有人建议一个安排方式, 你可以很容易检查它是否满足所有约束. COOK 证明了这个问题是 NP-complete 的, 即如果你有一个好的方法能解决晚宴安排问题, 那你就能解决所有的 NP 问题.

这听起来很困难, 因为你必须面对所有的 NP 问题, 而且现在你并不知道任何的 NP-complete 问题可以利用.COOK 用非确定性图灵机( Non-deterministic Turing Machine ) 巧妙地解决了这一问题.

正式地, NP 问题是用非确定性图灵机来定义的, 即所有可以被非确定性图灵机在多项式时间内解决的问题. 非确定性图灵机是一个特殊的图灵机, 它的定义抓住了"解容易被检查" 这一特性. 非确定性图灵机有一个"具有魔力的"猜想部件, 只要问题有一个解, 它一定可以猜中. 例如, 只要存在哪怕一个满足约束的晚宴安排方式, 或是一个满足旅行预算的行程安排, 都无法逃过它的法眼, 它可以在瞬间猜中. 在猜出这个解以后,检查确认部分和一台普通的确定性图灵机完全相同,也即是等价于任何一个实际的计算机程序.

COOK 证明了,任意一个非确定性图灵机的计算过程,即先猜想再验证的过程, 都可以被描述成一个 SAT 问题,这个 SAT 问题实际上总结了该非确定性图灵机在计算过程中必须满足的所有约束条件的总和(包括状态转移, 数据读写的方式等等), 这样, 如果你有一个能解决该 SAT 问题的好的算法, 你就可以解决相应的那个非确定性图灵机计算问题, 因为每个 NP 问题都不过是一个非确定性图灵机计算问题, 所以, 如果你可以解决 SAT , 你就可以解决所有 NP 问题. 因此, SAT 是一个 NP-complete 问题.

有了一个 NP-complete 问题, 剩下的就好办了, 我们不用每次都要和非确定性图灵机打交道, 而可以用前面介绍的两步走的方法证明其它的 NP-complete 问题. 迄今为止, 人们已经发现了成千上万的NP-complete 问题, 它们都具有容易被检查的性质, 包括前面介绍的推销员旅行问题. 当然更重要的是, 它们是否也容易被求解, 这就是著名的 P vs NP 的问题.