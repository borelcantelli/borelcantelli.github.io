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