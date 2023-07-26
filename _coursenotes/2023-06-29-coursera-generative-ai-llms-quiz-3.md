---
layout: post
title: "Generative AI with LLMs Week 3 quiz"
comments: true
tags: ['llms']
---

Here are course notes I am taking from the DeepLearning.ai course on Coursera: [Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms).

<!--more-->

# Week 3 quiz


## Question 1

Which of the following are true in regards to Constitutional AI? Select all that apply.

- [ ] For constitutional AI, it is necessary to provide human feedback to guide the revisions.
- [x] To obtain revised answers for possible harmful prompts, we need to go through a Critique and Revision process.
- [x] In Constitutional AI, we train a model to choose between different prompts.
- [x] Red Teaming is the process of eliciting undesirable responses by interacting with a model.

## Question 2
What does the "Proximal" in Proximal Policy Optimization refer to?

- [ ] The algorithm's proximity to the optimal policy
- [x] The constraint that limits the distance between the new and old policy
- [ ] The use of a proximal gradient descent algorithm
- [ ] The algorithm's ability to handle proximal policies.


## Question 3

"You can use an algorithm other than Proximal Policy Optimization to update the model weights during RLHF."

- [x] True
- [ ] False

## Question 4
In reinforcement learning, particularly with the Proximal Policy Optimization (PPO) algorithm, what is the role of KL-Divergence? Select all that apply.

- [ ] KL divergence is used to train the reward model by scoring the difference of the new completions from the original human-labeled ones.
- [x] KL divergence measures the difference between two probability distributions.
- [x] KL divergence is used to enforce a constraint that limits the extent of LLM weight updates.
- [ ] KL divergence encourages large updates to the LLM weights to increase differences from the original model.

## Question 5
Fill in the blanks: When fine-tuning a large language model with human feedback, the action that the agent (in this case the LLM) carries out is ________ and the action space is the _________.

- [ ] Processing the prompt, context window.
- [x] Generating the next token, vocabulary of all tokens.
- [ ] Calculating the probability distribution, the LLM model weights.
- [ ] Generating the next token, the context window

## Question 6
How does Retrieval Augmented Generation (RAG) enhance generation-based models?

- [x] By making external knowledge available to the model
- [ ] By applying reinforcement learning techniques to augment completions. 
- [ ] By increasing the training data size.
- [ ] By optimizing model architecture to generate factual completions.


## Question 7
How can incorporating information retrieval techniques improve your LLM application? Select all that apply.

- [x] Improve relevance and accuracy of responses
- [ ] Reduced memory footprint for the model
- [x] Overcome Knowledge Cut-offs
- [ ] Faster training speed when compared to traditional models


## Question 8

What is a correct definition of Program-aided Language (PAL) models?

- [ ] Models that assist programmers in writing code through natural language interfaces.
- [ ] Models that integrate language translation and coding functionalities.
- [x] Models that offload computational tasks to other programs.
- [ ] Models that enable automatic translation of programming languages to human languages.


## Question 9

Which of the following best describes the primary focus of ReAct?
- [ ] Investigating reasoning abilities in LLMs through chain-of-thought prompting.
- [x] Enhancing language understanding and decision making in LLMs.
- [ ] Studying the separate topics of reasoning and acting in LLMs.
- [ ] Exploring action plan generation in LLMs.

## Question 10

**Only partially correct. While the LangChain framework does provide prompt templates, agents, and memory components, this is not its primary purpose.**

What is the main purpose of the LangChain framework?
- [ ] To evaluate the LLM's completions and provide fast prototyping and deployment capabilities.
- [ ] To provide prompt templates, agents, and memory components for working with LLMs.
- [ ] To connect with external APIs and datasets and offload computational tasks.
- [x] To chain together different components and create advanced use cases around LLMs, such as chatbots, Generative Question-Answering (GQA), and summarization.


Coursera Honor Code  Learn more
I, Shao Ying Huang, understand that submitting work that isn’t my own may result in permanent failure of this course or deactivation of my Coursera account.
Last saved on Jul 22, 3:54 PM CDT
