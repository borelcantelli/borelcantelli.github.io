---
layout: default
title: Learning with the Q Function
parent: Reinforcement Learning
grand_parent: Machine Learning
nav_order: 2
has_children: true
permalink: ml/reinforcement-learning/q-learning
---

# Ways to Learn with the Q function

We do not learn the model of the environment here, i.e. we do not learn the transition probabilities. Instead, we pick an optimal action based on the current state. We can do this by learning the $Q$ function, which is the expected reward of taking an action $a$ in state $s$ and following the optimal policy thereafter.

## Monte Carlo Method

The $Q$ function is used to determine the best action to for a policy given a state. It is an expectation, and we can learn it with Monte Carlo methods. Such a method relies on repeated sampling $(s_t,a_t,r_t, s_{t+1})$ tuples to obtain numerical results. As the number of tuples collected increases, the estimates for $Q$ converge to the true value based on the law of large numbers. 

We define the $\hat{V}(s)$ function as the empirical mean of the discounted rewards observed at state $s$, and likewise for $\hat{Q}$, we define as the empirical mean of the discounted rewards observed at state $s$ having taken action $a$.

We take a collection of $(s_t,a_t,r_t, s_{t+1})$ called the **experience replay** tuples and use them to estimate the value function. We can then use the value function to update the policy.

1. Initialize a policy $\pi$, and note the current state $s$ that exists in the experience replay.
2. Evaluate $\hat{Q}$ for the current state $s$ by looking at the rewards in the experience replay.
3. Pick the action $a$ that maximizes $\hat{Q}(s,a)$ as our new policy $\pi$.
4. Generate a new experience replay of tuples $(s_t,a_t,r_t, s_{t+1})$ running $\pi$ on the environment
5. Repeat steps 2-4 until convergence.

Due to the randomness in Monte Carlo sampling, this policy may be very noisy to learn. However, this is the most naive way to learn an optimal policy. Notice that we do not actually need to evaluate $V$ anymore, since the Monte Carlo estimate of $Q$ only relies on rewards read off of the experience replay. But because we are reading off the experience replay, the replay needs to be complete, i.e. we need to have run the policy $\pi$ on the environment until the end of the episode. This way we can evaluate $G_t$ for each state-action pair.

## Temporal Difference Learning

Temporal difference learning is a method that combines the best of both worlds of dynamic programming and Monte Carlo methods. It is a very central idea to reinforcement learning. The core idea is **bootstrapping**. Instead of using $G_t$ by reading off the entire episode, we can learn $V$ and $Q$ by using existing estimates of $V$ and $Q$, and updating with rewards as the agent interacts with the environment. How do we get away without calculating $G_t$, which requires us to complete episodes? The main idea is to limit how much our estimate of $V$ and $Q$ is updated by the reward. We can do this by using a **learning rate** $\alpha$.

$$
\begin{align*}
V(S_t) &\leftarrow (1-\alpha)V(S_t) + \alpha G_t  \\
&\leftarrow V(S_t) - \alpha V(S_t) + \alpha G_t \\
&\leftarrow V(S_t) + \alpha(G_t - V(S_t)) \\
&\leftarrow V(S_t) + \alpha (R_{t+1} + \gamma V(S_{t+1}) - V(S_t))
\end{align*}
$$

**Exercise**: Derive the TD update for $Q(s,a)$
<details>
Q(s_t, a_t) + \alpha (R_{t+1} + \gamma Q(s_{t+1}, a_t) - Q(s_t, a_t))
</details>

The TD update is a very powerful idea. It allows us to learn the value function without having to complete episodes. We are able to do this because $V_t$ can be defined as a function of $G_t$. This is the essence of bootstrapping. We can use the current estimate of $V$ to update the estimate of $V$.

### TD Learning Methods (SARSA)

The most elementary TD learning method is called SARSA. SARSA is an on-policy method, meaning that the policy that is being evaluated is the same policy that is being updated. The update rule for SARSA is as follows:

Given an experience replay of $(s_t, a_t, r_t, s_{t+1})$ tuples:

1. Initialize $Q(s,a)$ for all $s,a$.
2. Pick an action $a_t$ from the current state $s$ using the arg-max of $Q$, or whatever policy you are using.
3. Take action $a_t$ and observe the reward $r$ and the next state $s'$.
4. Pick the next action $a'$ from state $s'$ using the same way as in step 2.
5. Update $Q(s,a)$ using the TD update rule: $Q(s_t, a_t) \leftarrow Q(s_t, a_t) + \alpha(r_t + \gamma Q(s', a') - Q(s, a_t))$.
6. Repeate steps 2-5 until convergence.

In SARSA, we take two steps. First we take an action $a_t$ from the current state $s$ using the policy $\pi$ or the arg-max policy. Then we observe the reward $r$ and the next state $s'$. We then pick the next action $a'$ from state $s'$ using the policy $\pi$. Now that we have $r$, $(s, s')$ and $(a, a')$, we have enough information to update $Q(s,a)$ using the TD update rule. Note that we chose the next action $a'$ the same way we picked an action in (2). This is why SARSA is an **on-policy** method.

### Q Learning

Q learning is an off-policy method. This means that the policy that is being evaluated is not the same policy that is being updated. The update rule for Q learning is as follows:

1. Initialize $Q(s,a)$ for all $s,a$, and note the current state $s_t$.
2. Pick an action $a_t$ from the current state by the arg-max of $Q$, or whatever policy you are using.
3. Take action $a$ and observe the reward $r$ and the next state $s'$.
4. We update $Q$ using $Q(s_t, a_t) \leftarrow Q(s_t, a_t) + \alpha(r + \gamma \max_{a\in A} Q(s', a) - Q(s_t, a_t))$.
5. Repeat steps 2-4 until convergence.

Note that we do not pick the second action $a'$, but rather we take the maximum of $Q(s', a')$ over all actions $a'$, unlike in SARSA.  It esimates the optimal $Q$ only by using the maximum of the current estimate of $Q$, regardless of the policy used in (2). This is why Q learning is an **off-policy** method.

