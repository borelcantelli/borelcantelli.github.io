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

From the steps taken above, its clear that if $x_j$ was not the unique maximum, the softmax of $x_j$ cannot be 1. If $x_j$ and $x_k$ where both maximums, then intuitively we would expect that class $j$ and $k$ share the mass of 1, i.e. the distribution induces by the softmax would pick $j$ and $k$ each 1/2. Generalizing, we expect if there were $m$ maximums, then the mass on each would be $1/m$. To see this, we modify the following proof in the unique maximum case:

Let $M$ be the set of maximums of $(x_1,...,x_n)$, and without loss, let $x_j$ be in $M$, i.e. $x_j$ is an arbitrary maximum from $(x_1,...,x_n)$. Let $M$ have cardinality $\|M\|$. Every element in $M$ is equal to each other, since they are all maximums. We call it $m$ so that $x_j = m$.

$$
\lim_{T\to 0^+} \frac{e^{x_j/T}}{\sum_{m \in M}e^{m/T}+\sum_{x_{i} \not\in M} e^{x_{i}/T}} = \lim_{T\to 0^+} \frac{e^{m/T}}{\|M\|e^{m/T}+\sum_{x_{i} \not\in M} e^{x_{i}/T}}
$$

Following the same steps as before:

$$
\lim_{T\to 0^+} \frac{e^{m/T}}{\|M\|e^{m/T}+\sum_{x_{i} \not\in M} e^{x_{i}/T}} = \lim_{T\to 0^+} \frac{1}{\frac{\|M\|e^{m/T}+\sum_{x_{i} \not\in M} e^{x_{i}/T}}{e^{m/T}}}
$$

Simplifying, we get:

$$
\lim_{T\to 0^+} \frac{1}{\|M\|+\sum_{x_{i} \not\in M} e^{(x_{i}-m)/T}} = \frac{1}{\|M\|}
$$

The second summation drops because of the same reason as before, $x_{i} < m$ so $e^{(x_{i} - m)/T} \to 0$ as $T\to 0^+$. This proves more rigorously, that if we have maximums $x_j = x_k$ in our list of numbers $(x_1,...,x_n)$, then the softmax will product a distribution that samples the $x_j$ and $x_k$ with equal weight, and will only sample classes $j$ and $k$ when $T$ approaches 0. Of course, our result is more general, and states that if have any set of maximums $M$, the classes associated with the maximums will be sampled with equal weight $1/\|M\|$. 