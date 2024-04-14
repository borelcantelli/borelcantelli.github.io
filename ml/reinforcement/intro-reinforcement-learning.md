---
layout: default
title: Reinforcement Learning
parent: Machine Learning
nav_order: 2
has_children: true
permalink: ml/reinforcement-learning
---

# Reinforcement Learning

Lorem ipsum

$$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... = \sum_{i=1}^\infty \gamma^k R_{t+k+1}$$

$$V_{\pi}(s) = E(G_t | S_t=s)$$

$$Q_{\pi}(s,a) = E(G_t | S_t = s, a_t=a)$$

Given a policy $\pi$, which is a probability distribution over the action space $a$:

$$V_{\pi}(s,a) = E(G_t|S_t=s)$$

$$= E_{a\sim \pi}(E(G_t|S_t=s,a_t=a))$$

$$= E_{a\sim \pi}(Q_{\pi})$$

$$= \sum_{a\in A} Q_{\pi} \pi(a)$$

Generalizing to:

$$\int_{a\in A} Q_{\pi}(s,a) d\pi(a)$$