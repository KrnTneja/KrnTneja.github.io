---
title: "ALC3: Active Label Correction for Modular AI Systems"
excerpt: "Novel method for improving LLM-based modular AI systems through iterative active label correction"
collection: projects
technologies: "Large Language Models, Active Label Correction, Natual Language Processing, Modular AI Systems, HuggingFace Transformers, OpenAI GPT-3.5"
impact: "Work presented at EMNLP 2024, our novel method ALC3 improved over existing baselines with 17-24% lower human feedback requirements"
header:
  teaser: /images/alc3-teaser.png
date: 2024-11-20
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/NCmZ1a9IdPY?si=A8uj8SRYJGY5AsGu" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Overview

Modular AI systems can be developed using LLM-prompts-based modules to minimize deployment time even for complex tasks. However, these systems do not always perform well and improving them using the data traces collected from a deployment remains an open challenge. The data traces contain LLM inputs and outputs, but the annotations from LLMs are noisy. We hypothesize that Active Label Correction (ALC) can be use on the collected data to train smaller task-specific improved models that can replace LLM-based modules. In this research work, we study the noise in three GPT-3.5-annotated datasets and their denoising with human feedback. We proposed a novel method ALC3 that iteratively applies three updates to the training dataset: auto-correction, correction using human feedback and filtering. Our results showed that ALC3 can lead to oracle performance with feedback on 17-24% fewer examples than the number of noisy examples in the dataset across three different NLP tasks.

### Related Papers

[EMNLP 2024 (Main)](https://arxiv.org/abs/2401.05467), [CHAI Workshop @ IJCAI 2022](https://arxiv.org/pdf/2206.05182), [arXiv:2204.10357](https://arxiv.org/abs/2204.10357)


## Problem Statement

Modular AI systems using LLM-based modules face several challenges:
- **Noisy Annotations**: LLM outputs contain inherent noise and inconsistencies
- **Cascade Effects**: Errors in one module propagate throughout the system
- **Limited Robustness**: Zero-shot learning limitations affect system reliability
- **Domain Shift**: Performance degradation in deployed environments
- **Fine-tuning Constraints**: Difficulty in fine-tuning LLMs for all sub-tasks

## ALC3 Methodology

To address the above challenges, we propose collecting data traces containing LLM inputs and outputs from deployed AI systems, and training smaller task-specific models that can replace LLM calls. We propose a method based on Active Label Correction (ALC) (Rebbapragada et al. 2012) for using human feedback to improve the quality of data obtained from modular AI systems and evaluate its efficacy for three publicly-available NLP datasets. The data traces contain examples with low-quality LLM annotations, but ALC can utilize human expertise in an efficient manner by seeking annotations for examples that are most likely misannotated. We show that synthetic noise models employed by previous works are not adequate to capture noisy LLM annotations which makes it challenging to predict misannotated examples in ALC. The proposed method ALC3 (i) auto-corrects potentially misannotated examples using model predictions, (ii) filters out the confusing examples in addition to (iii) using human feedback. 

Perfect misannotation prediction on a dataset with N%  noisy examples will require human annotation on N%  of data to clean it. Our results show that the three-step process of ALC3 can lead to oracle test performance with feedback on 17-24% fewer examples than N% (perfect predictor) for three NLP tasks with different data sizes (5k-100k) and complexity. The proposed method promotes the idea of leveraging the powerful zero-shot learning capabilities of LLMs for rapidly deploying modular AI systems and improving their quality as they accumulate data from the real world. We focus on discriminative tasks but the proposed method also extends to generative tasks in principle.


![ALC3 Methodology Overview](https://arxiv.org/html/2401.05467v3/x2.png)
*Proposed process for improving LLM-based modular AI systems using ALC3. The inputs and noisy labels from a zero/few-shot learner-based module are used to obtain a trained model. Model predictions on the noisy training dataset are computed for the next three steps. (i) Auto-correction updates the labels where model prediction contradicts the original label with very high confidence. (ii) Human annotation is used to verify and update a fixed number of confusing examples. (iii) Filtering removes some of remaining examples that are deemed noisy based on model predictions. The process is performed iteratively until a stopping condition. Only human annotations are retained after each iteration, iteration two is shown with columns 6, 7, 8, and 9 for illustration.*



Find out more details in our [EMNLP 2024](https://arxiv.org/abs/2401.05467) paper.


### Technologies and Tools

- HuggingFace Transformers (models and training routines)
- OpenAI API (GPT-3.5)

### Team

Karan Taneja (Project Lead), Harsh Sikka, Ashok Goel (Advisors)