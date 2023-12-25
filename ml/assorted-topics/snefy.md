---
layout: default
title: Squared Neural Families
parent: Assorted Topics
grand_parent: Machine Learning
nav_order: 1
has_children: true
permalink: ml/assorted-topics/snefy
---

# Squared Neural Families

This is based on [Squared Neural Families](https://arxiv.org/pdf/2305.13552.pdf) from NeurIPS 2023. They authors propose a new class of models based on neural networks that serve as density functions. A little more formally, they have created a parameterization of density models, whose parameters are based on the weights and biases of a network.

## Prerequisites

### From Statistics 

It is common in Bayesian statistics to deal with complicated density functions, and oftentimes, the normalizing constant is unknown. The most common way to normalize a probability density function is by the following:

$$
f(x) = \frac{g(x)}{\int g(x) d\mu(x)} 
$$

where $\mu(x)$ is the measure to which $g$ is integrated.

Integration of $g$ may be intractable, so we invoke the law of large numbers and perform stochastic integration. $\mu(x)$ luckily gives us a base measure, and we can generate samples from the random variable associated with the base measure. Then $\int g(x)d\mu(x) = E(g(X))$. So $E(g(X))$ can be approximated with $\frac{1}{N} \sum_{i=1}^N g(X_i)$ where $X$ is sampled from some underlying distribution with respect to the base measure $\mu$. This converges in probability to $E(g(X))$ per LLN.

### From Linear Algebra

Matrix multiplication is the core of neural networks. The weights and biases are matrices, and activations are applied element-wise. Likewise, integration of a matrix is also performed element-wise.


## Squared Neural Families

Suppose we have a function $f(x; \theta)$ which is a neural network of the form $f(x; \theta) = V\sigma(Wx +B)$. This is a 2 layer neural network, with no bias on the second layer, and activation function $\sigma(.)$. 


