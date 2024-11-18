---
layout: post
title: "Running pytest coverage check locally"
comments: true
tags: ['se']
---

<!--more-->

DDPM = Denoising Diffusion Probablistic Model
Johnathan AJJ
https://arxiv.org/abs/2006.11239

Need to add extra noise in the step

UNet is originally used for image segmentation.

UNet input and output are of the same size.

Embed information about the input -> downsample
Upsample with same number of upsampling blocks

DDIM = Denoising Diffusion Implicit Models
https://arxiv.org/abs/2010.02502

Removed all randomness, remove markov process

Textual inversion:
https://huggingface.co/docs/diffusers/training/text_inversion

Stable diffusion uses "latent diffusion" which operates on image embeddings directly to make the process even more efficient.

- cross-attention
- text conditioning
- classifier-free guide

Research on sampling methods because it is still slower than other generative models at inference time

