---
layout: post
title: 'Reasoning with o1 - short course from DeepLearning.ai'
comments: true
tags: ['coursera', 'deepLearning.ai', 'backlog']
---

Here are some notes taking from the Short Course [Reasoning with o1](https://www.deeplearning.ai/short-courses/reasoning-with-o1/) offered by DeepLearning.ai.

<!--more-->

# Introduction to o1

## Breakthrough \#1

A breakthrough in reasoning: the ability to think for a longer time (at inference time) and get better results.

## Breakthrough \#2

Teaching it to verify outputs via consensus voting. (See [Large Language Monkeys: Scaling Inference Compute with Repeated Sampling](https://arxiv.org/abs/2407.21787))

Improvements observed in:
- ML Benchmarks
- Exams
- PhD-Level Science Questions (GPQA Diamond)
- MMLU Categories

## How does OpenAI o1 work?
- usees large-scale RL to generate a chain of thought (CoT) [Wei et al. 2022](Chain-of-Thought Prompting Elicits Reasoning in Large Language Models) before answering
- CoT is longer and high-quality than what is attained via prompting
- **[CoT contains behavior like]**:
	- Error correction
	- Trying multiple strategies
	- Breaking down problems into smaller steps
- **[Example CoTs on the research [blog post](https]**://openai.com/index/learning-to-reason-with-llms/)!
- o1 made progress in **Abstract reasoning** compared to 4o, e.g., being able to correctly classify 16 words into 4 categories.

## The Generator-Verifier Gap
- For some problems, verifying a good solution is easier than generating one
	- Many puzzles (Sudoku, for example)
	- Math
	- Programming
- Examples where verification isn't much easier
	- Information retrieval (What is the capital of Bhutan?)
	- Image recognition
- When a generator-verifier gap exists and *we have a good verifier*, we can **spend more compute on inference to achieve better performance**

## Where might this be used?

- **[Data Analysis]**: Interpreting complex datasets (e.g., genomic sequencing results in biology) and performing advanced statistical reasoning.
- **[Mathematical Problem-Solving]**: Deriving solutions or proofs for challenging mathematical questions or in physics theory.
Experimental Design: Proposing experimental setups in chemistry to test novel reactions or interpreting complicated physics experiments' outcomes.
- **[Scientific Coding]**: Writing and debugging specialized code for computational fluid dynamics models or astrophysics simulations.
Biological & Chemical Reasoning: Solving advanced biology or chemistry questions that require deep domain knowledge.
- **[Algorithm Development]**: Aiding in creating or optimizing algorithms for data analysis workflows in computational neuroscience or bioinformatics.
- **[Literature Synthesis]**: Reasoning across multiple research papers to form coherent conclusions in interdisciplinary fields such as systems biology.

## Recap

- The ol family of models scale compute at inference time by producing tokens to **reason** through the problem
- ol gives you more **intelligence** as a trade-off against higher **latency** and **cost**
- It can also perform well at tasks that require a **test and learn approach** where it can iteratively verify its results
- Some great emerging use cases are **planning**, **coding**, and **domain-specific reasoning** like law and STEM subjects

# Prompting o1

## How to prompt the o1 model to achieve good results?

### Simple & direct
Write prompts that are straightforward and concise. Direct instructions yuield the best results with the o1 model.

### No explicit CoT required
You can skip step-by-step ('Chain of Thought') reasoning prompts. The o1 models can infer and execute these itself without detailed breakdowns.

### Structure

Break complex prompts into sections using delimters like markdown, XML tags, or quotes.

This structured format enhances model accuracy - and simplifies your own troubleshooting.

### Show rather than tell

Rather than using excessive explanation, give a contextual example to give the model understanding of the broad domain of your task.

<!-- # Planning with o1

## Architecture

Scenario: a task that requires multi-step logic

# Coding with o1

https://onecompiler.com/react -->



