---
title: "摘要-hammer-3d-affordance-grounding"
type: source
tags: [来源, 论文, 3D-affordance, MLLM, CVPR2026]
sources: [raw/papers/2603.02329v1.pdf]
last_updated: 2026-05-06
---

## 核心摘要

HAMMER 提出一种利用多模态大语言模型（MLLM）进行意图驱动的 3D 可供性定位（Intention-Driven 3D Affordance Grounding）新框架。与现有方法不同，HAMMER 不依赖显式的对象属性描述文本或现成的 2D 分割器，而是将交互图像中蕴含的互动意图聚合成一个**接触感知嵌入**（Contact-Aware Embedding），并引导 MLLM 同时预测文本可供性标签，充分挖掘对象语义与上下文线索。

该框架设计了两个关键模块：（1）**层次化跨模态整合机制**（Hierarchical Cross-Modal Integration），利用 MLLM 隐藏状态中的互补信息增强 3D 点云特征表示；（2）**多粒度几何提升模块**（Multi-Granular Geometry Lifting），逐步将多尺度几何特性注入意图嵌入中，赋予其 3D 空间感知能力。在 PIAD、PIADv2 数据集及自建的损坏基准上，HAMMER 全面超越 GREAT、InteractVLM、IAGNet 等现有方法，并在 Unseen 场景下展现出显著更强的泛化能力和噪声鲁棒性。

论文已被 CVPR 2026 接收，代码与权重已开源。

## 关联连接

- [[HAMMER]] — 本文提出的框架
- [[Qwen2.5-VL]] — 使用的 MLLM 骨干模型
- [[PIAD]] — 核心评测数据集
- [[3D Affordance Grounding]] — 任务概念
- [[Cross-Modal Integration]] — 跨模态整合概念
- [[Multi-Granular Geometry Lifting]] — 多粒度几何提升概念
