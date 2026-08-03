# 3D-Reconstruction-Notes

> 三维重建 / 点云处理 暑期学习笔记
>
> 段杰豪 · 2026年7月–8月

---

## 📚 学习路线

```
第一周                      第二周                          第三周                      第四周
三维表示方法 ──────────→ 遥感图像语义分割 ──────────→ 点云语义分割 ──────────→ 高斯泼溅
(理论基础)                (SegEarth-OV3)                (PointNet / PointNet++)       (3DGS vs NeRF)
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

### [Week 4 — 高斯泼溅 & 新视角合成](Week4-Gaussian-Splatting/)

- **[理论笔记：如何从二维照片重建三维世界？](Week4-Gaussian-Splatting/theory.html)**
  - 从多视图几何到神经渲染的完整脉络
  - SfM（稀疏点云）→ MVS（稠密点云）→ 传统重建 vs 神经渲染
  - 3D Gaussian Splatting 核心原理：从点云到高斯椭球体的可微渲染

- **[实践报告：3DGS vs NeRF — 新视角合成对比实验](Week4-Gaussian-Splatting/experiments.html)**
  - 在多个场景（tandt_truck, tandt_train, db_drjohnson, db_playroom）上对比 3DGS 和 NeRF（Instant-NGP）
  - 训练速度、渲染质量（PSNR/SSIM/LPIPS）、推理 FPS 三维度评估
  - 实验结果可视化与对比分析

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

---

## 📖 参考资料

- [PointNet (CVPR 2017)](https://arxiv.org/abs/1612.00593)
- [PointNet++ (NeurIPS 2017)](https://arxiv.org/abs/1706.02413)
- [NeRF (ECCV 2020)](https://arxiv.org/abs/2003.08934)
- [3D Gaussian Splatting (SIGGRAPH 2023)](https://arxiv.org/abs/2308.04079)
- [Instant-NGP (SIGGRAPH 2022)](https://arxiv.org/abs/2201.05989)
- [SegEarth-OV3](https://github.com/earth-vision/SegEarth-OV3)
