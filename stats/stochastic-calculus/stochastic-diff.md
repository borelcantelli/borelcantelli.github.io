---
layout: default
title: Stochastic Calculus Basics
parent: Stochastic Calculus
grand_parent: Probability
nav_order: 1
has_toc: true
has_children: true
permalink: stats/stochastic-calculus/diffeq
---

# Basics of Stochastic Calculus
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


$\renewcommand{\reals}{\mathbb{R}}$ $\newcommand{\nats}{\mathbb{N}}$ $\newcommand{\ind}{\mathbb{1}}$  $\newcommand{\pr}{\mathbb{P}}$ $\newcommand{\cv}[1]{\mathcal{#1}}$ $\newcommand{\nul}{\varnothing}$ $\newcommand{\eps}{\varepsilon}$ $\newcommand{\E}{\mathbb{E}}$ $\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}$

# Motivations

In mathematics, modeling bacteria population growth can be done with differential equations. That is, suppose the rate of change in population $N(t)$ over time increases as a function of the population at time $t$. Then we arrive at the following differential equation:
$$\frac{\partial N(t)}{dt} = a(t) N(t)$$

Usually, $a(t)$ is assumed to be deterministic over $t$. However, this is not always the case. In fact, $a(t)$ can be stochastic. Then we may say that $a(t) = c + W_t$ where $W_t$ is some random variable, for example from Brownian motion. Then how does one solve the following equation?

In another case, suppose we want to integrate over some process $W_t$. For example, if $W_t$ is restricted to the positive numbers and is a process that tracts the stock price, and $X_t$ tracks the difference in number of shares held then $\int_0^T X_t dW_t$ gives the expected return of the trades after time $T$.

Traditional integration theory cannot help us, as $W_t$ need not be differentiable with respect to $t$. That is, we cannot decompose:
$$
\int_0^T X_t dW_t = \int_0^T X_t W_t' dt
$$

as in the case of Brownian motion, as $W_t$ is differentiable almost nowhere.

In both cases, the differential and the integral are not defined. To give some meaning to the above we consider the standard form of SDEs written as a function an initial condition, a drift term and a variance term:
$$d X_t = f(X_t, t)dt + \sigma(X_t, t)dW_t;\;\; X_0 = x$$

However since $dW_t$ does not exist, we should have a different method of defining the above, which is:
$$X_T = X_0 + \int_0^T f(X_s, s) ds + \int_0^T \sigma(X_s, s)dW_s$$

A new problem arises from this, namely how to define such an integral with respect to $W_s$, a sample path of Brownian motion.

