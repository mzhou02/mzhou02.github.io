---
layout: page
title: Spectral Adaptation
description: SVD-Transformer implementation with efficient fine-tuning using Singular Value Decomposition. Trains vanilla transformers on Wiki40B and fine-tunes on conversational datasets.
img: 
importance: 1
category: machine learning
github: https://github.com/mzhou02/Spectral-Adaptation
mathjax: true
---

Full implementation available at: [https://github.com/mzhou02/Spectral-Adaptation](https://github.com/mzhou02/Spectral-Adaptation)

## Overview

This project implements a vanilla transformer architecture with efficient fine-tuning capabilities using Singular Value Decomposition (SVD). The approach enables parameter-efficient adaptation of large language models to new domains while preserving the structure of pretrained weights.

## Key Innovation: SVD Fine-Tuning

Given a weight matrix $$W \in \mathbb{R}^{m \times n}$$, SVD factorizes it as:

$$ W = U \Sigma V^\top $$

where:
- $$U \in \mathbb{R}^{m \times r}$$ and $$V \in \mathbb{R}^{n \times r}$$ are (semi-)orthogonal matrices
- $$\Sigma = \mathrm{diag}(\sigma_1, \sigma_2, \dots, \sigma_r)$$ is the diagonal matrix of singular values

### Fine-Tuning Approach

Instead of updating the entire weight matrix, we:
1. **Freeze** $$U$$ and $$V$$ matrices
2. **Train only** a new set of scalar multipliers $$z \in \mathbb{R}^r$$ applied to singular values:

$$\Sigma' = \mathrm{diag}(z_1 \sigma_1, z_2 \sigma_2, \dots, z_r \sigma_r)$$

3. **Reconstruct** the adapted weight matrix:

$$W' = U \Sigma' V^\top$$
