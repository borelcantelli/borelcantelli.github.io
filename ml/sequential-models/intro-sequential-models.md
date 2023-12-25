---
layout: default
title: Sequential Models
parent: Machine Learning
nav_order: 1
has_children: true
permalink: ml/sequential-models
---

# Sequential Models

Sequential models are models that can either:

1. Take a scalar and output a sequence
2. Take a sequence and output a sequence
3. Take a sequence and output a scalar

The most important quality of a sequence model is the ability to handle sequences in a manner that respects ordering in the sequence. That is, it processes the data in a sequential manner, in its input and output.

The most common sequence models are RNNs, LSTMs, and more recently transformers. A significant use case of these models is in language modelling, and tokenization and embedding space becomes an important concept. 