# 空间分析课程复习笔记 - 第10讲 时空分析

本笔记基于《Lect.10 Spatio-Temporal Analysis -2026p.pdf》课件整理，严格遵循课件的结构与内容顺序。

## 一、 为什么要研究时空问题 (Why Study Spatio-Temporal Issues)

* 哲学三问（Paul Gauguin 高更的画作）：“我们从哪里来？我们是谁？我们到哪里去？”一切事物都存在于空间和时间中。
* **核心地位**：空间和时间是人类生存和生活的重要知识，也是信息发展和应用的核心要素。
* **经典案例**：全球土地覆盖变化（Song et al., Nature, 2018），通过纬度剖面分析了1982-2016年树冠覆盖、短植被和裸地的时空变化。南亚森林火灾的时空热点识别（2019）。

---

## 二、 相关概念 (Some Concepts)

### 1. 什么是时空？(What is spatio-temporal?)

* 描述一个现象在特定位置和时间的状态。

### 2. 时空数据的定义与分类 (Spatio-temporal data types)

时空数据 (Spatio-temporal data) 是在特定位置和时间收集的数据，反映了事物在空间和时间上的双重属性。
* **四种常见的时空数据类别**：
    1.  **事件数据 (Event data)**：特定位置和时间发生的离散事件（如：犯罪、交通事故、地震、雷击）。
    2.  **轨迹数据 (Trajectory data)**：记录移动物体的运动路线（如：飞机、船只、车辆、人的运动轨迹）。
    3.  **点参考数据 (Point reference data)**：在移动或固定的参考点收集的连续时空场数据（如：气象气球收集的地表温度、交通传感器读数）。
    4.  **栅格数据 (Raster data)**：在固定网格中收集的时空场观测数据（如：遥感影像数据、洪水范围）。

### 3. 时间的性质 (The nature of "Temporal")

* **时间概念化 (Conceptualizations of time)**：时间可以是线性的（单向）、循环的（周期性重复）等。**ArcGIS 系统默认假设时间是线性的**。
* **时间的相对性 (Time is relative to something)**：
    * **时钟驱动时间 (Clock-driven time)**：以特定的时间刻度为准（如传感器每10分钟读数一次，视频长度10分钟）。
    * **事件驱动时间 (Event-driven time)**：以某个特定事件为基准同步（如自然灾害发生前后，经济危机前后）。
    * **状态驱动时间 (State-driven time)**：以状态改变为基准同步（如河流的水位超过警戒线）。
* **时间的形式 (Format)**：
    * 时刻 (A moment in time)：特定的单一时间戳 (single time stamp)。
    * 时段 (A duration of time)：有一段开始和结束时间。
* **频率 (Frequency)**：
    * 规则频率 (Regular)：时间间隔恒定。
    * 不规则频率 (Irregular)：时间间隔不同。

### 4. 时空分析定义 (Spatio-temporal analysis)

* **定义**：时空（数据）分析是一个新兴的研究领域。得益于新型计算技术的发展与应用，该领域专注于从（海量的）时空数据中获取时空信息或知识。
* 当数据采集跨越了时间与空间维度，并且至少包含一个空间属性和一个时间属性时，时空模型便应运而生。
* 时空数据集中的 **“事件（event）”** 描述了在特定时间 $t$ 和特定位置 $x$ 处发生的时空现象。
    * 例如，2000年1月至2025年1月期间中国城市扩张的模式。其中的时间属性是指该空间对象（城市）具有现实有效性的时间戳或时间区间。
* **应用场景**：涵盖了地球科学、生物学、生态学、气象学、医学、交通运输以及林业等诸多领域的实际案例。

---

## 三、 时空分析方法 (Methods for Spatio-Temporal Analysis)

时空分析主要包括四个方面：可视化、管理、分析和共享。

### 1. 可视化时空数据 (Visualizing spatio-temporal data)

* **形式**：在 ArcGIS 中可以以 2D 甚至 3D 的形式呈现时空数据。
* **实现步骤**：
    * 第 1 步：在图层属性中启用时间 (Enable time on a layer)。
    * 第 2 步：使用 **时间滑块 (Time Slider)** 控制可视化播放、暂停、步进等。

### 2. 管理时空数据 (Managing spatio-temporal data)

* **存储**：将时间数据存储在特定的日期字段中，并对其建立索引以提高性能。也支持特定的字符串或数字格式。
* **形状或位置随时间变化**：可以将每个阶段作为独立的要素存储，或以累积的方式显示。
* **数据管理工具**：
    * 日期格式转换：使用 `Convert Time Field` 工具。
    * 处理跨度时间：若有开始时间需计算结束时间，使用 `Calculate End Time`。
    * 属性转置：若时序数据分散在多个列中，需使用 `Transpose Fields` 工具转为“一对多”的表结构。
    * 连接表：时间数据存放在单独表中，使用 `Add Join`。
    * 时区管理：使用 `Convert Time Zone` 工具处理跨时区数据，处理夏令时（建议标准化为 UTC 或 GMT）。

### 3. 分析时空数据 (Analyzing spatio-temporal data)

主要包含三种处理时间维度的方法框架：

#### 方法一：每个时间步长单独分析 (1st Approach)
* **机制**：对每个时间步长的数据进行单独、分开的分析。
* **操作**：几乎所有的地理处理 (GP) 工具都能识别时间设置；或者将时间属性指定为 **Case field（案例/分组字段）**（如计算不同年份的平均中心 Mean Center 或 方向分布 Directional Distribution）。
* **结果**：输出结果是单一图层或每个时间步长单独的图层。

#### 方法二：使用时空约束进行分析 (2nd Approach)
* **机制**：在分析中同时考虑空间和时间的约束，用**时空窗口 (ST window)**（指定的临界距离和固定的时间间隔）来定义要素间的关系。
* **操作与工具**：
    * 探索性工具：热点分析 (Hot Spot Analysis)、聚类和异常值分析、空间约束多元聚类等。
    * **【时间序列预测工具 (Time Series Forecasting Tools)】**：
        * 曲线拟合预测 (Curve Fit Forecast)。
        * 指数平滑预测 (Exponential Smoothing Forecast)。
        * **基于森林的预测 (Forest-based Forecast)**：利用随机森林对每个位置的时间序列进行预测。
        * 按位置评估预测 (Evaluate Forecasts by Location)：比较并选择最优模型。
* **结果**：输出结果为一个单独的图层，表示考虑了时空约束后的汇总分析结果。

#### 方法三：所有时间步长同时在时空中进行分析 (3rd Approach)
* **机制**：对所有时间步长的数据同时在空间和时间维度上进行系统性分析。
* **操作**：使用 **时空模式挖掘工具箱 (Space-Time Pattern Mining Tools)**。这要求数据必须预先构建为**时空立方体 (Space-Time Cube)**。

**【重要概念】：时空立方体与条柱 (Space-Time Cube and Bin)**
* **时空立方体**本质上是一个**矢量数据结构**（如 `.nc` NetCDF文件）。
* **条柱 (Bin)**：立方体由许多条柱组成，每个 Bin 在空间 $(x, y)$ 和时间 $(t)$ 上都有固定的位置。覆盖相同空间的 Bins 具有相同的 Location ID；覆盖相同时段的 Bins 具有相同的 Time step ID。立方体结构始终是矩形的（无数据的位置计数为零）。

该工具箱包含以下三种核心分析：
1. **新兴时空热点分析 (Emerging Hot Spot Analysis)**：
    * **功能**：不仅识别热点和冷点，还能评估其**时间趋势**（如新增、连续、加剧、持续、减少、零星、震荡、历史热点等）。
    * **原理**：先计算每个 Bin 的 **Getis-Ord $Gi^*$ 统计量**；再使用 **Mann-Kendall 趋势检验** 评估各位置热点/冷点的变化趋势。
    * *【考点】Mann-Kendall 趋势检验*：非参数秩相关分析。通过比较相邻时间段的Bin值（后值大于前值记为+1，小于记-1，相等记0），将比较结果求和，并转化为 Z得分 和 p值 判断趋势（正Z表示上升趋势，负Z表示下降趋势）。
2. **局部异常值分析 (Local Outlier Analysis)**：
    * **功能**：判断随时间推移，各位置是属于空间聚类还是空间异常值。
    * **原理**：计算每个 Bin 的 **Anselin Local Moran's I** 统计量。生成诸如高-高、低-低聚类或高-低、低-高异常值的 2D 地图分类结果。
3. **时间序列聚类 (Time Series Clustering)**：
    * **功能**：根据时间序列特征的相似性对数据集合进行分区和聚类。
    * **原理**：可基于 特征值 (Value)、相关性 (Profile-Correlation) 或 傅里叶变换特征 (Profile-Fourier) 进行相似性度量与聚类。

### 4. 共享时空数据 (Sharing spatio-temporal data)

* **形式**：可以将时空数据共享为：地图或图层包 (Map or layer package)、导出的图片序列、动画视频 (Video)、启用时间的 Web 图层/服务、以及 Web 地图册。
* **Web 共享注意事项**：
    * 发布到 ArcGIS Online 时，需启用时间属性。
    * **注意**：应避免使用图层组 (group layers)；不要在时间字段上使用定义查询 (definition query)。

---

## 四、 案例 (Cases)

课件中列举了以下应用时空分析的真实研究案例：
* **网约车出行 (Ride-hailing travel)**：识别网约车出行的时空热点。
* **降雨变异性 (Rainfall variability)**。
* **绿色增长 (Green growth)**。
* **树木死亡率 (Tree-mortality)**。
* **全球土地覆盖变化 (Global land cover changes)**。
