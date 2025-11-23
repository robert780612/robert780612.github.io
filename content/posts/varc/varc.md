---
title: 'ARC Is a Vision Problem!'
date: '2025-11-23T04:48:51Z'
draft: false
showToc: true
TocOpen: true
tags: ["diffusion", "generative models", "VARC"]
author: "Robert"
description: "Vision ARC (VARC): Reframing Abstraction and Reasoning Corpus (ARC) as a Vision Problem using Vision Transformers."
---

## ARC: A Benchmark for Few-Shot Abstract Reasoning

The Abstraction and Reasoning Corpus (ARC) is a prominent benchmark for few-shot abstract reasoning over colored grids, widely regarded as a challenge toward AGI.
It requires models to deduce implicit, human-understandable transformation rules from a limited set of input-output examples.
Historically, ARC has often been treated as a language-oriented problem, addressed by Large Language Models (LLMs).

## Vision ARC (VARC): Reframing Abstraction and Reasoning Corpus (ARC) as a Vision Problem

In this work, the authors propose Vision ARC (VARC), a novel framework that reframes ARC entirely into a vision paradigm, yielding two major contributions:

1. Vision-Centric Formulation: VARC treats the benchmark as a vision problem, formulating it as an image-to-image translation task.
   * The input grids are represented as images on a "canvas," where each raw pixel is an input token.
   * The distinct colors, including the background, form a small vocabulary of $C+1$ tokens. (i.e., $C$ colors plus 1 background token.)
   * The architecture, a Vision Transformer (ViT), naturally incorporates visual priors. To further enforce critical inductive biases like translation invariance, the authors adopt scale and translation augmentations.
2. Test-Time Training (TTT) for Generalization: VARC is trained from scratch solely on the ARC training data and achieves few-shot generalization to unseen tasks through a two-stage process:
   * Offline Training: A ViT backbone shares parameters across all training tasks.
   * Test-Time Training (TTT): When faced with a new, unseen test task, the core ViT parameters are frozen. Only task-conditional tokens are randomly initialized and then quickly trained on the few provided demo examples for that specific task. This rapid adaptation tunes the general-purpose model to the unique logic of the new task.

VARC achieves a state-of-the-art accuracy of 60.4% (with ensembling) among methods trained only on ARC data, substantially outperforming other scratch-trained models and closing the gap to human performance.

## Discussion

### Is it too specialized?

While VARC is a domain-specific model trained solely on ARC data, its success is significant. It outperforms generalist models by leveraging specific visual inductive biases (like scale and translation invariance) rather than relying on the massive logical priors found in LLMs.

### The Future of Visual Reasoning Can this idea be applied to general vision reasoning?

Potentially. VARC suggests that treating reasoning tasks as image-to-image translation problems—combined with Test-Time Training—is a viable alternative to current chain-of-thought prompting methods. It opens the door for "System 2" thinking (slow, adaptive reasoning) in pure vision models.

## References

* [ARC Is a Vision Problem!](https://arxiv.org/abs/2511.14761)
