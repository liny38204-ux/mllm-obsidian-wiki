---
title: "Qwen2.5-VL"
type: entity
tags: [模型, MLLM, 视觉语言模型, Qwen]
sources: [raw/papers/2603.02329v1.pdf]
last_updated: 2026-05-06
---

## 定义

Qwen2.5-VL 是通义千问系列的多模态视觉语言模型（Vision-Language Model），具备强大的视觉-语言理解能力。在 HAMMER 框架中被选为核心 MLLM 骨干，用于从交互图像中提取意图信息。

## 关键信息

- **版本**：3B 和 7B 两种规模
- **在 HAMMER 中的角色**：处理交互图像，提取接触感知嵌入（Contact-Aware Embedding），并预测可供性标签
- **优势**：相比 LISA、LISA++ 等专门微调的分割模型，Qwen2.5-VL 凭借更广泛的视觉-语言理解和世界知识，在 3D 可供性定位任务上取得更优性能
- **在 HAMMER 中的选择**：HAMMER 选择 3B 版本，在性能与计算效率之间取得平衡
- **技术报告**：arXiv:2502.13923 (2025)

## 关联连接

- [[HAMMER]] — 使用该模型的框架
- [[摘要-hammer-3d-affordance-grounding]] — 来源摘要
