# FM algo的优势：

### 全面性
能对u2i （根据用户的偏好，推荐item）,u2u （推荐其它用户喜欢的item）,i2u（基于）,i2i（连连看-当点击某个物品时，推荐相似物品）都进行推荐

### 效果好
wi得到user/item的一阶特征，wij <vi,vj>得到user/item的二阶特征， 一阶特征对应LR的记忆性，二阶交互特征对应泛华能力，因为我们对每一个特征得到一个embedding，而embedding的泛化能力很强。

### 上线速度快
因为线下直接计算item和user相对应的embedding，所以线上直接使用user embedding和items做点积，得到topn的item

# FM 精排

### 样本的选择：
正样本为曝光点击的样本  
负样本为曝光未点击的样本，如果是true负,负样本为点击item之前的item

### 特征：
user特征: e.g. id, age, user profile etc
item特征: e.g. id, tag, type，name etc
user和item交叉特征：age和item type的交叉特征 etc

### 模型构建
使用FM模型进行计算
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
因为负样本是从items库里随机抽取，所以其跟用户的匹配很低，所以模型很容易学习出来。为了增加模型学习的难度，我们可以使用热度高的样本作为负样本。
经典方法是使用negative sampling的采样方法来抽取负样本

### 特征:
对于召回来说，因为数据量大，所以不使用item和user的交叉特征来计算。

### 模型构建：
使用FM模型进行计算
时间复杂度能减少到O(kn)
where k is the dimension of feature, n is the number of features

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


