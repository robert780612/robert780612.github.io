---
title: "Back to Basics: Let Denoising Generative Models Denoise"
date: 2025-11-23T04:40:51Z
tags: ["diffusion", "generative models", "JiT"]
author: "Robert"
draft: false
description: "Just Image Transformer (JiT): A novel approach to image generation by predicting images directly."
showToc: true
TocOpen: true
---
<!-- 
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

## References

- [Just image Transformer (JiT) for Pixel-space Diffusion](https://github.com/LTH14/JiT)
- [MIT何恺明团队新作：让扩散模型回归“去噪”本质，简单Transformer即可实现SOTA性能](https://zhuanlan.zhihu.com/p/1974130263698216733) -->

## Back to Basics: Let Denoising Generative Models Denoise

"Back to Basics: Let Denoising Generative Models Denoise" is a paper published by Tianhong Li and Kaiming He in November 2025. This article introduces a new image generation model called **Just Image Transformer (JiT)**, which primarily challenges the shortcomings of current diffusion models.

Current mainstream diffusion concepts (**$\epsilon$-prediction**) involve taking an image with added noise, asking the model to guess "what noise is in this picture," and then removing the noise to produce a clear image.

JiT proposes a different perspective (**$x$-prediction**): given a noisy image, the model is asked to guess "what this image originally looked like."

## What is the advantage of $x$-prediction?

We generally consider clear images to be structurally distinct and governed by certain rules; whereas noise has no rules and is difficult to predict.

**The question then arises: Why should we ask a model to predict irregular noise?** Would the results be better if we simply output the clean image directly?

Based on this reasoning, the paper proposes a simpler approach: **"Guess the original image."** JiT inputs noisy pixel-level patches directly into a standard Vision Transformer and directly outputs clean pixel-level patches.

Because the task has been simplified (predicting the original image), there is no longer a need for VQVAEs or Tokenizers for compression. The model operates directly on the original image and uses a simple loss function: **"timestep-weighted pixel-wise MSE."**

## JiT Architecture

The architecture of JiT is remarkably simple and consists of the following parts:

   1. **Noisy Patch Extraction**: Pixel-level patches are extracted from the noisy image; these patches serve as the input for the model.
   2. **Vision Transformer**: A standard Vision Transformer is used as the core model, responsible for processing the input noisy patches and generating the corresponding clean patches.
   3. **Patch Reconstruction**: The clean patches output by the model are reassembled into the complete image.
   4. **Loss Function**: A timestep-weighted pixel-wise MSE is used as the loss function to ensure the model effectively learns the mapping from noisy patches to clean patches.

## References

- [Just image Transformer (JiT) for Pixel-space Diffusion](https://github.com/LTH14/JiT)
- [MIT何恺明团队新作：让扩散模型回归“去噪”本质，简单Transformer即可实现SOTA性能](https://zhuanlan.zhihu.com/p/1974130263698216733)
