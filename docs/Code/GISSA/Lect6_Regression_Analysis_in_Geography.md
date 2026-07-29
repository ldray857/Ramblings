# 空间分析课程复习笔记 - 第6讲 地理数据的回归分析

本笔记基于《Lect. 6 Regression Analysis for Geographical Data》课件整理。

## 一、 回归分析简介 (Introduction to Regression Analysis)

* **概念**：回归分析用于模拟因变量（响应变量，dependent/response variable）和一组自变量（解释变量，independent/explanatory variables）之间的**因果关系 (causal relationship)**。它用于指定和检验变量间的函数关系。
* **作用**：提供一个简化现实的变量关系视图；提供用数据拟合模型的方法；提供评估变量重要性和模型正确性的手段。
* **回归类型的分类**：
    * **按自变量数量**：双变量回归 (Bivariate regression)、多元回归 (Multivariate regression)。
    * **按关系类型**：线性回归 (Linear regression)、非线性回归 (Non-linear regression)。
    * **按是否考虑空间关系**：传统回归 (Conventional regression)、空间回归 (Spatial regression)。
    * **按参数估计方法**：普通最小二乘法 (OLS)、最大似然法 (ML)、支持向量机 (SVM)、随机森林 (RF)、深度学习 (DL) 等。

---

## 二、 传统回归分析 (Conventional Regression Analysis)

### 1. 双变量线性回归 (Bivariate Linear Regression)

* **模型方程**：
    * **真实回归线 (True regression line)**： $y = \alpha + \beta x + \varepsilon$ （$\alpha$ 为真实截距，$\beta$ 为真实斜率，$\varepsilon$ 为真实随机误差）。
    * **拟合回归线 (Best-fitting line)**： $\hat{y} = a + bx$ （$\hat{y}$ 为因变量的预测值，$a$ 为估计截距，$b$ 为估计斜率，即自变量每变化一单位，因变量预期的变化量）。
    * 每个观测值 $y$ 可表示为： $y = a + bx + e = \hat{y} + e$ （$e$ 为残差 residual）。
* **普通最小二乘法 (Ordinary Least Squares, OLS)**：
    * **目标**：寻找使观测点到直线的**残差平方和最小化**的最佳拟合线（因数学便利性选择平方）。
    * $Q = \min \sum\limits_{i=1}^{n}(y_i - \hat{y}_i)^2 = \sum\limits_{i=1}^{n}(y_i - a - bx_i)^2 \rightarrow \min$
* **回归方差分解 (Explained and Unexplained Sums of Squares)**：
    * **总平方和 ($T$, Total sum of squares)** = **回归/已解释平方和 ($U$, Regression/Explained)** + **残差/未解释平方和 ($Q$, Residual/Unexplained)**。
    * 决定系数 $r^2$ 或 $R^2$ (Determination coefficient)：表示加权的自变量组合与因变量之间的最大相关性。
* **显著性检验**：
    * **方差分析 (ANOVA, F-test)**：用于决定回归是否成功解释了 $y$ 的显著变异部分。
        * $F = \frac{U/f_u}{Q/f_Q} = \frac{r^2(n-2)}{1-r^2}$ （服从自由度为 1 和 $n-2$ 的 F 分布）。若 $F > F_c$（临界值），则拒绝零假设 $r^2=0$。该 F 统计量是测试相关系数等于零的 t 统计量的平方。
    * **斜率检验 (Tests for Beta, t-test)**：检验真实斜率 $\beta = 0$ 的零假设（多元回归必须做）。 $t = \frac{b - \beta}{s_b}$。
* **置信区间 (Confidence intervals)**：
    * 有两个置信区间计算公式。对于“个体观测值 (individual observations)”预测的置信区间中包含 $+1$ 的项，因此其置信区间比“均值观测 (mean observations)”的置信区间要宽。
* **双变量回归操作7个步骤 (Illustration)**：
    1. 建立理论 (Establish theory)
    2. 检查数据 (Examine data：散点图、计算相关系数)
    3. 设定模型 (Specify regression model)
    4. 进行回归分析并评估性能 (Conduct regression，利用 $F$ 分布检验 $r^2$)
    5. 检查残差 (Examine residuals：构建残差图，残差应随机分布)
    6. 写出回归方程 (Specify equation)
    7. 构建预测的置信区间 (Construct confidence intervals)
* **ArcGIS Pro 中的工具**：**广义线性回归 (Generalized linear regression, GLR)** 工具。可以拟合连续型(OLS)、二值型(Logistic) 和计数型(Poisson) 模型。

### 2. 双变量非线性回归 (Bivariate Nonlinear Regression)

* **基本思想**：对于非线性关系，通过替换原始变量将它们转换为线性关系。
    * **指数曲线 (Exponential curve)**: $y = de^{bx} \rightarrow y' = \ln y, x' = x$
    * **对数曲线 (Logarithmic Curve)**: $y = a + b\ln x \rightarrow y' = y, x' = \ln x$
    * **幂函数曲线 (Power function curve)**: $y = dx^b \rightarrow y' = \ln y, x' = \ln x$
    * **双曲线 (Hyperbolic Curve)** 和 **S型曲线 (S-curve)**。
* **分形维数 (Fractional dimension)**：由 Benoit Mandelbrot 提出自相似性。可以利用双对数回归模型计算分形维数（如森林景观的面积和周长关系， $D = 2/\alpha$）。

### 3. 多元回归 (Multivariate Regression)

* **模型方程**： $\hat{y} = a + b_1x_1 + b_2x_2 + \dots + b_px_p$。在 $p+1$ 维空间的超平面中最小化残差平方和。系数表示在控制其他变量不变的情况下，自变量增加一单位对因变量的影响。
* **多重共线性假设 (Multi-collinearity)**：
    * 假设自变量之间**没有多重共线性**（自变量之间的相关性不应过高）。
    * **影响**：如果共线性很高，系数估计对个体观察变得极其敏感，微小的数据增删会导致系数剧变；且系数方差膨胀，导致本不显著的自变量表现为显著。
    * **解决方法**：逐步回归 (Stepwise Regression)、主成分分析 (PCA)、随机森林 (Random forest) 等。
* **设定错误 (Misspecification Error)**：若遗漏了重要解释变量，会导致现有自变量的真实效应被高估或低估，影响 $t$ 检验准确性。
* **模型对比检验 ($R^2$ 与 调整后 $R^2$)**：
    * $Adjusted R^2 = 1 - \frac{(1-R^2)(n-1)}{n-p-1}$ （$n$为样本数，$p$为自变量数。调整后$R^2$严格小于原$R^2$）。调整后 $R^2$ 惩罚了无关变量的增加，用于多元回归模型性能对比。

---

## 三、 地理加权回归 (Geographically Weighted Regression, GWR)

### 1. 典型的线性回归与 GWR 模型的对比 (Typical Linear Regression vs. GWR Model)

#### 典型的线性回归 (Typical Linear Regression)
* **基本概念**：回归建立了一个因变量 (dependent variable) 与一组自变量 (independent variables) 之间的关系。
* **模型方程**：典型的线性回归模型如下：
  $$ y_i = \beta_0 + \beta_1 x_{1i} + \beta_2 x_{2i} + \dots + \beta_m x_{mi} + \varepsilon_i $$
  *其中 $y_i$ 是因变量，$x_{ki}$ ($k$ 从 $1$ 到 $m$) 是一组自变量，$\varepsilon_i$ 是残差，所有这些变量值都在位置 $i$ 处。*
* **应用于空间数据的局限性**：当应用于空间数据时，可以看出，它假设了一个**平稳的空间过程 (stationary spatial process)**。
  * 即：在研究区域的所有部分，相同的刺激会引起相同的反应 (The same stimulus provokes the same response in all parts of the study region)。
  * 但这对于空间过程来说是**极其站不住脚的 (Highly untenable for spatial process)**。

#### GWR 模型 (GWR Model)
* **基本概念**：GWR 是一种用于分析关系中空间变化的**局部统计技术 (Local statistical technique)**。
* **理论基础**：它假设存在空间非平稳性 (Spatial non-stationarity) 并将对其进行测试。这主要基于**“地理学第二定律” (Second Law of Geography)**。
* **核心优势**：直接解决非平稳性问题 (Addresses the non-stationarity directly)。它允许变量之间的关系在空间上发生变化，即回归系数 $\beta_k$ 不需要各处都相同。
* **模型演变与本质 (The essence of GWR)**：
  在线性形式中，GWR 从全局回归系数转变为局部回归系数：
  $$ y_i = \beta_{0i} + \beta_{1i} x_{1i} + \beta_{2i} x_{2i} + \dots + \beta_{mi} x_{mi} + \varepsilon_i = \beta_{0i} + \sum_{k=1}^m \beta_{ki} x_{ki} + \varepsilon_i $$
  进一步表示为带有地理坐标的形式：
  $$ y_i = \beta_0(u_i, v_i) + \sum_{k=1}^m \beta_k(u_i, v_i) x_{ki} + \varepsilon_i $$
  *其中 $u_i$ 和 $v_i$ 是位置 $i$ 的坐标。*
  **结论**：$\beta_k$ 不再是各处保持不变，而是根据位置 ($i$) 发生变化 (Instead of remaining the same everywhere, $\beta_k$ now vary in terms of locations)。

### 2. 空间非平稳性 / 异质性 (Spatial Non-stationarity / Heterogenicity)

* **定义**：同样的刺激在研究区域的不同部分会引起不同的反应。
    * **全局模型 (Global models)**：假定空间过程是平稳的，参数独立于位置 (location independent)（如传统OLS）。如果将非平稳过程强制用全局模型拟合，可能得出错误结论，且模型残差可能存在高度的空间自相关。
    * **局部模型 (Local models)**：全局模型的空间分解，结果是**依赖于位置的 (location dependent)**。
* **关系在空间上变化的原因**：
    1.  抽样变异 (Sampling variation)
    2.  干扰变异，非真实空间非平稳性 (Nuisance variation)
    3.  关系跨空间内在不同，即**真正的空间非平稳性 (Real spatial non-stationarity)**
    4.  模型设定错误 (Model misspecification)
* **辛普森悖论 (Simpson's Paradox)**：揭示了空间异质性导致对真实关系误判的风险。全局数据拟合可能呈现一种负相关，但在局部空间群组内，真实的局部关系其实是正相关。

### 3. GWR 模型的参数估计与优化

* **参数估计**：使用**局部加权最小二乘法** (Local Weighted Least Squares)，其中权重矩阵与位置相关联。
    * $\hat{\beta}_k(u_i, v_i) = [X^T W(u_i, v_i) X]^{-1} X^T W(u_i, v_i) Y$
    * $W(u_i, v_i)$ 是空间权重矩阵 (对角线值为权重 $w_{ij}$，也叫核函数)。
* **空间权重方案 (Typical weighting types)**：
    * 距离阈值 (Distance threshold)、反距离 (Inversed distance)。
    * **高斯或类高斯函数 (Gaussian or Gaussian-like)**：多数方案倾向于此类，反映了多数空间过程的依赖类型。
    * **双平方函数 (Bi-Square Function)**：结合了距离阈值和高斯函数的优点。
* **带宽机制 (Bandwidth)**：高斯或类高斯函数可以是固定或自适应的。
    1.  **固定带宽 (Fixed Bandwidth)**：搜索半径恒定。**问题**：在数据稀疏处可能产生大的估计方差，在数据密集处又会掩盖细微的局部变化。在极度稀疏处可能因样本数不足而无法计算参数。
    2.  **自适应带宽 (Adaptive Bandwidth)**：根据数据密度自动调整。数据密集时带宽变短，稀疏时带宽变长（常用寻找K最近邻方法）。
* **空间带宽的优化 (Optimization of Bandwidth)**：
    * GWR 结果对权重函数的具体形式不敏感，但**对带宽的取值极其敏感**。最优带宽需满足：
    1.  **最小交叉验证得分 (Least CV Score, Cross-Validation)**：留一法验证，最小化模型预测值与观测值之间的差异平方和。
    2.  **最小赤池信息量准则 (Least AIC)**：基于信息论。AIC权衡了模型的**拟合优度 (goodness of fit)** 和 **简单性 (simplicity)**。它同时应对了模型**过拟合 (overfitting)**和**欠拟合 (underfitting)**的风险。

### 4. GWR 检验 (Tests of GWR)

判断 GWR 模型是否真的优于简单的全局线性回归模型：
1. **局部拟合度衡量 (Local Goodness-of-Fit Measures)**：
    * **局部 $R^2$ (Local $R^2$)**：计算每个位置加权的 $R^2$，指示局部的解释能力。
    * **残差空间分析 (Residual spatial analysis)**：使用 Moran’s I 检查残差是否为随机分布（空间自相关应不显著）。
2. **全局模型对比 (Global Model Comparison)**：
    * **调整后 $R^2$ 或 $AICc$**：对比 GWR 和全局模型。如果 GWR 的 $AICc$ 显著更低，则局部模型表现更好。
    * **蒙特卡洛检验 (Monte Carlo Tests)**：检验局部系数是否真的在空间上发生显著变化。通过随机打乱因变量，破坏原有的空间关系以生成零分布，对比观测系数与零分布。

---

## 四、 GWR 扩展及其他回归方法 (GWR Expansion & Other Regression Methods)

### 1. GWR 扩展模型

* **多尺度地理加权回归 (MGWR, Multiscale GWR)**（ArcGIS Pro中支持）：MGWR 在 GWR 基础上建立。传统 GWR 强制所有解释变量共享同一个带宽，而 MGWR 允许**每个解释变量拥有自己独立的带宽**，这意味着每个变量可以在不同的空间尺度上发挥作用。
* **时空加权回归模型 (GTWR)**：Geographically and Temporally Weighted Regression Model。
* **地理神经网络加权回归 (GNNWR)**。
* **地理时空神经网络加权回归 (GTNNWR)**。
* **地理卷积神经网络加权回归 (GCNNWR)**。

### 2. 其他回归方法 (Other Regression Methods)

引入机器学习与深度学习回归处理复杂问题：
* **随机森林回归 (Random forest, RF regression)**。
* **支持向量机回归 (Support vector machine, SVM regression)**。
* **深度学习回归 (Deep learning regression)**：如比较利用五种机器学习和五种卷积神经网络 (CNN) 通过无人机高光谱图像预测玉米产量的案例。

### 3. 案例研究 (Some Cases)

* 从时空视角理解建成环境对网约车出行的影响（2022）。
* 基于多尺度的 GIS 模型评估城市社会脆弱性与相关风险（2023）。
* 北京多种出行模式下绿地可达性与房价的关系（2024）。
