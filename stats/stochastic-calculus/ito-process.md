---
layout: default
title: Ito Process
parent: Stochastic Calculus
nav_order: 2
has_toc: true
---

# Ito Process
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

$\renewcommand{\reals}{\mathbb{R}}$ $\newcommand{\nats}{\mathbb{N}}$ $\newcommand{\ind}{\mathbb{1}}$  $\newcommand{\pr}{\mathbb{P}}$ $\newcommand{\cv}[1]{\mathcal{#1}}$ $\newcommand{\nul}{\varnothing}$ $\newcommand{\eps}{\varepsilon}$ $\newcommand{\E}{\mathbb{E}}$ $\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}$

# Ito Process

The Ito-Doeblin theorem from the previous section was specifically for intergrating with respect to a Brownian motion $W(t)$. Now we extend this to a more general case, **Ito processes**.

An Ito process is defined as the following:

$$
X(T) = X(0) + \int_0^T \Delta(u)d W_u + \int_0^T \Theta(u) du
$$

and in differential form:

$$\partial X(t) = \Delta(t) \partial W(t) + \Theta(t)\partial t$$

Here, $W_u$ is a Brownian motion, and $\Delta(u)$ and $\Theta(u)$ are adapted processes. Also $\delta(u)^2$ is integrable and $\Theta(u)$ is absolutely integrable.

The Ito process is defined as a sum of an Ito integral and a Lebesgue intergal. We denote the Ito portion as $I(T) = \int_0^T \Delta(u)dW(u)$ and the Lebesgue portion as $L(T) = \int_0^T \Theta(u)du$. The quadratic variation of such an Ito process is $QV_X(T) = \int_0^T \Delta(u)^2 du$ as demonstrated below.

## Quadratic Variation for Ito Process

For a given partition, denote $$\vert \cv{T}_n \vert$$ as the length of the largest partition. For a given $n$, let the partition be $0 < t_1 < ... < T$. Then the quadratic variation without the limit (via expanding squares) is:

$$
\sum_{j=0}^{n-1}(X(t_{j+1}) - X(t_j))^2 = \sum_{j=0}^{n-1}(I(t_{j+1}) - I({t_j}))^2 + \sum_{j=0}^{n-1}(L(t_{j+1}) - L({t_j}))^2 + 2\sum_{j=0}^{n-1}(I(t_{j+1}) - I({t_j}))(L(t_{j+1}) - L({t_j}))
$$

We shall see that several terms go to 0 in the limit as required by the definition of QV, and the only term remaining will be $\int_0^T \Delta(t)^2dt$.

In the limit, the $\sum_{j=0}^{n-1}(I(t_{j+1}) - I({t_j}))^2$ term becomes $QV_{I}(T) = \int_0^T \Delta^2(u)du$.

We bound the second term by:
$$
\begin{align*}
    \sum_{j=0}^{n-1}(L(t_{j+1}) - L({t_j}))^2 &\leq \max_{(t_j, t_{j+1})\in\cv{T}_n}\{R(t_{j+1})-R({t_j})\}\sum_{j=0}^{n-1}\vert R(t_{j+1})-R({t_j})\vert \\
    &= \max_{(t_j, t_{j+1})\in\cv{T}_n}\{R(t_{j+1})-R({t_j})\}\left\lvert \int_0^T \Theta(u)du \right\rvert \\
    &\leq \max_{(t_j, t_{j+1})\in\cv{T}_n}\{R(t_{j+1})-R({t_j})\} \int_0^T \vert \Theta(u) \vert du \\
    &\to 0
\end{align*}
$$

We bound the third term with 

$$
\begin{align*}
2\sum_{j=0}^{n-1}(I(t_{j+1}) - I({t_j}))(L(t_{j+1}) - L({t_j})) &\leq 2\max_{(t_j, t_{j+1})\in\cv{T}_n}\{(I(t_{j+1}) - I({t_j}))\}\sum_{j=0}^{n-1}\vert L(t_{j+1}) - L({t_j})\vert \\
&= 2\max_{(t_j, t_{j+1})\in\cv{T}_n}\{(I(t_{j+1}) - I({t_j}))\}\left\lvert \int_0^T \Theta(u)du \right\rvert \\
&\leq 2\max_{(t_j, t_{j+1})\in\cv{T}_n}\{(I(t_{j+1}) - I({t_j}))\} \int_0^T \vert \Theta(u) \vert du \\
&\to 0
\end{align*}
$$

So all that remains after taking limits, is $QV_I(T) = \int_0^T \Delta^2(u)du$. 

## Definition of Integral of Ito Process

For a given adapted processs $\Gamma(t)$, it can be integrated with respect to the Ito process $X(t)$ as $\int_0^T \Gamma(t)dX(t)$. It is defined as follows:

$$
\int_0^T \Gamma(t)dX(t) = \int_0^T \Gamma(t)\Delta(t)dW(t) + \int_0^T \Gamma(t) \Theta(t) dt
$$

That is, we split up the two parts of the Ito process and integrate with respect to the Brownian motion portion, and then again with respect to the Lebesgue portion.

## Ito-Doeblin Theorem for Ito Processes

The Ito-Doeblin theorem for an Ito process is analogous to that in the Brownian motion only case. It states that (in both integral and differential form):

$$
f(T, X(T)) = f(0, X(0)) + \int_0^T f_t(t,X(t))dt + \int_0^T f_x(t,X(t))dX(t) + \frac{1}{2}\int_0^T f_{xx}(t,X(t))dX(t)dX(t) 
$$

$$
df(t,X(t)) = f_t(t,X(t))dt + f_x(t,X(t))dX(t) + \frac{1}{2}f_{xx}(t,X(t))dX(t)dX(t)
$$

Note the final $dX(t)dX(t)$ is a product of the quadratic variation on $X(t)$ which is $\int_0^T \Delta^2(u)du$ as opposed to the quadratic variation on $W(t)$ which is $T$.

The proof is as follows, with Taylor expansion:

$$
\begin{align*}
f(T,X(T)) - f(0,X(0)) &= \sum_{j=0}^{n-1}f_t(t_j, X(t_j))(t_{j+1}-t_j) + \sum_{j=0}^{n-1}f_x(t_j, X(t_j))(X(t_{j+1})-X(t_j)) + \\
&\qquad \qquad \frac{1}{2}\sum_{j=0}^{n-1}f_{xx}(t_j, X(t_j))(X(t_{j+1})-X(t_j))^2 + \frac{1}{2}\sum_{j=0}^{n-1}f_{tt}(t_j, X(t_j))(t_{j-+1}-t_j)^2 + \\
&\qquad \qquad \sum_{j=0}^{n-1}f_{tx}(t_j, X(t_j))(t_{j+1}-t_j)(X(t_{j+1})-X(t_j)) + R
\end{align*}
$$

The first line are the first order terms, the second line are the quadratic terms, and the third line is the cross term + higher order terms. 

$\frac{1}{2}\sum_{j=0}^{n-1}f_{tt}(t_j, X(t_j))(t_{j+1}-t_j)^2$ converges to 0 in the same manner as discussed before. So does the cross term. The first order terms also converge to their integral counter parts. The quadratic $X(t)$ term results in $\int_0^T f_{xx}(t_j, X_j) d(X,X)t$ and since $QV_X(T) = \int_0^T \Delta^2(u)du$ then $d(QV_X(T)) = \Delta^2(u)du$. Thus the second term becomes $\int_0^T f_{xx}(t,X(t))\Delta^2(u)du$. 

We can further simplify $\int_0^T f_x(t,X(t))dX(t)$ as $\int_0^T f_x(t,X(t))\Delta(t)dW(t) + \int_0^T f_x(t, X(t))\Theta(t)dt$. This is by definition of the integral of Ito processes.

# Usages of Ito-Doeblin

We can use Ito-Doeblin to solve stochastic integrals and also stochastic differential equations. 


## Stochastic Integration 

While we solved $\int_0^T W_t dW_t$ by way of sums, we can also solve it using Ito-Doeblin, and in general, it is easier to solve it this way. Let $g(W_t) = \frac{1}{2}W_t^2$ so that $g' = W_t$ and $g'' =1$. Then we have:
$$
\begin{align*}
dg(W_t) &= g'(W_t) dW_t + \frac{1}{2}g''(W_t)dt \\
&= W_t dW_t + \frac{1}{2}dt \\
\int_0^T dg(W_t) &=  \int_0^T  W_t dW_t + \frac{1}{2}\int_0^T dt \\
\int_0^T W_t dW_t &= \int_0^T dg(W_t)  - \frac{1}{2}\int_0^T dt \\
&= \frac{1}{2}W_T^2 - \frac{1}{2}T
\end{align*}
$$
This result agrees with what we have above.

It is important to note that the resulting integral is a stochastic process evaluate at time $T$ and is not a ``numeric'' solution in the Lebesgue sense, even though this is a definite integral. Say we wish to evaluate $\int_0^T W_t^2 dW_t$. First, let $g(W_t) = \frac{1}{3}W_t^3$ which is what we would expect from ordinary calculus, or the Stratanovich integral. Then $g'(W_t) = W_t^2$ and $g''(W_t) = 2W_t$.

$$
\begin{align*}
d\left(\frac{1}{3}W_t^3\right) &= g'(W_t) dW_t + \frac{1}{2}g''(W_t) dt \\
&= W_t^2 dW_t + 2W_t dt \\
\frac{1}{3}W_t^3 &= \int_0^T W_t^2 dW_t + 2\int_0^T W_t dt \\
\int_0^T W_t^2 dW_t &= \frac{1}{3}W_T^3 - 2\int_0^T W_t dt  
\end{align*}
$$

Here, $\int_0^T W_t dt$ is a stochastic process evaluated at time $T$.

## Stochastic Differential Equations

A standard Ito process can be an asset price obeying:

$$
d S_t = \mu S_t dt + \sigma S_t dW_t
$$

That is, the stochastic process tracking the stock price at time $t$ given by $S_t$ changes by some multiplicative drift term and some random noise term supplied by $W_t$, Brownian motion. Then to find $S_t$, we can use a candidate function. It turns out $g(S_t) = \log S_t$. Then $g'(S_t) = \frac{1}{S_t}$ and $g''(S_t) = =\frac{1}{S_t^2}$. By Ito-Doeblin:

$$
\begin{align*}
d\log S_t &= \frac{1}{S_t} dS_t - \frac{1}{2S_t^2}(dS_t)^2 \\
&= \frac{1}{S_t} dS_t - \frac{1}{2S_t^2}(dS_t)^2 \\
&= \frac{1}{S_t} (\mu S_t dt + \sigma S_t dW_t) - \frac{1}{2S_t^2}(\mu S_t dt + \sigma S_t dW_t)^2 \\
&= \mu dt + \sigma dW_t - \frac{1}{2S_t^2}(\mu^2 S_t^2 dtdt + 2\mu\sigma S_t^2 dt dW_t + \sigma^2 S_t^2 dW_t dW_t) \\
&= \mu dt + \sigma dW_t - \frac{1}{2}(\sigma^2 dW_t dW_t) \\
&= \mu dt + \sigma dW_t - \frac{1}{2} \sigma^2 dt \\
&= \left(\mu - \frac{1}{2}\sigma^2\right)dt + \sigma dW_t \\
\int_0^T d\log S_t &= \left(\mu - \frac{1}{2}\sigma^2\right)\int_0^T dt + \sigma \int_0^T dW_t \\
\log S_T &= \log S_0 + \left(\mu - \frac{1}{2}\sigma^2\right) T + \sigma W_T \\
S_T &= S_0\exp\left(\left(\mu - \frac{1}{2}\sigma^2\right) T + \sigma W_T\right)
\end{align*}
$$

Thus the solution to the differential equation provides the formula for the stochastic process tracking the stock price that obeys the differential equation above. 



