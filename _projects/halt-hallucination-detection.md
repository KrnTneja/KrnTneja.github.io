---
title: "HALT: Hallucination Assessment via Log-probs as Time series"
excerpt: "A lightweight black-box hallucination detector using token log-probabilities as a time series"
collection: projects
technologies: "LLMs, Time-series Classification, GRU, Calibration Modeling, vLLM, PyTorch"
impact: "arXiv preprint; introduces HUB benchmark spanning 10 LLM capabilities; 30× smaller and 60× faster than encoder-based baselines"
header:
  teaser: /images/halt-teaser.png
date: 2026-02-01
---

## Overview

Hallucinations remain a major obstacle for large language models (LLMs), especially in safety-critical domains. **HALT** (Hallucination Assessment via Log-probs as Time series) is a lightweight hallucination detector that uses only the top-20 token log-probabilities from LLM generations as a time series. Unlike white-box methods, HALT does not require access to hidden states or attention maps; unlike typical black-box methods, it operates on log-probabilities rather than surface-form text, enabling stronger domain generalization and compatibility with proprietary LLMs without access to internal weights.

HALT uses a gated recurrent unit (GRU) combined with entropy-based features to learn model-specific calibration bias—how a model’s confidence patterns relate to correctness. We also introduce **HUB** (Hallucination detection Unified Benchmark), which unifies prior datasets into ten capabilities: reasoning tasks (Algorithmic, Commonsense, Mathematical, Symbolic, Code Generation) and general-purpose skills (Chat, Data-to-Text, Question Answering, Summarization, World Knowledge). HUB covers both *factual* and *logical* hallucinations (e.g., flawed reasoning traces). While being about 30× smaller, HALT outperforms Lettuce, a finetuned ModernBERT-base encoder, and achieves roughly 60× speedup on HUB. We release two variants, HALT-L (trained on Llama 3.1-8B log-probabilities) and HALT-Q (Qwen 2.5-7B), showing that compact sequence models can capture temporal uncertainty patterns that aggregate confidence metrics miss.

### Related Papers

[arXiv preprint (2026)](https://arxiv.org/pdf/2602.02888)

### Technologies and Tools

LLMs, Time-series Classification, GRU, Calibration Modeling, Token Log-probabilities, Entropy-based Features, vLLM, PyTorch, HUB Benchmark

### Team

Ahmad Shapiro, Karan Taneja, Ashok Goel (Georgia Institute of Technology)
