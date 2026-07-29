# 空间分析课程复习笔记 - 第12讲 地理空间大数据分析

本笔记基于《Lect.12 Geospatial Big Data Analysis 2026.pdf》课件整理，严格遵循课件的结构与语言表述。

## 一、 地理空间大数据回顾 (Review of Geospatial Big Data)

### 1. 应用案例 (Application Cases)

课件展示了多个在顶级期刊（如 Nature, Science 等）发表的地理空间大数据应用案例：
1. **全球光伏太阳能发电场清单 (Nature, 2021)**：利用 550 TB 的 Sentinel-2 和 SPOT 影像，识别了近 7 万个光伏设施。
2. **首套全球城市土地利用数据集 (RSE, 2025)**：提出基于 POI 提示优化的城市土地利用制图框架 (PPUL-Net)，构建了 10 米分辨率的全球城市土地利用数据集 (GULU)。
3. **利用在线数据测量全球移民流动 (PNAS, 2025)**：Meta 团队基于 30 亿用户数据构建全球迁徙流量图谱。
4. **全球尺度树木密度测绘 (Nature, 2015)**：估算全球约有 3.04 万亿棵树。
5. **西非撒哈拉和萨赫勒地区树木计数 (Nature, 2020)**：采用高分影像和 UNet 语义分割识别出 18 亿棵树。
6. **21世纪高分辨率全球森林覆盖变化图 (Science, 2013)**：利用 GEE 处理海量像素。
7. **全球河流和溪流范围 (Science, 2018)**：计算出全球 >30 米宽度河流总长度。
8. **绘制世界自由流动的河流 (Nature, 2019)**：利用 DEM 和大坝数据分析河流连通性。
9. **全球河冰的过去和未来 (Nature, 2020)**。

### 2. 地理空间大数据的定义 (Definition of Geospatial Big Data)

* **大数据 (Big Data)**：指数据集如此之大，以至于无法由常见的数据库管理系统进行有效处理的数据集 (Dasgupta, 2013)。
* **地理空间大数据/空间大数据 (Geospatial/spatial Big Data 或 Big Geo-Data)**：指**超过当前计算系统处理能力**的空间数据集。虽然没有标准的最小大小阈值，但在 2013 年，1 PB (1000 TB) 或更大的数据通常被认为是大数据。

### 3. 地理空间大数据的 5V 特征 (Five V’s of Geospatial Big Data)

地理空间大数据同样具有大数据的 5V 特征：
1. **Volume（体量大）**：采集、存储和计算量庞大。指每秒产生的数据体量。
2. **Velocity（速度快）**：数据增长快、处理速度要求快。指数据产生和在不同数据集之间发生变化的速度。
3. **Variety（模态多）**：种类和来源多样化。可以使用结构化以及非结构化数据。
4. **Veracity（真伪难辨）**：数据的准确性和可信度低。涉及数据的可靠性和信任，需要验证和校验数据。
5. **Value（价值低）**：数据价值密度相对较低。获取大数据很好，但只有当我们能将其转化为价值时才有用。

* **大数据技术体系**：数据、算法、算力、平台。

### 4. 地理空间大数据的来源 (Sources of Geospatial Big Data)

大数据主要来自：
* **用户生成内容 (User-Generated Content)**：如智能手机打字、在线互动产生的数据。
* **传感器数据 (Sensor Data)** 产生自：
    * 卫星遥感 (Satellite remote sensing)
    * 航空（有人或无人机 UAV）测量 (Aerial surveying)
    * 雷达/激光雷达扫描 (Radar/Lidar Scanning)
    * 传感器网络 (Sensor networks)
    * 数码相机 (Digital cameras)
    * RFID 读数位置 (Location of readings of RFID)
    * 移动设备 (Mobile devices)
    * 物联网 (Internet of things)
    * 模型 (Models, such as WRF, PLUS, CASE...)
    * 全球导航卫星系统 (GNSS，包括北斗 BeiDou、GPS、伽利略 Galileo 以及 GNSS 使能设备)。
* **所有这些“智能 (Smart)”的“物体 (Things)”**：如 PC、平板电脑、智能手机、智能手表可穿戴设备、联网汽车、智能家居、智慧城市等。

**国际与国内主要数据平台源**：
* 国际：Openstreetmap, ArcGIS online, World Bank, Google Earth Engine, Eurostat, UN data library, US Census Bureau 等。
* 国内：国家地球系统科学数据中心 (www.geodata.cn)、国家综合地球观测数据共享平台、地理空间数据云、航天宏图 Pie engine 遥感与地理信息云服务平台、深时数据地球等。

### 5. 地理空间大数据的挑战 (Challenges of Geospatial Big Data)

* **数据库设计 (Database design)**：需要处理数据的多样性、海量存储 (volume) 以及读写速度 (velocity)。
* **算法和方法 (Algorithms and Methods)**：5V 特征挑战了传统的算法和方法，需要新的方法来理解这些数据。
* **网络限制 (Network limitations)**：传输大体量或高速度的数据存在瓶颈。
* **计算系统 (Computational Systems)**：普通台式 PC 通常无法处理大量或高速数据，需要超级计算机 (Supercomputers)。
* **地理可视化 (Geovisualization)**：快速对所有这些多样化的数据进行可视化是非常具有挑战性的。

---

## 二、 地理空间大数据的分析方法 (Methods for Geospatial Big Data Analysis)

主要分为 ArcGIS 中的方法、其他平台或开源方法、以及自主开发的方法。

### 1. ArcGIS 中的方法 (Methods in ArcGIS)

#### 1.1 GeoAnalytics 桌面工具 (GeoAnalytics Desktop Tools)

* **原理**：利用 **Apache Spark** 在桌面机器上提供了一个**并行处理框架 (parallel processing framework)**。
* **功能**：通过汇总 (summarizing)、聚合 (aggregation)、回归 (regression)、检测 (detection) 和聚类 (clustering)，可以对大数据进行可视化、理解和交互。能够让用户通过模式、趋势和异常来深入了解数据。
* **使用方式**：与 ArcGIS Pro 中的其他桌面地理处理工具以相同方式集成和运行。
* **工具集分类**：
  * **Summarize Data (汇总数据)**：Aggregate Points, Describe Dataset, Join Features, Summarize Center And Dispersion, Summarize Attributes, Summarize Within.
  * **Find Locations (查找位置)**：Detect Incidents, Find Similar Locations, Find Dwell locations.
  * **Analyze Patterns (分析模式)**：Calculate Density, Find Hot Spots, Find Point Clusters, Find Similar Locations, Forest-based Classification and Regression, Generalized Linear Regression.
  * **Use Proximity (使用邻近性)**：Create Buffers, Group By Proximity.
  * **Manage Data (管理数据)**：Calculate Field, Clip Layer, Overlay Layers, Dissolve Boundaries.

**【重点工具功能解析】**：
1. **聚合点 (Aggregate Points)**：将点聚合到面要素或条柱 (bins) 中。返回包含点计数以及可选统计信息的面。
2. **连接要素 (Join Features)**：基于空间、时间或属性关系（或其组合），将属性从一个图层连接到另一个图层（支持时空连接 spatiotemporal join）。
3. **重新构建轨迹 (Reconstruct Tracks)**：从启用了时间 (time-enabled) 的输入数据创建线或多边形轨迹。
4. **范围内汇总 (Summarize Within)**：将一个多边形图层与另一图层叠加，以汇总每个多边形内的点数、线长度或多边形面积，并计算属性字段统计信息。
    * *案例*：给定流域边界和土地利用边界，计算每个流域内每种土地利用类型的总英亩数。
5. **检测事件 (Detect Incidents)**：创建一个图层，用于检测满足给定条件的要素。
    * *术语 Track (轨迹)*：由轨迹标识符字段确定的、按时间排序的、时间类型为“瞬间 (instant)”的要素序列。
    * *术语 Incident (事件)*：满足感兴趣条件的要素。
6. **查找依靠/停留位置 (Find Dwell Locations)**：使用**给定时间和距离阈值**，查找移动对象停止或停留的位置。
    * *原理*：输入必须是表示瞬间的启用时间的点要素。停留位置被定义为在一段时间内几乎没有或没有移动的连续观测。根据应用领域，也称为停留点 (stay points) 或空闲检测 (idle detection)。输出可以是点、凸包 (convex hulls) 或平均中心。
7. **查找相似位置 (Find Similar Locations)**：根据要素属性，识别出与一个或多个输入参考要素最相似（或最不相似）的候选要素。

#### 1.2 GeoAnalytics 服务器工具 (GeoAnalytics Server Tools)

* **选择服务器工具优于桌面工具的情况**：
    1. 数据存储在**托管 (hosted)** 要素图层中。
    2. 分析输出将位于 ArcGIS Enterprise 中。
    3. 将使用**多台机器**进行分布式分析。
    4. 将使用 Linux、Web 应用程序或 Server 机器来完成分析。
    5. 将使用大文件集合或大数据文件共享源（如云存储、HDFS 或 Hive）。

#### 1.3 其他 ArcGIS 工具 (Other Method Tools)

* 除了 GeoAnalytics 工具外，ArcGIS 平台中的 **时空模式挖掘工具 (Space Time Pattern Mining Tools)** 和 **影像分析工具 (Image Analyst Tools)** 等也可用于分析地理空间大数据。

### 2. 其他平台或开源方法 (Methods in Other Platforms or from Open Sources)

* **国际/国内精选平台**：
    * **Google Earth Engine (GEE)**。
    * **Supermap Platform (超图)**。
    * **Movebank**：一个在线平台 (online platform)，帮助全球研究人员和野生动物管理者**管理、共享、分析和存档动物移动数据 (animal movement data)**。
    * **航天宏图 Pie Engine**。
* **开源方法 (Open Sources)**：
    * 了解最新的机器学习模型：**Papers With Code** (https://paperswithcode.com)
    * 从代码开始构建：**GitHub** (https://github.com)
