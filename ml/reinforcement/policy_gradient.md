---
layout: default
title: Policy Gradient
parent: Reinforcement Learning
grand_parent: Machine Learning
nav_order: 3
has_children: true
permalink: ml/reinforcement-learning/policy-gradient
---

# Policy Gradient

Previous methods of reinforcement learning have focused on learning the value function, which is the expected reward of taking an action $a$ in state $s$ and following the optimal policy thereafter. However, if we parameterize the policy with $\theta$ parameters, we can also learn the policy directly. This is called **policy gradient** methods, where $\pi(a_t \vert s_t; \theta)$ is the probability of taking action $a_t$ in state $s_t$ given the policy parameters $\theta$, and the parameters are updated via gradient methods.

We first define an expected return function:

$$
J(\theta) = \int_{s\in S} V_{\pi_\theta}(s)d\mathbb{P}_{\pi_\theta}(s) = \int_{s\in S} \int_{a\in A} Q_\pi(s,a) d\pi(a\vert s) d\mathbb{P}_{\pi_\theta}(s)
$$

That is, the expected return is the expected value of the value function $V$ under the policy $\pi_\theta$. We can also write this as:

$$
J_\theta = \mathbb{E}_{s\sim S} (V_{\pi_\theta}(s)) = \mathbb{E}_{s\sim S} \left( \mathbb{E}_{a\sim \pi_\theta} (Q_\pi(s,a)) \right)
$$

Here $s\sim S$ and $\mathbb{P}(s)$ is the stationary distribution of states in the Markov chain transitions under policy $\pi_\theta$.

Policy gradient aims to find the use gradient ascent to maximize $J$ with respect to $\theta$. We need to perform a gradient update by getting the derivative. However taking the derivative of $J$ is slow and prone to error since we can only approximate it. Instead, we can use the **score function** to estimate the gradient of $J$. To do so, we must invoke the **policy gradient theorem**.

## Policy Gradient Theorem

Provided we have an expected rewards 

$$J_\theta = \int_{s\in S} \int_{a\in A} Q_\pi(s,a) d\pi(a\vert s) d\mathbb{P}_{\pi_\theta}(s)$$

we wish to differentiate this with respect to $\theta$. 

$$
\begin{align*}
\nabla _\theta J_\theta &= \nabla _\theta \int_{s\in S} \int_{a\in A} Q_\pi(s,a) d\pi(a\vert s) d\mathbb{P}_{\pi}(s) \\
&= \int_{s\in S} \int_{a\in A} Q_\pi(s,a) \nabla _\theta d\pi(a\vert s; \theta) d\mathbb{P}_{\pi}(s) \\
&= \int_{s\in S} \int_{a\in A} Q_\pi(s,a) \frac{\nabla _\theta \pi(a\vert s; \theta)}{\pi(a\vert s; \theta)} d\pi(a\vert s; \theta) d\mathbb{P}_{\pi}(s) \\
&= \int_{s\in S} \int_{a\in A} Q_\pi(s,a) \nabla _\theta \log (\pi(a\vert s; \theta)) d\pi(a\vert s; \theta) d\mathbb{P}_{\pi}(s) \\
&= \int_{a \in A} Q_\pi(s,a) \nabla _\theta \log (\pi(a\vert s; \theta)) d\pi(a\vert s; \theta) \\
&= \mathbb{E}_{\pi_\theta} (\nabla _\theta \log (\pi(a\vert s; \theta))Q_\pi(s,a) )
\end{align*}
$$

So the gradient of $J(\theta)$ is $\mathbb{E}_{\pi_\theta} (\nabla_\theta \log (\pi(a\vert s; \theta))Q_\pi(s,a))$. The full proof for the policy gradient theorem can be found in the Sutton and Barto 13.1. We skip several steps. One might wonder why $\pi$ and $\pi; \theta$ are applied differently, and this is due to the product rule which is used in the full proof. The key trick is the reparameterization trick, where $\frac{\nabla f}{f} = \nabla \log f$.



