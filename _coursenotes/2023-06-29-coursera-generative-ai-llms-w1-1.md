---
layout: post
title: "Generative AI with LLMs Week 1 (1)"
comments: true
tags: ['llms']
---

Here are course notes I am taking from the DeepLearning.ai course on Coursera: [Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms).

<!--more-->

# Instructors

- Antje Barth
  - Principal Developer Advocate, Generative AI, Amazon Web Services (AWS)
- Chris Fregly
	- Principal Solutions Architect, Generative AI, Amazon Web Services (AWS)
- Shelbee Eigenbrode
	- Principal Solutions Architect, Generative AI, Amazon Web Services (AWS)
- Mike Chambers
	- Developer Advocate, Generative AI, Amazon Web Services (AWS)

# Course Introduction

- LLMs have been underestimated by many people as a developer tool. 
- Generative AI and LLMs specifically are a *general purpose technology* like deep learning and electricity.

# Introduction to LLMs and the generative AI project lifecycle

## Transformer

- Deep dive into transformer
- Amazing how long the transformer architecture has been around for long and still remain state-of-the-art

## Generative AI

- Generative AI Project Lifecycle
- foundation model off the shelf v.s.  pre-training your own model

## Generative AI & LLMs

*By Mike Chambers*

- Examples of foundation models/base models:
	- BERT
	- GPT
	- FLAN-T5
	- LLaMa
	- PaLM
	- BLOOM
- More parameters, more model memories, more sophisticated tasks enabled.
- Interact with language models is quite different than other machine learning and programming paradigms:
	- LLMs take natural language or human written instructions
	- Prompt: The text that you pass to an LLM
	- Context window: The space or memory that is available to the prompt (typically a few thousand words)
	- Inference: the act of using the model to generate text

[Link](https://community.deeplearning.ai/c/generative-ai-with-large-language-models/328) to Community for discussion.

## LLM use cases and tasks

- Next word prediction:
	- a basic chatbot
	- text generation:
		- write an essay based on a prompt
		- summarize conversations with dialogue provided as prompt 
- Translation tasks:
	- traditional translation between two different languages, e.g. French and German, English and Spanish
	- translate natural language to machine code
- Smaller, focused tasks:
	- information retrieval
	- named entity recognition
- Augmenting LLMs by connecting them to external data sources or using them to invoke external APIs
	- provide the pre-trained model with unseen information 
	- let your model interactions with the real-world

## Generating Text

### Previous generation: Recurrent neural networks (RNNs)

limited by the **amount of compute and memory** needed to perform well at generative tasks

### Transformer

- can be scaled efficiently to use multi-core GPUs
- can parallel process input data, making use of much larger training datasets
- can learn to pay attention to the meaning of the words it's processing
- ["Attention is all you need"](https://arxiv.org/abs/1706.03762)

## Transformers Architecture

(come back later)

## Generating Text with Transformers

(come back later)

## Prompting and prompt engineering

### Terminology

- Prompt: text fed into the model
- Inference: the act of generating text
- Completion: the output text
- Context window: memory available to use for the prompt (typically a few thousand words)
- Prompt engineering: the process of developing and improving the prompt
- In-context learning: including examples or additional data in the prompt (inside the context window)

### In-context learning (ICL)

- Zero-shot inference
- One-shot inference
- Few-shot inference (typically, won't gain much after 5 or 6 shots)

`Image: zero-one-few.png`

## Generative configuration

### Inference parameter (not training parameter):

- Max new tokens: limit the number of tokens that the model will generate, i.e., a cap on the number of times the model will go through the selection process
- Sample top K
- Sample top P
- Temperature

### Greedy v.s. random sampling

- The output from the transformer's softmax layer is a probability distribution across the entire dictionary of words
- greedy decoding: always choose the word with the highest probability
  - can work very well for short generation 
  - susceptible to repeated words or repeated sequences of words
- Random(-weighted) sampling: 
  - easiest way to introduce some variability
  - select a token using a random-weighted strategy across the probabilities of all tokens
  - likelihood of word repetition reduced
  - may be too creative, producing words that cause the generation to wander off into topics or words that just don't make sense

### Top-k sampling

- select an output from the top-k results after applying random-weighted strategy using the probabilities

### Top-p sampling

- select an output using the random-weighted strategy with the top-ranked consecutive results by probability and with a cumulative probability <= p

### Temperature

- Influences the shape of the probability distribution that the model calculates for the next token
- The higher the temperature, the higher the randomness
- The temperature value is a scaling factor that's applied within the final softmax layer of the model that impacts the shape of the probability distribution of the next token
- Changing the temperature actually alters the predictions that the model will make

`Image: inference-parameter-temperature`

## Generative AI project lifecycle

`image: Generative-AI-project-lifecycle`

## Introduction to AWS labs

`Lab instructions`

- Amazon SageMaker Studio
- Go to terminal
  - launch -> studio
- paste step 8 command to terminal. Then can see notebook

## Lab 1 walkthrough

`Lab instructions`

