---
layout: default
title: Reinforcement Learning
parent: Machine Learning
nav_order: 2
has_children: true
permalink: ml/reinforcement-learning
---

# Reinforcement Learning

There are three primary components in reinforcement learning: **agent**, **environment**, and **rewards**. The agent interacts with the environment by taking actions, and receives rewards based on the actions taken. The agent learns to make decisions that maximize the rewards over time.

The agent's job is to take actions. Actions can be discrete, such as *move up*, *down*, *left*, or *right*. It can also be continuous, for example, the agent can control the speed of a car or angle of a robotic arm. Actions can also be stochastic, where the agent can take an action that is sampled from some distribution. We can reaonably expect the type of actions that the agent can take to depend on the state the agent is in, so this distribution is conditioned on the current state. We refer to this distribution as the **policy** denoted as $\pi(a_t \vert s_t)$. If $\pi$ is deterministic, then the agent is called a **deterministic policy**. If $\pi$ is stochastic, then the agent is called a **stochastic policy**. Note: A deterministic policy is a stochastic policy that is degenerate.

When the agent takes an action, it preturbs the environment, bringing it to a new state $s_{t+1}$. The environment will also provide a reward $r_{t}$ to the agent. (Note that there is a large area of research that studies how agents learn when $r$ is absent or uninformative). 

Now that the agent has information on the new state $s_{t+1}$ at which it has arrived, as well as the quality of the action taken to arrive $s_{t+1}$ via the reward, it can update its decision making process. If the agent received a positive reward for taking action $a_t$ in state $s_t$, then it should hopefully be more likely to take that action when in that state, again in the future. If the agent received a negative reward, then it will be less likely to take that action again in the future. This is the essence of reinforcement learning.

In all, the agent starts with a randomly initialized policy. It updates the policy through iterations of taking actions, observing rewards, and updating the policy. The agent can, but is not required to, learn how the environment works. This could potentially help the agent make better decisions in selecting actions. That is, the agent can learn the transition probabilities of the environment, and the reward structure. If the agent has learned the possible states it can transition to after taking an action $a$ at state $s$, it can use this information to make better decisions.

## Environment Model

An environment is an MDP. That is, the next state and reward only depends on the previous state and action taken. It does not rely on the entire history of states and actions. We can model the environment as a Markov chain, with states as nodes and edges as actions. We model the environment with transition probabilities:

$$
\mathbb{P}(S'=s', R'=r \vert S=s, A=a)
$$

This is the probability of observing that the next state is $s'$ with reward $r$, provided we are currently in state $s$ and took action $a$.

We can marginalize out $R$ and get the state transitions: $\mathbb{P}(S'=s'\vert S=s, A=a)$ which we represent with $\mathbb{P}_{s'|s,a}$. 

As an agent can learn the transition probabilities, it can use this information to make better decisions. For example, if the agent knows that taking action $a$ in state $s$ will lead to state $s'$ with high probability, and that the reward for this transition is high, through experience, then the agent can take action $a$ in state $s$ with high confidence. Learning this probability puts an extra burden on the training process, so some models don't learn the transition probabilities, and instead focus on learning the policy. This gives rise to the notions of **model-free** and **model-based** reinforcement learning.

- Model-Free: The agent does not learn the transition probabilities. It only learns the policy without learning how states and actions transition to other states in the environment.
- Model-Based: The agent learns the transition probabilities. It learns the policy as well as the transition probabilities and incorporates this information to make better decisions.

Most research these days are focused on model-free learning. 

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

## Derivations of V and Q as Recursive Functions (Bellman Equations)

The Bellman equations are a set of equations that describe the optimal policy. They are recursive in nature, and are used to find the optimal policy. We can derive these equations from the definitions of $V$ and $Q$.

Let's dissect the value and quality function.

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
Hint: Follow the same path as with the $V$ example above. Argue that $Q(s,a) = \mathbb{E}(R_{t+1} + \gamma V(s')\vert S_t=s, A_t=a). Argue that $V(s')$ is just a marginalized $Q(s',a')$ over the action space $a'$.

## Derivations of V and Q as functions of States, Actions, and Rewards

The equations above are still not very useful for the agent. We need to define $V$ and $Q$ in terms of the state, action, and reward spaces so the agent can make updates based on states and actions it has observed and taken.

$$V_{\pi}(s) = \mathbb{E}_{a\sim \pi}(Q_\pi(s,a)) = \int_{a\in A} Q(s,a) d\pi(a)$$

For notation, we denote $R(s,a)$ as $\mathbb{E}(R_{t+1}|S_t=s, A_t=a)$. This is the expected reward for taking action $a$ in state $s$.

$$\\
\begin{align*}
Q(s,a) &= \mathbb{E}(R_{t+1} + \gamma V(s')\vert S_t=s, A_t=a) \\
&= R(s,a) + \gamma \mathbb{E}(V(s') \vert S_t=s, A_t=a)\\ 
&= R(s,a) + \gamma \mathbb{E}_{s'|s,a}(V(s'))\\ 
&= R(s,a) + \gamma \int_{s'\in S} V(s') d\mathbb{P}_{s'|s,a} \\ 

We know that $V(s)$ can be represented as $\int_{a\in A} \pi(a|s)Q(s,a) da$, which is the marginalization of $Q(s,a)$ over the distribution of actions:

$$V(s) = \int_{a\in A} Q(s,a) d\pi(a|s)$$

Substituting this into the $Q$ equation:

$$Q(s,a) = R(s,a) + \gamma \int_{s'\in S} \int_{a'\in A} Q(s',a') d\pi(a'|s') d\mathbb{P}_{s'|s,a}$$

We have written this as an integral, but it can be written as a summation as well. This is the Bellman equation for $Q$.

$$Q(s,a) = R(s,a) + \gamma \sum_{s'\in S}\mathbb{P}_{s'|s,a} \int_{a'\in A} Q(s',a') \pi(a'|s')$$

We write this in form of an integral to make clear what is being summed over, and what is being integrated over. There are some marginalizations going on here, and it is not clear what is being marginalized over what. Thus we use integral notation to be clear.





