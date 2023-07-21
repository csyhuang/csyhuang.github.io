---
layout: post
title: "Generative AI with LLMs Week 2 (2)"
comments: true
tags: ['llms']
---

Here are course notes I am taking from the DeepLearning.ai course on Coursera: [Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms).

<!--more-->

<h1>Topic: Parameter efficient fine-tuning (PEFT)</h1>

# Parameter efficient fine-tuning (PEFT)

## Full fine-tuning of large LLMs is challenging

- Trainable weights
Additional 12-20x weights may be too large to handle on consumer hardware:
- Temp memory
- Forward Activations
- Gradients 
- Optimizer states

## PEFT

- can perform on single GPU
- less prone to catastrophic forgetting
- PEFT fine-tuning saves spaces and is flexible
- efficient adaptation of the original model to multiple tasks

## PEFT Trade-offs
- Parameter efficiency
- Memory efficiency
- Model performance
- Inference cost
- Training speed

## PEFT methods

- Lialin et al. 2023, "[Scaling Down to Scale Up: A Guide to Parameter-Efficient Fine-Tuning](https://arxiv.org/abs/2303.15647)"

### Selective
- *Select* subset of initial LLM parameters to fine-tune
- Researchers have found that the performance of these methods is *mixed*
- there are significant trade-offs between *parameter efficiency* and *compute efficiency*

### Reparameterization
- *Reparameterize* model weights using a low-rank representation
- LoRA

### Additive
- *Add* trainable alyers or parameters to model
- Adapters:
	- add new trainable layers to the architecture of the model
	- typically inside the encoder or decoder components after the attention or feed-forward layers
- Soft Prompts:
	- keep the model architecture fixed and frozen
	- focus on manipulating the input to achieve better performance
	- can be done by:
		- adding trainable parameters to the prompt embeddings
		- keeping the input fixed and retraining the embedding weights


# PEFT techniques 1: LoRA (Low-Rank Adaptation of Large Language Models)

`Slide p.95` captures the idea!

- this model has the same number of parameters as the original: little to no impact on inference latency
- Researchers have found that applying LoRA to just the *self-attention layers* of the model is often enough to fine-tune for a task and achieve performance gains
- (in principle, you can also use LoRA on other components like the feed-forward layers)
- biggest savings in trainable parameters achieved by applying LoRA to attention layers' weights matrices

## Concrete example using base Transformer as reference

`Slide p.96` captures the idea!

## LoRA: Low Rank Adaption of LLMs

`Slide p.99` captures the idea!

##  Sample ROUGE metrics for full vs. LoRA fine-tuning

Comparable evaluation metrics!

## Choosing the LoRA rank (trade-offs)

- Hu et al. 2021, “LoRA: Low-Rank Adaptation of Large Language Models”
- Effectiveness of higher rank appears to plateau
- Relationship between rank and dataset size needs more empirical data

# PEFT techniques 2: Soft prompts

## Prompt tuning is not prompt engineering!

Limitations of prompt engineering:
- require a lot of manual effort to write and try different prompts
- limited by the length of the context window
- not guaranteed to achieve desired performance

## Prompt tuning adds trainable "soft prompt" to inputs

- add additional trainable tokens to your prompt
- let supervised learning process to determine their optimal values
- the set of trainable tokens is called a "soft prompt"
	- it gets prepended to embedding vectors that represent your input text
	- its vectors have the same length as the embedding vectors of the language tokens
	- including somewhere between 20 and 100 virtual tokens can be sufficient for good performance

## Soft prompts

- virtual tokens that can take on any value within the continuous multidimensional embedding space
- via supervised learning, the model learns the values for these virtual tokens that maximize performance for a given task

## Full Fine-tuning vs prompt tuning

### Full fine-tuning
- the training data set consists of input prompts and output completions or labels
- the weights of the large language model are updated during supervised learning. 

### prompt tuning
- the weights of the large language model are frozen
- the underlying model does not get updated - the embedding vectors of the soft prompt gets updated over time to optimize the model's completion of the prompt

- Prompt tuning is a very parameter efficient strategy because only a few parameters are being trained.
- You can train a set of soft prompts for one task and a different set for another.

## Performance of prompt tuning

- Lester et al. 2021, "The Power of Scale for Parameter-Efficient Prompt Tuning"
- prompt tuning doesn't perform as well as full fine tuning for smaller LLMs.
- prompt tuning performance increases as the model size increases
- As number of model parameters get to around 10B, prompt tuning can be as effective as full fine tuning

## Interpretability of soft prompts

- issue: interpretability of learned virtual tokens
- because the soft prompt tokens can take any value within the continuous embedding vector space, the trained tokens don't correspond to any known token, word, or phrase in the vocabulary of the LLM
- an analysis of the nearest neighbor tokens to the soft prompt location shows that they form tight semantic clusters
- i.e., the words closest to the soft prompt tokens have similar meanings
