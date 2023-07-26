---
layout: post
title: "Generative AI with LLMs Week 3 (2)"
comments: true
tags: ['llms']
---

Here are course notes I am taking from the DeepLearning.ai course on Coursera: [Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms).

<!--more-->

<h1>Topic: LLM-powered applications</h1>

# Model optimizations for deployment

## Distillation

`W3 Slide p.78-79`

- The combined distillation and student losses are used to update the weights of the student model via *back propagation*
- In practice, distillation is not as effective for generative decoder models. 
- Distillation is typically more effective for *encoder-only models*, such as BERT that have a lot of *representation redundancy*

## Post-Training Quantization (PTQ)

- `W3 Slide p.81`
- quantization often results in a small percentage reduction in model evaluation metrics (often be worth the cost savings and performance gains).

## Pruning

- `W3 Slide p.82`

# Generative AI Project Lifecycle Cheat Sheet

- `W3 Slide p.83`

# Using the LLM in applications

## Models having difficulty
- Information is out-of-date
- Wrong answer in math question (arithmatic since LLM is trained on prediction next token, not calculations)
- Hallucination

## Retrieval Augmented Generation (RAG)

(back later)

- Allow the LLM to access information in external data set
- `W3 Slide p.92`
- Lewis et al. 2020 "[Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401)"
- Avoid model hallucinating

## RAG integrates with many types of data sources

External information sources:
- Documents
- Wikis
- Expert Systems
- Web pages
- Databases
- Vector Store

### Vector Store

- vector representations of text
- enable a fast and efficient kind of relevant search based on similarity

### Data preparation for vector store for RAG

Two considerations for using external data in RAG:
1. Data must fit inside context window
  - Prompt context limit: few 1000 tokens
  - Single document is too large to fit in the window
  - Split long sources into short chunks (e.g. LangChain can do that)
2. Data must be in format that allows its relevance to be assessed at inference time: **Embedding vectors**
  - Prompt text converted to embedding vectors
  - Process each chunk with LLM to produce embedding vectors
  - Vector store allows fast searching of datasets and efficient identification of semantically-related text

## Vector database search
- Each text in vector store is identified by a key
- Enables a **citation** to be included in completion

## Knowledge check

Q: How does Retrieval Augmented Generation (RAG) enhance generation-based models?

- [x] By making external knowledge available to the model
- [ ] By applying reinforcement learning techniques to augment completions.
- [ ] By optimizing model architecture to generate factual completions.
- [ ] By increasing the training data size.

```
Correct: The retriever component retrieves relevant information from an external corpus or knowledge base, which is then used by the model to generate more informed and contextually relevant responses. This incorporation of external knowledge enhances the quality and relevance of the generated content.
```

# Interacting with external applications

- `W3 Slides P.104,105`

# Helping LLMs reason and plan with Chain-of-Thought Prompting

- Cannot handle mutli-step reasoning problem
- Strategy: to prompt the model to break the problem down into steps
- `W3 Slides P.108`
- Wei et al. 2022, “[Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903)”

# Program-aided language models (PAL)

- `W3 Slides P.113`
- Gao et al. 2022, “[PAL: Program-aided Language Models](https://arxiv.org/abs/2211.10435)”
- pairs an LLM with an external code interpreter
- use chain of thought prompting to generate executable Python scripts
- python scripts is then passed to an interpreter to execute
- Example: `W3 Slides P.115`

# ReAct: Combining reasoning and action

## ReAct: Synergizing Reasoning and Action in LLMs

- Tailored thought reasoning + action planning
- Proposed by researchers from Princeton and Google in 2022
- Yao et al. 2022, “[ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629)”
- Two benchmarks:
  - HotPot QA: multi-step quesetion answering
  - Fever: Fact verification

## Steps

1. Question
2. Thought
3. Action
4. Observation (Conclusion from action) Go back to 2

Reiterate until the full question is solved.

## ReAct instructions define the action space

```
Solve a question answering task with interleaving Thought, Action, Observation steps.

Thought can reason about the current situation, and Action can be three types:

1. Search[entity]: searches the exact entity on Wikipedia and returns the first paragraph if it exists. If not, it will return some similar entities to search.
2. Lookup[keyword]: returns the next sentence containing keyword in the current passage.
3. Finish[answer]: returns the answer and finishes the task.

Here are some examples.
```
- `W3 Slides P.131`


## LnagChain

- provides you with modular pieces that contain the components necessary to work with LLMs:
  - (1) prompt templates for many different use cases that you can use to format both input examples and model completions
  - (2) memory that you can use to store interactions with an LLM
  - (3) pre-built tools that enable you to carry out a wide variety of tasks, including:
    - calls to external datasets, and 
    - various APIs
- (4) Agents: to interpret the input from the user and determine which tool or tools to use to complete the task
- `W3 Slides P.132`

# ReAct: Reasoning and action

[This paper](https://arxiv.org/abs/2210.03629) introduces ReAct, a novel approach that integrates verbal reasoning and interactive decision making in large language models (LLMs). While LLMs have excelled in language understanding and decision making, the combination of reasoning and acting has been neglected. ReAct enables LLMs to generate reasoning traces and task-specific actions, leveraging the synergy between them. The approach demonstrates superior performance over baselines in various tasks, overcoming issues like hallucination and error propagation. ReAct outperforms imitation and reinforcement learning methods in interactive decision making, even with minimal context examples. It not only enhances performance but also improves interpretability, trustworthiness, and diagnosability by allowing humans to distinguish between internal knowledge and external information.

In summary, ReAct bridges the gap between reasoning and acting in LLMs, yielding remarkable results across language reasoning and decision making tasks. By interleaving reasoning traces and actions, ReAct overcomes limitations and outperforms baselines, not only enhancing model performance but also providing interpretability and trustworthiness, empowering users to understand the model's decision-making process.

`ReAct.png`

**Image:** The figure provides a comprehensive visual comparison of different prompting methods in two distinct domains. The first part of the figure (1a) presents a comparison of four prompting methods: Standard, Chain-of-thought (CoT, Reason Only), Act-only, and ReAct (Reason+Act) for solving a HotpotQA question. Each method's approach is demonstrated through task-solving trajectories generated by the model (Act, Thought) and the environment (Obs). The second part of the figure (1b) focuses on a comparison between Act-only and ReAct prompting methods to solve an AlfWorld game. In both domains, in-context examples are omitted from the prompt, highlighting the generated trajectories as a result of the model's actions and thoughts and the observations made in the environment. This visual representation enables a clear understanding of the differences and advantages offered by the ReAct paradigm compared to other prompting methods in diverse task-solving scenarios.

# LLM application architectures

(back later)

- `W3 Slide P.142`
















