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

Suppose we have a function $f(x; \theta)$ which is a neural network of the form $f(x; \theta) = V\sigma(Wx +B)$. This is a 2 layer neural network, with no bias on the second layer, and activation function $\sigma(.)$. $f$ is not guaranteed to be $\geq 0$, which is a requirement for normalizable functions for use as probabilities. To rectify this, simply square the function (in the case of vector valued $f$, take the square of the Euclidean norm).

Let $\sigma(\Theta) := \sigma(Wx+B)$. Thus $\|f\|_2^2 = f^T f$ gives:

$$
(f^T f)(x) = \sigma^T(\Theta)V^T V \sigma(\Theta) \geq 0
$$

To normalize this function, divide it by its integral, with respect to the measure of $x$.

$$
d\nu(x) = \frac{f^Tf}{\int f^T f d\mu(x)} = \frac{\sigma^T(\Theta)V^T V \sigma(x)}{\int \sigma^T(\Theta)V^T V \sigma(\Theta) d\mu(x) }
$$

To understand this function better, let $x \in \mathbf{R}^{N x 1}$, $W \in \mathbf{R}^{M \times N}$ and $V \in \mathbf{M \times O}$. Thus the network $f$ takes the $N$ vector and casts it to $O$ dimensional space, with one hidden layer of size $M$. 

$Wx + B$ is a $M \times 1$ vector, and the activation is applied element-wise, so it does not affect dimensionality. $\sigma(\Theta)^T$ is $1 \times M$ and $V^T$ is $M \times O$ therefore, $\sigma(\Theta)^T V^T$ is $1 \times O$. Likewise, $V\sigma(\Theta)$ is $O \times 1$, so $f^T f$ is representable as a scalar, which is the same as its trace.

In fact, this all holds true even if we have $Wt(x) + B$, where $t(.)$ is some function applied onto $x$ either element-wise or batch-wise. What exactly goes into the scalar of $f^T f$? Note that $\sigma(\Theta)$ is:

$$
\begin{bmatrix}
\sigma(W_{1\cdot} \cdot t(x) + B_1) \\
\sigma(W_{2\cdot} \cdot t(x) + B_2) \\
\vdots \\
\sigma(W_{m\cdot} \cdot t(x) + B_m) 
\end{bmatrix}
$$

So $f = V\sigma(\Theta)$ is:

$$
\begin{bmatrix}
V_{1\cdot} \sum_{i=1}^m \sigma(W_{i\cdot} \cdot t(x) + B_i) \\
V_{2\cdot} \sum_{i=1}^m \sigma(W_{i\cdot} \cdot t(x) + B_i) \\
\vdots \\
V_{O\cdot} \sum_{i=1}^m \sigma(W_{i\cdot} \cdot t(x) + B_i) \\
\end{bmatrix}
$$

Thus $f^T f$ becomes:

$$
\sum_{k=1}^m \sum_{i=1}^m \sigma(W_{i\cdot} \cdot t(x) + B_i)^T V_{k\cdot}^T V_{k\cdot}  \sigma(W_{i\cdot} \cdot t(x) + B_i) 
$$

This is the trace of $\sigma^T(\Theta) V^T V \sigma(\Theta)$. By the cycle property of the trace:

$$
tr(\sigma^T V^T V \sigma) = tr(V^T V \sigma \sigma^T)
$$

Therefore, we can conclude that 

$$
d\nu(x) = \frac{tr(V^T V \sigma(Wx +B) \sigma^T(Wx +B))}{\int tr(V^T V \sigma(Wx +B) \sigma^T(Wx +B)) d\mu(x)}
$$

Here $V^T V$ is an $M \times M$ matrix, and $\sigma(Wx +B) \sigma(Wx + B)$ is also $M \times M$. The trace of this matrix is the sum of its diagonal which is a finite sum, so we can interchange the trace and integration in this case. So now we have:

$$
d\nu(x) = \frac{tr(V^T V \sigma(Wx +B) \sigma^T(Wx +B))}{tr \left(\int V^T V \sigma(Wx +B) \sigma^T(Wx +B) d\mu(x)\right)}
$$

