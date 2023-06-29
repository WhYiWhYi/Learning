## 1 Multiclass分类策略
Multiclass classification is a classification task **with more than two classes** and each sample can **only be labeled as one class**
### 1.1 One-vs-Rest
One-vs-Rest策略也被称为One-vs-All策略。该策略为每一个class拟合一个分类器。每个分类器将基于class-other class进行拟合。这是最常见的策略
* 投入单个样本进行预测时，A、B、C、D四个class基于class-other class会输出四组结果：A的概率/非A的概率、B的概率/非B的概率、...。如果4个分类器的结果分别：A 80%、B 50%、C 70%、D60%，A的概率相较于其他分类器中的分类概率是最大的，则输出预测结果A

优势
* 计算效率：n个class仅需要构建n个分类器
* 可解释性：由于每一class有且仅由一个分类器代表，因此有可能通过检查其相应的分类器来得到该类别的相关信息
```python
from sklearn.multiclass import OneVsRestClassifier

OneVsRestClassifier(LinearSVC().fit(x, y).predict(x))
```
### 1.2 One-vs-One
One-vs-One策略为每一对class构建一个分类器。在进行预测时预测结果将选择获得投票最多的类别。在两个class投票数相同时，其会选择**具有最高总体分类置信度（将基础二元分类器计算的一堆分类置信度相加）的class**
* 投票：A、B、C、D四个class可以组成6对，构建6个分类器。对单个样本进行预测时，6个分类器分别得到6个预测结果，投票即为取六个预测结果中数量最多的class作为输出
* n个class需要构建n \* (n-1) / 2个分类器，因此模型构建速度要慢于One-vs-Rest策略
* 对于核算法（kernel algorithms）可能是有利的
## 2 Multilabel classification
Multilabel classification (closely related to **multioutput classification**) is a classification task **labeling each sample with m (can be 0 to n) labels from n possible classes**
* 多标签分类可以被认为是预测样本的属性，这些属性之间并非互斥的（可以存在相互关联）
* 从形式上看，多标签分类对每个样本的每个class都分配一个二进制输出。正面类别使用1表示，负面类别使用0或-1表示，这与二元分类任务相似，如使用MultiOutputClassifier。但后者为独立处理每个标签，而多标签分类器则是同时处理多个类，且可同时考虑它们之间的相关性
### 2.1 Target format
多标签分类中的 ```y``` 的有效表示是一个密集或稀疏的**二进制矩阵**，其形状为 (n_samples, n_classes)
```python
# 密集矩阵示例：每一列代表一个类别，每一行中的1代表该样本在该类别下被标记的正类，0代表反类
# 密集矩阵可以通过MultiLabelBinarizer创建
y = [[1, 0, 0, 1],
	 [0, 0, 1, 1],
	 [0, 0, 0, 0]]
```
### 2.2 MultiOutputClassifier
多标签分类可通过 ```MultiOutputClassifier``` 添加至任意的分类器。这一策略目的在于**扩展估计器**，其可以为每一个预测结果y匹配一个估计器，使若干个估计不同目标函数的估计器（f1, f2, f3, ...）在同一个特征集合X上训练，并分别预测一系列输出结果(y1, y2, y3, ...)
### 2.3 ClassifierChain
Classifier chains: a way of combining **a number of binary classifiers** into a single multi-label model that is capable of **exploiting correlations among targets**
* 在一个多标签分类模型中纳入多个二元分类器，以探索不同标签之间的联系

对于一个具有n个class的多标签分类问题，n个二元分类器被被分配到一个0 ~ n-1之间的整数（这些整数定义了classifier chain中模型的顺序）
* 拟合：第m个分类器将在可用的训练数据上进行拟合，拟合的特征还包括前面m-1个分类器的**真实标签信息**
* 预测：进行预测时，第m个分类器之前的m-1个分类器的真实标签信息不可用，相反，每个模型的**预测结果**将被传递给链中的后续模型
* 显然，链的顺序十分重要。链中的第一个模型没有其他标签的信息，而链中的最后一个模型则包括此前所有其他模型预测标签作为特征。一般来说，无法得知链中模型的最佳顺序，所以通常会对许多随机排列的链进行拟合，并将它们的预测值平均到一起

```python
from sklearn.datasets import make_multilabel_classification
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.multioutput import ClassifierChain

x, y = make_multilabel_classification(n_samples=12, n_classes=3, random_state=0)
x_train, x_test, y_train, y_test = train_test_split(x, y, random_state=0)

base_lr = LogisticRegression(solver='lbfgs', random_state=0)  # 估计器
chain = ClassifierChain(base_lr, order='random', random_state=0)  # 设置链

chain.fit(x_train, y_train).predict(x_test)  # 进行结果预测
>>> array([[1., 1., 0.],
           [1., 0., 0.],
           [0., 1., 0.]])
chain.predict_proba(X_test)  # 结果预测可能性
>>> array([[0.8387..., 0.9431..., 0.4576...],
           [0.8878..., 0.3684..., 0.2640...],
           [0.0321..., 0.9935..., 0.0625...]])
```
## 3 Multiclass-multioutput classification
Multiclass-multioutput classification (multitask classification): a classification task which **labels each sample with a set of non-binary properties**
* 多分类多输出分类（多任务分类）中，每个样本都具有一个非二元的属性集合
* 属性的数目，以及各属性的类别数目均大于2
* 一个估计器可以处理若干个联合分类任务
* This is both a generalization of the multilabel classification task, which only considers binary attributes, as well as a generalization of the multiclass classification task, where only one property is considered（意思是Multiclass-multioutput classification是multilabel和multiclass的升级版？）
* 所有能用于多分类多输出分类（多任务分类）的分类器，都支持多标签分类任务（多分类多输出分类的一个特殊情况）

如，对一组水果图像的“水果类型”和“颜色”属性进行分类。“水果类型”的属性有几个可能的类别：“苹果”、“梨”和“橙”。属性“颜色”的类别为：“绿色”、“红色”、“黄色”和“橙色”。每个样本都是一个水果的图像，“水果类型”和“颜色”属性各一个标签输出，每个标签都是相应属性的可能类别之一
```python 
from sklearn.datasets import make_classification
from sklearn.multioutput import MultiOutputClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.utils import shuffle
import numpy as np

x, y1 = make_classification(n_samples=10, n_features=100, n_informative=30, n_classes=3, random_state=1)

y2 = shuffle(y1, random_state=1)
y3 = shuffle(y1, random_state=2)
y = np.vstack((y1, y2, y3)).T  # 按竖直方向堆积（增加行数）后转置，人为创造multi-label
>>> [[2 2 0]
     [1 2 1]
     [2 1 0]
     ...]

n_samples, n_features = x.shape # 样本数、特征数：10、100
n_outputs = y.shape[1] # y的列数（properties数量）
n_classes = 3  # 原始y1的分类有三种：0、1、2，现在为每一类label的class都为3种

forest = RandomForestClassifier(random_state=1)
multi_target_forest = MultiOutputClassifier(forest, n_jobs=2)
multi_target_forest.fit(x, y).predict(x)
>>> array([[2, 2, 0],
           [1, 2, 1],
           [2, 1, 0],
           ...])
```
### 2.1 Target format
多分类多输出分类中的 ```y``` 的有效表示是一个密集的矩阵，其形状为 (n_samples, n_classes)，是一种一维多class变量的列式连接
```python
# 密集矩阵示例：
y = [['apple' 'green']
     ['orange' 'orange']
     ['pear' 'green']]
```

