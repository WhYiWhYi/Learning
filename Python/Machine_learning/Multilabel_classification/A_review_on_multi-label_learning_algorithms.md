## 大纲
笔记来源：https://www.cnblogs.com/liaohuiqiang/p/9339996.html

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
	```python
	sklearn.metrics.accuracy_score(y_true, y_pred)
	```
	2. Hamming Loss：衡量错分的标签比例，正确标签未被预测、以及错误标签被预测的标签占比，**值越小越好**。其中 $\Delta$ 表示两个集合的对称差，返回只在其中一个集合中出现的那些值
	$$
	\frac 1 p \sum_{i=1}^p \frac 1 q |h(x^i) \Delta y^i|
	$$
	```python
	sklearn.metrics.hamming_loss(y_true, y_pred)
	```
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
	2. Coverage：依据排序，衡量排序好的标签列表平均需要移动多少步（需要多少长度）才能覆盖所有的真实的相关标签集。
	$$
	Coverage(f) = \frac 1 p \sum_{i=1}^p max_{y_j \in y^i}\ rank_f{(x^i, y_j)-1}
	$$
	```python
	sklearn.metrics.coverage_error(y_true, y_score)
	# y_score是对每个label预测的confidence，一般即为分类函数的输出（y_pred）
	```
	3. Ranking Loss：依据排序，衡量反序标签对（不相关标签比相关标签的相关性大）的占比。其中 $\overline {y_i}$ 为 $y^i$ 在 $Y$ 上的补集，$y_{j1}$ 从相关标签集 $y^i$ 中取，而 $y_{j2}$ 从不相关标签集 $\overline {y_i}$ 中取，两两组合形成标签对
	$$
	rloss(f) = \frac 1 p \sum_{i=1}^p \frac 1 {|y^i||\overline {y_i}|} |\{(y_{j1},y_{j2})|f(x^i, y_{j1})\leq f(x^i, y_{j2}),(y_{j1}, y_{j2})\in(y^i \times \overline {y^i})\}|
	$$
	```python
	sklearn.metrics.label_ranking_loss(y_true, y_score)
	```
	4. Average Precision：衡量比特定标签更相关的那些标签的排名的占比
	$$
	avgprec(f) = \frac 1 p \sum_{i=1}^p \frac 1 {|y^i|} \sum_{y_{j1}\in{y_i}} \frac {|\{y_{j2}|rank_f(x^i,y_{j2})\leq rank_f(x^i,y_{j1}),y_{j2}\in y^i\}|}{rank_f(x^i, y_{j1})}
	$$
	```python
	sklearn.metircs.label_ranking_average_precision_score(y_true,y_score)
	```
### 2.2 基于标签的评价指标
先考虑单个标签在所有样本上的表现，然后对多个标签取平均值
* 分类任务：B~macro~、B~micro~（macro/micro-averaging）（$B \in \{Accuracy, Precision, Recall、F^\beta\}$ ）
	1. TP、FP、TN、FN（对于第 $j$ 个label）
		* TP：真label被预测为真
		$$
		TP_j = |\{x_i|y_j \in Y_i \wedge y_j \in h(x_i),1 \leq i \leq m\}|
		$$
	
		* FP：假label被预测为真
		$$
		FP_j = |\{x_i|y_j \notin Y_i \wedge y_j \in h(x_i),1 \leq i \leq m\}|
		$$
		* TN：假label被预测为假
		$$
		TN_j = |\{x_i|y_j \notin Y_i \wedge y_j \notin h(x_i),1 \leq i \leq m\}|
		$$
		* FN：真label被预测为假
		$$
		FN_j = |\{x_i|y_j \in Y_i \wedge y_j \notin h(x_i),1 \leq i \leq m\}|
		$$
	2. Accuracy、Precision、Recall、F~β~（对于第 $j$ 个label）：也即常规指标
	$$
	Accuracy_j = \frac {TP_j+TN_j}{TP_j+FP_j+TN_j+FN_j}
	$$
	$$
	Precision_j = \frac {TP_j}{TP_j+FP_j}
	$$
	$$
	Recall_j = \frac {TP_j}{TP_j+FN_j}
	$$
	$$
	F^\beta = \frac {(1+\beta^2)\cdot TP_j}{(1+\beta^2)\cdot TP_j+\beta^2\cdot FN_j+FP_j}
	$$
	```python
	sklearn.metrics.fbeta_score(y_true, y_pred, beta, average='binary')
	sklearn.metrics.f1_score(y_true, y_pred, average='binary')
	# average参数可选'micro'、'macro'
	```
	3. Macro-averaging：**每一个class具有相同的权重**（更注重样本量更少的class时可选择macro，各class的样本量差不多，则macro和micro无太大差异）。先对单个标签下的数量特征计算得到常规指标（2.中的Accuracy等），再对多个标签取平均
	$$
	B_{macro}(h) = \frac 1 q \sum_{j=1}^qB(TP_j,FP_j,TN_j,FN_j)
	$$
	4. Micro-averaging：**每一个样本具有相同的权重，样本量更多的class对计算的影响更大**（注重样本量更多的class时选择micro，各class的样本量差不多，则macro和micro无太大差异）。先对多个标签下的数量特征取平均，再根据数量特征计算得到常规指标
	$$
	B_{micro}(h) = B(\sum_{j=1}^qTP_j, \sum_{j=1}^qFP_j,\sum_{j=1}^qTN_j,\sum_{j=1}^qFN_j)
	$$
* 排序任务：AUC~macro~、AUC~micro~
	
	1. AUC~macro~：依据排序，衡量排序正确（指依据 $$f(\cdot,\cdot)$$ 函数，对于相关标签的打分大于不相关标签的打分）的数据对的占比。同理，先对单个标签下进行计算，再平均。$$Z_j = \{x^i|y_j \in y^i,1\leq i\leq p\}$$ 表示含有 $$y_j$$ 标签的样本数量，$$\overline{Z_j} = \{x^i|y_j \notin y^i,1\leq i\leq p\}$$ 则是不含 $y_j$ 标签的样本数量
	$$
	AUC_macro = \frac 1 q \sum_{j=1}^q \frac {|\{(x',x'')|f(x',y_j)\geq f(x'',y_j),(x',x'')\in Z_j\times \overline Z_j\}|}{|Z_j||\overline {Z_j}|}
	$$
	2. AUC~micro~：依据排序，衡量排序正确的数据对的占比，直接把多个标签考虑在内进行计算。$S^+ = \{(x^i,y_j)|y_j\in y^i,1 \leq i \leq p\}$ 表示相关的样本标签对，$S^- = \{(x^i,y_j)|y_j\notin y^i,1 \leq i \leq p\}$ 表示的时不相关的样本标签对
	
	$$
	AUC_micro = \frac {|\{(x',x'',y',y'')|f(x',y')\geq f(x'',y''),(x',y')\in S^+,(x'',y'')\in S^-\}|}{|S^+||S^-|}
	$$
## 3学习算法
### 3.1 基于问题转化的算法
将多标签问题转化为其他学习场景，比如转为二分类、标签排序、多分类
1. Binary Relevance：**一阶策略**。将多个标签分离：对于 $q$ 个标签，建立 $q$ 个数据集和 $q$ 个二分类器进行预测，是最简单最直接的方法
2. Classifier Chains：**高阶策略**。在训练阶段，首先按用户自定义顺序对 $q$ 个标签排序，对于每一个标签都构建一个二分类数据集。对于第 $j$ 个标签构建的数据集中，$x^i$ 会包含前 $j-1$ 个标签值，以这样chain的关系构建 $q$ 个数据集，训练 $q$ 个分类器。测试阶段中，$q$ 个分类器将被顺序调用，数据集 $1$ ~ $j$ 对应的二分类器所预测的结果将会作为数据集 $j+1$的数据集组成
	* 该算法实际也通过用户自定义排序的方式考虑了多个标签之间的联系
	* 算法的好坏受到标签顺序的影响。可以使用集成的方式，使用多个随机序列，对每个随机序列使用一部分数据集进行训练
	* 缺失了平行计算的机会，因为其需要链式调用进行预测
3. Calibrated Label Ranking：**二阶策略**。将多标签学习问题转化为**标签排序问题**，通过成对比较来实现标签间的排序。对 $q$ 个标签可以构建 $q(q-1)/2$ 个标签对，因此可构成 $q(q-1)/2$ 个数据集。带有不同相关性的标签对：$y_j$ 和 $y_k$ 的样本才会纳入数据集 $D_{jk}$ 中（注意到此处每个样本 $x^i$ 会被包含在 $|y^i||\overline {y^i}|$ 个分类器中），基于该数据集训练一个分类器，对样本 $x^i$ 属于标签 $y_j$ 还是 $y_k$ 进行判断和投票。在预测阶段，每个样本和某个标签会产生一系列投票，根据投票行为来进行最终预测
	* 前面的二分类器方法使用One-vs-Rest的方式，本算法使用的是One-vs-One的方式，可缓和类间不均衡的问题
	* 算法复杂性较高，构建分类器的数量（$q(q-1)/2$）表现为二次增长
4. Random k-labelsets：**高阶策略**。其基本思想为将 $2^q$ 个可能的标签集映射成 $2^q$ 个自然数，从而将多标签学习问题转化为多分类问题。为了克服LP算法的局限性，本算法使用的LP分类器仅训练 $Y$ 中一个长度为 $k$ 的随机子集以收缩样本空间，然后集成大量（n个）的LP分类器来预测。最后进行预测时需计算两个指标：标签 $j$ 能达到的最大投票数和实际投票数，预测时以0.5为阈值进行预测，得到标签集
	* 训练一个多分类模型，最后依据输出的自然数映射回标签集的算法称为LP（Label Powerset）算法，其具有两个主要局限性：LP预测的标签集是训练集中已经出现的，其无法泛化到未见过的标签集；产生的类别数太多，模型效能太低
### 3.2 基于算法改进的算法
1. ML-kNN：多标签k-NN，**一阶策略**。通过计算先验概率：样本 $x$ 带有或不带有标签 $y_j$ 的概率，以及两个条件概率值：样本 $x$ 带有或不带有标签 $y_j$ 的条件下，它有 $C_j$个邻居带有标签 $y_j$ 的概率（详细计算及推导过程见原文），可依据贝叶斯规则和后验概率最大化，计算出样本的标签集
	* 此算法并非k-NN和独立二分类的简单结合，其中还运用了贝叶斯来推理邻居信息
2. ML-DT：多标签决策树，**一阶策略**。使用决策树的思想来处理多标签数据。在数据集 $D$ 中使用第 $i$ 个特征，设置划分值为 $\upsilon$，计算出信息增益。通过递归构建一颗决策树，每次选取特征和划分值，使信息增益最大。在进行预测时，向下遍历决策树的结点，找到叶子结点，若 $p_j$ 大于0.5则表示含有标签 $y_j$
	* 此算法并非决策树和独立二分类的简单结合
3. Rank-SVM：**二阶策略**。使用最大间隔思想来处理多标签数据，其考虑了系统对相关标签和不相关标签的排序能力（详细计算及推导过程见原文）
	* 整体和SVM模型原理类似，通过最小化样本 $x_i$ 到每一个“相关-不相关”标签对的超平面的距离来得到间隔。其余相似的地方还有对 $w$ 和 $b$ 进行缩放变换、引入松弛变量、使用核技巧来解决非线性问题等
4. CML：**二阶策略**。使用 $(x,y)$ 表示任意一个多标签样本，其中 $y = (y_1, y_2,...,y_q) \in \{-1,+1\}^q$。算法的任务等价于学习一个联合概率分布 $p(x,y)$，用 $H_p(x,y)$ 表示给定概率分布 $p$ 时 $(x,y)$ 的信息熵。基于最大熵原则，认为熵最大的模型即为最好的模型
## 4 相关任务
1. 多实例学习（Multi-instance learning）：每个样本由多个实例和一个标签组成，多个实例中至少一个为正，则认为该样本为正。与多标签学习的输出空间模糊相反，多实例学习是输入空间模糊
2. 有序分类（Multi-task learning）：对于每个标签，不再是简单地判断是/否，而是改成一系列的等级排序。即将 $y_j = \{-1,+1\}$ 替换为 $y_j = \{m_1, m_2,...m_k\}$，其中 $m_1 < m_2 <...<m_k$
3. 多任务学习（Multi-task learning）：同时训练多个任务，相关任务之间的训练信息会帮助其他任务。如目标定位既要识别有没有目标（分类问题），又要定位出目标的位置（回归问题）
4. 数据流学习（Data streams classificaion）：真实世界的目标是在线生成和实时产生的，如何处理这些数据就是数据流学习要做的事。一个关键的挑战就是“概念漂移”（目标变量的统计特性随着时间的推移以不可预见的方式变化），一般处理方式有：当一大批新数据到来时更新分类器；维持一个检测器来警惕概念漂移；假定过去数据的影响会随着时间而衰减等