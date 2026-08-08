# 3D-Reconstruction-Notes

> 三维重建 / 点云处理 暑期学习笔记
>
> 段杰豪 · 2026年7月~8月

---

## 📚 学习路线

```
第一周                      第二周                          第三周                       第四周                         第五周
三维表示方法 ──────────→ 遥感图像语义分割 ──────────→ 点云语义分割 ──────────→ 新视角合成 ──────────→ 建筑重建
(理论基础)                (SegEarth-OV3)                (PointNet / PointNet++)       (3DGS vs NeRF)              (BWFormer + City3D)
```

---

## 目录

### [Week 1 — 三维表示方法总结](Week1-3D-Representation/3d-representation-methods-summary.md)

从零开始了解三维重建中的六种主流表示方法：点云、体素、网格、隐式 SDF、NeRF、3DGS。涵盖每种方法的本质、数据结构、优劣势、核心转换算法（SfM、MVS、ICP、Poisson 重建、Marching Cubes、TSDF 融合等），以及方法之间的转换关系。

### [Week 2 — 遥感图像语义分割](Week2-Remote-Sensing-SegEarth/SegEarth-OV3-LoveDA-Report.md)

复现 SegEarth-OV3（基于 SAM3 的开放词汇遥感语义分割）在 LoveDA 数据集上的实验。分析模型的三步推理流程（ViT 特征提取 → 文本引导双头分割 → 存在性过滤）、失败模式与 domain gap 的关系。

### [Week 3 — 点云语义分割 & PointNet++](Week3-PointCloud-PointNet/)

- **[理论笔记：从点云到 PointNet++](Week3-PointCloud-PointNet/PointNet-Learning-Notes.md)**
  - 点云 vs 图像的数据结构差异
  - PointNet 如何解决置换不变性（对称函数 + Max Pooling）和刚体变换不变性（T-Net）
  - PointNet++ 层次化特征提取：Set Abstraction（FPS + Ball Query + Mini-PointNet）+ Feature Propagation

- **[实践报告：PointNet++ on DALES](Week3-PointCloud-PointNet/PointNet++-DALES-Experiment.md)**
  - 在 DALES 航空 LiDAR 数据集上实现 8 类语义分割（mIoU 68.5%, OA 96.7%）
  - tile 级 vs block 级数据划分的教训（+13.6% mIoU）
  - 稀有类别（卡车 0.2%）的挑战与改进方向

### [Week 4 — 新视角合成：3DGS vs NeRF](Week4-3DGS-NeRF-Comparison/)

- **[理论笔记：如何从二维照片重建三维世界？](Week4-3DGS-NeRF-Comparison/novel-view-synthesis-theory.md)**
  - 新视角合成（NVS）问题定义
  - 传统管线：COLMAP/SfM → MVS → 网格重建 → 纹理贴图
  - NeRF：隐式 MLP + Ray Marching + Instant-NGP 加速
  - 3DGS：显式高斯椭球 + Splatting + 自适应密度控制
  - NeRF→INGP→3DGS 演化逻辑：逐步去 MLP 化

- **[对比实验：3DGS vs Instant-NGP](Week4-3DGS-NeRF-Comparison/3dgs-vs-nerf-experiments.md)**
  - 4 个场景（Train, Truck, Playroom, DrJohnson）定量 + 视觉对比
  - 3DGS 全面领先（PSNR +0.40~5.00 dB），Playroom 差距最大（+5.00 dB）
  - 核心发现：数据质量是两者的共同上限

### [Week 5 — 建筑重建：BWFormer + City3D](Week5-Building-Reconstruction/bwformer-city3d-report.md)

- 复现 BWFormer（CVPR 2025）——基于 Transformer 的建筑 3D 线框重建
  - 2.5D 化简策略：2D 角点检测 + 高度回归 + 边预测
  - 第一次训练（BF16, 50 epoch）结果未达预期，深入分析 5 个可能瓶颈
  - 第二次训练（FP32, 100 epoch）改进中
- 配置 City3D 作为对比基线——传统 PolyFit 几何方法
  - C++ 编译 + SCIP 开源求解器 + 数据格式适配
  - 两种方法互补：深度学习精度上限高 vs 传统方法无需训练

---

## 🔑 核心概念速查

| 概念 | 一句话解释 |
|------|-----------|
| **点云** | 传感器扫什么就存什么，最原生 |
| **体素** | 3D 版像素，CNN 友好但内存爆炸 |
| **网格** | 顶点+三角面，工业标准交付格式 |
| **隐式 SDF** | 连续数学函数描述形状，水密光滑 |
| **NeRF** | 神经网络记光场，照片级渲染 |
| **3DGS** | NeRF 的质量 + 实时的速度 |
| **PointNet** | 首个直接吃原始点云的深度网络 |
| **PointNet++** | 点云上的 CNN 金字塔——局部→全局层次化特征 |
| **SegEarth-OV3** | SAM3 + 文本 prompt → 遥感零样本语义分割 |
| **SfM** | Structure from Motion — 从多图恢复相机位姿和稀疏点云 |
| **MVS** | Multi-View Stereo — 从已知位姿的多图重建稠密点云 |
| **可微渲染** | 渲染过程可求导，让梯度从像素反传回 3D 参数 |
| **Instant-NGP** | 多分辨率哈希编码 + 微型 MLP，NeRF 训练从小时级降到秒级 |
| **Splatting** | 高斯椭球一次性投影到屏幕（vs NeRF逐像素射光线） |
| **Alpha Blending** | NeRF 和 3DGS 共享的体积渲染公式 |

---

## 📖 参考资料

- [PointNet (CVPR 2017)](https://arxiv.org/abs/1612.00593)
- [PointNet++ (NeurIPS 2017)](https://arxiv.org/abs/1706.02413)
- [NeRF (ECCV 2020)](https://arxiv.org/abs/2003.08934)
- [Instant-NGP (SIGGRAPH 2022)](https://arxiv.org/abs/2201.05989)
- [3D Gaussian Splatting (SIGGRAPH 2023)](https://arxiv.org/abs/2308.04079)
- [SegEarth-OV3](https://github.com/earth-vision/SegEarth-OV3)
