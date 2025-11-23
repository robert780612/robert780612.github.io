---
title: "Just Image Transformer (JiT)"
date: 2025-11-23
tags: ["diffusion", "generative models"]
author: "Robert"
draft: false
description: "Just Image Transformer (JiT): A novel approach to image generation using denoising generative models."
---

Back to Basics: Let Denoising Generative Models Denoise是黎天鴻跟何愷明在2025年11月發表的文章。
這篇文章提出一種新的影像生成模型Just Image Transformer (JiT)，主要挑戰現在diffusion模型的缺陷。
現今主流diffusion的概念(eps-prediction)是拿一張加了雜訊的圖片，讓模型去猜這圖裡「有什麼雜訊」，並將雜訊從圖片中移除，以產生清楚的圖片。
而JiT提出不同的觀點(x-prediction)，有一張加了雜訊的圖片，讓模型去猜這圖「原本長什麼樣子」。
那x-prediction好在哪裡呢？
我們一般會認為清楚的圖片是結構清晰並且有一定規則；而雜訊則沒有規則，難以預測。
那問題就來了，為何我們要讓模型去預測沒有規則的雜訊？如果直接輸出乾淨的圖片，效果會不會更好？
基於上述理由，這篇文章提出了更簡單的做法「猜原始圖片」，JiT 直接將 noisy pixel-level patch 輸入標準的 Vision Transformer，並直接輸出 clean pixel-level patch。
因為任務變簡單了(猜原圖)，我們就不再需要VQVAE 或 Tokenizer 進行壓縮，直接在原圖進行操作，而且使用簡單的損失函數「對timestep加權的pixel-wise MSE」。

## JiT 架構
JiT 的架構非常簡單，主要分為三個部分：
1. **Noisy Patch Extraction**: 從加了雜訊的圖片中提取 pixel-level patch，這些 patch 將作為模型的輸入。
2. **Vision Transformer**: 使用標準的 Vision Transformer 作為核心模型，負責處理輸入的 noisy patch 並生成對應的 clean patch。
3. **Patch Reconstruction**: 將模型輸出的 clean patch 重組回完整的圖片。
4. **Loss Function**: 使用對 timestep 加權的 pixel-wise MSE 作為損失函數，確保模型能夠有效地學習從 noisy patch 到 clean patch 的映射。

