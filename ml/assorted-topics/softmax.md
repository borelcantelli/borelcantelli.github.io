---
layout: default
title: Softmax
parent: Assorted Topics
grand_parent: Machine Learning
nav_order: 2
has_children: true
permalink: ml/assorted-topics/softmax
---

# Softmax Function

The softmax function is a popular transformation of discrete data in machine learning. It is used to convert numbers such as scores resulting from an activation in $\mathbb{R}$ to $[0, 1]$. It is not always understood though.

Consider the Log-Sum-Exp formula, $f: \mathbb{R}^n \to \mathbb{R}$

$$
f(x_1,...,x_n) = \log \sum_{i=1}^n e^{x_i}
$$

This approximates the maximum function. To see this, consider how we build the log-sum-exp function. ($x_(n)$ denotes the maximum).

$$
\sum x_i \leq nx_{(n)}
$$

Because $e^x$ is a monotonically increasing function, and $x \leq e^x$ always

$$
e^{x_(n)} \leq \sum_{i=1}^n e^{x_i} \leq ne^{x_{(n)}}
$$

At this point, the bound seems very wide. However we can take logs to shrink this bound.

$$
x_{(n)} \leq \log \sum_{i=1}^n e^{x_i} \leq ne^{x_{(n)}} \leq x_{(n)} + \log n
$$

So the log-sum-exp function $f$ is at least as large as $x_{(n)}$ and is no larger than the max $+\log n$.

