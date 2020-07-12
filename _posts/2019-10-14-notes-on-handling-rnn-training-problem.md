---
layout: post
title: Common issues in RNN training
tags: ['ds']
comments: true
---

Exploding gradients and vanishing gradients are two common issues with the training of RNN.

To avoid exploding gradients, one may use:  

- Truncated Back-propagation through time (BPTT) 
- Clip gradients at threshold 
- RMSprop to adjust learning rate 

Vanishing gradients are harder to detect. To avoid it, one may use:

- Weight initialization
- ReLu activation functions
- RMSprop
- LSTM, GRUs