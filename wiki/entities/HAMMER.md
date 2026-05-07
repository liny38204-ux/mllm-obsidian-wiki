---
title: "HAMMER"
type: entity
tags: [框架, 3D-affordance, MLLM, CVPR2026]
sources: [raw/papers/2603.02329v1.pdf]
last_updated: 2026-05-06
---

## 定义

HAMMER（**H**arnessing **M**LLM via Cross-Modal Integration for Intention-Driven 3D **A**ffordance **G**rounding）是一个利用 MLLM 进行意图驱动 3D 可供性定位的框架。由香港理工大学与华中科技大学联合提出，已被 CVPR 2026 接收。

## 关键信息

- **核心思路**：不生成显式物体属性描述、不依赖现成 2D 分割器，而是将交互图像中的意图聚合成接触感知嵌入（Contact-Aware Embedding）
- **三大模块**：
  1. **Affordance-Guided Intention Embedding**：通过 MLLM 提取接触感知嵌入，同时预测文本可供性标签
  2. **Hierarchical Cross-Modal Integration**：利用 MLLM 隐藏状态增强 3D 点云特征
  3. **Multi-Granular Geometry Lifting**：渐进注入多尺度几何特征，赋予嵌入 3D 空间感知
- **MLLM 骨干**：Qwen2.5-VL-3B（平衡性能与效率）
- **3D 骨干**：PointNet++（默认）/ Uni3D（可选）
- **训练策略**：LoRA 微调 MLLM，三阶段训练流水线
- **性能**：在 PIAD 和 PIADv2 上全面超越 GREAT、InteractVLM、IAGNet；训练未见类别 aIOU 提升 5.39%
- **鲁棒性**：在噪声损坏基准上展现极强鲁棒性，远超 GREAT
- **项目页**：https://rayyoh.github.io/Hammer/

## 关联连接

- [[摘要-hammer-3d-affordance-grounding]] — 来源摘要
- [[3D Affordance Grounding]] — 任务概念
- [[Cross-Modal Integration]] — 跨模态整合概念
- [[Multi-Granular Geometry Lifting]] — 几何提升概念
- [[Qwen2.5-VL]] — MLLM 骨干
- [[PIAD]] — 评测数据集
