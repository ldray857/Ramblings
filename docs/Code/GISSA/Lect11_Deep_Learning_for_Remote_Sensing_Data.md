# 空间分析课程复习笔记 - 第11讲 遥感深度学习：对象检测与像素级分类

本笔记基于《Lect.11 Deep Learning for remote sensing Data-2026.pdf》课件整理，严格遵循课件的语言表述。

## 一、 深度学习 (Deep Learning, DL) 概述

* **CNN, DL, ML 和 AI 的关系 (The Relationships of CNN, DL, ML and AI)**：
    * **人工智能 (AI)**：包含机器学习。
    * **机器学习 (ML)**：包含深度学习。
    * **深度学习 (DL)**：包含卷积神经网络 (CNN)。
    * CNN 是一种神经网络，广泛应用于遥感、AI导航等领域。
* **深度学习的应用 (Applications of Deep Learning)**：
    * 电子游戏（如AlphaGo、AlphaGo Zero）、智能导航 (Intelligent navigation)、面部识别 (Facial Identification)、AI 创作 (AI Creation)、AI 解释 (AI Interpretation, 如 CHATGPT, Deepseek)、元宇宙 (Metaverse)。
    * 图像分类 (Image Classification)、对象检测 (Object Detection)、像素分类 (Pixel Classification)、实例分割 (Instance Segmentation)、图像识别 (Image Recognition, 如受损房屋分类、棕榈树识别、土地利用分类、建筑物提取)。
* **近期相关本科毕业论文题目 (2025/2026)**：涉及大模型、多模态、生成式AI、图卷积、注意力机制、深度学习少样本分类等在遥感与地信中的应用。

## 二、 人工神经网络 (Artificial Neural Network, ANN)

### 1. 从人类神经元到人工神经元 (From Human Neurons to Artificial Neurons)

* **结构对应**：
    * 树突 (dendrites) $\rightarrow$ **Input (输入)** 与 **Dendrite (树突)**
    * 细胞核/细胞体 (nucleus/cell body) $\rightarrow$ **Summation (求和 $\sum$)** 与 **Cell body (细胞体)**，并应用**阈值 (Threshold)** 与激活函数 $f(.)$。
    * 轴突 (axon) $\rightarrow$ **Axon (输出)**。

### 2. 多层感知机模型 (Multilayer Perceptron, M-P/MLP Model)

* **模型公式**：
    $$ y = f(g(X)) $$
    其中，$X$ 是输入 ($x_1, x_2, \dots, x_n$) 及对应的权重 ($\omega_1, \omega_2, \dots, \omega_n$)；$g$ 是**组合函数 (Combination Function)**；$f$ 是**激活函数 (Activation Function)**。
* **两种主流组合函数 (Combination Functions)**：
    1.  **加权求和 (Weighted Sum)**：
        $$ g(X) = \sum_{i=1}^{n} \omega_i x_i - \theta \quad \text{或} \quad y = f\left(\sum_{i=1}^{n} \omega_i x_i - \theta\right) $$
        *用于普通感知机、BP神经网络、卷积网络、Transformer网络中。*
    2.  **径向距离 (Radial Distance)**：
        $$ \|X - C\| = \sqrt{\sum_{i=1}^{n} (x_i - c_i)^2} \quad \rightarrow \quad y = f\left(\sqrt{\sum_{i=1}^{n} (x_i - c_i)^2}\right) $$
        *主要用于径向基神经网络，用于判断样本离中心点有多远。*

### 3. 激活函数 (Activation Functions)

* **为什么要引入激活函数**：
    1. 没有激活函数，多层网络等价于单层线性模型，无法拟合复杂数据。
    2. 限制输出范围，防止数据爆炸。
    3. 提升模型表达能力，实现分类、拟合、特征区分等。
    4. 加快收敛、抑制梯度消失/爆炸。
* **常见激活函数类型 (Types)**：
    * 阈值函数 (Threshold)、线性函数 (Linear)、饱和线性函数 (Saturating Linear)、双曲正切Sigmoid函数 (Hyperbolic tangent Sigmoid)、高斯函数 (Gaussian)。
    * **Sigmoid**: $Sigmoid(x) = \frac{1}{1 + e^{-x}} \in (0, 1)$。以及用于多分类的 **SoftMax**。
    * **ReLU (Rectified Linear unit)**: $Max(0, x)$。

### 4. 人工神经网络的两个主要问题 (Two main issues in ANNs)

* **架构 (Architecture)**：如何互连各个单元（神经元）？
    * **前馈架构 (Feedforward Architectures)**：无循环 (without loops)、静态的 (static)。例如：**CNN**。
    * **反馈架构 (Feedback/Recurrent Architectures)**：有循环 (with loops)、动态非线性动力系统。例如：**LSTM**。包含侧抑制连接和反馈连接。
* **学习途径 (Learning Approach)**：如何自动确定连接权重甚至ANN的结构？
    * 解决这两个问题就能得到一个具体的 ANN。

### 5. 前馈架构细节与学习途径 (Feedforward Architecture & Learning Approach)

* **前馈架构**：通常包含输入层 (Input Layer)、多个隐藏层 (Hidden Layers)、输出层 (Output Layer)。
    * **核心**：ANN 的本质是用 $Y = AX + B$ 的组合来逼近输入和输出的映射。$W$（权重）和 $B$（偏置）参数可以通过损失函数的反向传播来确定/调整。
* **学习途径**：是让（模型）会正确的权重和结构的一整套方法，包括：
    1.  **损失函数 (Loss Function)**：是模型学习途径中的目标函数，告诉模型要往哪个方向努力。用于描述前馈网络结果与真实值之间的偏差，误差被反向传播以具体调整网络参数。
        * **均方误差 (Mean Squared Error)**：$E = \frac{1}{2}\sum_k (y_k - t_k)^2$
        * **交叉熵误差 (Cross Entropy Error)**：$E = -\sum_k t_k \log y_k$ ($y_k, t_k$ 分别为输出值和真实值)
    2.  **权重更新规则**：如反向传播+梯度下降 (Chain Rule, Back-Propagation, Gradient Descent)。计算梯度 $\rightarrow$ 按梯度下降最快的方向更新权重。
    3.  **优化算法**：如 SGD, ADAM, RMSprop。决定怎么改权重才能让损失变小，是学习的行动方案。
    4.  **学习策略**：如学习率、迭代次数、正则化。决定学多久、学多快、怎么避免学歪。
* **分类**：监督学习 (Supervised, 如 Alphago) 和 无监督学习 (Unsupervised, 如 Alphazero)。
* **ANN 分类器示例**：
    * 单个神经元相当于将输入数据分为两类。
    * 神经网络的多层特征识别：第一层识别颜色和简单纹理；第二层识别更详细的纹理（如布料、树叶）；第三层识别黄烛光、蛋黄；第四层识别狗脸、七星瓢虫；第五层识别花朵、圆屋顶、键盘等。

---

## 三、 卷积神经网络 (Convolutional Neural Networks, CNN)

* **定义**：CNN 是一类深度前馈人工神经网络，已成功应用于分析视觉图像。**LeNet** 是第一个提出的 CNN，也是所有 CNN 的基础架构（包含输入、卷积层 Convolutions、下采样层 Subsampling、全连接层 Full connection、高斯连接 Gaussian connections 等）。
* **全连接层参数数量 (Number of Parameters of Fully Connected Layer)**：
    * 一层的参数量 $= N_{in} \times N_{out} + N_{out} \text{ (bias)}$。例如：$3 \times 300 \times 300$ 输入节点到 $100$ 个输出节点，参数量为 $2.7 \times 10^8$。
* **卷积层参数数量 (Number of Parameters of Convolutional Layer)**：
    * 一层的参数量 $= (K_h \times K_w \times C_{in}) \times C_{out} + C_{out}$。例如：$5 \times 5$ 的核，$3$ 个输入波段，$10$ 个输出波段，参数量为 $(5 \times 5 \times 3) \times 10 + 10 = 760$。
    * *对比可见，卷积层参数量远小于全连接层。*
* **卷积层运算 (Convolutional Layer)**：$Y = XW + B \rightarrow Y = X \otimes G$
    * 运算过程为：$\text{Input} \otimes \text{Kernel} \rightarrow \text{Summation} + \text{Bias} \rightarrow \text{Output}$。
    * **窗口 (Window)**：尺寸大小为 $W \times H$。
    * **步幅 (Stride)**：卷积核滑动的步长。
    * **填充 (Padding)**：在边缘补充像素（如 Padding=0, 1, 2）以控制输出尺寸。
* **池化层 (Pooling Layer)**：
    * 通常设置滑动窗口（如 $2\times2$）和步长（如 Stride=2）来进行下采样。
    * **最大值池化 (Max-Pooling)**：取滑动窗口内的最大值。
    * **平均值池化 (Average-Pooling)**：取滑动窗口内的平均值。
* **典型的 CNNs (Typical CNNs)**：
    * **LeNet**：早期经典结构。
    * **AlexNet** (2012)：激活函数采用 **ReLU**，使用**局部响应归一化 (Local Response Normalization)**。
    * **VGG** (Visual Geometry Group, 2014)：
        * 使用较小的 $3 \times 3$ 卷积核代替 AlexNet 的大卷积核。卷积核越大，参数越多，感受野也越大。
        * **使用多个 $3\times3$ 堆叠的优势**：堆叠的卷积层能获得与大卷积核相同的感受野（例如 2个 $3\times3$ 等效于 1个 $5\times5$；3个 $3\times3$ 等效于 1个 $7\times7$）。
        * 这样可以带来**更强的非线性 (More non-linearity)** 并产生**更少的参数量 (Less parameters to learn)**（如 $3 \times 3^2 = 27$ 小于 $7^2 = 49$）。
    * **U-Net / UNet++**：
        * 呈 "U" 型架构，包含**收缩路径 (Contracting path / Encoder)** 和**扩张路径 (Expansive path / Decoder)**，常用于医学与遥感图像分割。
        * 核心包含 $3\times3$ 卷积与 ReLU、$2\times2$ 最大池化、$2\times2$ 上卷积 (up-conv) 等操作。
        * **跳跃连接 (Copy and crop)**：将收缩路径的高分辨率特征图复制并拼接到扩张路径中。
    * **ResNet**：包含基础和瓶颈**残差块 (Basic and bottleneck residual blocks)**。通过跳跃连接（加法）解决了**梯度消失或梯度爆炸 (Gradient vanishing or gradient exploding)** 问题。
    * **ResUNet / ResUNet++**, **DeepLabV3+** 等。
* **为什么 CNN 对遥感图像处理如此重要 (Why CNN is so important for remote sensing image processing)**：
    1.  **局部感受野**：适合识别道路、建筑物边缘、农田纹理；
    2.  **权重共享**：减少参数，提高效率；
    3.  **多层特征**：浅层识别边缘纹理，深层识别地物对象；
    4.  **多尺度结构**：适合遥感影像中大小不同的地物。

---

## 四、 其他深度学习网络 (Other DL Networks)

* **循环神经网络 (Recurrent neural networks, RNNs)**：例如长短期记忆网络 (LSTM)。
* **生成对抗网络 (Generative Adversarial Networks, GANs)**。
* **基于 Transformer 的网络 (Transformer-based Networks)**：核心思想是 **自注意力机制 (Self-Attention)**。
    * ViT (Vision Transformer)：一张图片相当于 16x16 个单词。
    * TNT, Swin Transformer, Swin-Unet 等。
* **Transformer + CNN 混合网络 (Hybrid Networks)**：几乎所有的基础模型 (foundation model) 都在使用。

---

## 五、 ArcGIS Pro 中对象检测与像素级分类的 DL 方法 (DL Methods in ArcGIS Pro)

* **支持的库和框架**：ArcGIS 中集成了 PyTorch, Keras, TensorFlow, scikit-learn 等深度学习库。
* **应用场景 (Application Scenarios)**：
    * **目标检测 (Object Detection)**：SSD, RetinaNet, YOLOv3, Fast-RCNN。识别物体位置。
    * **像素分类/语义分割 (Semantic Segmentation)**：对每一个像素划分类别。U-Net, PSPNET, Deeplab。
    * **对象分类 (Object Classification)**：对图像进行分类（是否含某物）。Feature Classifier。
    * **实例分割 (Instance Segmentation)**：在像素分类基础上切割出轮廓。MaskRCNN。
    * 其他包括：点云分割、边缘检测、图像翻译 (Pix2Pix, CycleGAN)、变化检测、图像标注、道路提取等。ArcGIS Pro 2.8 及以上支持超过 25 种深度学习模型。
* **深度学习工作流 (Workflow for DL in ArcGIS Pro)**：
    1.  **配置环境**：安装DL库，检查显卡、显存。
    2.  **步骤1：构建训练数据 (Prepare Training Data)**：
        * **标记对象 (Label Objects)**：使用 `Training Samples Manager` 标记对象，工具支持已有样本导入和编辑。
        * **导出切片 (Create Image Chips)**：使用 `Export Training Data for Deep Learning` 导出数据。
    3.  **步骤2：训练模型 (Train a Model)**：
        * **Epoch (训练轮次)**：默认为 20。
        * **Batch size (批量大小)**：一次计算的样本量，根据算力设置。
        * **Learning Rate (学习率)**：默认为空值，会根据曲线自主选择。
        * **Backbone Model (骨干模型)**：指定预先配置的网络（如 ResNet-34）。
        * **处理器类型 (Processor Type)**：CPU 或 GPU。
        * **训练结果文件**：`html`（精度指标页面）、`emd`（推断工具必须指定的配置文件，记录了框架、精度、类别信息）、`pth`（训练好的模型文件）、`dlpk`（深度学习包文件，用于web端）。
        * **结果评估 (Evaluating the Results)**：
            * **总体精度 (Overall Accuracy, OA)**：$OA = \frac{TP + TN}{TP + FP + FN + TN}$。
            * **精确率 (Precision)**：$Precision = \frac{TP}{TP + FP}$（查准能力）。
            * **召回率 (Recall)**：$Recall = \frac{TP}{TP + FN}$（查全能力）。
            * **F1-Score**：$F1-Score = \frac{2 \times (Precision \times Recall)}{Precision + Recall}$（类别不平衡时更稳健）。
            * **交并比 (IoU) 与 平均交并比 (mIoU)**：$IoU_c = \frac{TP_c}{TP_c + FP_c + FN_c}$。交集与并集之比。
    4.  **步骤3：使用模型 (Use the Model)**：进行推理（如像素分类、对象分类、目标识别）。
    5.  **步骤4：后处理 (Post Processing)**：利用 ArcGIS 提供的 1500+ 数据处理和分析工具（如众数滤波、栅格转面、细化、平滑PAEK、规则化建筑物面等）进行优化。

---

## 六、 研究案例 (Cases)

课件列举了几个使用卫星数据进行深度学习分析的实际文献案例：

1. 使用卫星图像评估全球土地覆盖变化：2001年至2020年的间歇性和长期土地覆盖变化。
2. 用于一般时间序列分析的时间二维变化建模 (2022)。
3. 使用 Sentinel-2 数据精确大规模监测网箱和筏式水产养殖的注意力融合深度学习模型 (2024)。
4. 使用指数-特征-空间注意力融合深度学习模型从 Sentinel-2 大规模绘制覆膜土地图 (2025)。
5. 通过调整 transformer 模型对用于大面积土地覆盖测图的原始不规则时间序列 (CRIT) 进行分类 (2024)。
