---
layout: post
title: "Generative AI with LLMs Week 2 quiz"
comments: true
tags: ['llms']
---

Here are course notes I am taking from the DeepLearning.ai course on Coursera: [Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms).

<!--more-->

# Week 2 quiz

## Question 1

Fill in the blanks: __________ involves using many prompt-completion examples as the labeled training dataset to continue training the model by updating its weights.  This is different from _________ where you provide prompt-completion examples during inference.

- [ ] Prompt engineering, Pre-training
- [x] Instruction fine-tuning, In-context learning
- [ ] Pre-training, Instruction fine-tuning
- [ ] In-context learning, Instruction fine-tuning 

## Question 2
Fine-tuning a model on a single task can improve model performance specifically on that task; however, it can also degrade the performance of other tasks as a side effect.  This phenomenon is known as: 
- [ ] Model toxicity
- [ ] Catastrophic loss
- [ ] Instruction bias
- [x] Catastrophic forgetting

## Question 3
Which evaluation metric below focuses on precision in matching generated output to the reference text and is used for text translation?

- [ ] HELM
- [ ] ROUGE-2
- [ ] ROUGE-1
- [x] BLEU

## Question 4
Which of the following statements about multi-task finetuning is correct? Select all that apply:

- [x] Multi-task finetuning can help prevent catastrophic forgetting.
- [x] FLAN-T5 was trained with multi-task finetuning.
- [ ] Multi-task finetuning requires separate models for each task being performed.
- [ ] Performing multi-task finetuning may lead to slower inference.

## Question 5
"Smaller LLMs can struggle with one-shot and few-shot inference:"

- [x] True
- [ ] False

## Question 6
Which of the following are Parameter Efficient Fine-Tuning (PEFT) methods? Select all that apply.
- [x] Additive
- [x] Reparameterization
- [ ] Subtractive
- [x] Selective

## Question 7
Which of the following best describes how LoRA works?
- [x] LoRA decomposes weights into two smaller rank matrices and trains those instead of the full model weights.
- [ ] LoRA trains  a smaller, distilled version of the pre-trained LLM to reduce model size
- [ ] LoRA freezes all weights in the original model layers and introduces new components which are trained on new data.
- [ ] LoRA continues the original pre-training objective on new data to update the weights of the original model.


## Question 8
What is a soft prompt in the context of LLMs (Large Language Models)?
- [x] A set of trainable tokens that are added to a prompt and whose values are updated during additional training to improve performance on specific tasks.
- [ ] A strict and explicit input text that serves as a starting point for the model's generation.
- [ ] A technique to limit the creativity of the model and enforce specific output patterns.
- [ ] A method to control the model's behavior by adjusting the learning rate during training.

## Question 9
"Prompt Tuning is a technique used to adjust all hyperparameters of a language model."
- [ ] True
- [x] False

## Question 10
"PEFT methods can reduce the memory needed for fine-tuning dramatically, sometimes to just 12-20% of the memory needed for full fine-tuning."

Is this true or false?

- [x] True
- [ ] False
