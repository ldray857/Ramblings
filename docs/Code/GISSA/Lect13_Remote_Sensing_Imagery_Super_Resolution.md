# 空间分析课程复习笔记 - 第13讲 遥感影像超分辨率重建 (Super-Resolution for Remote Sensing Image, RSI)

本笔记基于《Lect. 13 Super-Resolution for Remote Sensing Image (RSI)》课件整理，严格遵循课件的语言表述。

## 一、 遥感影像超分辨率重建回顾 (Review of RSI Super-Resolution)

### 1. 定义与类型 (Definition and Types of RSISR)
*   **图像超分辨率定义 (Definition of Image Super-Resolution)**：超分辨率 (SR) 是指从同一场景的一幅或多幅低分辨率 (low-resolution) 图像重建或生成高分辨率 (high-resolution) 图像的技术。
*   **RSISR 的类型 (Types of RSISR)**：
    *   **根据输入划分 (According to the input)**：
        1.  **单图像超分辨率 (Single-Image Super-Resolution, SISR)**：从一张低分辨率图像重建高分辨率图像。
        2.  **多图像/多帧超分辨率 (Multi-Image / Multi-Frame Super-Resolution, MISR/MFSR)**：从同一场景的多张低分辨率图像重建高分辨率图像，通常伴随亚像素偏移。
    *   **根据是否使用深度学习划分**：传统方法 (Traditional Methods) 与 深度学习方法 (Deep Learning Methods)。
    *   **根据是否使用监督方法划分**：有监督方法 (Supervised methods) 与 无监督方法 (Unsupervised methods)。
    *   **根据遥感影像模态划分**：光学 (Optical)、合成孔径雷达 (SAR)、红外 (Infrared) 和 激光雷达 (LiDAR) 超分辨率方法。

### 2. 为什么 RSISR 如此重要？ (Why Is RSISR Important?)
从 Web of Science 的数据来看，自2014年起关于 RSI 的 SR 算法论文数量逐年攀升。需要 RSISR 的根本原因是：许多对地观测应用需要比原始卫星或机载传感器所能提供的**更精细的空间细节 (finer spatial details)**。
1.  **克服传感器分辨率的限制 (To overcome sensor resolution limitations)**：遥感传感器受硬件、轨道高度、成本、重访频率和物理成像条件的限制。高分卫星昂贵且覆盖范围或重访时间有限。超分提供了一种**不改变传感器硬件**就能提高影像空间分辨率的方法。
2.  **获取更详细的地面信息 (To obtain more detailed ground information)**：低分辨率影像经常丢失小尺度的空间细节。超分可以使对象和边界（如建筑物、道路、河流、车辆、船舶、城市结构）更加清晰。
3.  **减少混合像元问题 (To reduce mixed-pixel problems)**：在中低分辨率影像中，一个像素可能包含多种土地覆盖类型。超分有助于提供更精细的空间信息，改善土地覆盖分类和目标提取。
4.  **改善下游遥感任务 (To improve downstream remote sensing task)**：高分辨率影像可以改善土地利用/覆盖分类、目标检测、语义分割、变化检测、灾害评估、城市监测、农业和环境监测等。
5.  **提升现有遥感数据的价值 (To enhance the value of existing remote sensing data)**：提升历史低分辨率档案影像的视觉质量和分析可用性，使其对长期研究（如城市扩张、植被变化、气候分析）更有价值。
6.  **平衡空间、光谱和时间分辨率的权衡 (To balance spatial, spectral, and temporal resolution trade-offs)**。

### 3. RSISR 的应用场景 (Application Scenarios of RSISR)
*   **核心作用**：在不改变传感器硬件条件的情况下，提高遥感影像的空间分辨率，从而增强目标细节表达、解译能力和下游任务性能。
*   **应用场景1：细节增强**。增强纹理、边缘和形状信息，使小目标或边界更清晰，有助于自动识别与人工判读。提升：目标可分辨性、边界完整性、纹理清晰度、检测与分割精度。
*   **应用场景2：下游任务提升类**。分类、目标检测、分割、变化检测。
*   **应用场景3：专业监测类**。农业、林业、城市、灾害、军事。
*   **应用场景4：数据融合与预处理**。多源分辨率匹配、高光谱空间增强、历史数据再利用。

*开源数据集包括：UCMD, WHU-RS19, RSSCN7, AID, KOSD, SIRI-WHU, NWPU-RESISC45, PatternNet, DOTA, SpaceNet2, DFC2019, OPTIMAL-31, RSI-CB。*

---

## 二、 传统超分方法 (Traditional Methods for RSISR)

主要包括插值法、基于稀疏编码法、基于回归法、全色锐化法等。

### 1. 插值法 (Interpolation)
*   **代表算法**：最近邻插值、双线性插值、**双三次插值 (Cubic Convolution)**。
*   **原理**：通过已知像素点的几何位置关系，直接估计未知像素的灰度值。
*   **双三次卷积插值核心思想**：
    *   插值新像素值，不取最近点、不取平均。
    *   以待求像素为中心，选取周围 4行4列共 16 个原始像素。
    *   用三次多项式卷积核计算每个像素的权重，加权求和得到新像素灰度值。
    *   **标准三次卷积基函数** ($x$ 为目标点与原始像素的距离偏移量)：
        $$ W(x) = \begin{cases} 1 - 2|x|^2 + |x|^3, & 0 \le |x| < 1 \\ 4 - 8|x| + 5|x|^2 - |x|^3, & 1 \le |x| < 2 \\ 0, & |x| \ge 2 \end{cases} $$
*   **局限性**：本质是信号重采样，无真实信息恢复，图像边缘易模糊或出现锯齿，高频细节丢失严重。

### 2. 基于稀疏编码法 (Sparse coding-based methods)
包含三个算法步骤：
1.  **字典学习 (Dictionary Learning)**：从配对的低/高分训练图像块中学习 LR/HR 字典，建立稀疏表示对应关系。
2.  **稀疏编码 (Sparse Coding)**：对输入LR图像块提取特征，并在LR字典上求解稀疏系数。
3.  **高分重建 (HR Reconstruction)**：利用稀疏系数在HR字典中重建对应的HR图像块并融合。

### 3. 基于回归法 (Regression-based methods)
将超分视为回归问题，学习 LR 和 HR 图像之间的函数映射。通常提取 LR 特征训练回归模型，再预测 HR 图像。
包含核心回归 (Kernel Regression)、支持向量回归 (SVR)、广义回归神经网络 (GRNN) 等。

### 4. 全色锐化 (Pan Sharpening)
使用较高分辨率的全色影像（或波段）与较低分辨率的多光谱数据集融合，生成具备全色栅格分辨率的多光谱数据集。
*   **Brovey 变换**：将重采样的多光谱像素乘以（相应全色像素亮度 / 所有多光谱亮度总和）。
    *   3个波段：$Red\_out = Red\_in / [(Blue\_in + Green\_in + Red\_in) \times Pan]$
*   **Esri 全色锐化**：使用加权平均和附加近红外波段创建。$WA = W_R R + W_G G + W_B B$ （权重和为1，取决于灵敏度曲线与全色波段重叠程度），$ADJ = pan - WA$，最终输出如 $Red\_out = R + ADJ$。
*   **Gram-Schmidt**：核心是用**向量正交化**分离空间与光谱信息。视波段为高维向量，逐次正交化得到互不相关分量，用高分 PAN 替换第一正交分量，再逆变换重建高分多光谱影像。
*   **IHS 变换 (Intensity-Hue-Saturation)**：将多光谱转为 IHS 空间。保留色调 H（地物颜色属性）和饱和度 S，用高分 PAN 替换亮度 I（明暗/空间纹理信息），再逆变换回 RGB。

### 5. 传统方法的弱点 (Weaknesses)
*   研究主要集中在 2016 年之前。计算相对简单，适合硬件受限场景。
*   **严重依赖局部像素关系**，无法充分提取深层特征和高级语义信息，导致性能不佳。
*   在高倍数 (如 4x 或 8x) 重建任务中，往往无法恢复足够细节，导致模糊或失真。例如，回归方法极易受噪声影响，降低重建质量。

---

## 三、 深度学习超分方法 (Deep Learning Methods for RSISR)

### 1. 基于 CNN 的方法 (CNN-Based RSISR)
*   **SRCNN (Super-Resolution CNN)**：
    *   于 2014 年提出，是深度学习图像超分领域的**开山之作**，奠定了基于 CNN 的单图像超分辨研究范式。
    *   流程：图像块提取 (对LR图像插值放大后利用卷积提取浅层特征) $\rightarrow$ 非线性映射 $\rightarrow$ 图像重建。
    *   **优点**：结构简洁，效果显著优于传统方法。
    *   **缺点**：计算效率偏低 (在放大后的 HR 特征图上运算)，感受野较小且依赖预处理插值。
*   **msiSRCNN**：第一个基于 CNN 且利用遥感数据 (Sentinel-2) 训练和微调 SRCNN 的 RSISR 模型。
*   **ESPCN (Efficient Sub-Pixel CNN)**：
    *   **核心思想**：提出 **亚像素卷积层 (Sub-pixel Convolution)**，引入高效亚像素卷积实现端到端上采样。彻底颠覆了传统“先插值放大、后卷积”的流程。网络在 LR 特征图上完成所有卷积，最后通过亚像素卷积层的像素重排机制映射为 HR 图像。极大降低了计算复杂度。
    *   **优点**：推理速度极快；重建效果优于 SRCNN。
    *   **缺点**：网络结构相对简单，感受野较小，对复杂纹理恢复能力不如深层网络，大尺度受限。
*   **其他**：EDSR (移除批量归一化，深层残差)、RCAN (通道注意力)、RNAN、HAN 等。

### 2. 基于 Transformer 的方法 (Transformer-based RSISR)
结合 CNN 局部特征提取能力与 Transformer 全局特征建模优势。
*   **SwinIR** (2021)：基于窗口自注意力机制的经典超分模型。
*   **HAT** (2023)：结合通道注意力与窗口自注意力、重叠交叉注意力模块。
*   **TTST** (2024)：针对遥感图像特性设计，采用 Top-k Token 可选择注意力机制。

### 3. 其他深度学习方法
*   基于 GAN 的 RSISR。
*   基于 Mamba 的 RSISR。
*   基于 Diffusion model (扩散模型) 的 RSISR。
*   Hybrid RSISR methods (混合方法)。

---

## 四、 性能评估方法 (Performance Evaluation Methods)

*   **定性评估 (Qualitative evaluation)**：
    *   视觉比较 (Visual Comparison)
    *   专家视觉评估 (Expert visual evaluation)
    *   应用引导评估 (Application-guided evaluation)
*   **定量评估 (Quantitative evaluation)**：
    *   **MSE**：均方误差 (mean square error, 课件中写作 mean square area)
    *   **RMSE**：均方根误差 (root mean square error, 课件写作 root mean square area)
    *   **PSNR**：峰值信噪比 (peak signal-to-noise ratio)
    *   **SSIM**：结构相似性 (structural similarity)
    *   **LPIPS**：感知图像块相似性 (learned perceptual image patch similarity)

---

## 五、 未来展望 (Future Prospects)

1.  在 RSISR 中进行真实世界退化建模 (Real-world degradation modeling in RSISR)。
2.  RSISR 中的基础模型和迁移学习 (Foundation models and transfer learning in RSISR)。
3.  多模态 RSISR 方法 (Multi-modal RSISR methods)。
4.  更好的评估指标和基准测试 (Better evaluation metrics and benchmarks)。
5.  基于 Diffusion-Mamba 的 RSISR 方法。

---

## 六、 研究案例 (Cases)

课件列举了几个相关的文献案例：
1.  通过带有分层密集采样和链式训练的生成对抗网络进行单幅遥感图像超分辨率。
2.  使用 Landsat 光谱-时间指标的超分辨率和中心块分类以 10米 分辨率绘制 2000 年至 2022 年城市建成区类型。
3.  SeG-SR: 通过视觉-语言模型将语义知识集成到遥感图像超分辨率中。
4.  通过 STARS-Net (一种时空注意力参考的超分网络) 从 1985 到 2015 检索 10米 分辨率的历史图像。
