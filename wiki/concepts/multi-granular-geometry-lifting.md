---
title: "Multi-Granular Geometry Lifting"
type: concept
tags: [概念, 3D-geometry, 特征提升, 几何感知]
sources: [raw/papers/2603.02329v1.pdf]
last_updated: 2026-05-06
---

## 定义

多粒度几何提升（Multi-Granular Geometry Lifting）是 HAMMER 框架中用于将 2D 意图嵌入提升为 3D 空间感知嵌入的模块。它通过逐步注入多尺度几何特征，使原本缺乏空间信息的接触感知嵌入获得 3D 几何感知能力。

## 关键信息

- **设计动机**：MLLM 提取的接触感知嵌入富含 2D 交互线索，但缺乏精确 3D 定位所需的空间信息
- **技术路线**：
  - 从 3D 解码器的中间层提取多尺度点特征（粗粒度 → 细粒度）
  - 逐层通过交叉注意力 + 残差连接 + FFN 将几何线索渐进注入嵌入中
  - 最终得到几何增强的 3D 感知嵌入 $f^{3D}_c$
- **与 InteractVLM 的对比**：InteractVLM 通过多视图投影+反投影将 2D 接触图提升到 3D，需要相机参数且可能产生几何不一致。HAMMER 的几何提升是通用的、无参化的方式
- **消融结果**：移除该模块后，PIAD Seen aIOU 从 22.20% 降至 20.46%，Unseen aIOU 从 13.71% 降至 10.34%
- **替代策略对比**：Single Lift（仅用最后阶段特征）和 Concat. Lift（简单拼接）均不如多粒度渐进式提升

## 关联连接

- [[HAMMER]] — 使用该技术的框架
- [[Cross-Modal Integration]] — 配套模块
- [[3D Affordance Grounding]] — 应用任务
- [[摘要-hammer-3d-affordance-grounding]] — 来源摘要
