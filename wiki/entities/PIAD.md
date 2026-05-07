---
title: "PIAD"
type: entity
tags: [数据集, 3D-affordance, benchmark]
sources: [raw/papers/2603.02329v1.pdf]
last_updated: 2026-05-06
---

## 定义

PIAD（**P**art **I**nteraction **A**ffordance **D**ataset）是 3D 可供性定位领域的标准评测数据集。包含 PIAD（原始版本，ICCV 2023）和 PIADv2（扩展版本，CVPR 2025）。

## 关键信息

- **任务类型**：Intention-Driven 3D Affordance Grounding（意图驱动 3D 可供性定位）
- **输入**：3D 点云 + 对应的交互图像
- **输出**：3D 可供性映射（每个点的可供性分数）
- **PIAD 评测划分**：Seen（已见物体）+ Unseen（未见物体）
- **PIADv2 评测划分**：Seen + Unseen Object（未见物体类别）+ Unseen Affordance（未见可供性类型）
- **可供性类别示例**：grasp, lift, open, lay, sit, support, wrap, pour, move, display, push, listen, wear, press, cut, stab 等
- **评测指标**：aIOU, AUC, SIM, MAE
- **HAMMER 在 PIAD Unseen 上的 aIOU**：13.71%（vs GREAT 8.32%）
- **HAMMER 在 PIADv2 Unseen Object 上的 aIOU**：24.28%（vs GREAT 19.16%）

## 关联连接

- [[HAMMER]] — 在该数据集上达到 SOTA 的框架
- [[摘要-hammer-3d-affordance-grounding]] — 来源摘要
- [[3D Affordance Grounding]] — 任务概念
