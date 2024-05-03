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

## Log-Sum-Exp

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
e^{x_{(n)}} \leq \sum_{i=1}^n e^{x_i} \leq ne^{x_{(n)}}
$$

At this point, the bound seems very wide. However we can take logs to shrink this bound.

$$
x_{(n)} \leq \log \sum_{i=1}^n e^{x_i} \leq ne^{x_{(n)}} \leq x_{(n)} + \log n
$$

So the log-sum-exp function $f$ is at least as large as $x_{(n)}$ and is no larger than the $x_{(n)}+\log n$.

## Deriving Log-Sum-Exp

What happens when we derive $f$ with respect to $x_i$? What does it mean? The classical interpretation of $\partial f / \partial x_i$ is that for a unit change in $x_i$, how much does $f$ change. That is, if I increase $x_i$, how much can I increase the estimate of the maximum, holding all else constant. $f$ is a monotonically increasing function with respect to each $x_i$, so we expect this derivative to be positive. This derivative turns out to be the softmax.

$$
\frac{\partial}{\partial x_j} \log \sum_{i=1}^n e^{x_{i}} = \frac{1}{\sum_{i=1}^n e^{x_{i}}} \frac{\partial}{\partial x_j}\sum_{i=1}^n e^{x_{i}} = \frac{e^{x_j}}{\sum_{i=1}^n e^{x_{i}}}
$$

This holds true for any $j$. This derivative is bounded by $(0,1]$ So increasing $x_i$ by a 1 will lead to a positive increase in the approximation of the maximum by at most 1. Of course, this insight is not very meaningful in the realm of machine learning, but it shows the relationship between softmax, and the approximation of the maximum value. 

We typically just view the softmax as a way to form a distribution over the $n$ classes $(x_1,...,x_n)$ where $x_1$ represents how likely the first class is to be sampled. The denominator of the softmax provides a normalizing term to form valid probabilities.

## Temperature

Temperature, a non-negative value, is typically incorporated into the softmax function by dividing all $x_i$ by the temperature parameter $T$. Temperature serves to dampen or sharpen the value of $\frac{e^{x_i}}{\sum_{i=1}^n e^{x_i}}$. This modulates how much of an effect incrementing $x_i$ should have on the log-sum-exp approximation of the maximum. Nonetheless, the effect should still be bounded by 1. As such, when viewed as a probability mass function, the temperature will also modulate our probabilities of each class.

What happens when we let $T \to \infty$? 

$$
\lim_{T\to\infty} \frac{e^{x_j/T}}{\sum_{i=1}^n e^{x_{i}/T}} = \frac{1}{\sum_{i=1}^n 1} = \frac{1}{n}
$$

This holds for any $j$. This means that the distribution over the classes becomes uniform.

What happens when we let $T \to 0$ from above (since $T > 0$)?

Suppose $x_j$ was the unique maximum.

$$
\lim_{T\to 0^+} \frac{e^{x_j/T}}{e^{x_j/T}+\sum_{1 \leq i \neq j \leq n} e^{x_{i}/T}} = \lim_{T\to 0^+} \frac{1}{\frac{e^{x_j/T}+\sum_{1 \leq i \neq j \leq n} e^{x_{i}/T}}{e^{x_j/T}}}
$$

Then we can divide the numerator and the denominator in the denominator of the overall fraction.

$$
\lim_{T\to 0^+} \frac{1}{1 + \frac{\sum_{1 \leq i \neq j \leq n} e^{x_{i}/T}}{e^{x_j}}} = \frac{1}{1 + \sum_{1 \leq i \neq j \leq n} e^{(x_{i}-x_{j})/T}}
$$

Now we know that $x_j > x_i$ since it is the unique maximum. Then $x_i - x_j < 0$. Therefore, $e^{(x_{i}-x_{j})/T} \to 0$ as $T \to 0^+$. So we have:

$$
\lim_{T\to 0^+} \frac{1}{1 + \sum_{1 \leq i \neq j \leq n} e^{(x_{i}-x_{j})/T}} = 1
$$

This means that the softmax of the unique maximum $x_j$ will be 1. This means the softmax of $x_i$ for $i \neq j$ must be 0. Sampling from this distribution will always give class $j$. As $x_j$ was the unique maximum, the softmax will provide a probability distribution that always selects $j$. So functionally, the softmax with temperature 0, is equivalent to the argmax function.

From the steps taken above, its clear that if $x_j$ was not the unique maximum, the softmax of $x_j$ cannot be 1.
