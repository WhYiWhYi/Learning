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
ax.set_xlabel('x_label', fontsize=18, fontfamily='sans-serif', fontstyle='italic')
ax.set_ylabel('y_label', fontsize='x-large', fontstyle='oblique')
ax.legend()

# 文件储存
plt.savefig()
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
**因为图的种类和对应的参数都太多，因此只列出部分常用图类型的链接速查，常用图会稍微进行一些详细说明**

通常通过```plt.subplots()``` 创建Axes对象（实例）

* 创建得到的对象（实例）类型为 ```<class ;matplotlib.axes._axes.Axes>```，也即 ```matplotlib.axes.Axes``` 对象（关于该类型对象的说明：https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes）

使用Axes对象画图时可用功能如下所示（见https://matplotlib.org/stable/api/axes_api.html#axis-limits中的Table of Contents）
* Plotting（画图）
	* Basic（基础图形）
		* ```.plot```: 线性图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.plot.html#matplotlib.axes.Axes.plot
			
			* Format Strings
				```python
				fmt = '[marker][line][color]'
				
				'-g'  # 绿色实线
				'^k:'  # 带有上三角标记的黑色点线
				```
			```python
			ax.plot(x, y ,color='', marker='o', markersize=10, linestyle='-', linewidth=2)
			```
		* ```.errorbar```: 带有误差线的线性图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.errorbar.html#matplotlib.axes.Axes.errorbar
		* ```.scatter```: 散点图（+气泡图）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.scatter.html#matplotlib.axes.Axes.scatter
			```python
			ax.scatter(x, y, s=size, c=colors, marker='o', alpha=0.5)
			```
		* ```.step```: 阶梯图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.step.html#matplotlib.axes.Axes.step
		* ```.fill_between```: 填充两条水平曲线间的区域。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.fill_between
		* ```.fill_betweenx```: 填充两条垂直曲线间的区域。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.fill_betweenx.html#matplotlib.axes.Axes.fill_betweenx
		* ```.bar```: 柱形图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.bar
			```python
			ax.bar(x, y, label=label, color=color)  # x是x轴（标签），y是数据
			```
		* ```.barh```: 水平柱形图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.barh.html
		* ```.broken_barh```: 悬浮柱状图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.broken_barh.html#matplotlib.axes.Axes.broken_barh
		* ```.bar_label```: 带标签的柱形图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.bar_label.html#matplotlib.axes.Axes.bar_label
		* ```.stem```: 茎叶图（从基线到y坐标的垂直线并在顶端放一个标记，类似柱形图）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.stem.html#matplotlib.axes.Axes.stem
		* ```.pie```: 饼图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.pie.html#matplotlib.axes.Axes.pie
		* ```.stackplot```: 堆叠图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.stackplot.html#matplotlib.axes.Axes.stackplot
		* ```.vlines```: 垂直线图（在每一个x处，绘制由ymin-ymax的垂直线，类似柱形图）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.vlines.html#matplotlib.axes.Axes.vlines
		* ```.hlines```: 水平线图（在每一个y处，绘制由xmin-xmax的水平线，类似水平柱形图）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.hlines.html#matplotlib.axes.Axes.hlines
		* ```.fill```: 填充图（填充多边形）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.fill.html#matplotlib.axes.Axes.fill
	* Spans（在Axes中添加线或区域，其中添加区域类似于.fill）
		
		可以参考这个示例：https://matplotlib.org/stable/gallery/subplots_axes_and_figures/axhspan_demo.html#sphx-glr-gallery-subplots-axes-and-figures-axhspan-demo-py
		* ```.axhline```: 添加水平线。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.axhline.html#matplotlib.axes.Axes.axhline
		* ```.axhspan```: 添加水平区域。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.axhline.html#matplotlib.axes.Axes.axhspan
		* ```.axvline```: 添加垂直线。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.axvline.html#matplotlib.axes.Axes.axvline
		* ```.axvspan```: 添加垂直区域。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.axvline.html#matplotlib.axes.Axes.axvspan
		* ```.axline```: 添加（无限长）的直线，和```.plot```画线有点类似。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.axline.html#matplotlib.axes.Axes.axline
	* Spectral（处理频谱图）
	* Statistics（统计类图）
		* ```.boxplot```: 箱型图（box）和须状图（whisker）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.boxplot.html#matplotlib.axes.Axes.boxplot
			```python
			ax.boxplot(x, notch=True, vert=True, patch_artist=True, labels=labels, showfliers=True)
			```
		* ```.violinplot```: 小提琴图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.violinplot.html#matplotlib.axes.Axes.violinplot
			```python
			ax.violinplot(x, pos, points=60, widths=0.7, showmeans=True, showextrema=True, bw_method=0.5)
			```
		* ```.bxp```: 绘制箱型图时的功能函数
		* ```.violin```: 绘制小提琴图时的功能函数。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.violin.html#matplotlib.axes.Axes.violin
	* Binned（分仓图/分档图，其实算是一种频数统计？）
		* ```.hexbin```: 六边形分仓图（每个箱子表现为六边形，颜色代表每个仓内数据点的数量）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.hexbin.html#matplotlib.axes.Axes.hexbin
			```python
			ax.hexbin(x, y, gridsize=50, cmap='inferno')
			```
		* ```.hist```: 频数分布柱形图（对x中的数据进行分仓，计算每个仓中的数据点的数量，注意是频数不是频率）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.hist.html#matplotlib.axes.Axes.hist
			
			* 该频数分布柱形图可以通过 ```ax.plot``` 绘制拟合曲线
			```python
			ax.hist(x, bins=n_bins)
			```
		* ```.hist2d```: 2D的频数分布柱形图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.hist2d.html#matplotlib.axes.Axes.hist2d
		* ```.stairs```: 逐步恒等函数，如带边线的直线（line with bounding edges）或填充图（filled plot），看图形类似阶梯图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.stairs.html#matplotlib.axes.Axes.stairs
	* Contours（等高线图）
		* ```.contour```: 等高线图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.contour.html#matplotlib.axes.Axes.contour
		* ```.clabel```: 为等高线图添加标签。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.clabel.html#matplotlib.axes.Axes.clabel
		* ```.contourf```: 填充等高线图。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.contourf.html#matplotlib.axes.Axes.contourf
	* 2D arrays（二维图像）
		* ```.imshow```: 二维规则栅格图（热图）https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.imshow.html#matplotlib.axes.Axes.imshow
			```python
			ax.imshow(x, cmap=cmap)  # x是二维数组
			```
		* ```.matshow```: 二维矩阵图（和```.imshow```有点像）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.matshow.html#matplotlib.axes.Axes.matshow
		* ```.pcolor```: 使用不规则矩形网格创建伪彩色图形（和前面那俩差不多啊）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.pcolor.html#matplotlib.axes.Axes.pcolor
	* Unstructured triangles
		* ```.tripcolor```: 创建非结构化三角形网格的伪彩色图（就把色块换成了三角形）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.tripcolor.html#matplotlib.axes.Axes.tripcolor
		* ```.triplot```: 创建非结构化的三角形网格作为线和/或标记。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.triplot.html#matplotlib.axes.Axes.triplot
	* Text and annotations（文字和注释）
		* ```.annotate```: 为指定座标添加注释（指定坐标点和注释文字位置）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.annotate.html#matplotlib.axes.Axes.annotate
		* ```.text```: 在指定位置添加文字（指定注释文字位置）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.text.html#matplotlib.axes.Axes.text
			```python
			ax.text(x, y, s=text, h='center', v='center')  # 设置位于中心位置
			```
		* ```.table```: 为Axes添加一个列表。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.table.html#matplotlib.axes.Axes.table
		* ```.arrow```: 为Axes添加一个箭头。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.arrow.html#matplotlib.axes.Axes.arrow
		* ```.secondary_xaxis```: 为Axes添加第二个x轴（top、bottom、left、right）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.secondary_xaxis.html#matplotlib.axes.Axes.secondary_xaxis
		* ```.secondary_yaxis```: 为Axes添加第二个y轴（top、bottom、left、right）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.secondary_yaxis.html#matplotlib.axes.Axes.secondary_yaxis
	* Vector fields（矢量图）
* Clearing（Axes元素清除）
	
	* ```.cla``` 或 ```.clear```: Axes的元素清除。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.clear.html#matplotlib.axes.Axes.clear
* Appearance（外观）
	* ```.axis```: 方便地获取或设置一些坐标轴属性。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.axis.html#matplotlib.axes.Axes.axis
	* ```.set_axis_on``` 和 ```.set_axis_off```: 设置x轴和y轴打开或关闭（影响坐标轴线、坐标轴标签、刻度线、刻度标签、网格）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_axis_on.html#matplotlib.axes.Axes.set_axis_on
	* ```.get_frame_on``` 和 ```.set_frame_on```: 获取/设置是否在Axes绘制了轴边框。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_frame_on.html#matplotlib.axes.Axes.set_frame_on
	* ```.get_axisbelow``` 和 ```.set_axisbelow```: 获取/设置坐标轴是否在图像下层。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_axisbelow.html#matplotlib.axes.Axes.set_axisbelow
	* ```.grid```: 配置网格线。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.grid.html#matplotlib.axes.Axes.grid
	* ```.get_facecolor``` 和 ```.set_facecolor```: 获取/设置Axes的前景色。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_facecolor.html#matplotlib.axes.Axes.set_facecolor
* Axis/Limits（坐标轴设定）
	* ```.get_xaxis``` 和 ```.get_yaxis```: 获取x轴和y轴实例。
	* Axis limits and direction（轴范围值设置）
		* ```.invert_xaixs``` 和 ```.invert_yaxis```: 反转x轴/y轴。
		* ```.get_xlim``` 和 ```.get_ylim```，以及 ```.set_xlim``` 和 ```.set_ylim```: 获取/设置x轴和y轴的视图范围值（下限和上限，理解的是Axes展示的范围，比如在生成的fig里也可以放大或缩小Axes）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xlim.html#matplotlib.axes.Axes.set_xlim
		* ```.get_xbound``` 和 ```.get_ybound```，以及 ```.set_xbound``` 和 ```.set_ybound```: 获取/设置x轴和y轴的下限和上限，如果未设置，则不修改先攻的轴边界。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xbound.html#matplotlib.axes.Axes.set_xbound
	* Axis labels, title, and legend（标签、标题和图例）
		* ```.get_xlabel``` 和 ```.get_ylabel```，以及 ```.set_xlabel``` 和 ```.set_ylabel```: 获取/设置x轴和y轴的标签。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xlabel.html#matplotlib.axes.Axes.set_xlabel
		* ```.label_outer```: 若有Axes有多个subplots时，只展示在最外侧subplots的label。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.label_outer.html#matplotlib.axes.Axes.label_outer
		* ```.get_title``` 和 ```.set_title```: 获取/设置图表标题。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_title.html#matplotlib.axes.Axes.set_title
		* ```.legend```: 设置图例。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.legend.html#matplotlib.axes.Axes.legend
			```python
			ax.legend()  # 全自动生成
			ax.legend([line1, line2, line3], ['label1', 'label2', 'label3'])  # 自定义生成
			```
		* ```.get_legend``` 和 ```.get_legend_handles_labels```: 获取图例/获取图例的handles和标签。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.get_legend.html#matplotlib.axes.Axes.get_legend
	* Axis scales（坐标轴尺度）
		
		* ```.get_xscale``` 和 ```.get_yscale```，以及 ```.set_xscale``` 和 ```.set_yscale```: 获取/设置x轴和y轴的尺度（linear, log, symlog, logit, ...）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xscale.html#matplotlib.axes.Axes.set_xscale
	* Autoscaling and margins（坐标轴数据范围）
		* ```.use_sticky_edges```: 自动设置Axes的边距（紧贴）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.use_sticky_edges.html#matplotlib.axes.Axes.use_sticky_edges
		* ```.margins```、```.set_xmargin```、```.set_ymargin```: 设置Axes的边距/分别设置x轴、y轴上数据边距（这个边距指的是frame框内的数据距离frame的边距，设置为负数时图像会放大）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.margins.html#matplotlib.axes.Axes.margins
		* ```.autoscale```: 通过手动设置，自动计算评估设置数据的范围（x轴/y轴）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.autoscale.html#matplotlib.axes.Axes.autoscale
	* Aspect ratio（Axes的纵横比）
		* ```.get_aspect``` 和 ```.set_aspect```: 获取/设置scale的纵横比（即y_scale/x_scale）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_aspect.html#matplotlib.axes.Axes.set_aspect
		* ```.get_box_aspect``` 和 ```.set_box_aspect```: 获取/设置Axes box（即frame）的纵横比。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_box_aspect.html#matplotlib.axes.Axes.set_box_aspect
	* Tick and tick labels（刻度线，x轴和y轴一致，此处只列x轴方法）
		* ```.get_xticks``` 和 ```.set_xticks```: 获取/设置x轴的位置（x轴上设置刻度线的位置，并非所有位置都有刻度线）和标签（可自行设置major或minor）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xticks.html#matplotlib.axes.Axes.set_xticks
		* ```.get_xtickslabels``` 和 ```.set_xtickslabels```: 获取/设置x轴的标签（可自行设置major或minor）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xticklabels.html#matplotlib.axes.Axes.set_xticklabels
		* ```.get_xmajorticklabels``` 和 ```.get_xminorticklabels```: 获取x轴的主刻度线/次刻度线的标签。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.get_xmajorticklabels.html#matplotlib.axes.Axes.get_xmajorticklabels
		* ```.minorticks_on``` 和 ```.minorticks_off```: 显示/不显示次刻度线。
		* ```.ticklabel_format```: 设置刻度线标签格式。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.ticklabel_format.html#matplotlib.axes.Axes.ticklabel_format
		* ```.tick_params```: 设置刻度线、刻度线标签、网格线的外观。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.ticklabel_format.html#matplotlib.axes.Axes.ticklabel_format
* Units（坐标轴单位）
* Adding artists（在Axes中添加Atrists）
* Twinning and sharing（同x/y坐标，多y/x坐标）
	* ```.twinx``` 和 ```.twiny```: 创建于原始Axes共享x轴/y轴的Axes，新Axes的x轴/y轴不可见，但保留了独立的y轴/x轴。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.twinx.html#matplotlib.axes.Axes.twinx
	* ```.sharex``` 和 ```.sharey```: 设置与原始Axes共享x轴/y轴（可以对齐多幅子图的坐标轴。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.sharex.html#matplotlib.axes.Axes.sharex
* Axes position（Axes位置）
	* ```.get_position``` 和 ```.set_position```: 获取/设置Axes的position。Axes有两个position，一个原始位置（original position）是为Axes分配的位置，一个活动位置（active position）是Axes实际绘制的位置。这些位置通常是一致的。除非为坐标轴设置了固定的aspect（```.set_aspect```）。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_position.html#matplotlib.axes.Axes.set_position
	* ```.get_anchor``` 和 ```.set_anchor```: 获取/设置anchor。在设置了固定长宽（fixed aspect）时，Axes的实际绘制区域（active position）可能小于Bbox区域（original position）。此时设置anchor用于定义绘图区域在可用空间内的位置。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_anchor.html#matplotlib.axes.Axes.set_anchor
	* ```.get_axes_locator``` 和 ```.set_axes_locator```: 获取/设置Axes的定位器。https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_axes_locator.html#matplotlib.axes.Axes.set_axes_locator
* Async/event based
* Interactive
* Children
* Drawing
* Projection
* Other
## 一些常用参数的设置
### 颜色（均为按顺序排列）
#### 内置颜色条
matplotlib自带的Colormap分两种，ListedColormap（非连续）和LinearSegmentedColormap（连续）
```python
import numpy as np
from matplotlib import cm

# ListedColprmap
# 取某种颜色
ax.bar(x, y, color=plt.cm.Accent(4))  # 从Accent中取色

# 取多种颜色
ax.bar(x, y, color=plt.cm.Accent(range(5)))
ax.bar(x, y, color=plt.cm.get_cmap('Accent')(range(5)))
ax.bar(x, y, color=plt.get_cmap('Accent')(range(5)))

# LinearSegmentedColormap
# 取一种颜色
ax.bar(x, y, color=plt.get_cmap('Blues')(3))

# 取多种颜色
ax.bar(x, y, color=plt.get_cmap('Blues')(np.linspace(0, 1, 5)))
```
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
#### .plot的快捷颜色
```python
'b'  # 蓝
'g'  # 绿
'r'  # 红
'c'  # 青
'm'  # 品红
'y'  # 黄
'k'  # 黑
'w'  # 白
```
### 字体
#### 简单进行全局字体设置
```python
plt.rc('font', family='Times New Roman')
```
#### rc设置
调用rc设置的多种方法
```python
# 放在代码最开始即可成为全局设置
plt.rcParams['font.family'] = 'Times New Roman'  # 单属性调用
plt.rc('font', family='Times New Roman')  # 可一次调整一个group下的多个参数
```
rc下的group及常用group的常用参数
* lines（线条设置）
	```python
	#lines.linewidth: 1.5               # line width in points
	#lines.linestyle: -                 # solid line
	#lines.color:     C0                # has no affect on plot(); see axes.prop_cycle
	#lines.marker:          None        # the default marker
	#lines.markerfacecolor: auto        # the default marker face color
	#lines.markeredgecolor: auto        # the default marker edge color
	#lines.markeredgewidth: 1.0         # the line width around the marker symbol
	#lines.markersize:      6           # marker size, in points
	#lines.dash_joinstyle:  round       # {miter, round, bevel}
	#lines.dash_capstyle:   butt        # {butt, round, projecting}
	#lines.solid_joinstyle: round       # {miter, round, bevel}
	#lines.solid_capstyle:  projecting  # {butt, round, projecting}
	#lines.antialiased: True            # render lines in antialiased (no jaggies)
	```
* patches（填充2D空间的图形对象）
    ```python
    #patch.linewidth:       1.0    # edge width in points.
    #patch.facecolor:       C0
    #patch.edgecolor:       black  # if forced, or patch is not filled
    #patch.force_edgecolor: False  # True to always use edgecolor
    #patch.antialiased:     True   # render patches in antialiased (no jaggies)
    ```
* font（字体）
    ```python
    #font.family:  sans-serif	# 'Times New Roman', 'Arial'
    #font.style:   normal		# {normal, italic, oblique}
    #font.variant: normal		# {normal, small-caps}
    #font.weight:  normal		# {normal, bold, bolder, lighter, 100-900}
    #font.stretch: normal		# {ultra-condensed, extra-condensed, condensed, 
                                # semi-condensed, normal, semi-expanded, expanded, 
                                # extra-expanded, ultra-expanded, wider, narrower}
    #font.size:    10.0			# float or str: {P}xx-small, x-small, small, medium, 
                                # large, x-large, xx-large, larger, smaller}

    # 设置中文字体
    plt.rcParams['font.sans-serif'] = ['SimHei']
    plt.reParams['axes.unicode_minus'] = False  # 正常显示负号
    ```
* text（text内容）
	```python
	#text.color: black
	```
* axes（Axes）
    ```python
    #axes.facecolor:     white   # axes background color
    #axes.edgecolor:     black   # axes edge color
    #axes.linewidth:     0.8     # edge line width
    #axes.grid:          False   # display grid or not
    #axes.grid.axis:     both    # which axis the grid should apply to
    #axes.grid.which:    major   # grid lines at {major, minor, both} ticks
    #axes.titlelocation: center  # alignment of the title: {left, right, center}
    #axes.titlesize:     large   # font size of the axes title
    #axes.titleweight:   normal  # font weight of title
    #axes.titlecolor:    auto    # color of the axes title, auto falls back to
                                 # text.color as default value
    #axes.titley:        None    # position title (axes relative units). None = auto
    #axes.titlepad:      6.0     # pad between axes and title in points
    #axes.labelsize:     medium  # font size of the x and y labels
    #axes.labelpad:      4.0     # space between label and axis
    #axes.labelweight:   normal  # weight of the x and y labels
    #axes.labelcolor:    black
    #axes.axisbelow:     line    # draw axis gridlines and ticks:
                                 #     - below patches (True)
                                 #     - above patches but below lines ('line')
                                 #     - above all (False)
                
    #axes.formatter.limits: -5, 6  # use scientific notation if log10
                                   # of the axis range is smaller than the
                                   # first or larger than the second
    #axes.formatter.use_locale: False  # When True, format tick labels
                                       # according to the user's locale.
                                       # For example, use ',' as a decimal
                                       # separator in the fr_FR locale.
    #axes.formatter.use_mathtext: False  # When True, use mathtext for scientific
                                         # notation.
    #axes.formatter.min_exponent: 0  # minimum exponent to format in scientific notation
    #axes.formatter.useoffset: True  # If True, the tick label formatter
                                     # will default to labeling ticks relative
                                     # to an offset when the data range is
                                     # small compared to the minimum absolute
                                     # value of the data.
    #axes.formatter.offset_threshold: 4  # When useoffset is True, the offset
                                         # will be used when it can remove
                                         # at least this number of significant
                                         # digits from tick labels.
    #axes.spines.left:   True  # display axis spines
    #axes.spines.bottom: True
    #axes.spines.top:    True
    #axes.spines.right:  True
    #axes.unicode_minus: True  # use Unicode for the minus symbol rather than hyphen.
    #axes.prop_cycle: cycler('color', ['1f77b4', 'ff7f0e', '2ca02c', 'd62728', '9467bd', '8c564b', 'e377c2', '7f7f7f', 'bcbd22', '17becf'])
                      # color cycle for plot lines as list of string color specs:
                      # single letter, long name, or web-style hex
                      # As opposed to all other parameters in this file, the color
                      # values must be enclosed in quotes for this parameter,
                      # e.g. '1f77b4', instead of 1f77b4.
    #axes.xmargin:   .05  # x margin.  See `axes.Axes.margins`
    #axes.ymargin:   .05  # y margin.  See `axes.Axes.margins`
    #axes.zmargin:   .05  # z margin.  See `axes.Axes.margins`
    #axes.autolimit_mode: data  # If "data", use axes.xmargin and axes.ymargin as is.
                                # If "round_numbers", after application of margins, 
                                # axis limits are further expanded to the nearest 
            					# "round" number.
    #polaraxes.grid: True  # display grid on polar axes
    #axes3d.grid:    True  # display grid on 3D axes

    #axes3d.xaxis.panecolor:    (0.95, 0.95, 0.95, 0.5)  # background pane on 3D axes
    #axes3d.yaxis.panecolor:    (0.90, 0.90, 0.90, 0.5)  # background pane on 3D axes
    #axes3d.zaxis.panecolor:    (0.925, 0.925, 0.925, 0.5)  # background pane on 3D axes
    ```
* ticks（刻度线）
    ```python
    #xtick.top:           False   # draw ticks on the top side
    #xtick.bottom:        True    # draw ticks on the bottom side
    #xtick.labeltop:      False   # draw label on the top
    #xtick.labelbottom:   True    # draw label on the bottom
    #xtick.major.size:    3.5     # major tick size in points
    #xtick.minor.size:    2       # minor tick size in points
    #xtick.major.width:   0.8     # major tick width in points
    #xtick.minor.width:   0.6     # minor tick width in points
    #xtick.major.pad:     3.5     # distance to major tick label in points
    #xtick.minor.pad:     3.4     # distance to the minor tick label in points
    #xtick.color:         black   # color of the ticks
    #xtick.labelcolor:    inherit # color of the tick labels or inherit from xtick.color
    #xtick.labelsize:     medium  # font size of the tick labels
    #xtick.direction:     out     # direction: {in, out, inout}
    #xtick.minor.visible: False   # visibility of minor ticks on x-axis
    #xtick.major.top:     True    # draw x axis top major ticks
    #xtick.major.bottom:  True    # draw x axis bottom major ticks
    #xtick.minor.top:     True    # draw x axis top minor ticks
    #xtick.minor.bottom:  True    # draw x axis bottom minor ticks
    #xtick.alignment:     center  # alignment of xticks

    #ytick.left:          True    # draw ticks on the left side
    #ytick.right:         False   # draw ticks on the right side
    #ytick.labelleft:     True    # draw tick labels on the left side
    #ytick.labelright:    False   # draw tick labels on the right side
    #ytick.major.size:    3.5     # major tick size in points
    #ytick.minor.size:    2       # minor tick size in points
    #ytick.major.width:   0.8     # major tick width in points
    #ytick.minor.width:   0.6     # minor tick width in points
    #ytick.major.pad:     3.5     # distance to major tick label in points
    #ytick.minor.pad:     3.4     # distance to the minor tick label in points
    #ytick.color:         black   # color of the ticks
    #ytick.labelcolor:    inherit # color of the tick labels or inherit from 
    							  # ytick.color
    #ytick.labelsize:     medium  # font size of the tick labels
    #ytick.direction:     out     # direction: {in, out, inout}
    #ytick.minor.visible: False   # visibility of minor ticks on y-axis
    #ytick.major.left:    True    # draw y axis left major ticks
    #ytick.major.right:   True    # draw y axis right major ticks
    #ytick.minor.left:    True    # draw y axis left minor ticks
    #ytick.minor.right:   True    # draw y axis right minor ticks
    #ytick.alignment:     center_baseline  # alignment of yticks
    ```
* grids（网格）
    ```python
    #grid.color:     "#b0b0b0"  # grid color
    #grid.linestyle: -          # solid
    #grid.linewidth: 0.8        # in points
    #grid.alpha:     1.0        # transparency, between 0.0 and 1.0
    ```
* legend（图例）
    ```python
    #legend.loc:           best
    #legend.frameon:       True     # if True, draw the legend on a background patch
    #legend.framealpha:    0.8      # legend patch transparency
    #legend.facecolor:     inherit  # inherit from axes.facecolor; or color spec
    #legend.edgecolor:     0.8      # background patch boundary color
    #legend.fancybox:      True     # if True, use a rounded box for the
                                    # legend background, else a rectangle
    #legend.shadow:        False    # if True, give background a shadow effect
    #legend.numpoints:     1        # the number of marker points in the legend line
    #legend.scatterpoints: 1        # number of scatter points
    #legend.markerscale:   1.0      # the relative size of legend markers vs. original
    #legend.fontsize:      medium
    #legend.labelcolor:    None
    #legend.title_fontsize: None    # None sets to the same as the default axes.
    #legend.borderpad:     0.4  # border whitespace
    #legend.labelspacing:  0.5  # the vertical space between the legend entries
    #legend.handlelength:  2.0  # the length of the legend lines
    #legend.handleheight:  0.7  # the height of the legend handle
    #legend.handletextpad: 0.8  # the space between the legend line and legend text
    #legend.borderaxespad: 0.5  # the border between the axes and legend edge
    #legend.columnspacing: 2.0  # column separation
    ```
* boxplot
    ```python
    #boxplot.notch:       False
    #boxplot.vertical:    True
    #boxplot.whiskers:    1.5
    #boxplot.bootstrap:   None
    #boxplot.patchartist: False
    #boxplot.showmeans:   False
    #boxplot.showcaps:    True
    #boxplot.showbox:     True
    #boxplot.showfliers:  True
    #boxplot.meanline:    False

    #boxplot.flierprops.color:           black
    #boxplot.flierprops.marker:          o
    #boxplot.flierprops.markerfacecolor: none
    #boxplot.flierprops.markeredgecolor: black
    #boxplot.flierprops.markeredgewidth: 1.0
    #boxplot.flierprops.markersize:      6
    #boxplot.flierprops.linestyle:       none
    #boxplot.flierprops.linewidth:       1.0

    #boxplot.boxprops.color:     black
    #boxplot.boxprops.linewidth: 1.0
    #boxplot.boxprops.linestyle: -

    #boxplot.whiskerprops.color:     black
    #boxplot.whiskerprops.linewidth: 1.0
    #boxplot.whiskerprops.linestyle: -

    #boxplot.capprops.color:     black
    #boxplot.capprops.linewidth: 1.0
    #boxplot.capprops.linestyle: -

    #boxplot.medianprops.color:     C1
    #boxplot.medianprops.linewidth: 1.0
    #boxplot.medianprops.linestyle: -

    #boxplot.meanprops.color:           C2
    #boxplot.meanprops.marker:          ^
    #boxplot.meanprops.markerfacecolor: C2
    #boxplot.meanprops.markeredgecolor: C2
    #boxplot.meanprops.markersize:       6
    #boxplot.meanprops.linestyle:       --
    #boxplot.meanprops.linewidth:       1.0
    ```
* contour
	```python
	#contour.negative_linestyle: dashed  # string or on-off ink sequence
	#contour.corner_mask:        True    # {True, False}
	#contour.linewidth:          None    # {float, None} Size of the contour line
                                     # widths. If set to None, it falls back to
                                     # `line.linewidth`.
	#contour.algorithm:          mpl2014 # {mpl2005, mpl2014, serial, threaded}
	```
* errorbar
	```python
	#errorbar.capsize: 0  # length of end cap on error bars in pixels
	```
* hist
	```python
	#hist.bins: 10  # The default number of histogram bins or 'auto'.
	```
* scatter
	```python
	#scatter.marker: o         # The default marker type for scatter plots.
	#scatter.edgecolors: face  # The default edge colors for scatter plots.
	```
* savefig
	```python
	#savefig.dpi:       figure      # figure dots per inch or 'figure'
#savefig.facecolor: auto        # figure face color when saving
#savefig.edgecolor: auto        # figure edge color when saving
#savefig.format:    png         # {png, ps, pdf, svg}
#savefig.bbox:      standard    # {tight, standard}
                                	# 'tight' is incompatible with pipe-based 
      							# animation backends (e.g. 'ffmpeg') but will 
          						# work with those based on temporary filess
#savefig.pad_inches:  0.1       # padding to be used, when bbox is set to 'tight'
#savefig.directory:   ~         # default directory in savefig dialog, gets 
  								# updated after interactive saves, unless set to 
      							# the empty string (i.e. the current directory); 
          						# use '.' to start at the current directory but 
            					# update after interactive saves
  #savefig.transparent: False     # whether figures are saved with a transparent
                              	# background by default
	#savefig.orientation: portrait  # orientation of saved figure, for PostScript 
									# output only
	```
### marker种类
具体展示可见：https://matplotlib.org/stable/gallery/lines_bars_and_markers/marker_reference.html#sphx-glr-gallery-lines-bars-and-markers-marker-reference-py
```python
'.'  # point marker
','  # pixel marker
'o'  # circle marker
'v'  # triangle_down marker
'^'  # triangle_up marker
'<'  # triangle_left marker
'>'  # triangle_right marker
'1'  # tri_down marker
'2'  # tri_up marker
'3'  # tri_left marker
'4'  # tri_right marker
'8'  # octagon marker
'9'  # square marker
'p'  # pentagon marker
'P'  # plus (filled) marker
'*'  # star marker
'h'  # hexagon1 marker
'H'  # Hexagon2 marker
'+'  # plus marker
'x'  # x marker
'X'  # x (filled) marker
'D'  # diamond marker
'd'  # thin_diamond marker
'|'  # vline marker
'_'  # hline marker
```
### 线型种类
```python
'-'  # solid line style
'--'  # dashed line style
'-.'  # dash-dot line style
':'  # dotted line style
```