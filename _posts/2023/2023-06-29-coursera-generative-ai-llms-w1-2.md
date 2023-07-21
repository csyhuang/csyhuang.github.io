---
layout: post
title: "Generative AI with LLMs Week 1 (2)"
comments: true
tags: ['llms']
---

Here are course notes I am taking from the DeepLearning.ai course on Coursera: [Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms).

<!--more-->

# LLM pre-training and scaling laws

## Pre-training large language models

Consideration for choosing a model:

- Foundation model: Pretrained LLM
- Train your own model: Custom LLM

Hubs from HuggingFace/PyTorch have *model cards* that describe important details including:

- the best use cases for each model
- how it was trained
- known limitations.

## Autoencoding (encoder-only) models

- Pre-trained method: Masked Language Modeling (MLM)
- Objective: Reconstruct text ("denoising")
- Bidirectional context: the model has an understanding of the full context of a token, not just of the words that come before
- Good use cases:
  - Sentiment analysis
  - Named entity recognition
  - Word classification
- Example models:
	- BERT
	- RoBERTa

## Autoregressive (decoder-only) models

- Pre-trained method: causal language modeling (CLM)
- Objective: predict next token (full language modeling)
- The model:
  - has no knowledge of the end of the sentence
  - iterates over the input sequence one by one to predict the following token
- Unidirectional context
- Good use cases:
  - Text generation
  - Other emergent behavior
  	- depends on model size
  - note: although larger decoder-only models show strong zero-shot inference abilities, and can often perform a range of tasks well
- Example models:
	- GPT
	- BLOOM

## Sequence-to-sequence (encoder-decoder) model

- Pre-training: vary from model to model
- Example: T5 pre-trains the encoder using span corruption
	- masks random sequences of input tokens
	- those mass sequences are then replaced with a unique Sentinel token (x)
	- Sentinel tokens are special tokens added to the vocabulary, but do not correspond to any actual word from the input text
	- The decoder is then tasked with reconstructing the mask token sequences auto-regressively
	- The output is the Sentinel token followed by the predicted tokens
- Good use cases:
	- Translation
	- Text summarization
	- Question answering
	- (generally useful in cases where you have a body of texts as both input and output)
- Example models:
  - T5
  - BART

Summary: `model-arch-and-pre-training-objectives`

Model size v.s. time
`model-size-vs-time`

## Computational challenges of training LLMs

(pending)

## Optional video: Efficient multi-GPU compute strategies

(pending)

(papers)

## Scaling laws and compute-optimal models

Chinchilla paper

## Pre-training for domain adaptation

## Domain-specific training: BloombergGPT

Reading•. Duration: 10 minutes10 min

















