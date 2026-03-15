# Biomedical Small Language Model (SLM) for Hallmarks of Cancer Classification

## Overview

This project builds an **end-to-end biomedical language modeling
pipeline** that trains a **Small Language Model (SLM)** on PubMed
abstracts and fine-tunes it using **soft prompt tuning** to perform
**multi-label classification of the Hallmarks of Cancer**.

The system includes: - Large-scale biomedical data preprocessing -
Transformer architecture implemented from scratch in PyTorch - Language
model pretraining on PubMed abstracts - Parameter-efficient fine-tuning
using prompt tuning - Constrained decoding for structured predictions

------------------------------------------------------------------------

## Project Pipeline

PubMed XML\
↓\
Streaming XML Parsing\
↓\
Title + Abstract Extraction\
↓\
Tokenization (Moses)\
↓\
BioGPT BPE Encoding\
↓\
Memmap Dataset Creation\
↓\
SLM Pretraining (GPT-style Transformer)\
↓\
Soft Prompt Tuning\
↓\
Constrained Decoding\
↓\
Hallmarks of Cancer Classification

------------------------------------------------------------------------

## Key Features

### Transformer Implementation from Scratch

The project implements a GPT-style Transformer architecture including: -
Causal Self-Attention - Multi-Head Attention - Residual Connections -
Layer Normalization - Feedforward Networks with GELU - Positional
Embeddings - Weight tying between embedding and output layers

### Large-Scale Data Processing

-   Streaming XML parsing using `lxml`
-   Extraction of **Title + Abstract**
-   Efficient dataset storage using **NumPy memmaps**

Pretraining dataset size: \~10GB PubMed abstracts

### Domain-Specific Tokenization

The project uses **BioGPT BPE tokenization** for biomedical terminology.

Pipeline: 1. Moses tokenizer 2. FastBPE encoding 3. BioGPT vocabulary
mapping

------------------------------------------------------------------------

## Model Architecture

Configuration used:

Layers: 6\
Hidden size: 384\
Attention heads: 6\
Context length: 128 tokens\
Parameters: \~30M

Architecture:

Token Embedding + Position Embedding\
↓\
Transformer Blocks × 6\
↓\
LayerNorm\
↓\
Language Modeling Head

------------------------------------------------------------------------

## Training Strategy

### SLM Pretraining

Objective: **Causal Language Modeling**

Example:

Cancer cells resist → apoptosis

Training techniques: - Mixed precision training (FP16/BF16) - Gradient
clipping - Cosine learning rate schedule - Warmup learning rate - AdamW
optimizer

------------------------------------------------------------------------

## Fine-Tuning: Soft Prompt Tuning

Instead of updating the full model, a **soft prompt vector** is learned.

Input format:

\[Soft Prompt\] + Abstract → Model → Hallmark Labels

Benefits: - Efficient training - Few parameters updated - Preserves
pretrained knowledge

------------------------------------------------------------------------

## Constrained Decoding

During inference, model outputs are restricted to:

-   Valid hallmark labels
-   Separator tokens
-   End-of-sequence token

This converts the language model into a **structured multi-label
classifier**.

------------------------------------------------------------------------

## Hallmarks of Cancer Labels

1.  Activating invasion and metastasis
2.  Avoiding immune destruction
3.  Cellular energetics
4.  Enabling replicative immortality
5.  Evading growth suppressors
6.  Genomic instability and mutation
7.  Inducing angiogenesis
8.  Resisting cell death
9.  Sustaining proliferative signaling
10. Tumor promoting inflammation

------------------------------------------------------------------------

## Evaluation Metrics

The system is evaluated using:

-   Micro F1 Score
-   Macro F1 Score
-   Precision
-   Recall
-   Per-class F1

------------------------------------------------------------------------

## Technologies Used

-   Python
-   PyTorch
-   NumPy
-   FastBPE
-   Sacremoses
-   lxml
-   tqdm

------------------------------------------------------------------------

## Future Improvements

Potential improvements include: - Larger transformer models - Longer
context lengths - LoRA fine-tuning - Retrieval-augmented biomedical
models - Explainability methods

------------------------------------------------------------------------

## Author

Pranav Pothan
