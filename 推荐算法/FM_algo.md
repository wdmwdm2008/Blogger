# FM algo的优势：

### 全面性
实现一个FM召回，能实现四种召回方式。e.g. u2i （根据用户的偏好，推荐item）,u2u （推荐其它用户喜欢的item）,i2u（在user faiss库中检索可能对它感兴趣的user，把item给这些用户push出去，达到提高用户粘性，减少用户流失的目的）,i2i（连连看-当点击某个物品时，推荐相似物品），还包括对新用户，新物料的冷启动

### 效果好 （记忆与扩展）
wi得到user/item的一阶特征，wij <vi,vj>得到user/item的二阶特征， 一阶特征对应LR的记忆性 （对高频具有很好的记忆性），二阶交互特征对应泛华能力，因为我们对每一个特征得到一个embedding，而embedding的泛化能力很强，能自觉挖掘低频，长尾模式。

### 便于上线
FM的排序，虽然需要做二阶特征交叉，但是其只需要O(kn)的时间复杂度就可以完成预测
召回由于数据量大，所以dnn只能线下计算user embedding，不仅降低了用户的覆盖率，而且对于用户实时兴趣的捕捉大打折扣。FM召回，只需要把一系列的feature embedding进行相加，就可以对任何在线用户生成最新的user embedding.从而基于用户最新的兴趣，从千万量级候选item中完成实时召回。

# FM 精排

### 样本的选择：
正样本为曝光点击的样本  
负样本为曝光未点击的样本，如果是true负,负样本为点击item之前的item

### 特征：
user特征: e.g. id, age, 用户长短期画像，点击/收藏/购买历史 etc
item特征: e.g. id, tag, type，name， 物料的后验指标 etc
user和item交叉特征：age和item type的交叉特征 etc
统计意义上的交叉: eg. 用户携带的tag与物料携带的tag之间的重合度
属性特征最重要，所以可以对离散特征进行分桶转化为ID类特征


### 模型构建
使用FM模型进行计算，其label=1代表点击，label=0代表曝光未点击。
目标函数可用交叉熵来计算
时间复杂度能减少到O(kn)
where k is the dimension of feature, n is the number of features

### 上线：
线上直接计算模型的结果


# FM 召回

### 样本构建：
正样本：曝光点击样本  
负样本：items库里随机抽取  

##### 热度打压
因为大部分正样本都是热卖item，为了也能推荐非热卖产品，所以需要尽可能多的使用非热卖产品作为正样本
原理是热度越高，其作为正样本的概率越低

##### hard negative
因为负样本是从items库里随机抽取，所以其跟用户的匹配很低，所以模型很容易学习出来,达不到锻炼模型的目的。为了增加模型学习的难度，所以需要选取一些匹配度适中，单用户又未点击的item，增加模型在训练时候的难度，让模型能专注在细节，这就是所谓的hard negative.

选取方式：1. 我们可以使用热度高的样本作为负样本。经典方法是使用negative sampling的采样方法来抽取负样本  
2. 使用上一版本的召回模型召回的item作为负样本。

### 特征:
由于item embedding是离线生成，而user embedding是在线生成，所以不能使用user和item之间的交叉特征。

### 模型构建：
使用FM模型进行计算。由于随机从item库中抽取items，所以会有噪声。因此模型算法使用pairwise ltr来进行相对排序。目标函数使用hinge loss来进行计算。


### 上线：
离线计算user和item的embedding
在线直接计算coscine score of between user embedding and item embedding, 然后得到topn个item

# 参考
### embedding的多用途
可计算i2u,i2i,u2u等

### 冷启动和新物料
由于使用的是embedding计算，所以FM对这两个也非常友好

### 全局特征和局部特征
xgboost我们能得到全部样本的特征的重要度，但是fm我们能得到每个样本的特征重要度。
how?

### fm在dnn中有用吗？
有。因为在dnn中认为是重要的特征，在FM中也相应的认为是重要的特征。反之亦然。
例如：有一次我们先验认为某个特征很重要，但是进入fm模型后发现其特征不重要，然后我们检查这个特征的样本，最后发现是在生成这个特征时计算错了，更改后，fm的模型效果相应提升了。
而且这个相应的在dnn模型中也是这样的。所以fm模型能对dnn模型起作用。



# Reference
掌握原理： 美团的《深入FFM原理与实践》
FM用于召回： 《推荐系统召回四模型之：全能的FM模型》
亲手实践：《alphaFM》

