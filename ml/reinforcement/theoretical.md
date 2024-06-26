---
layout: default
title: Theoretical Foundations
parent: Reinforcement Learning
grand_parent: Machine Learning
nav_order: 1
has_children: true
permalink: ml/reinforcement-learning/theoretical-foundations
---

# Theoretical Foundations

We present some defintions and derivations that are foundational to understanding reinforcement learning. We start with the definitions of the value and quality functions, and then derive the Bellman equations. We then discuss the optimal policy and how to solve the Bellman equations for the optimal policy. All of this is theoretical, and none of this is actually implemented in practice since the information required to solve for optimal policy usually is not available. Nonetheless, these theoretical foundations are important to understand the algorithms that are used in practice.

Much of theoretical reinforcement learning draws upon stochastic optimization, however we present in a statistically oriented manner.

## Environment Model

An environment is an MDP. That is, the next state and reward only depends on the previous state and action taken. It does not rely on the entire history of states and actions. We can model the environment as a Markov chain, with states as nodes and edges as actions. We model the environment with transition probabilities:

$$
\mathbb{P}(S'=s', R'=r \vert S=s, A=a)
$$

This is the probability of observing that the next state is $s'$ with reward $r$, provided we are currently in state $s$ and took action $a$.

We can marginalize out $R$ and get the state transitions: $\mathbb{P}(S'=s'\vert S=s, A=a)$ which we represent with $\mathbb{P}_{s'\vert s,a}$. 

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

The last line above is equivalent by law of iterated expectation. For a heursitic proof, consider the space $(G, S_{t+1}, S_t)$. We can partition $S_t$ and evaluate integrate over $G_{t+1}$ for each $s$ partition, regardless of how $S_{t+1}$ will be partitioned. However, we can view this as partitioning $S_t$, and collecting the $S_{t+1}$ that lie in each $S_t$. Then we further partition $G_{t+1}$ for each $S_{t+1}$ partition, and integrate over the $S_t \times S_{t+1}$ pieces. These two give the same result. 

Continuing from above:

$$V(s) = \mathbb{E}(R_{t+1} \vert  S_t=s) + \gamma \mathbb{E}(V(s') \vert  S_t=s) = \mathbb{E}(R_{t+1} + \gamma V(s')\vert S_t=s)$$

Note how we created a recursive definition of $V$. This expectation is over the reward space, and the value functions - functions of future states. 

**Exercise**: Show that $Q(s, a) = \mathbb{E}(R_{t+1} + \gamma Q(s',a')\vert S_t=s, A_t=a)$.
Hint: Follow the same path as with the $V$ example above. Argue that $Q(s,a) = \mathbb{E}(R_{t+1} + \gamma V(s')\vert S_t=s, A_t=a)$. Argue that $V(s')$ is just a marginalized $Q(s',a')$ over the action space $a'$.

## Derivations of V and Q as functions of States, Actions, and Rewards

The equations above are still not very useful for the agent. We need to define $V$ and $Q$ in terms of the state, action, and reward spaces so the agent can make updates based on states and actions it has observed and taken.

$$V_{\pi}(s) = \mathbb{E}_{a\sim \pi}(Q_\pi(s,a)) = \int_{a\in A} Q(s,a) d\pi(a)$$

For notation, we denote $R(s,a)$ as $\mathbb{E}(R_{t+1}\vert S_t=s, A_t=a)$. This is the expected reward for taking action $a$ in state $s$.

$$\\
\begin{align*}
Q(s,a) &= \mathbb{E}(R_{t+1} + \gamma V(s')\vert S_t=s, A_t=a) \\
&= R(s,a) + \gamma \mathbb{E}(V(s') \vert S_t=s, A_t=a)\\ 
&= R(s,a) + \gamma \int_{s'\in S} V(s') d\mathbb{P}_{s'\vert s,a} 
\end{align*}
$$ 

We know that $V(s)$ can be represented as $\int_{a\in A} \pi(a\vert s)Q(s,a) da$, which is the marginalization of $Q(s,a)$ over the distribution of actions:

$$V(s) = \int_{a\in A} Q(s,a) d\pi(a\vert s)$$

Substituting this into the $Q$ equation:

$$Q(s,a) = R(s,a) + \gamma \int_{s'\in S} \int_{a'\in A} Q(s',a') d\pi(a'\vert s') d\mathbb{P}_{s'\vert s,a}$$

We have written this as an integral, but it can be written as a summation as well. This is the Bellman equation for $Q$.

$$Q(s,a) = R(s,a) + \gamma \sum_{s'\in S}\mathbb{P}_{s'\vert s,a} \sum_{a'\in A} Q(s',a') \pi(a'\vert s')$$

We write this in form of an integral to make clear what is being summed over, and what is being integrated over. There are some marginalizations going on here, and it is not clear what is being marginalized over what. Thus we use integral notation to be clear.


This tells us that our $Q$ value for a given state is a function of our imminent reward and the discounted expected value of $Q$ at next state (which is a joint expectation between next state and current action).

**Exercise**: Derive the Bellman equation for $V$ a similar way as we did for $Q$ above. That is, show that $V(s) = \int_{a\in A} (R(s,a) + \gamma \int_{s'\in S} V(s') d\mathbb{P}_{s'\vert s,a}) \pi(a\vert s) $. *Hint*: We integrate out the action space $a$ to get state marginal $V(s)$, thus $R(s,a)$ stays inside the integral (expectation).

## Optimal Policy

Instead of learning the value function, we can learn the optimal policy directly. The optimal policy is the policy that maximizes the expected return. We can define the optimal value function as the the value function that achieves that maximum possible value or $Q$.

$$V^*(s) = \max_{\pi} V_{\pi}(s)$$

$$Q^*(s,a) = \max_{\pi} Q_{\pi}(s,a)$$

The optimal policy is the policy that achieves the optimal value function. We can define the optimal policy as:

$$\pi^*(a\vert s) = \arg\max_{\pi} V_\pi(s)$$

$$\pi^*(a\vert s) = \arg\max_{\pi} Q_\pi(s,a)$$

Simply put, the optimal policy is the one that optimizes the value function or $Q$ function.

<details>
<summary>Let $Q$ and $V$ be only positively valued. Let $\pi$ be the policy that optimizes $V$ to give $V^*$. Prove that there exists a policy that optimizes $Q$ that is produces a policy with a higher $V$ than $V^*$. *Note*: We optimized $V$ but $V$ is an average over all possible actions per state. $Q$ is a function of both state and action spaces, and thus can capture more information about the environment than $V$.
</summary>

Provided that the $Q$ and $V$ functions are non-negative (integrability is assumed), the policy that maximizes the $Q$ function produces a policy that will be more optimal than the policy that maximizes the $V$ function. This is because the $Q$ function is a function of both the state and action spaces, and thus can capture more information about the environment than the $V$ function. We can see this from the following inequality:

$$V^*(s) = \sup_{\pi} \int_{a\in A} Q(s,a)d\pi(a|s) \leq \int_{a\in A} \sup_\pi Q(s,a) d\pi(a|s) = \int_{a\in A} Q^*(s,a) d\pi(s|a)$$

Note this does not necessarily hold true in general cases, since $V$ or $Q$ could be negative depending on how rewards are defined.
</details>


### Optimal Values and Bellman Equations

To find the optimal $V$ and $Q$, we simply replace the expectation with respect to action, with the best action we can take when rewriting the Bellman equations. Intuitively, we no longer consider the average action, but rather the best action.

$$V^*(s) = \sup_{a\in A} (R(s,a) + \gamma \int_{s'\in S} V(s') d\mathbb{P}_{s'\vert s,a}) $$

Then optimal $Q^*$ is:  

$$Q^*(s,a) = R(s,a) + \gamma \int_{s'\in S} V^*(s') d\mathbb{P}_{s'\vert s,a}$$

We also can write optimal $V^*$ as 

$$V^*(s) = \sup_{a\in A} Q^*(s,a)$$

which makes an alternative formulation of $Q^*$ as

$$Q^*(s,a) = R(s,a) + \gamma \int_{s'\in S} \sup_{a'\in A} Q^*(s',a') d\mathbb{P}_{s'\vert s,a}$$

where the $V^\*$ is replaced with the $\sup_{a'\in A} Q^\*(s',a')$.

### Solving the Optimal Bellman Equations

Suppose we have full information on the model and the rewards. We can solve the Bellman equations for the optimal $V$ and $Q$ functions. We can then use these functions to find the optimal policy. This can be done in a dynamic programming approach, where we iteratively update the $V$ or $Q$ functions until they converge to the optimal values. 

#### Evaluating the Policy

For any given policy $\pi$, we take $V_{t+1}(s)$, the value of state $s$ at time $t+1$ as:

$$\mathbf{E}_\pi(r + \gamma V_t(s')\vert S_t=s) = \int_{a\in A} \int_{s' \times r} (r + \gamma V_t(s')) d\mathbb{P}(s', r \vert s, a)d\pi(a|s)$$

Note here that $s'$ is the state at $t+1$.

#### Improving the Policy

This evaluates the policy by taking the value of state $s$, after an action has been taken. To improve the policy, we need to understand how to evaluate the actions taken by the agent. This information can be given by the $Q$ function which is a function of both state and action. The $Q$ value is given by:

$$Q_\pi(s,a) = \mathbb{E}(R_{t+1} + \gamma V_\pi(S')\vert S_t = s, A_t=a) = \int_{s\times r \in (S, R)}r + \gamma V_\pi(s') d\mathbb{P}(s', r\vert s, a)$$

Having the $Q$ value defined, we define $\pi'(s)$, the update of the policy, as 

$$\pi'(s) = \arg\max_{a\in A} Q_\pi(s, a)$$

We are guaranteed an improvement since

$$
\begin{align*}
Q_\pi(s, \pi'(s)) &= \sup_{a\in A} Q_\pi(s, a)\\
\end{align*}
$$

As seen before, $\sup_{a\in A}Q_\pi(s,a) \geq V_\pi(s)$ leveraging the maximal properties of averaging. So we have developed a policy that is better than the previous policy when conditioned on state $s$. 

#### Policy Iteration

Policy iteration is the iterative process of evaluating the policy, and then improving the policy. This process is repeated until the policy converges to the optimal policy. The policy iteration algorithm is as follows:

1. Initialize the policy $\pi$ randomly, note the current state.
2. Evaluate the policy $\pi$ by calculating the $V$ function.
3. Take an action based on $\pi$, and improve the policy by calculating the $Q$ function and updating the policy with the argmax. (This is called a greedy policy update)
4. Repeat steps 2 and 3 until the policy converges to the optimal policy.

In step 3, we take an action based on the policy, and then update the policy based on the $Q$ function. This is called an **on-policy** algorithm, since we learn the policy while interacting with the environment using the said policy. An **off-policy** algorithm is one where we learn the policy while interacting with the environment using a different policy, for example, we can evaluate the policy by randomly sampling an action to take in our updates to $Q$.

Unfortunately, the model for the environment is not typically known, so we cannot directly calculate the optimal $V$ or $Q$. We can only estimate these values. This is where we change the paradigm from dynamic programming, to a stochastic approach where the agent learns the optimal policy through experience. We explore some ways tp still extract a meaningful optimal policy despite the lack of a model in the next sections.

