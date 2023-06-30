## 大纲
1. 相关定义：学习任务、三种策略
2. 评价指标：基于样本的评价指标、基于标签的评价指标
3. 学习算法：基于问题转化的算法（4个）、基于算法改进的算法（4个）
4. 相关任务：多实例学习、有序分类、多任务学习、数据流学习
## 1 相关定义
1. 学习任务

	$X = \mathbb{R}^d$ 是 $d$ 维的输入空间，$Y = \{y_1, y_2, y_3, ...y_q\}$ 是带有 $q$ 个可能标签的标签空间。训练集 $D = \{(x^i, y^i)| 1 \leq i \leq m\}$ ，$m$ 表示训练集大小，$i$ 表示样本序数。$x^i \in X$ 是一个 $d$ 维的向量，$y^i \subseteq Y$ 是 $Y$ 的一个标签子集
	
	学习任务就是要学习一个多标签分类器 $h(\cdot)$，预测 $h(x) \subseteq Y$ 作为 $x$ 的正确标签集。常见的做法是学习一个衡量 $x$ 和 $y$ 相关性的函数 $f(x, y_j)$，希望 $f(x, y_{j1}) > f(x, y_{j2})$，其中 $y_{j1} \in y, y_{j2} \notin y$。$h(x)$ 可以由 $f(x)$ 衍生得到，$h(x) = \{y_j | f(x, y_j) > t(x), y_j \in y\}$。$t(x)$ 在式中扮演阈值函数的角色，用于把标签空间对分成相关的标签集和不相关的标签集。阈值函数可以由训练集产生，也可设置为常数。当 $f(x,y_j)$ 返回的是一个概率值时，阈值函数可设为常数0.5
2. 三种策略

	多标签学习的主要难点在于**输出空间的爆炸增长**，如20个标签的输出空间就有 2^20^，为了应对指数复杂度的标签空间，需要挖掘标签之间的相关性。如一个图像被标注的标签由热带雨林和足球，那么它具有巴西标签的可能性就很高。**如何有效挖掘标签之间的相关性**，是多标签学习成功的关键。根据对相关性挖掘的强弱，可以把多标签算法分为三类
	* 一阶策略（first-order strategy）：忽略标签之间的相关性，比如把多标签分解成多个独立的二分类问题（简单高效）
	* 二阶策略（second-order strategy）：考虑标签之间的成对关联，比如为相关标签和不相关标签排序
	* 高阶策略（high-order strategy）：考虑多个标签之间的关联，比如对每个标签考虑所有其他标签的影响（效果最优）
## 2 评价指标
### 2.1 基于样本的评价指标
先对单个样本评估表现，然后对多个样本取平均值
* 分类任务：Subset Accuracy、Hamming Loss、Accuracy~exam~、Precision~exam~、Recall~exam~、F^β^ ~exam~
	
	1. Subset Accuracy：衡量正确率。预测的样本集和真实的样本集完全一样才算正确，**值越高越好**。其中 $p$ 表示测试集样本数，$1\{...\}$ 表示括号内条件为真时返回1，否则返回0
	$$
	\frac 1 p \sum_{i=1}^p 1 \{h(x^i)=y^i\}
	$$
	2. Hamming Loss：衡量错分的标签比例，正确标签未被预测、以及错误标签被预测的标签占比，**值越小越好**。其中 $\Delta$ 表示两个集合的对称差，返回只在其中一个集合中出现的那些值
	$$
	\frac 1 p \sum_{i=1}^p \frac 1 q |h(x^i) \Delta y^i|
	$$
	3. Accuracy、Precision、Recall、F值：单标签学习中的评估指标扩展，**值越大越好**
	$$
	Accuracy(h) = \frac 1 p \sum_{i=1}^p \frac {|h(x^i) \bigcap y^i|}{|h(x^i) \bigcup y^i|}
	$$
	$$
	Precision(h) = \frac 1 p \sum_{i=1}^p \frac {|h(x^i) \bigcap y^i|}{|h(x^i)|}
	$$
	$$
	Recall(h) = \frac 1 p \sum_{i=1}^p \frac {|h(x^i) \bigcap y^i|}{|y^i|}
	$$
	$$
	F^\beta (h) = \frac {(1+ \beta^2) \cdot Precision(h) \cdot Recall(h)}{\beta^2 \cdot Precision(h) \cdot Recall(h)}
	$$
* 排序任务：One-error、Coverage、Ranking Loss、Average Precision
	
	1. One-error：衡量预测到的最相关标签不在真实标签中的样本占比，**值越小越好**
	$$
	OneError(f) = \frac 1 p \sum_{i=1}^p 1 \{[arg \ max_{y_j \in Y} f(x^i, y_j)] \notin y^i\}
	$$
	2. Coverage：衡量排序好的标签列表平均需要移动多少步才能覆盖真实的相关标签集

### 2.2 基于标签的评价指标
先考虑单个标签在所有样本上的表现，然后对多个标签取平均值
* 分类任务：B~macro~、B~micro~（macro/micro-averaging）（$B \in \{Accuracy, Precision, Recall、F^β\}$ ）
* 排序任务：AUC~macro~、AUC~micro~



