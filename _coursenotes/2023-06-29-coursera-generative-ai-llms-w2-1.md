---
layout: post
title: "Generative AI with LLMs Week 2 (1)"
comments: true
tags: ['llms']
---

Here are course notes I am taking from the DeepLearning.ai course on Coursera: [Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms).

<!--more-->

<h1>Topic: Fine-tuning LLMs with instruction</h1>

# Instruction fine-tuning

Limitations of in-context learning:

- In-context learning may not work for smaller models
- Example take up space in the context window

Instead, try **fine-tuning** the model.

- Pre-training: train the LLM using vast amounts of unstructured textual data via selfsupervised learning
- Fine-tuning: supervised learning process where you use a data set of labeled examples to update the weights of the LLM to improve performance of specific task
	- example: pronpt-completion pairs

## Fine-tune LLMs with instruction prompts

- Model: pre-trained LLM
- Data: Each prompt/completion pair includes a specific "instruction" to the LLM
	- Classify this review: I loved this DVD!
		- Sentiment: Positive
	- Classify this review: I don't like this chair.
		- Sentiment: Negative
- Result: fine0tuned LLM
- Some other examples:
	- Summarize the folowing text:
		- \[EXAMPLE TEXT\]
		- \[EXAMPLE COMPLETIION\]
	- Translate this sentence to ...
		- \[EXAMPLE TEXT\]
		- \[EXAMPLE COMPLETIION\]
- Full fine-tuning updates all parameters, thus requires enough memory and compute budget to store and process all the gradients, optimizers and other components

## Sample prompt instruction templates

`Amazon polarity template (check slides P.17)`

https://github.com/bigscience-workshop/promptsource/blob/main/promptsource/templates/amazon_polarity/templates.yaml

## LLM fine-tuning process

- The output of an LLM is a probability distribution across tokens
- Compare the distribution of the completion and that of the training label by using standard cross-entropy function to calculate loss between the two token distributions
- Then use the calculated loss to update your model weights in standard back-propagation
- In this course, fine-tuning is in general referring to "instruction fine-tuning"

# Fine-tuning on a single task

1. Pre-trained LLM
2. Single-task training dataset, e.g. summarization. Often, only 500-1000 examples needed to fine-tune a single task.
3. Obtained Instruct LLM

## Catastrophic forgetting

- Potential downside to fine-tuning on a single task: **Catastrophic forgetting**
- Fine-tuning can significantly increase the performance of a model on a spcific task...
- But it can lead to reduction in ability on other tasks

## How to avoid catastrophic forgetting

- First, note that you might not have to (if it does not affect your task)!
- If you want to maintain multi-task generalized capabilities, perform fine-tuning on multiple tasks at the same time (500-1000 examples across *many* tasks)
- Cosider **Parameter Efficient Fine-tuning (PEFT)**:
  - preserves the weights of the original LLM and trains only a small number of task-specific adapter layers and parameters
  - shows greater robustness to catastrophic forgetting since most of the pre-trained weights are left unchanged

# Knowledge check

Q: Which of the following are true in respect to Catastrophic Forgetting? Select all that apply.


- [x] Catastrophic forgetting occurs when a machine learning model forgets previously learned information as it learns new information.

```
The assertion is true, and this process is especially problematic in sequential learning scenarios where the model is trained on multiple tasks over time.
```

- [x] One way to mitigate catastrophic forgetting is by using regularization techniques to limit the amount of change that can be made to the weights of the model during training.

```
One way to mitigate catastrophic forgetting is by using regularization techniques to limit the amount of change that can be made to the weights of the model during training. This can help to preserve the information learned during earlier training phases and prevent overfitting to the new data.
```

- [x] Catastrophic forgetting is a common problem in machine learning, especially in deep learning models.

```
This assertion is true because these models typically have many parameters, which can lead to overfitting and make it more difficult to retain previously learned information.
```

- [ ] Catastrophic forgetting only occurs in supervised learning tasks and is not a problem in unsupervised learning.

# Multi-task instruction fine-tuning

- training dataset is comprised of example inputs and outputs for *multiple tasks*, e.g.,
	- summarization,
	- review rating, 
	- code translation, and 
	- entity recognition. 
- Ddrawback to multitask fine-tuning: a lot of data required
- the resulting models are often very capable and suitable for use in situations where good performance at many tasks is desirable

## Instruction fine-tuning with FLAN

- FLAN = Fine-Tuned Language Net
- FLAN models refer to a specific set of instructions used to perform instruction fine-tuning
- Authors: "metaphorical dessert to the main course of pre-training"
- FLAN-T5: the FLAN instruct version of the T5 foundation model
- FLAN-T5: the FLAN instruct version of the PALM foundation model

## FLAN-T5: Fine-tuned version of pre-trained T5 model
- FLAN-T5 is a great, general purpose, instruct model
- Chung et al. 2022, "[Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416)"
- `W2 Slides P. 36`

## SAMSum: a dialogue dataset

- Sample prompt training dataset (samsum) to fine-tune FLAN-T5 from pretrained T5
- The dialogues and summaries were crafted by linguists for the express purpose of generating a high-quality training dataset for language models
- The linguists were asked to create conversations similar to those that they would write on a daily basis, reflecting their proportion of topics of their real life messenger conversations
- Language experts then created short summaries of those conversations that included important pieces of information and names of the people in the dialogue
- `Slide P.38, P.40`

## Improving FLAN-T5's summarization capabilities

- to build an app to support your customer service team, process requests received through a chat bot
- Goal: Summarize conversations to identify actions to take
- However, the examples in the dataset are mostly conversations between friends about day-to-day activities
- Can perform additional fine-tuning of the FLAN-T5 model using a dialogue dataset that is much closer to the customer service chats
- Further fine-tune FLAB-T5 with domain-specific instruction dataset (dialogsum)
	- 13000+ support chat dialogues and summaries
	- not part of the FLAN-T5 training data (unseen data)

## Example support-dialog summarization

- `slide P.44`
- https://huggingface.co/datasets/knkarthick/dialogsum/viewer/knkarthick--dialogsum/

## Summary before fine-tuning FLAN-T5 with our dataset

- `slide P.45`
- Missing important information such as Mike asking for information to facilitate check-in 
- invented information that was not included in the original conversation, i.e. hotel name and city located

## Summary after fine-tuning FLAN-T5 with our dataset

- `slide P.48`
- no fabricated information
- everyone's name in the conversation

## Knowledge check

What is the purpose of fine-tuning with prompt datasets?

- [ ] To increase the computational resources required for training a language model.

- [ ] To eliminate the need for instructions and prompts in training a language model.

- [ ] To decrease the accuracy of a pre-trained language model by introducing new prompts.

- [x] To improve the performance and adaptability of a pre-trained language model for specific tasks.

```
This option accurately describes the purpose of fine-tuning with prompt datasets. It aims to improve the performance and adaptability of a pre-trained language model by training it on specific tasks using instruction prompts.
```

# Scaling instruct models

[This paper](https://arxiv.org/abs/2210.11416) introduces FLAN (Fine-tuned LAnguage Net), an instruction finetuning method, and presents the results of its application. The study demonstrates that by fine-tuning the 540B PaLM model on 1836 tasks while incorporating Chain-of-Thought Reasoning data, FLAN achieves improvements in generalization, human usability, and zero-shot reasoning over the base model. The paper also provides detailed information on how each these aspects was evaluated.

`Scaling-instruct-models.png`

Here is the image from the lecture slides that illustrates the fine-tuning tasks and datasets employed in training FLAN. The task selection expands on previous works by incorporating dialogue and program synthesis tasks from Muffin and integrating them with new Chain of Thought Reasoning tasks. It also includes subsets of other task collections, such as T0 and Natural Instructions v2. Some tasks were held-out during training, and they were later used to evaluate the model's performance on unseen tasks.

# Model evaluation

## ROUGE

- ROUGE-1, ROUGE-L, ROUGE clipping
- Used for text summarization
- Compares a summary to one or more reference summaries

## BLEU

- Used for text translation
- Compares to human-generated translations
- BLEU metric = Avg(precision across range of n-gram sizes)

(Come back later)

# Benchmarks

- Make use of pre-existing datasets and associated benchmarks
- Selecting the right evaluation dataset is vital
- Datasets that isolate specific model skills:
	- reasoning
	- common sense knowledge
	- potential-risk focused:
		- disinformation 
		- copyright infringement
- Using unseen data to evaluate model would give a more accurate and useful sense of model's capability

## GLUE

- General Language Understanding Evaluation
- Wang et al. 2018, “GLUE: A Multi-Task Benchmark and Analysis Platform for Natural Language Understanding”
- a collection of natural language tasks, such as sentiment analysis and question-answering
- encourage the development of models that can generalize across multiple tasks

## SuperGLUE

- Wang et al. 2019, “SuperGLUE: A Stickier Benchmark for General-Purpose Language Understanding Systems“
- new tasks/more challenging versions of the same tasks, e.g.:
	- multi-sentence reasoning
	- reading comprehension

## Benchmarks for massive models

### Massive Multitask Language Understanding (MMLU)
- Hendrycks, 2021. "Measuring Massive Multitask Language Understanding"

### BIG-bench
- Suzgun et al. 2022. "Challenging BIG-Bench tasks and whether chain-of-thought can solve them"
- consists of 204 tasks, ranging through linguistics, childhood development, math, common sense reasoning, biology, physics, social bias, software development, etc.
- three different sizes: 
	BIG-bench HARD, BIG-bench, Lite
- running these large benchmarks can incur large inference costs

### Holistic Evaluation of Language Models (HELM)
- `Slide p.74`
- Also assess faireness, bias, and toxicity
- continuously evolve with the addition of new scenarios, metrics, and models

