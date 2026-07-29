# 空间分析课程复习笔记 - 第9讲 机器学习分类

本笔记基于《Lect. 9 Machine Learning Classification -2026.pdf》课件整理，严格遵循课件的语言表述。

## 一、 什么是机器学习 (What is Machine Learning)

* **定义**：机器学习是人工智能和计算机科学的一个分支，专注于使用数据和算法来模仿人类学习的方式，逐渐提高其准确性。
    * 它是不断发展的大数据科学领域的重要组成部分。
    * 通过使用统计方法，训练算法进行分类或预测，从而揭示数据挖掘项目中的关键见解。
* **机器学习、人工智能与深度学习的关系**：
    * **人工智能 (Artificial Intelligence, AI)**：能够感知、推理、行动和适应的程序（自1956年）。
    * **机器学习 (Machine Learning)**：性能随着时间推移暴露于更多数据而提高的算法（自1980年代）。
    * **深度学习 (Deep Learning)**：机器学习的子集，多层神经网络从海量数据中学习（自2010年代）。
    * 机器学习、深度学习和神经网络都是人工智能的子领域。深度学习实际上是机器学习的子领域，深度神经网络是深度学习的子领域。
* **机器学习的工作原理 (How Machine Learning works)**，主要分为三部分：
    1. **决策过程 (A Decision Process)**：通常用于进行预测或分类。基于一些有标签或无标签的输入数据，算法将产生对数据模式的估计。
    2. **误差函数 (An Error Function)**：用于评估模型的预测。如果有已知样本，误差函数可以进行比较以评估模型的准确性。
    3. **模型优化过程 (An Model Optimization Process)**：如果模型能更好地拟合训练集数据，则调整权重以减少已知样本与模型估计之间的差异。算法将重复此评估和优化过程，自主更新权重，直到达到准确度阈值。
* **机器学习方法的三大主要类别 (Three Primary Categories of Machine Learning Methods)**：
    1. **监督机器学习 (Supervised machine learning)**：使用**有标签 (labeled)** 数据集来训练算法，以准确地分类数据或预测结果。模型在交叉验证过程中调整权重，以避免过拟合或欠拟合。常见方法包括神经网络、朴素贝叶斯、线性回归、逻辑回归、**随机森林 (random forest)**、支持向量机 (SVM) 等。
    2. **无监督机器学习 (Unsupervised machine learning)**：使用机器学习算法来分析和聚类**无标签 (unlabeled)** 数据集。发现隐藏模式或数据分组，无需人为干预。常用于降维（如主成分分析 PCA、奇异值分解 SVD），其他算法包括 k-means 聚类、ISODATA、概率聚类等。
    3. **半监督学习 (Semi-supervised learning)**：又称少样本学习 (few-shot learning)，是两者的折中。训练期间使用较小的标记数据集来指导特征提取和对较大未标记数据集的分类。解决没有足够的标记数据（或无法负担标记足够数据）来训练监督学习算法的问题。

---

## 二、 随机森林分类 (Random Forest Classification)

### 1. 决策树 (Decision Tree)

* **概念**：决策树是一种决策支持工具，使用树状模型来展示决策及其可能后果，包括偶然事件结果、资源成本和效用。它是展示仅包含条件控制语句的算法的一种方式。常用于图像分类等。
* **两类节点**：
    * **内部节点 (Internal node)**：根据对应属性可以取到的不同值将数据划分为不同分支。
    * **终端/叶节点 (Terminal/Leaf node)**：决定分配给样本的类别。
* **构建机制 (Mechanism to Construct)**：采用自上而下、贪婪搜索算法 (top-down, greedy search approach)。
    * 选择最佳属性 $a^*$ 放在树的根部；将训练集分为各个子集，每个子集的样本在 $a^*$ 上有相同的值；在每个新子集上递归应用该算法，直到样本具有相同的类别或样本很少。
* **拆分数据 (Splitting Data)**：
    * 哪种属性/特征是拆分数据的最佳选择？可以测量数据集的复杂性，并比较按不同属性分类结果的复杂性。**如果某个属性分类后的结果复杂性降低得更多，该特征就是最佳的**。
* **利用信息熵 (Information Entropy) 和信息增益 (IG) 拆分数据**：
    * **信息熵**：与随机样本集 $S$ 相关的随机变量的熵定义为 $H(S) = -\sum_{i=1}^{K} p_i \log_2 p_i$（$p_i$ 是第 $i$ 个结果的概率）。**熵越大，数据或信息越复杂。**
    * **信息增益 (Information Gain, IG)**：表示两个信息熵之间的差异。特征选择性越好，信息增益越大。$IG(S, X) = H(S) - \sum_{v \in Values(X)} (S_v/S)H(S_v)$。
* **典型决策树算法**：
    * **ID3**：迭代二分法，使用信息增益，只能处理离散特征，无缺失值处理，无剪枝。
    * **C4.5**：引入增益率，支持连续特征和缺失值处理，加入剪枝。
    * **C5.0**：C4.5 的改进版。
    * **CART (分类和回归树)**：使用**基尼指数 (Gini Index)** 代替熵，支持回归任务。$Gini(S) = 1 - \sum P_i^2$。基尼指数值越小，数据纯度越高。
* **数据过拟合 (Data Overfitting)**：
    * 树长得太深，捕获了数据中的“偏差/噪声”，损害了对未知样本的预测能力。这称为过拟合。
    * 导致原因：随机误差或噪声；巧合模式（小样本导致偏离模式）。
    * 解决办法：1. 提前停止树的生长（实践中很难实施）；2. **决策树剪枝 (Pruning)**：让算法停止甚至过拟合，然后将剪枝作为后处理步骤（此方法广受欢迎）。

### 2. 集成方法 (Ensemble methods)

* 单个决策树速度极快，但表现不佳。如果我们学习多棵树会怎样？我们需要确保所有树学到的不都是一样的。
* **Bagging (并行模式)**：
    * **Bootstrap aggregating**：一种能产生**低方差 (low variance)** 结果的方法。
    * **Bootstrap (有放回的抽样方法)**：随机以不同方式拆分数据。
    * **过程**：从数据集中有放回抽取 B 个样本，未抽中的称为 OOB (Out-of-Bag)。利用每个样本训练一个基分类器。最后聚合结果：分类通过**多数投票 (majority vote)**，回归通过平均值。
    * *随机森林是典型的 Bagging 集成方法。*
* **Boosting (串行模式)**：
    * 顺序拟合：为模型的残差拟合决策树，目标变量是残差而非 Y，逐步将新树加入函数以更新残差。
    * **顺序训练**：基学习器按顺序生成，后续模型关注前序模型的错误。**权重调整**：动态调整样本权重。**聚合方式**：加权投票或加权求和。
    * 常见算法：XGBoost, AdaBoost, GBDT, LightGBM。

### 3. 随机森林 (Random Forest)

* **定义**：由 Leo Breiman (2001) 提出。RF 是一种基于分类和回归树 (CART) 组合的集成学习技术。
* 每棵树通过 bagging 独立采样训练数据集和变量子集进行训练。
* **特点**：RF 通过使用**输入特征的随机子集的最佳拆分**来使树生长，而不是使用所有变量的最佳拆分。这降低了树之间的相关性和泛化误差。通常使用 Gini Index 作为拆分选择的测量标准。每棵树尽可能地生长，**没有剪枝**。
* RF 对处理大型数据集和多变量具有许多优势，且不易过拟合。
* 两个主要参数：每个节点随机子集中的活动变量数 ($m$)，以及森林中的树木数 ($B$)。
* **误差率影响因素**：
    1. **森林中任意两棵树之间的相关性**：相关性增加，错误率增加。
    2. **森林中每棵单树的强度**：单棵树强度增加（低误差），错误率降低。
    * 减小 $m$ 会同时降低相关性和强度。通常会有一个“最佳”的 $m$ 范围。
* **调参建议 (Tuning)**：
    * 对于分类，$m$ 的默认值为 $\sqrt{p}$ ($p$ 为总特征数)，最小节点大小为 1。
    * 对于回归，$m$ 的默认值为 $p/3$，最小节点大小为 5。
* **袋外误差估计 (Out-of-Bag Error Estimation, OOB)**：
    * 在 RF 中，**不需要**进行交叉验证或提供独立的测试集就能获得测试误差的**无偏估计**。
    * 每棵树约有 1/3 的样本（OOB 样本）未被使用。用它们作为测试集，得到每次的分类预测，计算 OOB 误差。
* **变量重要性 (Variable importance)**：
    * 在 OOB 数据中随机排列某个变量 $m$ 的值，计算未排列的 OOB 数据与排列了变量 $m$ 的 OOB 数据之间分类正确票数的差值平均数，即为变量 $m$ 的重要性得分。
* **迭代 (特征交互, Iterations)**：
    * 实验性功能：如果树中变量 $m$ 的拆分使得变量 $k$ 的拆分可能性系统地减小或增加，说明存在交互。
* **随机森林的优点 (Features and Advantages)**：
    * 最准确的学习算法之一。在大型数据库上运行高效。
    * 能处理数千个输入变量而无需删除。估计变量的重要性。
    * 在森林构建过程中生成无偏泛化误差估计。
    * 对估算缺失数据有效。在类别不平衡数据集中有平衡误差的方法。可保存森林以备后用。计算样本间的近似度，可用于聚类、异常值检测。可扩展到无标签数据。提供检测变量相互作用的方法。
* **随机森林的局限性 (Limitations)**：
    * 在一些具有噪声分类/回归任务的数据集上观察到了**过拟合**。
    * 对于包含不同层级/分类数 (number of levels) 的类别变量，随机森林会**偏向于包含更多层级的属性**。因此，对于这类数据，其变量重要性得分不可靠。
* **评估指标 (Evaluation Metrics)**：
    * precision = TP / (TP + FP)
    * recall = TP / (TP + FN)
    * IoU = TP / (TP + FP + FN)
    * kappa = 2(TP$\cdot$TN - FN$\cdot$FP) / ((TP+FP)(FP+TN) + (TP+FN)(FN+TN))

---

## 四、 研究案例 (Cases)

课件列举了几个应用机器学习分类技术的最新遥感与地理分析案例：
* Mapping global water-surface photovoltaics with satellite images-2023
* Mapping crop type in Northeast China during 2013-2021 using automatic sampling and tile-based image classification-2023
* Seamless and automated rapeseed mapping for large cloudy regions using time-series optical satellite imagery-2024
* Characterizing land use changes triggered by crop-aquaculture co-cultivation from 2013 to 2022 based on a robust classification framework: Illustration in Jianghan Plain, China, 2026, RSE
