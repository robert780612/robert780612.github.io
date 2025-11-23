---
date: '2025-11-23T04:48:51Z'
draft: false
title: 'ARC Is a Vision Problem!'
showToc: true
TocOpen: true
tags: ["diffusion", "generative models"]
author: "Robert"
description: "Vision ARC (VARC): Reframing Abstraction and Reasoning Corpus as a Vision Problem using Vision Transformers."
---


The Abstraction and Reasoning Corpus (ARC) is a prominent benchmark for few-shot abstract reasoning over colored grids, widely regarded as a challenge toward AGI. It requires models to deduce implicit, human-understandable transformation rules from a limited set of input-output examples. Historically, ARC has often been treated as a language-oriented problem, addressed by Large Language Models (LLMs).

## Vision ARC (VARC): Reframing Abstraction and Reasoning Corpus (ARC) as a Vision Problem

In this work, the authors propose Vision ARC (VARC), a novel framework that reframes ARC entirely into a vision paradigm, yielding two major contributions:

1. Vision-Centric Formulation: VARC treats the benchmark as a vision problem, formulating it as an image-to-image translation task.
   * The input grids are represented as images on a "canvas," where each raw pixel is an input token.
   * The distinct colors, including the background, form a small vocabulary of $C+1$ tokens.
   * The architecture, a Vision Transformer (ViT), naturally incorporates visual priors. To further enforce critical inductive biases like translation invariance, the authors adopt scale and translation augmentations.
2. Test-Time Training (TTT) for Generalization: VARC is trained from scratch solely on the ARC training data and achieves few-shot generalization to unseen tasks through a two-stage process:
   * Offline Training: A ViT backbone shares parameters across all training tasks.
   * Test-Time Training (TTT): When faced with a new, unseen test task, the core ViT parameters are frozen. A small set of task-conditional parameters (e.g., tokens) are randomly initialized and then quickly trained on the few provided demo examples for that specific task. This rapid adaptation tunes the general-purpose model to the unique logic of the new task.

VARC achieves a state-of-the-art accuracy of 60.4% (with ensembling) among methods trained only on ARC data, substantially outperforming other scratch-trained models and closing the gap to human performance.

## References

* [ARC Is a Vision Problem!](https://arxiv.org/abs/2511.14761)

## 中文版

Abstraction and Reasoning Corpus (ARC) 乃為評估機器於 few-shot abstract reasoning 領域能力之核心 benchmark，傳統上被定性為 language-oriented problem。本研究提出 Vision ARC (VARC) 框架，主張 ARC 任務的本質特性為視覺與結構化問題，故應納入 Computer Vision 領域探討。

## 一、核心方法論：視覺範式與兩階段適應性訓練

VARC 框架基於兩項核心創新：

1. 視覺範式重塑： VARC 將 ARC 任務重塑為 image-to-image translation。模型輸入為 $H \times W$ 的 canvas，每個像素賦予 token 編碼（總計 $C+1$ 個 token），應用 Vision Transformer (ViT) 架構。研究團隊導入 data augmentation，融入 translation invariance 等視覺上的 inductive biases，以增強其 generalizability。

2. 兩階段訓練協議： 採用結構化流程滿足 few-shot 條件。Phase 1 為離線訓練 (Offline Training)（trained from scratch on only ARC training set），共享 ViT backbone parameters 奠定通用知識。Phase 2 實施 測試時訓練 (TTT)：當面臨新 test task 時，核心 ViT backbone parameters 凍結 (frozen)，僅對 task-conditional tokens 進行快速訓練，利用 demo examples 實現高效 adaptation。

## 二、研究成果與學術意義

VARC 框架於 ARC-1 benchmark 測試中，取得了 60.4% 的準確率（藉由 ensembling），顯著超越所有從零開始訓練的既有方法，表現逼近人類平均解決水平。本研究驗證了抽象推理能力可透過純粹的 vision model 實現，並揭示高效 few-shot adaptation 機制（例如 TTT）對於解決複雜結構化視覺推理問題至為關鍵，為推進 AGI 發展提供重要的視覺範式啟示。