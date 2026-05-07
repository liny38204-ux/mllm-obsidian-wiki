---
title: "3D Affordance Grounding"
type: concept
tags: [概念, 3D-vision, affordance, MLLM, embodied-AI]
sources: [raw/papers/2603.02329v1.pdf]
last_updated: 2026-05-06
---

## 定义

3D 可供性定位（3D Affordance Grounding）指在 3D 物体表示（如点云）上定位并预测潜在的可交互区域（可供性区域）。这一任务模仿人类通过观察演示来感知物体功能的先天能力，是具身 AI 和机器人操控的核心子任务。

## 关键信息

- **任务形式**：
  - **Intention-Driven（意图驱动）**：给定交互图像 + 3D 点云，预测点云上的可供性映射
  - **Language-Driven（语言驱动）**：给定自然语言指令 + 3D 点云，定位对应可供性区域
- **可供性类型示例**：grasp（抓取）、lift（提起）、open（打开）、sit（坐）、support（支撑）、pour（倾倒）、cut（切割）、press（按压）等
- **核心挑战**：
  - 图像/语言输入与 3D 几何之间的模态差异
  - 物体类别泛化（未见物体/未见可供性）
  - 噪声和遮挡下的鲁棒性
- **标准评测数据集**：PIAD（ICCV 2023）、PIADv2（CVPR 2025）、SceneFun3D
- **评测指标**：aIOU、AUC、SIM、MAE

## 关联连接

- [[HAMMER]] — 当前 SOTA 框架
- [[PIAD]] — 标准评测数据集
- [[摘要-hammer-3d-affordance-grounding]] — 来源摘要
