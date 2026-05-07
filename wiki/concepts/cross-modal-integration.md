---
title: "Cross-Modal Integration"
type: concept
tags: [概念, 跨模态, MLLM, 特征融合]
sources: [raw/papers/2603.02329v1.pdf]
last_updated: 2026-05-06
---

## 定义

跨模态整合（Cross-Modal Integration）指将来自不同模态（如图像、文本、3D 点云）的信息进行有效融合的技术。在 HAMMER 中，特指利用层次化交叉注意力机制将 MLLM 的隐藏状态信息融合到 3D 点云特征中。

## 关键信息

- **在 HAMMER 中的实现**：Hierarchical Cross-Modal Integration
  - 将 PointNet++ 编码的点特征作为 Query，MLLM 隐藏状态作为 Key/Value
  - 通过交叉注意力（Cross-Attention）让每个点选择性关注相关交互线索
  - 分两个阶段：Stage I 进行全局上下文整合，Stage II 进行细粒度语义增强
- **设计动机**：MLLM 的隐藏状态天然包含任务相关内容，利用这些信息可缓解模态间差异，促进更连贯的特征表达
- **消融结果**：移除该模块后，PIAD Seen aIOU 从 22.20% 降至 21.15%，Unseen aIOU 从 13.71% 降至 10.50%
- **相关方法对比**：与 GREAT 的多分支融合（图像+文本+点云直接融合）不同，HAMMER 直接利用 MLLM 内部表征

## 关联连接

- [[HAMMER]] — 使用该技术的框架
- [[3D Affordance Grounding]] — 应用任务
- [[Multi-Granular Geometry Lifting]] — 配套模块
- [[摘要-hammer-3d-affordance-grounding]] — 来源摘要
