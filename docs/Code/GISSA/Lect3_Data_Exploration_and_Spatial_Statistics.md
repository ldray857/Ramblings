# 空间分析课程复习笔记 - 第3讲 空间数据探索与空间统计

本笔记基于《Lect.3 Exploring Spatial Data and Spatial Statistics.pdf》课件整理。

## 一、 为什么在空间分析中需要数据探索和统计 (Why Data Exploration and Statistics in Spatial Analysis)

* **统计思维的重要性**：统计报表与概率思维在日常生活中很常见（如降雨概率、患病风险等）。著名作家H.G. Wells曾言：“统计思维总有一天会像读写能力一样，成为高效公民所必需的能力。”
* **地理问题中的应用**：统计分析在地理研究中发挥着核心作用，如探究种族与社会资本在不同县的空间关联、估算全球树木数量、测量全球河流总长度等。超过50%的以地理为焦点的论文都至少使用了一种主流的定量统计方法。

---

## 二、 描述性统计 (Descriptive Statistics)

地理数据的一个关键特征是：**它们通常被视为来自更大总体（population）的样本（sample）**。

* **描述性统计 (Descriptive statistics)**：指用于描述和总结“样本”特征的特定方法。它属于**探索性 (exploratory)** 技术。
* **推断性统计 (Inferential statistics)**：指利用样本来推断“总体”特征的方法。它属于**验证性 (confirmatory)** 方法。

### 核心统计度量指标及公式

1.  **均值 (Mean)**：代表数据集的集中趋势。
    * 样本均值公式： $\bar{x} = \frac{1}{n}\sum\limits_{i=1}^{n} x_i$
2.  **中位数 (Median)**：将排序后的数据一分为二的值。奇数个数据取中间值，偶数个数据取中间两值的平均数。
3.  **众数 (Mode)**：数据集中出现频率最高的值。
4.  **极差 (Range)**：最低值和最高值之间的差值。
    * 公式： $R = \max_i\{x_i\} - \min_i\{x_i\}$
5.  **四分位距 (Interquartile Range, IQR)**：第75个百分位数和第25个百分位数之间的差异。
6.  **方差 (Variance) 与 标准差 (Standard Variance)**：衡量所有观测值与均值之间差异的平均水平。
    * 总体方差： $\sigma^2 = \frac{1}{N}\sum\limits_{i=1}^{N}(x_i - \mu)^2$
    * 总体标准差： $\sigma = \sqrt{\frac{1}{N}\sum\limits_{i=1}^{N}(x_i - \mu)^2}$
    * **样本方差 ($S^2$)** (使用n-1作为分母校正自由度)： $S^2 = \frac{1}{n-1}\sum\limits_{i=1}^{n}(x_i - \bar{x})^2$
    * **样本标准差 ($S$)**： $S = \sqrt{\frac{1}{n-1}\sum\limits_{i=1}^{n}(x_i - \bar{x})^2}$
7.  **Z得分 (Z-Score)**：用于比较不同均值和变异程度的分布，将观测值标准化（转化为均值为0，标准差为1的标准正态分布）。表示观测值偏离均值多少个标准差。
    * **【重要公式】**： $Z_i = \frac{X_i - \bar{X}}{S}$
8.  **变异系数 (Variogram / Coefficient of Variation, $C_v$)**：衡量相对离散程度，便于均值不同的数据集比较。
    * **【重要公式】**： $C_v = \frac{S}{\bar{x}} = \frac{1}{\bar{x}}\sqrt{\frac{1}{n-1}\sum\limits_{i=1}^{n}(x_i - \bar{x})^2}$
9.  **偏度 (Skewness)**：测量数据的非对称程度。
    * **【重要公式】**： $skewness = \frac{\sum\limits_{i=1}^{n}(x_i - \bar{x})^3}{nS^3}$
    * 解释：$g_1 < 0$ 为负偏态（左偏）；$g_1 = 0$ 为对称分布；$g_1 > 0$ 为正偏态（右偏）。
10. **峰度 (Kurtosis)**：测量直方图的陡缓程度，使用四次方。
    * **【重要公式】**： $kurtosis = \frac{\sum\limits_{i=1}^{n}(x_i - \bar{x})^4}{nS^4}$
    * $g_2 = kurtosis - 3$。当 $g_2 > 0$ 时分布比正态更陡（尖峰），$g_2 = 0$ 为标准正态分布，$g_2 < 0$ 时比正态分布平缓（平峰）。

---

## 三、 统计分布 (Statistical Distributions)

### 1. 正态曲线/钟形曲线 (The Normal / Bell-Shaped Curve)

* **视觉表现**：数据的分布表现为中间高、两端低的对称钟形曲线。一般来说，许多事件都发生在分布的中间，而两端极端事件较少。
* **三大核心特征**：
    1.  **均值、中位数和众数彼此相等** (Mean, median, and mode are equal to one another)。
    2.  **关于均值完全对称** (Perfectly symmetrical about the mean)。
    3.  **尾部是渐近的** (Tails are asymptotic)：曲线向水平轴无限延伸，越来越靠近，但永远不接触水平轴。这意味着观测到无穷大处 $x$ 的概率非常接近于零。

### 2. 标准正态分布与 Z得分 (Standard Normal Distribution & Z-Score)

由于不同的正态分布具有不同的均值和变异程度（标准差），直接比较它们非常困难（相对指标）。因此，常通过 **Z得分 (Z-score)** 将数据标准化。

* **Z得分意义**： 表示某个观测值距离均值有多少个标准差。得分低于均值为负，高于均值为正，标准化后使得跨分布的得分具有**可比性**。
* **标准正态分布**：经过 Z得分 转换后的连续数据将服从 **均值为 0、标准差为 1** 的标准正态分布 ($\mu = 0, \sigma = 1$)。
* **曲线下的概率面积（经验法则）**：
    * $\pm 1$ 个标准差内 ($Z \in [-1, 1]$)：涵盖约 **68.26%** 的观测案例。
    * $\pm 2$ 个标准差内 ($Z \in [-2, 2]$)：涵盖约 **95.44%** 的观测案例。
    * $\pm 3$ 个标准差内 ($Z \in [-3, 3]$)：涵盖约 **99.74%** 的观测案例。
* **统计检验应用**：
    * 标准正态分布曲线下的概率面积总和为 1，以均值0为界左右各占 0.5。
    * 在实际分析中，我们通常计算在给定区间（例如 $a < Z < b$）内的概率。通过查阅 **Z得分表 (Z-score table)**，可以方便地得出任意区间的概率。
    * 一些简单的统计检验本质上就是利用这些概率来判断两个参数是否具有显著差异，或者这种差异是否仅仅归因于抽样误差 (sampling error)。

---

## 四、 探索性空间数据分析 (Exploratory Spatial Data Analysis, ESDA)

### 1. 核心理念与概念 (The Basic Ideas and Concepts)

* **基本理念**：**“让数据自己说话”** (Let the data speak for themselves)。不受限于方法原理，也不拘泥于数据测量精度。
* **概念**：“ESDA 是一种日益普及的基于 GIS 的技术，它使用户能够描述和可视化空间分布，识别非典型位置或空间离群值，发现空间关联模式、聚类或热点，并提示空间体制或其他形式的空间异质性。”
* **基本内容**：(1) 检查数据错误；(2) 获取空间数据的分布特征；(3) 初步调查空间数据模式。

### 2. 常规视觉探索方法 (Conventionally Visual Exploration Methods)

* **直方图 (Histogram)**：垂直条形图，高度代表类别的频率。
* **饼图 (Pie Chart)**：显示整体的按比例细分，适用于类别较少（少于6或7个）的分类数据。
* **折线图 (Line Graph)**：显示变量随时间的变化趋势，适用于数值数据。
* **散点图 (Scatter Plot)**：展示两个变量之间的关联类型（正/负相关）和强度（强/弱）。
* **箱线图 (BoxPlot)**：水平线表示中位数，矩形上下端表示第25和第75百分位数，可以直观看到异常值。
* **平行坐标图 (Parallel Coordinate Plot)**：用垂直标尺显示每个变量并通过线连接各个案例。可以通过选择一条线来突出显示特定对象。

### 3. ESDA 的空间方法 (Methods for ESDA)

* **专题图 (Thematic Maps)**：将属性数据直接映射到地理空间位置上，用于直观展示分布。
* **条件分级图 (Conditional Choropleth Plots)**：基于不同条件变量进行分级设色的空间地图展示。
* **【重要考点】半变异函数/半变异图 (Semivariance / Semivariogram)**：
    * **【重要公式】**： $\gamma(h) = \frac{1}{2N(h)} \sum\limits_{d_{ij}=h-\Delta/2}^{h+\Delta/2} (z_i - z_j)^2$
    * **参数解释**：$z_i, z_j$ 代表点$i$和$j$的特定变量得分；$h$ 为固定的间隔距离(lag)；$d_{ij}$ 为点$i$和$j$间的距离；$\Delta$ 为中心距为$h$的带的宽度；$N(h)$ 为距离在$h$范围内的样本对数量。
    * **含义**：通过半变异云 (Semivariance cloud)，可用于探索空间自相关性或寻找局部/全局的空间离群点 (outliers)。
    * **图形要素**：块金值 (Nugget)、基台值 (Sill)、变程 (Range) 以及 间隔距离 (h)。
* **泰森多边形 (Voronoi map / Thiessen polygons / Proximity polygons)**：
    * **概念**：对于平面上的一组点，存在一组关联区域。每个区域内的任何位置都比其他任何点更靠近该区域的中心点。这些区域被视为点集的对偶 (dual)。它与德劳内三角网 (Delaunay Triangulation / TIN) 密切相关。
* **映射数据探索 (Mapped data & Mapped box plot)**：
    * 为映射的点数据集及其关联的连续属性权重探索变异的其他方法。
    * **地图刷选 (Brushing)**：例如在半变异函数散点图中选中具有最高半变异值的数据点，地图上的对应空间位置也会被高亮显示。
    * **映射箱线图 (Mapped box plot)**：在地图中结合箱线图，直观显示空间数据的异常值和分布情况。
* **趋势分析 (Trends Analysis)**：
    * 用于检查研究区域内是否存在任何简单的空间趋势（通常通过3D透视图呈现数据值随X和Y坐标的变化）。
* **地图交互性 (Map Interactivity)**：
    * 通过动态对比和交互多张地图（例如对比1981年和1991年的地图）来直观揭示空间结构的剧烈变化。
* **其他ESDA方法 (Others)**：
    * 空间自相关分析 (Autocorrelation analysis)、热点分析 (Hot spot analysis)、探索性时空数据分析 (ESTDA) 等。
