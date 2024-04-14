---
layout: default
title: Reinforcement Learning
parent: Machine Learning
nav_order: 2
has_children: true
permalink: ml/reinforcement-learning
---

# Reinforcement Learning

There are three primary components in reinforcement learning: agent, environment, and rewards. The agent interacts with the environment by taking actions, and receives rewards based on the actions taken. The agent learns to make decisions by maximizing the rewards over time.

In reinforcement learning, the agent learns to make decisions by performing certain actions and receiving rewards. The model learns to perform the best action that maximizes the reward over time. That is, the model learns by interacting with the environment, and receives rewards. These rewards are used to iterate the model as it continues to interact and collect experience.


## Model

An environment is an MDP. That is, the next state and reward only depends on the previous state and action taken. It does not rely on the entire history of states and actions. We can model the environment as a Markov chain, with states as nodes and edges as actions. We model the environment with transition probabilities:

$$
\mathbb{P}(S'=s', R'=r \vert S=s, A=a)
$$

This is the probability of observing that the next state is $s'$ with reward $r$, provided we are currently in state $s$ and took action $a$.

We can marginalize out $R$ and get the state transitions: $\mathbb{P}(S'=s'\vert S=s, A=a)$. 

This gives the notion of model-free and model-based RL.


## Definitions

$$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... = \sum_{i=0}^\infty \gamma^k R_{t+k+1}$$

$$V_{\pi}(s) = \mathbb{E}(G_t \vert  S_t=s)$$

$$Q_{\pi}(s,a) = \mathbb{E}(G_t \vert  S_t = s, a_t=a)$$

Given a policy $\pi$, which is a probability distribution over the action space $a$:

$$
\begin{align*}
V_{\pi}(s) &= \mathbb{E}(G_t \vert S_t=s)\\
&= \mathbb{E}_{a\sim \pi}(\mathbb{E}(G_t \vert S_t=s,a_t=a))\\
&= \mathbb{E}_{a\sim \pi}(Q_{\pi})(s,a)\\
&= \sum_{a\in A} Q_{\pi}(s,a) \pi(a) \\
\end{align*}
$$

Generalizing to:

$$\int_{a\in A} Q_{\pi}(s,a) d\pi(a)$$

Primarily, we view $V$ as the marginalize $Q$, with respect to the distribution over the action space.

But $Q$ is also an expectation, so:

$$V_{\pi}(s) = \int_{a\in A} \mathbb{E}(G_t\vert s_t=s, a_t=a) d\pi(a)$$

We understand this to mean 

1. $\mathbb{E}(G_t \vert s_t=s, a_t=a)$ the future discounted rewards given we are at state $s$ and take action $a$.
2. Marginalize by action $a$ to make this only depend on the state $s$, which gives the value $V$. 
3. We arrive at our $(s,a)$ pairs via policy $a_t \sim \pi(a_t\vert s_t)$, for some given initialized $s_0$.

## Derivations of V and Q as Recursive Functions

## Derivations of V and Q as Recursive Functions

Let's dissect the value and quality function more:

$$\\

\begin{align*}
V(s) &= \mathbb{E}(G_t \vert  S_t=s)\\

&= \mathbb{E}(R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... \vert  S_t=s)\\

&= \mathbb{E}(R_{t+1} + \gamma(R_{t+2} + \gamma R_{t+3} + ...) \vert  S_t=s)
\end{align*}
$$

Noting this recursive definition, we replace everything in $\gamma(...)$ with $G_{t+1}$.

$$\\

\begin{align*}
V(s) &= \mathbb{E}(R_{t+1} + \gamma G_{t+1}\vert S_t=s) \\
&= \mathbb{E}(R_{t+1} \vert  S_t=s) + \gamma \mathbb{E}(G_{t+1} \vert  S_t=s)\\
&= \mathbb{E}(R_{t+1} \vert  S_t=s) + \gamma \mathbb{E}(\mathbb{E}(G_{t+1}|S_{t+1} = s') \vert  S_t=s)
\end{align*}
$$

We have equivalency by law of iterated expectation. For a heursitic proof, consider the space $(G, S_{t+1}, S_t)$. We can partition $S_t$ and evaluate integrate over $G_{t+1}$ for each $s$ partition, regardless of how $S_{t+1}$ will be partitioned. However, we can view this as partitioning $S_t$, and collecting the $S_{t+1}$ that lie in each $S_t$. Then we further partition $G_{t+1}$ for each $S_{t+1}$ partition, and integrate over the $S_t \times S_{t+1}$ pieces. These two give the same result. 

Continuing from above:

$$V(s) = \mathbb{E}(R_{t+1} \vert  S_t=s) + \gamma \mathbb{E}(V(s') \vert  S_t=s) = \mathbb{E}(R_{t+1} + \gamma V(s')\vert S_t=s)$$

Note how we created a recursive definition of $V$. This expectation is over the reward space, and the value functions - functions of future states. 

**Exercise**: Show that $Q(s, a) = \mathbb{E}(R_{t+1} + \gamma Q(s',a')\vert S_t=s, A_t=a)$.
Hint: Follow the same path as with the $V$ example above. Start with the definition and factor out the $\gamma$ to get an expectation of a reward + a discounted $G$. Invoke law of iterated expectation, and substitute with $Q(s',a')$.





