---
title: "ARA 研究档案"
date: 2026-06-29
weight: 5
author: "nio (Houming818) & Codex Review"
description: "SameTime ARA 研究档案的手机阅读版：claim、predict、experiment、evidence、decision 和 next step。"
tags: [ARA, TreeHeap, SPR, SameTime]
---

# ARA 研究档案

这是 SameTime 仓库中 `ara/` 的网页阅读镜像，方便在手机上查看。
每个 claim 链到 evidence，每个 experiment 对应可执行脚本。

## 总览

- [ARA 论文式总览（PAPER.zh.md）](/ara/paper.html) —— 所有 claim 的总表
- [中文镜像说明（TRANSLATION.zh.md）](/ara/translation.html)

---

## M0 — TreeHeap 数学地基

### 逻辑层（/logic）

| 文件 | 内容 |
|------|------|
| [problem](/ara/m0-treeheap-math/logic/problem.html) | 问题定义 |
| [claims](/ara/m0-treeheap-math/logic/claims.html) | M0-C01..C11, M0-SOFT-C01..C08, M0-EXIST-C01..C03, M0-DIFF-C01 |
| [predicts](/ara/m0-treeheap-math/logic/predicts.html) | P-MATH01..02, P-SOFT01..05, P-DIFF01 等 |
| [experiments](/ara/m0-treeheap-math/logic/experiments.html) | 实验设计：pass gate、模型变体、数据集 |
| [soft_treeheap](/ara/m0-treeheap-math/logic/soft_treeheap.html) | Soft TreeHeap 设计文档 |
| [algebra](/ara/m0-treeheap-math/logic/solution/algebra.html) | TreeHeap 算子代数定义 |

### 证据层（/evidence）

实验们的 evidence summary：

- [treeheap_math_probe](/ara/m0-treeheap-math/evidence/treeheap_math_probe/readme.html) — M0 闭包、非交换、逆操作验证
- [primitive_plus_probe](/ara/m0-treeheap-math/evidence/primitive_plus_probe/readme.html) — 地址寻址 plus 操作验证
- [trainability_quiz](/ara/m0-treeheap-math/evidence/trainability_quiz/readme.html) — 线性/XOR/模加法 入门测试
- [soft_plus_probe](/ara/m0-treeheap-math/evidence/soft_plus_probe/readme.html) — Soft Plus 梯度可达 + 低温坍缩
  - [GLM audit summary](/ara/m0-treeheap-math/evidence/soft_plus_probe/glm_audit_summary.html)
- [structural_c05_probe](/ara/m0-treeheap-math/evidence/structural_c05_probe/readme.html) — C05 结构必要性验证
- [kernel_convolution_ops_probe](/ara/m0-treeheap-math/evidence/kernel_convolution_ops_probe/readme.html) — C08 核卷积操作统一
- [deductive_inductive_kernel_probe](/ara/m0-treeheap-math/evidence/deductive_inductive_kernel_probe/readme.html) — C09 演绎 vs 归纳核验证
- [treeheap_diff_algebra_probe](/ara/m0-treeheap-math/evidence/treeheap_diff_algebra_probe/readme.html) — M0-DIFF-C01 差分代数
- [algebraic_decoder_probe](/ara/m0-treeheap-math/evidence/algebraic_decoder_probe/readme.html) — 代数解码器实验
- [M0 evidence 总览](/ara/m0-treeheap-math/evidence/readme.html)

---

## S1 — Echo / 路径路由

### 逻辑层

| 文件 | 内容 |
|------|------|
| [problem](/ara/s1-echo/logic/problem.html) | S1 问题定义 |
| [claims](/ara/s1-echo/logic/claims.html) | S1-C01..C30 + S1-WM-C01..C02 + S1-WMT-ECHO-C01 + S1-MK-C01 |
| [experiments](/ara/s1-echo/logic/experiments.html) | E1..E11 实验设计 |
| [environment](/ara/s1-echo/src/environment.html) | 运行环境说明 |

### 证据层

- [shallow_treeheap_s1_probe](/ara/s1-echo/evidence/shallow_treeheap_s1_probe/readme.html) — S1-C30：浅层 TreeHeap OOD copy-by-address
- [s1_world_model_compound_probe](/ara/s1-echo/evidence/s1_world_model_compound_probe/readme.html) — S1-WM-C01：冻结 embedding 世界坐标（负面结果）
- [s1_corpus_embedding_kernel_probe](/ara/s1-echo/evidence/s1_corpus_embedding_kernel_probe/readme.html) — S1-WM-C02：本地语料 SGNS 坐标
- [s1_wmt_echo_kernel_probe](/ara/s1-echo/evidence/s1_wmt_echo_kernel_probe/readme.html) — S1-WMT-ECHO-C01：WMT echo 核验证
- [s1_wmt_multikernel_specialization_probe](/ara/s1-echo/evidence/s1_wmt_multikernel_specialization_probe/readme.html) — S1-MK-C01：多核特化（2049 词表）
- [s1_wmt_multikernel_specialization_probe_common512](/ara/s1-echo/evidence/s1_wmt_multikernel_specialization_probe_common512/readme.html) — S1-MK-C01：多核特化（513 词表）
- [s1_probabilistic_read_kernel_probe](/ara/s1-echo/evidence/s1_probabilistic_read_kernel_probe/readme.html) — 概率读核实验
- [s1_probabilistic_read_kernel_probe_b32](/ara/s1-echo/evidence/s1_probabilistic_read_kernel_probe_b32/readme.html) — 概率读核 batch=32
- [S1 evidence 总览](/ara/s1-echo/evidence/readme.html)

---

## S2 — 折叠堆栈 / 翻译

### 逻辑层

| 文件 | 内容 |
|------|------|
| [problem](/ara/s2-translation/logic/problem.html) | S2 问题定义 |
| [claims](/ara/s2-translation/logic/claims.html) | C-001..C-028 等 |
| [predicts](/ara/s2-translation/logic/predicts.html) | P-FRAME01 等 |
| [experiments](/ara/s2-translation/logic/experiments.html) | 实验设计 |
| [architecture](/ara/s2-translation/logic/solution/architecture.html) | 架构方案 |
| [theory](/ara/s2-translation/logic/solution/theory.html) | 理论基础 |
| [treeheap_algebra](/ara/s2-translation/logic/solution/treeheap_algebra.html) | TreeHeap 代数设计 |
| [environment](/ara/s2-translation/src/environment.html) | 运行环境 |

### 证据层

- [world_model_long_20260617](/ara/s2-translation/evidence/readme.html) — 10 小时守夜训练
- [frame_probe_2h_queue](/ara/s2-translation/evidence/frame_probe_2h_queue/output/readme.html) — Frame probe 诊断
- [S2 evidence 总览](/ara/s2-translation/evidence/readme.html)

---

## S3 — 生成

| 文件 | 内容 |
|------|------|
| [problem](/ara/s3-generation/logic/problem.html) | S3 问题定义（计划中） |

---

## 辅助页面

- [PUBLIC](/ara/public.html) — 公开镜像说明
- [README](/ara/readme.html) — ARA 目录结构说明
