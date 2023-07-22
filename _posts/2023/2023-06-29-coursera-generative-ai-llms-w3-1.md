---
layout: post
title: "Generative AI with LLMs Week 3 (1)"
comments: true
tags: ['llms']
---

Here are course notes I am taking from the DeepLearning.ai course on Coursera: [Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms).

<!--more-->

<h1>Topic: Reinforcement Learning from Human Feedback</h1>

# Aligning models with human values

Bad behavior of models:
- Toxic language
- Aggressive responses
- Providing dangerous information
- Desired HHH: Helpful? Honest? Harmless?

## Reinforcement learning from human feedback (RLHF)

`Instruct fine-tuned LLM -> Reinforcement Learning from Human feedback -> Human- aligned LLM`

- Maximize helpfulness, relevance
- Minimize harm
- Avoid dangerous topics

##  Reinforcement learning: fine-tune LLMs

`W3 Slide 14`

- rollout: the series of actions and corresponding states form a playout

# RLHF: Obtaining feedback from humans

(back later)

- Chung et al. 2022, “Scaling Instruction-Finetuned Language Models”
- Stiennon et al. 2020, “Learning to summarize from human feedback”

# RLHF: Reward model

(back later)

# RLHF: Fine-tuning with reinforcement learning

(back later)

# Optional video: Proximal policy optimization

By: Dr. Ehsan Kamalinejad

- Updates of LLM are small and within a bounded region, the *trust region*  -> more stable learning.
- Ideally, this series of small updates will move the model towards higher rewards

`W3 Slide p.45`

## PPO Phase 2: Calculate Entropy loss

- entropy allows the model to maintain creativity
- If you kept entropy low, you might end up always completing the prompt in the same way as shown here
- similar to the temperature setting of LLM that you've seen in Week 1
- The difference is that the temperature influences model creativity at the inference time, while the entropy influences the model creativity during training
- `W3 Slide p.47`

## Other remarks

- Q-learning is an alternate technique for fine-tuning LLMs through RL
- PPO is (more) popular because it has the right balance of complexity and performance
- researchers at Stanford published a paper describing a technique called *direct preference optimization* (DPO), which is a simpler alternate to RLHF
- Rafailov et al. (2023) "[Direct Preference Optimization:
Your Language Model is Secretly a Reward Model](https://arxiv.org/pdf/2305.18290.pdf)"

# RLHF: Reward hacking

- Reward hacking: the agent learns to cheat the system by favoring actions that maximize the reward received even if those actions don't align well with the original objective. 
- LLMs reward hacking: addition of words/phrases to completions resulting in high scores for the metric being aligned, but reduce the overall quality of the language
  - e.g. Detoxification using toxicity reward model leading the LLM to diverge too much from the initial language model
  	- "...the most awesome, most incredible thing ever."
  	- "Beautiful love and world peace all around."

# Avoiding reward hacking 

- Freeze the initial instruct LLM as performance reference
- Compare the "Reference model" and "RL-updated LLM" in terms of *KL Divergence Shift Penalty*
- KL(Kullback-Leibler) divergence: statistical measure of how different two probability distributions are
- KL divergence penalty gets added to reward model, which penalize the RL updated model if it shifts too far from the reference LLM that are too different
- You now need **two full copies of the LLM** to calculate the KL divergence, the frozen reference LLM, and the RL-updated PPO LLM.
- Combining RLHF with PEFT: 
  - only need to update the weights of a PEFT adapter, not the full weights of the LLM
  - you can reuse the same underlying LLM for both the reference model and the PPO model, which you update with a trained path parameters
  - reduces the memory footprint during training by approximately half
- `W3 Slide 58, 59`

## Evaluate the human-aligned LLM
- `W3 Slide 60`

## Knowledge checks

Q: How can RLHF align the performance of large language models with human preferences? Select all that apply

- [x] RLHF can enhance the interpretability of generated text

```
Correct: By involving human feedback, models can be tuned to provide explanations or insights into their decision-making processes, improving interpretability and allowing users to better understand the model's outputs.
```

- [ ] RLHF increases the model’s size by adding new parameters that represent human preferences

- [x] RLHF can help reduce model toxicity and misinformation

```
Correct:The human feedback helps guiding the answers, if the human feedback rewards when toxicity and misinformation is avoided, it will learn reduce that.
```

- [ ] Inference is faster after RLHF, improving the user experience

# KL divergence

`KL-divergence.png`

KL-Divergence, or Kullback-Leibler Divergence, is a concept often encountered in the field of reinforcement learning, particularly when using the Proximal Policy Optimization (PPO) algorithm. It is a mathematical measure of the difference between two probability distributions, which helps us understand how one distribution differs from another. In the context of PPO, KL-Divergence plays a crucial role in guiding the optimization process to ensure that the updated policy does not deviate too much from the original policy.

In PPO, the goal is to find an improved policy for an agent by iteratively updating its parameters based on the rewards received from interacting with the environment. However, updating the policy too aggressively can lead to unstable learning or drastic policy changes. To address this, PPO introduces a constraint that limits the extent of policy updates. This constraint is enforced by using KL-Divergence.

To understand how KL-Divergence works, imagine we have two probability distributions: the distribution of the original LLM, and a new proposed distribution of an RL-updated LLM. KL-Divergence measures the average amount of information gained when we use the original policy to encode samples from the new proposed policy. By minimizing the KL-Divergence between the two distributions, PPO ensures that the updated policy stays close to the original policy, preventing drastic changes that may negatively impact the learning process.

A library that you can use to train transformer language models with reinforcement learning, using techniques such as PPO, is TRL (Transformer Reinforcement Learning). In [this link](https://huggingface.co/blog/trl-peft)  you can read more about this library, and its integration with PEFT (Parameter-Efficient Fine-Tuning) methods, such as LoRA (Low-Rank Adaption). The image shows an overview of the PPO training setup in TRL.

# Scaling human feedback

- human effort required to produce the trained reward model in the first place is huge
- Potential solutions: to scale through model self-supervision: [Constitutional AI](https://arxiv.org/abs/2212.08073)
  - first proposed in 2022 by researchers at Anthropic
  - a method for training models using a set of rules and principles that govern the model's behavior
  - together with a set of sample prompts, these form the constitution
  - train the model to self critique and revise its responses to comply with those principles
  - not only useful for scaling feedback, it can also help address some unintended consequences of RLHF, e.g.
    - an aligned model may end up revealing harmful information when providing the most helpful response it can ("How to hack neighbor's wifi?")

## Constitution AI

- 2 distinct phases:
- Stage 1: Supervised Learning Stage
  - Red teaming: start your prompt the model in ways that try to get it to generate harmful responses
  - Fine-tuned LLM is trained based on the pairs of red team prompts and the revised constitutional responses
  - `W3 slide P.66 (67)`
- Stage 2:
  - Reinforcement learning by AI feedback (RLAIF)
  - `W3 slide P.68`

