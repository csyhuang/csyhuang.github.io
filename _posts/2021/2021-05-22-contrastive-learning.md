---
layout: post
title: Discussion on Contrastive Learning
comments: true
tags: ['machine_learning_journal_club']
---

I led the Machine Learning Journal Club discussion on the two papers:  
- Wang, G., Wang, K., Wang, G., Torr, P. H., & Lin, L. (2021). [Towards Solving Inefficiency of Self-supervised Representation Learning.](https://arxiv.org/abs/2104.08760) arXiv preprint arXiv:2104.08760.
- Chen, T., Kornblith, S., Norouzi, M., & Hinton, G. (2020, November). [A simple framework for contrastive learning of visual representations.](https://arxiv.org/abs/2002.05709) In International conference on machine learning (pp. 1597-1607). PMLR.

<!--more-->

You can find [here](https://docs.google.com/presentation/d/1ZS-L-o46h68Q78UR-7nCCRZls4a53M6L_73CHr6xFN4/edit?usp=sharing) the slides I made which provides an introduction to Contrastive Learning. Below are the main points from the slides:  
- Contrastive learning is a self-supervised method to learn a representation of objects by maximizing/minimizing distance between the same/different class(es)
- Contrastive learning benefits from data augmentation and increase in model parameters
- Under-clustering occurs when there is not enough negative samples to learn from; over-clustering occurs when the model overfits (memorize the data)
- To solve inefficiency issue, median(rank-k) triplet loss is used instead of the total loss

