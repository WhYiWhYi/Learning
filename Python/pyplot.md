## matplotlib画图层次说明
Figure: ```fig = plt.figure()```，画布，也可以理解为最原始的画图底板
Axes: ```ax = fig.add_subpolt(1,1,1)```，Axes对象，可简单理解为子图（如果Figure只有一张图，那就只有一个Axes，如果Figure有多张图，那就有多个Axes），也可以理解为要放到画布上的各种内容的集合（包括绘图用数据、标题、坐标轴及刻度线、图例等等）。**强烈建议使用ax进行绘图操作而不是直接使用plt**
Axis：```ax.xaxis/ax.yaxis```，坐标轴。需要注意的是每一个坐标轴也是由竖线和数字组成的，其中的组分也是一个subplot。因此 ```ax.xaxis``` 也存在axes对象，修改这个axes对象会修改xaxis图像的表现

实战绘画一次（示例）
```python
import numpy as np
import matplotlib.pyplot as plt

# 创建数据
A = np.arange(1, 5)
B = A ** 2
C = A ** 3

# 创建一个指定大小的画布，此处只包含一个axes
# fig是画布，所有的fig.xxx是对画布操作
# ax是画布上的axes，所有的ax.xxx是对该ax操作。如果创建的ax不止一个（nrows和ncols不为1），则返回的ax是一个包含若干axes的列表
# 可通过索引选取指定的axes（如ax[0]）
# 也可直接指定创建变量名（如fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2)
fig, ax = plt.subplots(figsize=(14, 7))

# 画图（线性图）
ax.plot(A, B)  # 在axes上画图，调用ax.xxx。A相当于x轴坐标，B相当于y轴坐标
ax.plot(B, A)

# 细节处理（标题、x轴、y轴名称）
ax.set_title('Title', fontsize=18)  # 设置标题
ax.set_xlabel('x_label', fontsize=18, fontfamilyy='sans-serif', fontstyle='italic')
ax.set_ylabel('y_label', fontsize='x-large', fontstyle='oblique')
ax.legend()
```
## matplotlib.pyplot.subplots创建Axes
用于创建一张图/画布（Figure），其中包含多幅子图（Axes）

官网的链接下方有很多示例代码：https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplots.html
```python
matplotlib.pyplot.subplots(nrows=1, ncols=1, *,
						   sharex=False, sharey=False,
						   squeeze=True,
						   width_ratios=None, height_ratios=None,
						   subplot_kw=None,
						   gidspec_kw=None,
						   **fig_kw)
```
* parameters
	* nrows, ncols (int, **default: 1**): 设置子图网格（subplot grid）的行列数
	* sharex, sharey (bool or {'none', 'all', 'row', 'col'}, **default: False**): 设置不同子图的x轴或y轴上是否共享相同的属性设置
	* squeeze (bool, **default: True**): 如果为True，返回的轴数组中增加额外的维度。N×M的图形在N>1和M>1的时候返回的是2D数组
	* width_ratios, height_ratios (array-like of length ncols/nrows, optional): 定义每一列/每一行的相对宽度，如果未设置，每一列/每一行将具有相同的宽度/高度。参数可通过gridspec_kw字典传递
	* subplot_kw (dict, optional): 可以通过该字典向subplots里传递add_subplot的参数
	* gridspec_kw (dict, optional): 可以通过该字典向subplots里传递GridSpec（用于确定figure中放置子图的位置）的参数
	* \*\*fig_kw: 可以通过此向subplots里传递pyplot.figure的参数（matplotlib.pyplot.figure(num=None, figsize=None, dpi=None, *, facecolor=None, edgecolor=None, frameon=True, FigureClass=<class 'matplotlib.figure.Figure'>, clear=False, **kwargs)）
		* num(int or str, optional): figure的唯一标识符，int将指向Figure.number属性，str将返回图的标签
		* figsize ((float, float), **default: (6.4, 4.8)**): 图像的宽、高（英寸）
		* dpi (float, **default: 100**): 图像的DPI
		* facecolor (color, default: 'white'): 背景色
		* edgecolor (color, default: 'white'): 边框色
		* frameon (bool, default: True): 边框
* return
	* fig: 即Figure对象
	* ax: 返回单个Axes对象（子图），或多个Axes对象的数组
## Axes说明及使用链接速查
通常通过```plt.subplots()``` 创建Axes对象（实例）
* 创建得到的对象（实例）类型为 ```<class ;matplotlib.axes._axes.Axes>```，也即 ```matplotlib.axes.Axes``` 对象（关于该类型对象的说明：https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.html）

使用Axes对象画图时可用功能如下所示（见https://matplotlib.org/stable/api/axes_api.html#axis-limits中的Table of Contents）
* Plotting（画图）
	* Basic（基础图形）
	* Spans
	* Spectral
	* Statistics
	* Binned
	* Contours
	* 2D arrays
	* Unstructured triangles
	* Text and annotations
	* Vector fields
* Clearing（Axes元素清除）
* Appearance（外观）
* Axis/Limits（坐标轴设定）
	* Axis limits and direction
	* Axis labels, title, and legend
	* Axis scales
	* Autoscaling and margins
	* Aspect ratio
	* Tick and tick labels
* Units（坐标轴单位设置）
* Adding artists（在Axes中添加Atrists）
* Twinning and sharing（同x/y坐标，多y/x坐标）
* Axes position
* Async/event based
* Interactive
* Children
* Drawing
* Projection
* Other
## 一些常用参数的设置
### 颜色（均为按顺序排列）
#### 2色
```python
c2 = [['#E3727A', '#3186C9'],
      ['#5A3B80', '#69AC4A'],
      ['#FF8347', '#7053AB'],
      ['#3B9E46', '#FF952F'],
      ['#58CFAA', '#A1CB4E'],
      ['#C78177', '#328785'],
      []]
```
#### 3色
```python
c3 = [['#A43736', '#FFBF3D', '#4192D0'],
      ['#902C2F', '#2F6C97', '#639F83'],
      ['#BB5E63', '#69B065', '#5A5F9E'],
      ['#F79437', '#3990DB', '#895AB5'],
      ['#A62B3E', '#FFD734', '#295CB9'],
      ['#9852A6', '#FF5643', '#2E61BC'],
      []]
```
#### 4色
```python
c4 = [['#000000', '#122043', '#FFAB03', '#F5F5F5'],
	  ['#F47E60', '#3E4161', '#87BEA2', '#FFD589'],
	  ['#234859', '#25A798', '#FFA95F', '#FD7251'],
	  ['#20A8CD', '#00304D', '#FFC100', '#FF8900'],
	  ['#76C6E8', '#003257', '#FF4042', '#D6192E'],
	  ['#F32E1F', '#FF925A', '#95CAF4', '#4B7BC4'],
	  ['#880000', '#D80E1E', '#6AA4CD', '#002F4E'],
	  ['#D4D3D5', '#DB8BF4', '#669EF2', '#00007F'],
	  ['#000000', '#9852A6', '#FF5643', '#2E61BC'],
	  ['#33A073', '#FF9946', '#5F9EE4', '#DC6F99'],
	  []]
```
#### 5色
```python
c5 = [['#234859', '#25A798', '#FDD069', '#FFA95F', '#FD7251'],
      ['#403990'，'#80A6E2', '#FBDD85', '#F46F43', '#CF3D3E'],
      ['#80AB1C', '#405335', '#99B69B', '#92E4CE', '#72C8B7'],
      ['#2779AC', '#F2811D', '#349D35', '#C72F2F', '#8E6FAD'],
      ['#B03D26', '#E0897E', '#005F81', '#9CCFE6', '#A5A7AB'],
      ['#FF318A', '#D07AAC', '#E9CE34', '#4283D5', '#5CCEA0'],
      ['#FFA980', '#FFCF54', '#7CAA3C', '#326607', '#F5EEB4'],
      ['#68B7C4', '#FFF0E4', '#F9ADCA', '#FFE4EF', '#BAB4C1'],
      []]
```
#### 6色
```python
c6 = [['#492841', '#437B82', '#C4C0AA', '#FFE2C0', '#F98A5D', '#F46E4C'],
      ['#914024', '#EF4F30', '#FFCB69', '#FEEACE', '#93C1A5', '#4BB697'],
      ['#F32E1F', '#FF925A', '#FFED95', '#F5FFFF', '#95CAF4', '#4B7BC4'],
      ['#237575', '#22A296', '#8BB57C', '#F4CA66', '#FAA65D', '#F76F4F'],
      ['#000000', '#3B0468', '#7F1673', '#C6314F', '#EF631B', '#ECB00A'],
      ['#B9004B', '#FF7242', '#FFE785', '#F8FFA4', '#81D9AC', '#4B6AC1'],
      ['#70C17F', '#C4DFA2', '#F7AE55', '#F9E9AB', '#7CA9CC', '#B7E1E9'],
      ['#F89538', '#4278B5', '#2F8A94', '#384E8F', '#6A7F8E', '#7E3C82'],
      ['#68B7C4', '#FFF0E4', '#F9ADCA', '#FFE4EF', '#BAB4C1', '#CBCCF2'],
      []]
```
#### 7色
```python
c7 = [['#93D6F8', '#20A8CD', '#0A6D90', '#00304D', '#FFC100', '#FFA500', '#FF8900'],
      ['#492841', '#437B82', '#C4C0AA', '#FFE2C0', '#F98A5D', '#F46E4C', '#EC4D38'],
      ['#224657', '#237575', '#22A296', '#8BB57C', '#F4CA66', '#FAA65D', '#F76F4F'],
      ['#4B0063', '#423E93', '#2D6C9A', '#189B96', '#2DC47B', '#96E439', '#FFF610'],
      []]
```
#### 8色
```python
c8 = [['#00B150', '#C5E0B5', '#33561C', '#FEFE00', '#606060', '#EE7E2E', '#9F4500', '#1C5E93'],
      []]
```
#### 8色++
```python
c12 = [['#51C4C2', '#0D8A8C', '#4583B3', '#F78E26', '#F172AD', '#F7AFB9', '#C63596', '#BE86BA', '#8B66B8', '#4068B2', '#512A93', '#223271'],
       []]
```
### 字体









