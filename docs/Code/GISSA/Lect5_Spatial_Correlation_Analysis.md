# 空间分析课程复习笔记 - 第5讲 空间相关性分析

本笔记基于《Lect. 5 Spatial Correlation Analysis 2026.pdf》课件整理，严格遵循课件的结构与语言表述。

## 一、 地理学中的传统相关性分析 (Traditional Correlation Analysis in Geography)

### 1. 皮尔逊相关系数 (Pearson’s Correlation Coefficient)

* **地理相关的类型 (Geographical correlation types)**：
    * 完全相关 (Complete correlation)
    * 无相关 (No correlation)
    * 统计相关 (Statistical correlation)
* **样本相关系数 (The sample correlation coefficient)**：
    $$ r = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^{n}(x_i - \bar{x})^2} \sqrt{\sum_{i=1}^{n}(y_i - \bar{y})^2}} = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{(n-1)s_x s_y} $$
    * 其中 $s_x$ 和 $s_y$ 分别是变量 $x$ 和 $y$ 的样本标准差。
    * 注意，$r$ 也等于标准分数 (z-scores) 乘积的和除以 $n-1$： $r = \frac{\sum_{i=1}^{n} z_{xi} z_{yi}}{n-1}$。
* **总体相关系数 (The population correlation coefficient, $\rho$)**：
    $$ \rho = \frac{E[(X - \mu_X)(Y - \mu_Y)]}{\sigma_X \sigma_Y} = \frac{Cov[X, Y]}{\sigma_X \sigma_Y} $$
    * 其中 $\mu_X$ 和 $\mu_Y$ 是变量在总体中的均值，$\sigma_X$ 和 $\sigma_Y$ 是标准差。
    * **协方差 (Covariance, $Cov[X, Y]$)**：是方差概念的直接扩展。方差是观测值偏离其均值的平方偏差的期望值；而协方差是两个变量各自偏离其均值的乘积的期望或平均值。变量 $X$ 与自身的协方差等于 $X$ 的方差。
    * 协方差的大小取决于测量单位。通过除以标准差的乘积，可以将其**标准化 (standardized)**，使其值处于 -1 到 +1 之间，这被称为**皮尔逊相关系数**。相关系数提供了两个变量之间线性关联的标准化测量。
* **协方差的正负**：如果绘制的 $(x, y)$ 点大多沿正斜率的线分布，则为正；沿负斜率的线分布，则为负。
* **相关系数的图解 (Illustrations of Correlation Coefficient)**：
    * **$r \in [-1, 1]$**。相关系数衡量的是变量之间**线性关联 (linear association)** 的强度。有可能两个变量之间存在强烈的非线性关联，而相关系数却接近于零。因此，绘制数据图非常重要。
    * 强烈的线性关联并不一定意味着两个变量之间存在**因果关系 (causal connection)**。例如，南极企鹅死亡率与英国煤炭产量之间曾发现强相关性；美国龙卷风数量与汽车交通量也被发现存在强联系。
* **相关系数与样本量 (The Correlation Coefficient and Sample Size)**：
    * **极其重要的一点**：相关系数受**样本量 (sample size)** 的影响。样本量大时，拒绝零假设 $\rho=0$ 要比样本量小时容易得多。
    * 没有任何经验法则（如认为 $r$ 达到 0.7 或 0.8 才算重要）可以直接判定 $r$ 是否显著，这完全取决于样本量。当数据集很大时，较低的 $r$ 值不应该像样本量较小时那样令人失望。例如，当 $n=1000$ 时，$r=0.4$ 可能非常有意义。
* **$r$ 的显著性检验 (A Significance Test for $r$)**：
    * 为了检验真实相关系数 $\rho = 0$ 的零假设，假设数据服从正态分布。检验统计量为：
      $$ t = \frac{r\sqrt{n-2}}{\sqrt{1-r^2}} $$
    * 如果零假设为真，该统计量服从自由度为 $n-2$ 的 t 分布。
* **相关系数矩阵 (Correlation Coefficient Matrix)**：$R$ 是对称矩阵，对角线上的元素 (catercorner elements) 全为 1。

### 2. 斯皮尔曼秩相关系数 (Spearman's Rank Correlation Coefficient)

* **适用情况**：当只有排序数据 (ranked data) 可用时，或者当不满足零假设正态性检验的前提时，适合使用斯皮尔曼秩/等级相关系数。
* **计算**：为每个变量分别排列名次，将最小值记为 1，最大值记为 $n$。
    $$ r_s = 1 - \frac{6\sum_{i=1}^{n} d_i^2}{n^3 - n} $$
    * 其中 $d_i^2$ 是观察值 $i$ 的排名差异的平方。
* **显著性检验**：检验统计量为 $t = \frac{r_s \sqrt{n-2}}{\sqrt{1 - r_s^2}}$，服从自由度为 $n-2$ 的 t 分布。（注：部分教材与课件表达上可能有差异，这里依据课件18页公式：检验统计量分布类似于基于 $\sqrt{n-1}$ 的推导，但最终是跟特定临界值比较）。
* **空间依赖对相关系数显著性检验的影响 (The effect of spatial dependence on significance tests)**：
    * 皮尔逊和斯皮尔曼的显著性检验都**假设 $x$ 和 $y$ 的观测值是独立的 (independent)**。
    * 当 $x$ 和 $y$ 变量来自空间位置时，这种独立性假设可能不满足。事实上，**空间数据经常表现出依赖性 (spatial data often exhibit dependence)**——某位置的值通常与附近位置的值相关。
    * 空间依赖会影响统计检验的结果。当相关系数落在阴影区域时，如果存在未解释的空间依赖，标准的统计检验会错误地暗示相关系数显著不为零。解释统计结果时必须牢记这一点。

### 3. 偏相关系数 (Partial Correlation Coefficient)

* **定义**：偏相关系数用于测量两个随机变量之间的关联程度，同时**消除了控制变量集的影响 (effect of a set of controlling random variables removed)**。
* **动机**：如果存在与研究变量均具有数值关联的第三个变量，直接使用相关系数会产生误导性结果。为了避免这种误导，可以通过计算偏相关系数来控制**混杂变量 (confounding variable)**。这也是在多元回归中引入右侧其他变量的原因。
* **三变量偏相关公式**：
    $$ r_{xy\cdot z} = \frac{r_{xy} - r_{xz}r_{yz}}{\sqrt{(1 - r_{xz}^2)(1 - r_{yz}^2)}} $$
* **高阶偏相关系数推导**：
    当有 $m$ 个变量，要计算固定其他 $m-2$ 个变量时，可以通过递归关系从低阶偏相关系数推导：
    $$ \rho_{ij\cdot S} = \frac{\rho_{ij\cdot S\setminus\{t\}} - \rho_{it\cdot S\setminus\{t\}} \rho_{jt\cdot S\setminus\{t\}}}{\sqrt{1 - \rho_{it\cdot S\setminus\{t\}}^2} \sqrt{1 - \rho_{jt\cdot S\setminus\{t\}}^2}} $$
* **显著性检验**：
    $$ t = \frac{r_{12\cdot34\dots m}}{\sqrt{1 - r_{12\cdot34\dots m}^2}} \sqrt{n - m - 2} $$
    其中 $m$ 是控制变量个数。

### 4. 复相关系数 (Multiple Correlation Coefficient)

* **定义**：复相关系数 ($R$) 衡量一个给定变量在多大程度上可以被一组其他变量的线性函数预测， $R \in [0, 1]$。
    $$ R = \sqrt{r_{yx}^T R_{xx}^{-1} r_{yx}} $$
    * 其中 $r_{yx}$ 为 $m \times 1$ 的列向量，每个元素是 $y$ 与每个自变量 $x_i$ 的皮尔逊相关系数；
    * $r_{yx}^T$ 是转置；
    * $R_{xx}$ 为 $m \times m$ 的自变量之间的相关系数矩阵；
    * $R_{xx}^{-1}$ 是自变量相关系数矩阵的逆矩阵。
* **决定系数 ($R^2$)**：表示在多元线性回归中，自变量 $x_1, x_2, \dots, x_m$ 共同解释的因变量 $y$ 的方差比例。
    $$ R^2 = r_{yx}^T R_{xx}^{-1} r_{yx} = \text{回归平方和} / \text{总平方和} $$
* **显著性检验 (Test)**：使用 F 检验：
    $$ F = \frac{SSR/(k-1)}{SSE/(n-k-1)} $$
    * $k = m+1$ (自变量数+1)。

---

## 二、 空间自相关分析 (Spatial Autocorrelation Analysis)

### 1. 空间自相关的概念 (The Concept of Spatial Autocorrelation)

* **地理学第一定律 (The first law of geography)**：Everything is related to everything else, but near things are more related than distant things.（一切事物都与其它事物相关，但近处的事物比远处的事物更相关）——Waldo Tobler。
    * 研究人员的一个常见目标是确定两个变量是否相互关联。并且研究人员对变量如何**共变 (covary)** 感兴趣。
    * 这第一定律是**空间依赖 (spatial dependence)** 和**空间自相关 (spatial autocorrelation)** 基本概念的基础，它被特别应用于空间插值的反距离权重法和支持克里金法的区域化变量理论。
* **空间自相关**：是利用空间统计学研究空间单元与其周围单元之间的空间相关性，从而分析这些空间单元的空间分布特征。
    * 如果空间自相关程度高，具有相同特征的空间现象将聚集在一起。
    * 相反，如果程度低，空间现象可能在空间中分散分布。
* **空间相关性分析 (Spatial Correlation Analysis)**：意味着应用相关性分析的方法来研究空间分布数据。

### 2. 空间权重矩阵 (Spatial Weights Matrix)

* 空间统计并不意味着对碰巧具有空间坐标的数据应用传统（非空间）统计方法。空间统计将空间和空间关系（面积、距离、长度等）**直接整合**到数学中。对于许多空间统计，这些空间关系通过**空间权重矩阵文件或表格 (spatial weights matrix)** 正式指定。
* **空间权重矩阵 (A spatial weights matrix)**：是数据集中要素之间**空间关系 (spatial relationships)** 的表示。它是数据集中要素之间存在空间关系的量化（或者至少，是您概念化这些关系的方式的量化）。
* 因为**空间权重矩阵强加了一种数据结构 (imposes a structure on your data)**，我们应该选择一个最能反映要素实际交互方式的**概念化模型 (conceptualization)**。

#### 【重点】空间关系的概念化 (The Conceptualization of Spatial Relationship)

在 ArcGIS Pro 等工具中，空间权重的构建概念可以归纳为以下几类：

**A. 距离 (Distance)**
1. **阈值距离 / 距离带 (Threshold distance / Distance band)**：
    * 公式：当 $d < D$ 时，$w_{ij} = 1$；否则 $w_{ij} = 0$。其中 $D$ 是确定的距离。
2. **反距离 (Inverse distance)**：
    * 公式：$w_{ij} = \frac{1}{d_{ij}^2}$。

**B. 多边形连续性 (Polygon Continuity)**
* 空间权重矩阵通常表示为 $W = \begin{bmatrix} w_{11} & \dots & w_{1n} \\ \vdots & \ddots & \vdots \\ w_{n1} & \dots & w_{nn} \end{bmatrix}$，其中 $w_{ij}$ 表示区域 $i$ 和 $j$ 之间的空间权重指数。
* 空间权重矩阵的构建通常基于两个特征：**连续性 (continuity)** 和 **距离 (distance)**。它也可以通过面积和可达性来构建。
* 基于连续性的空间权重矩阵可用于**矢量或栅格数据 (vector or raster data)**。如果用于栅格或格网数据，连续性可以通过以下方式表示：
    1. **仅相邻边 (Contiguity edges only, 即 Rook's case)**：如果具有公共边，则 $w_{ij} = 1$，否则为 $0$。
    2. **角点相邻 (Contiguity Corners, 即 Bishop's case)**：如果具有公共顶点，则 $w_{ij} = 1$，否则为 $0$。
    3. **边和角点相邻 (Contiguity edges corners, 即 Queen's case)**：如果具有公共边或顶点，则 $w_{ij} = 1$，否则为 $0$。

**C. 邻居 (Neighbors)**
1. **K个最近邻 (K nearest neighbors)**：
    * 机制：如果邻居数 K 为 8，则离目标要素最近的 8 个邻居将被包含在该要素的计算中。
    * **优势**：在要素**密度高**的地方，分析的**空间上下文将变小 (spatial context will be smaller)**；在密度稀疏的地方，空间上下文将变大。这种空间关系模型的优点在于，它确保了**每个目标要素都有一定数量的邻居**，即使要素密度在整个研究区域内差异很大。
2. **Delaunay 三角网 / 自然邻居 (Delaunay triangulation / Natural neighbors)**：
    * 机制：通过从点要素或多边形质心创建 Voronoi 三角形来构造邻居。由三角形边连接的节点被视为邻居。
    * **优势**：使用 Delaunay 三角网确保**每个要素至少有一个邻居**，即使数据包含岛屿 (islands) 或要素密度变化极大的情况。

**D. 其他 (Others)**
* 时空窗口 (Spatio-temporal Window)。
* **转换表 (Convert table)**：此选项可用于将 ASCII 空间权重矩阵文件转换为 SWM 格式的空间权重矩阵文件。

*注*：对于全局 $W$ 矩阵的对角线元素，**$w_{ii} = 0$**。一般来说，空间单元自身与自身不存在相邻关系。可以使用 ArcGIS Pro 中的“生成空间权重矩阵”工具或 Geoda 的 Weights Manager 创建。

### 3. 全局空间自相关 (Global Spatial Autocorrelation)

#### A. 莫兰指数 (Moran’s I)

* **定义**：由 Patrick Alfred Pierce Moran 于 1950 年首先提出，用于基于要素位置和属性值来衡量空间自相关性。空间自相关的核心思想是，**值在空间上不是独立的**（基于地理学第一定律）。
* **【公式】**：
    $$ I = \frac{n}{\sum_{i=1}^{n}\sum_{j=1}^{n}w_{ij}} \times \frac{\sum_{i=1}^{n}\sum_{j=1}^{n}w_{ij}(x_i - \bar{x})(x_j - \bar{x})}{\sum_{i=1}^{n}(x_i - \bar{x})^2} $$
    * 其中 $x_i, x_j$ 分别是区域 $i, j$ 的变量值；$w_{ij}$ 是空间权重。最常见的定义是**二元连通性 (binary connectivity)**：如果相邻则 $w_{ij}=1$，否则 $0$。
    * 如果在输入工具之前对变量值进行标准化处理，Moran's I 将落在 **$[-1, 1]$** 之间。
* **显著性检验 (Significant Testing)**：
    * 当区域数量较大时，$I$ 在无空间模式假设下的抽样分布趋于正态分布，可构建 Z 统计量： $Z(I) = \frac{I - E[I]}{\sqrt{Var(I)}}$，期望值 $E[I] = \frac{-1}{n-1}$。
    * 在 $\alpha=0.05$ 时，若 $Z(I) > 1.96$，为正空间自相关（聚集）；若 $Z(I) < -1.96$，为负空间自相关（离散）。
* **工具和报告 (Tool and Report)**：
    * **p值 (p-value)**：是一种概率。极小的 p 值意味着观察到的空间模式由随机过程产生是非常不可能的，因此可拒绝零假设。
    * **Z得分 (Z-scores)**：是标准差。
* **潜在应用 (Potential applications)**：
    * 通过寻找空间自相关最强的距离，帮助确定各种空间分析方法的合适邻域距离。
    * 衡量种族隔离随时间的整体趋势（增加或减少）。
    * 概括思想、疾病或趋势在时空中的扩散过程（隔离集中还是扩散）。

#### B. 全局 G 指数 (General G)

* **定义**：测量高值或低值聚集程度的统计量。
    $$ G(d) = \frac{\sum_{i=1}^{n}\sum_{j=1}^{n} w_{ij}(d)x_i x_j}{\sum_{i=1}^{n}\sum_{j=1}^{n} x_i x_j} $$
* **在无模式假设下的期望**：
    $$ E[G] = \frac{\sum_{i=1}^{n}\sum_{j=1}^{n} w_{ij}(d)}{n(n-1)} $$
* **结果解释**：
    * 如果 $G(d) > E(G)$ 且 Z 值显著，则为高-高聚类 (high-high cluster)。
    * 如果 $G(d) < E(G)$ 且 Z 值显著，则为低-低聚类 (low-low cluster)。
    * 如果 $G(d) \approx E(G)$，则为随机分布。
* **潜在应用**：
    * 寻找急诊室访问量的异常激增（可能指示局部健康问题爆发）。
    * 比较城市内不同类型零售业的空间模式（哪些类型聚集以利用比较购物，如汽车经销商；哪些类型排斥竞争，如健身中心）。
    * 总结空间现象聚类水平的变化（如使用 Getis-Ord General G 比较单一城市人口聚集水平随时间的变化）。

*(其他方法还包括 Join Counts 和 Bivariate Moran's I 等)*

### 4. 局部空间关联指标 (Local Indicators of Spatial Association, LISA)

#### A. 局部莫兰指数 (Local Moran’s I)

* **定义**：由 Luc Anselin 于 1995 年提出，用于识别给定要素集中具有高值或低值的空间聚类和异常值。
* 局部 Moran’s $I_i$ 的总和在一定的比例常数下等于全局 Moran’s $I$： $\sum I_i = I$。
* **【重要公式】**：
    $$ I_i = \frac{x_i - \bar{x}}{S_i^2} \sum\limits_{j \neq i} w_{ij}(x_j - \bar{x}) \quad \text{其中} \quad S_i^2 = \frac{\sum_{j \neq i}(x_j - \bar{x})^2}{n-1} $$
* **显著性检验**：$z_{I_i} = \frac{I_i - E[I_i]}{\sqrt{V[I_i]}}$，其中期望值 $E[I_i] = \frac{-\sum_{j=1, j \neq i}^{n} w_{ij}}{n-1}$。
* **四种分布象限**：高值聚类 (HH)、低值聚类 (LL)、低值被高值包围 (LH)、高值被低值包围 (HL)。
* **潜在应用**：
    * 研究区域内富裕与贫困之间最清晰的边界在哪里？
    * 区域内是否有异常支出模式的位置？
    * 研究区域内意外高发糖尿病的地点在哪里？

#### B. Getis-Ord $G_i^*$ 

* **公式**：
    $$ G_i^* = \frac{\sum_{j=1}^{n}w_{ij}x_j - \bar{X}\sum_{j=1}^{n}w_{ij}}{S \sqrt{\frac{n\sum_{j=1}^{n}w_{ij}^2 - (\sum_{j=1}^{n}w_{ij})^2}{n-1}}} $$
    其中 $w_{ij}(d)$ 可以是 0, 1 或其他值。
* **显著性检验**：每个要素都有其 $G_i^*$ 的 Z 得分、p 值和邻居数量。
* **工具和例子**：ArcGIS Pro 中的热点分析 (Hot Spot Analysis)。
* **潜在应用**：犯罪分析、流行病学、投票模式分析、经济地理、零售分析、交通事故分析和人口统计学。
    * 疾病爆发集中在哪里？
    * 哪里厨房火灾的比例超出了预期？
    * 疏散点应设在哪里？
    * 高峰发生在哪里/何时？我们应在何时何地分配更多资源？

---

## 三、 研究案例 (Study Cases)

课件中列举了一些空间统计应用的案例：
* 43个欧洲国家能源效率的时空影响因素：空间计量经济学分析 (2024)。
* 基于空间统计的中国雾霾和污染颗粒物时空分布特征 (2022)。
* 电动汽车充电城市公共服务的空间公平分析——中国城市的启示 (2022)。
* 多山城市景观生态风险评价及其驱动因素 (2023)。
