---
layout: post
title: "Generative AI with LLMs Week 1 quiz"
comments: true
tags: ['llms']
---

Here are course notes I am taking from the DeepLearning.ai course on Coursera: [Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms).

<!--more-->

# Week 1 quiz

## Question 1

Interacting with Large Language Models (LLMs) differs from traditional machine learning models.  Working with LLMs involves natural language input, known as a  _____, resulting in output from the Large Language Model, known as the ______ .

Choose the answer that correctly fill in the blanks.

- [x] prompt, completion
- [ ] tunable request, completion
- [ ] prompt, fine-tuned LLM
- [ ] prediction request, prediction response

>Correct: The input for working with LLMs is referred to as the prompt and the output from the LLM is referred to as the completion.

## Question 2
Large Language Models (LLMs) are capable of performing multiple tasks supporting a variety of use cases.  Which of the following tasks supports the use case of converting code comments into executable code?

- [ ] Information Retrieval
- [x] Translation
- [ ] Invoke actions from text
- [ ] Text summarization

> Correct: Translation focuses on converting languages, including coding languages so in this case the task focuses on translating code comments into executable code.

## Question 3
What is the self-attention that powers the transformer architecture?

- [ ] The ability of the transformer to analyze its own performance and make adjustments accordingly.
- [ ] A measure of how well a model can understand and generate human-like language.
- [x] A mechanism that allows a model to focus on different parts of the input sequence during computation.
- [ ] A technique used to improve the generalization capabilities of a model by training it on diverse datasets.

>Correct: Self-attention is a key component in models like Transformers, where it enables the model to attend to different words in the input sequence to capture their relationships and dependencies.

## Question 4
Which of the following stages are part of the generative AI model lifecycle mentioned in the course? (Select all that apply)
- [x] Defining the problem and identifying relevant datasets.
- [x] Deploying the model into the infrastructure and integrating it with the application.
- [x] Selecting a candidate model and potentially pre-training a custom model.
- [x] Manipulating the model to align with specific project needs.
- [ ] Performing regularization

## Question 5
"RNNs are better than Transformers for generative AI Tasks." 

- [ ] True
- [x] False

> Correct: While RNNs can be used for generative AI tasks, they struggle with compute and memory, making it hard to keep context in longer texts. The transformers architecture is more parallelizable and its dynamic attention mechanism helps to capture long-range dependencies in the input.

## Question 6
Which transformer-based model architecture has the objective of guessing a masked token based on the previous sequence of tokens by building bidirectional representations of the input sequence.

- [ ] Sequence-to-sequence
- [x] Autoencoder
- [ ] Autoregressive

## Question 7
Which transformer-based model architecture is well-suited to the task of text translation?

- [x] Sequence-to-sequence
- [ ] Autoregressive
- [ ] Autoencoder

## Question 8
Do we always need to increase the model size to improve its performance?
- [ ] True
- [x] False

> Correct: Recent trends show that we can build better LLMs without necessarily increasing model size year by year. Models like LLaMa and BloombergGPT have demonstrated the possibility of reducing model size while keeping great performance.

## Question 9
Scaling laws for pre-training large language models consider several aspects to maximize performance of a model within a set of constraints and available scaling choices.  Select all alternatives that should be considered for scaling when performing model pre-training?

- [x] Dataset size: Number of tokens
- [x] Compute budget: Compute constraints
- [ ] Batch size: Number of samples per iteration 
- [x] Model size: Number of parameters

## Question 10
"You can combine data parallelism with model parallelism to train LLMs."

Is this true or false?

- [x] True
- [ ] False

> Correct: Combining data parallelism with pipeline parallelism is known as 2D parallelism.We can achieve 3D parallelism by combining data parallelism with both pipeline parallelism and tensor parallelism simultaneously.
