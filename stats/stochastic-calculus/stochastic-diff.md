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

# Stochastic Integration

Stochastic integral theory is developed analogously to Lebesgue theory where integrals are first defined for indicator functions, then for simple functions, then for continuous function, so on. First, consider some function $\phi_t$ for which we wish to integrate with respect to a stochastic process $W_t$ over some interval $A$. Take a dyadic partition of $A$ and collect the partitions into $A_n$ where each higher value of $n$ denotes an even finer partition of $A$. Approximate the integal by as sum:

$$\sum_{(p_l, p_r) \in A_n} \phi_t(\omega) (W_{p_r} - W_{p_l})(\omega)$$

Here $t$ is simply any point $t \in (p_l, p_r)$ and $W_{p_r} - W_{p_l}$ is the differenced stochastic process to which we are integrating. Without proof, we state:

$$\lim_{n\to\infty} \sum_{(p_l, p_r) \in A_n} \phi_t(\omega) (W_{p_r} - W_{p_l})(\omega) = \int_0^T \phi_t(\omega) dW_t(\omega)$$

If we take $t$ to be the midpoint of each partition $(p_l, p_r)$ then the integral is called the *Stratonovich integral*. If we take $t = p_l$ in each partition, then we have the *Ito integral*. For smooth functions, the two integrals converge intuitively. However, we are concerned with integrating over stochastic processes, which may be differentiable almost nowhere such as Brownian motion. The difference between is by an additive factor of $\frac{1}{2}t$. 

$$I(T) = \int_0^T W_s(\omega) dW_s(\omega) = \frac{W_s(\omega)^2}{2} - \frac{1}{2}t;\;\;\; S(T) = \int_0^T W_s(\omega) dW_s(\omega) = \frac{W_s(\omega)^2}{2} $$

The Ito integral is a martingale which lends some nice properties of stochastic processes. For example, in the integral above $I(T)$, we can easily show that the integral is a martingale. 

The point of stochastic integration is to show that there is a way to define stochastic integration such that rules of ordinary calculus are obeyed. However there is another definition of stochastic integration, namely Ito integration, that presents more interesting properties, but differs slightly from the rules of ordinary calculus.

Some other formulas that will differ because we have defined integration this way is the chain rule and product rule of stochastic calculus.

## Ito Integration

In order to understand the chain rule and product rule, we should first understand **Ito isometry** and  **quadratic variation**. The isometry property provides a method to calculate variances of stochastic processes. Variation tracks the relative drift of of the stochastic process over time and will be useful in providing tools for the stochastic chain rule. Quadratic variation tracks the square difference between two time points of a stochastic process. For these, we consider simple processes then introduce a general framework to extend results to continuous processes.

### Ito Isometry

**Ito isometry** states that for any simple process $\Delta(t)$ which is constant on some partition of $T$, $t_1 < ... < t_k$, 
$$
\left(\int_0^T \phi(u) dW_u\right)^2 = \int_0^T \phi(u)^2 du
$$

For simple functions, $\int_0^T \phi(u) dW_u$ is defined as $\sum_{(t_j, t_{j+1})\in \cv{T}} \phi(t_j) (W_{j+1} - W_j)$ which is a sum over partitions of $T$ such that $\phi(t)$ is constant on $t \in [t_j, t_{j+1})$. Let the partition be broken into $k$ subintervals. Then by squaring the sum:
$$
I^2(T) = \left(\int_0^T \phi(u)dW_u\right)^2 = \left(\sum_{i=1}^k \phi(t_j)(W_{j+1}-W_j)\right)^2
$$
$$
= \sum_{i=1}^k \phi(t_j)^2(W_{j+1}-W_j)^2 + 2\sum_{1= i <j < k} \phi(t_i)\phi(t_j)(W_{t_i+1}-W_{t_i})(W_{t_j+1}-W_{t_j})
$$

Now we just need to pass this through the expectation operator. As $\phi(t_j)$ is constant in each subinterval, and $\E((W_t-W_s)^2) = t-s$ by property of variances of Brownian motion, the first sum becomes 

$$\sum_{i=1}^k \phi(t_j)^2\E((W_{j+1}-W_j)^2) = \sum_{i=1}^k \phi(t_j)^2(t_{j+1}-t_j)$$

The cross sum is 0, since the covariance of the difference of brownian motions are independent when considering the pair over different intervals. Thus by the Riemann definition, we have the following integral:

$$
\E(I^2(t)) = \sum_{i=1}^k \phi(t_j)^2(t_{j+1}-t_j) = \int_0^T \phi(u)^2 du
$$

### Quadratic Variation

Consider the **quadratic variation** of the Ito integral which is defined as $\langle I,I\rangle(t)$. Notationally, $\langle g,g\rangle (t)$ is defined as 

$$\langle g,g\rangle (T) = \lim_{n\to\infty} \sum_{(t_j, t_{j+1})\in \cv{T}_n} (g(t_{j+1}) - g(t_j))^2$$ 

where $\cv{T}_n$ is the set of subintervals of $[0,T)$ that get finer as $n\to\infty$. This essentially tracks how much movement the process $g$ changes over an interval $T$. There also is first order variation which measures the same limit over the absolute value of differences.

Now define $$ \abs{\cv{T}_n} $$ as the maximum interval length of the partition. As $n\to\infty$ then $\vert \cv{T}_n \vert \to 0$. Furthermore, for any interval $[t_j,t_{j+1}) \in \cv{T}_n$, we have $$\vert t_{j+1} - t_j\vert \leq \vert \cv{T}_n \vert$$. 

Back to the quadratic variation of the Ito integral, let $t_k < s_1 < ... < s_n < t_{k+1}$ where $\Delta$ is constant on $[t_k, t_{j+1})$. That is, we partition the regions where $\Delta$ is constant into $m+1$ partitions. Within each partition, the differenced Ito integral is 

$$
[I,I](t) = \lim_{n\to\infty} \sum_{(t_j, t_{j+1})\in \cv{T}_n} (I(t_{j+1}) - I(t_j))^2 = \lim_{n\to\infty} \sum_{(t_j, t_{j+1})\in \cv{T}_n} \sum_{k=0}^{m-1} (I(s_{k+1}) - I(s_k))^2
$$

Recall that $\Delta(u)$ is cadlag so the difference of the Ito integrals in the inner summand is: 

$$
\sum_{k=0}^{m-1} (I(s_{k+1}) - I(s_k))^2 = \sum_{k=0}^{m-1} (\Delta(t_{j+1})(W_{s_{k+1}}- W_{s_k}))^2 = \Delta(t_j)^2 \sum_{k=0}^{m-1} (W_{s_{k+1}}- W_{s_k})^2
$$

As we send $n\to \infty$, we make $$\vert \cv{T}_n\vert \to 0$$ thus making the intervals $[t_j, t_{j+1})$ to have length 0, we have the expression above become $\Delta(t_j)^2(t_{j+1} - t_j)$ where the $t_{j+1}-t_j$ is the total variation of Brownian motion $W_{t_{j+1}} - W_{t_j}$. Furthermore, as we sum over the partitions of $\cv{T_n}$, we get the integral:

$$
\sum_{k=0}^{m-1} (I(s_{k+1}) - I(s_k))^2 = \int_0^T \Delta^2 (u) du
$$


### Quadratic Variation for Brownian Motion

Earlier we made reference to the quadratic variation of Brownian motion $W_t$ to be $t$. Now we prove this. Let $$ \cv{T}_n $$ be a dyadic partition of the interval $[0,T)$ on which Brownian motion is defined. The goal is to show

$$
\lim_{n\to\infty} \sum_{(t_j ,t_{j+1}) \in \cv{T}_n} (W_{t_{j+1}} - W_{t_j})^2 \stackrel{\cv{P}}{\to} T
$$

by taking its expectation and variance and showing the limit of each converges to $T$ and $0$ respectively. As a rough sketch, we know that $W_{t_{j+1}} - W_{t_j} \stackrel{\cv{L}}{=} W_{t_{j+1}-t_j}$ so $\E((W_{t_{j+1}} - W_{t_j})^2) = t_{j+1}-t_j$. 
$$
\frac{1}{n}\sum_{(t_j ,t_{j+1}) \in \cv{T}_n} \E((W_{t_{j+1}} - W_{t_j})^2) = \sum_{j=0}^{n-1} t_{j+1}-t_j = T
$$

Of course the expectation will be $T$ in the limit as well. By independence of disjoint intervals for Brownian motion, $Var((W_{t_{j+1}}-W_{t_j})^2) = 2(t_{j+1} - t_j)^2$ since $W_{t_{j+1}}-W_{t_j} \sim N(0, t_{j+1}-t_j)$ and $(N(0,\sigma^2)/\sigma)^2 \sim \chi^2$ with degrees of freedom 1. Then $\frac{(W_{t_{j+1}}-W_{t_j})^2}{t_{j+1}-t_j} \sim \chi_1^2$ so the variance of just the numerator is $2(t_{j+1}-t_j)\cdot (t_{j+1}-t_j)$.

$$
Var\left(\sum_{(t_j ,t_{j+1}) \in \cv{T}_n} (W_{t_{j+1}} - W_{t_j})^2\right) \leq \sum_{(t_j ,t_{j+1}) \in \cv{T}_n} 2 \vert \cv{T}_n \vert (t_{j+1}-t_j)
$$

As $n\to \infty$, $\cv{T}_n \to 0$ thus the variance above tends to 0 as well. Thus the quadratic variation of $W_t$ for $t \in [0, T)$ is $T$ itself. This proof can be generalized to any interval $[T_1, T_2)$ so that the quadratic variation on that interval for $W_t$ is $T_2 -T_1$. 

Note the quadratic variation of $t$ on any interval is 0, since $$\sum_{(t_j ,t_{j+1}) \in \cv{T}_n} (t_{j+1}- t_j)^2 \leq \vert\cv{T}_n\vert \sum_{(t_j ,t_{j+1}) \in \cv{T}_n} (t_{j+1}- t_j) \to 0$$ as $n\to\infty$. By the same rational, it is straightfoward to show $$ [W_t, t](T) = 0 $$ too on a bounded set. Quadratic variation has to do with realized volatility when tracking stock prices.

In short, we can write quadratic variation in differential notation $dW_t dW_t = dt$ and $dW_t dt = 0$ and $dt dt = 0$ which is an informal notation.

## Defining the Ito Integral for a Continuous Process

To extend the integral from a elementary function scenario to a continuous function scenario, we take $\Delta_n(t)$ and form it dyadically like how simple functions approximate continuous functions in Lebesgue integration, so that as $n\to \infty$, $\Delta_n(t)$ approaches the continuous function. That is, for continuous function $\Delta(t)$ and an approximating simple function $\Delta_n(t)$,

$$
\int_0^T \Delta(t) dW(t) = \lim_{n\to\infty} \int_0^T \Delta_n(t) dW(t)
$$

We should discuss about why such an integral exists. 

### Existence of the Limit of the Ito Integral

For any continuous process $\Delta(t)$, $\Delta(t)$ can be approximated the following way:

$$
\Delta_n(t) = \sum_{k=0}^{n-1} \Delta\left(\frac{k}{T}\right) \ind\left(t \in \left[\frac{k T}{n}, \frac{(k+1)T}{n} \right)\right)
$$

As $\Delta(t)$ is continuous and $\Delta_n(t)$ is cadlag, then $\Delta_n(t) \to \Delta(t)$ as $n\to\infty$. We can arrive at the same result no matter how we partition $T$. 

Now let $I_n(T) = \int_0^T \Delta_n(t) dW(t)$. Since $I_n(t)$ is the Ito integral of a simple process, then by Ito isometry, $I_n^2(T) = \int_0^T \Delta_n^2(t)dt$. Furthermore, for another $m$, $I_m(t)$ is also the Ito integral of a simple process and by rule of integration $I_n(T)-I_m(t) = \int_0^T \Delta_n(t) - \Delta_m(t) dt$. 

Now $\Delta_n(t)-\Delta_m(t)$ is also simple processes so by Ito isometry $(I_n(T) - I_m(T))^2 = \int_0^T (\Delta_n(t) - \Delta_m(t))^2 dt$. Notice that $\Delta_n(t) - \Delta_m(t)$, for sufficiently large $m,n$, $(I_{n} - I_{m})^2$ will be 0. Thus $I_n$ is Cauchy in $L_2$, so the limit also exists in $L_2$

### Example 

A prime example for Ito integration is to integrate $\int_0^T W_t dW_t$. To do so, consider the ''dyadic'' approximation of $W_t$ which is the following (for simplicity, we divide by $n$ instead of $2^n$):
$$
\Delta_n(t) = \begin{cases}
0 & 0 < t < T/n \\
W\left(\frac{T}{n}\right) & T/n < t < 2T/n \\
W(2\cdot \frac{T}{n}) & 2T/n < t < 3T/n \\
\vdots & \vdots \\
W\left((n-1)\cdot \frac{T}{n}\right) & (n-1)T/n < t < T
\end{cases}
$$

This uses the left approximation of $W_t$ within each interval of length $T/n$. Then the integral can be worked out where $V(j) = W(j\cdot T/n)$:
$$
\begin{align*}
\frac{1}{2}\sum_{j=0}^{n-1} (V_{j+1}-V_j)^2 &= \sum_{j=0}^{n-1} \frac{1}{2}V_{j+1}^2 - V_{j+1}V_j + \frac{1}{2}V_j^2 && \text{expand}\\
&= \frac{1}{2} V_n^2 + \frac{1}{2}\sum_{k=0}^{n-1} V_k^2 - \sum_{j=0}^{n-1}V_{j+1}V_j + \frac{1}{2} \sum_{j=0}^{n-1}V_j^2 && \text{break up first term} \\
&= \frac{1}{2} V_n^2 + \sum_{k=0}^{n-1} V_k^2 - \sum_{j=0}^{n-1}V_{j+1}V_j && \text{combined term 2 and 4} \\
&= \frac{1}{2} V_n^2 + \sum_{k=0}^{n-1} V_j(-V_{j+1}+V_j) && \text{factor}
\end{align*}
$$

The Ito integral we are interested in uses the above property, where $V_j$ is the shorthand notation of $W(jT/n)$ and by definition of integrating simply processes,
$$
\lim_{n\to\infty} \sum_{j=0}^{n-1} V_j(V_{j+1}-V_j) = \frac{1}{2}V_n^2 - \frac{1}{2}\sum_{j=0}^{n-1}(V_{j+1}-V_j)^2 
$$

This term involves have the quadratic variation of the Brownian motion, which is $T$ as we are integrating over $[0,T)$. Therefore, as $n \to \infty$, $V_n \to W_T$ and by quadratic variation $\frac{1}{2}\sum_{j=0}^{n-1}(V_{j+1}-V_j)^2 \to \frac{1}{2}T$. So 

$$\int_0^T W(t) dW(t) = \frac{1}{2}W_T^2 - \frac{1}{2}T$$ 

Compare this with the ordinary calculus expression for $\int_0^T x dx = \frac{1}{2}T^2$. The Ito integral has an additional $\frac{1}{2}T$ due to the expression involving the quadratic variation of the Brownian motion. Essentially, Brownian motion moves so quickly that in infinitesimal time, we still need to account for variability in the Brownian motion which is so high that summing $(W_{t_{j+1}}-W_{t_j})^2$ will not tend to 0 as $t_{j+1} - t_j \to 0$. 

The Stratonovich integral (denoted as $\int_0^T W(t) \circ dW(t)$) provides a result that is consistent with ordinary calculus, however as the integral results in another stochastic process, we are interested in properties of the stochastic process. While the Stratonovich integral provides a nice consistent result, one property that the Stratonovich integral does not have is that the resulting process need not be a martingale. The Ito integral result is a martingale, as the reader should check. Finally, the Stratonovich integral is evaluated by taking midpoint values in each continuous approximation. This is not appropriate in finance as there is more value in pricing earlier i.e. for the smallest $t$ in $[t_j, t_{j+1})$. 

# Ito-Doeblin Formula

The chain rule and product rule in ordinary calculus are straightforward and can be immediately derived from definitions of the derivative. However as seen above, Ito integration need not obey ordinary calculus rules due to the quadratic variation. The variability in the Brownian motion example demonstrates one situation where Ito integration rules $\neq$ ordinary integration rules. Likewise, for derivatives, there are also differences in the rules of calculus. 

We present the Ito formula for differentiation a process of $X(t)$. This is also known as the chain rule analog to Ito calculus. For now, we consider the Brownian motion case.

Suppose $f(t,x)$ is a function of time $t$ and the process $x$ such that partial derivatives $f_x(t,x)$, $f_t(t,x)$, and $f_{xx}(t,x)$ are continuous. Then the **Ito-Doeblin formula for Brownian motion** is:

$$
f(T, X_T) = f(0, X_0) + \int_0^T f_t(t,X_t)dt + \int_0^T f_x(t,X_t) dX_t + \frac{1}{2}\int_0^T f_{xx}(t,X_t)dt
$$

where $f(0, X_0)$ is a given initialization. In differential form, we drop the integrals and the initialization term, and add a $\partial$ onto the $f(t,X_t)$ on the LHS. 

$$
\partial f(t, X_t) = f_t(t, X_t)\partial t + f_x(t,X_t)\partial X_t + \frac{1}{2}f_{xx}(t,X_t)\partial t
$$

To see this, consider the Taylor expansion, up to the second order: 

$$
\begin{align*}
    f(t_{j+1}, t_{j+1}) - f(t_{j}, t_{j}) &= f_t(t_j,x_j)(t_{j+1}-t_j) + f_x(t_j,x_j)(x_{j+1}-x_j) + \frac{1}{2}f_{xx}(x_j, t_j)(x_{j+1} - x_j)^2 \\
    &\qquad \qquad + f_{tx}(t_j, x_j)(t_{j+1} - t_j)(x_{j+1}-x_j) + \frac{1}{2}f_{tt}(t_j, x_j)(t_{j+1}-t_j)^2
\end{align*}
$$

Taking sums on both sides over the partitioned interval $[0, T)$, we have:

$$
\begin{align*}
f(T, W_T) - f(0, W_0) &= \sum_{i=0}^{n-1}f_t(t_j,W_j)(t_{j+1}-t_j) + \sum_{i=0}^{n-1} f_x(t_j,W_j)(W_{j+1}-W_j) + \\
&\qquad \qquad \frac{1}{2} \sum_{i=0}^{n-1} f_{xx}(W_j, t_j)(W_{j+1} - W_j)^2 + \frac{1}{2} \sum_{i=0}^{n-1}f_{tt}(t_j, W_j)(t_{j+1}-t_j)^2 + \\
&\qquad \qquad \sum_{i=0}^{n-1} f_{tx}(t_j, W_j)(t_{j+1} - t_j)(W_{j+1}-W_j)
\end{align*} 
$$

The first time converges to $\int_0^T f_t(t,W_t) dt$ as the partitions get finer. By our definition of Ito's integral, the seoncd and third term converges to $\int_0^T f_x(t_j, W_t)dW_t$ and $\int_0^T f_{xx}(t, W_t)dt$. The final two terms converge to 0 by bounding $t_{j+1} - t_j$ with $$\vert \cv{T}_n \vert$$ or $$\vert W_{t-j} - W_{t_j} \vert$$ which is also 0 in the limit. Then as $n\to\infty$, the entire sum goes to 0 as well. The higher order terms also converge to 0 in the same manner. There should be caution taken when discussing the $f_{xx}$ term as it is not exactly correct to say that $(W(t_{j+1})-W(t_j))^2 \implies dW(t)dW(t) \implies dt$. Instead, to show that 

$$
\frac{1}{2} \sum_{i=0}^{n-1} f_{xx}(W_j, t_j)(W_{j+1} - W_j)^2 \stackrel{n\to\infty}{\to} \frac{1}{2}\int_0^T f_{xx}(W(t), t)dt
$$

It would suffice to show that:

$$
I_n(T) = \sum_{i=0}^{n-1} f_{xx}(W_j, t_j)((W_{j+1} - W_j)^2 -(t_{j+1}-t_j)) \stackrel{n\to\infty}{\to} 0
$$

This is because we know by continuity of $f_{xx}$ that $\sum_{i=0}^{n-1} f_{xx}(t_{j+1}-t_j) \stackrel{n\to\infty}{\to} \int_0^T f_{xx}(t, W(t))dt$. To show the above the proof is outlined in two steps:

1. Show that $\E(I_n(T)^2) = \E\left(\sum_{k=0}^{n-1} f_{xx}^2(W(t_k))((W_{j+1} - W_j)^2 -(t_{j+1}-t_j))^2\right)$
2. Show that $\E(I_n^2(T)) \to 0$

Point number two is sufficient since if $\E(I_n^2(T)) \to 0$ then $I_\infty^2(T) = 0$ a.s. which means $I_\infty (T) = 0$ a.s. For ease of notation, denote $\Delta W_i = (W(t_{i+1}) - W(t_i))$ and $\Delta t_i = (t_{i+1}-t_i)$.

So for point 1, consider $I_n^2(T)$ which will contain $n$ terms of $f(t_i,W(t_i))^2((\Delta W_i)^2 -\Delta t_i)^2$ and also $\binom{n}{2}$ cross terms of $f(t_i, W(t_i))f(t_j, W(t_j))((\Delta W_i)^2 -\Delta t_i)((\Delta W_j)^2 -\Delta t_j)$ for $i\neq j$. 

We want these cross terms to be 0 before taking any sort of limit. To do so, recall $W(t_j)^2 -  t_j$ is a martingale, by property of Brownian motion. (Prove this by taking a Laplace transform of brownian motion and then deriving). Without loss, assume $i < j$.

$$
\begin{align*}
&\E\left(f_{xx}(t_i, W(t_i))f_{xx}(t_j, W(t_j))((\Delta W_i)^2 -\Delta t_i)((\Delta W_j)^2 -\Delta t_j)\right) \\
&= \E(\E(f_{xx}(t_i, W(t_i))f_{xx}(t_j, W(t_j))((\Delta W_i)^2 -\Delta t_i)((\Delta W_j)^2 -\Delta t_j)\vert \cv{F}_{t_j}))
\end{align*}
$$

As $i < j$, everything is $$\cv{F}_{t_j}$$ measurable except for $\Delta W_j^2 - \Delta t_j$. However, these things are independent of $$\cv{F}_{t_j}$$ by property of Brownian motion as $\Delta W_j \perp \cv{F}_{t_j}$. Thus,

$$
\begin{align*}
&\E(\E(f_{xx}(t_i, W(t_i))f(t_j, W(t_j))((\Delta W_i)^2 -\Delta t_i)((\Delta W_j)^2 -\Delta t_j)\vert \cv{F}_{t_j})) \\
&= \E(f_{xx}(t_i, W(t_i))f(t_j, W(t_j))((\Delta W_i)^2 -\Delta t_i)\E((\Delta W_j)^2 -\Delta t_j\vert \cv{F}_{t_j})) \\
&= \E(f_{xx}(t_i, W(t_i))f(t_j, W(t_j))((\Delta W_i)^2 -\Delta t_i)\E((\Delta W_j)^2 -\Delta t_j)) \\
&= 0
\end{align*}
$$

The last line is 0 since $\E((\Delta W_j)^2 -\Delta t_j)$ is the same as taking the variance of $\Delta W_j$ which is $t_{j+1} - t_{j}$. This makes the entire expectation 0. So all cross terms are 0. Thus we have shown that:

$$
\E(I_n(T)^2) = \E\left(\sum_{k=0}^{n-1} f_{xx}^2(t_k, W(t_k))((W_{j+1} - W_j)^2 -(t_{j+1}-t_j))^2\right)
$$

Now, we show the expectation converges to 0 as we increase the partitions. 

$$
\begin{align*}
\E\left(\sum_{k=0}^{n-1} f_{xx}^2(W(t_k))((\Delta W_j)^2 -\Delta t_j)^2\right) &\leq \max_{t_k} \vert f_{xx}(t_k, W(t_k))\vert^2 \sum_{k=0}^{n-1} \E\left(((\Delta W_j)^2 -\Delta t_j)^2\right) \\
& = \max_{t_k} \vert f_{xx}(t_k, W(t_k))\vert^2 \sum_{k=0}^{n-1} \Delta t_i^2 \E\left(((\Delta W_1)^2 -1)^2\right)\\
&\leq (\max_{i} \Delta t_i) \max_{t_k} \vert f_{xx}(t_k, W(t_k))\vert^2 \sum_{k=0}^{n-1} \Delta t_i \E\left(((\Delta W_1)^2 -1)^2\right) \\
&\to 0
\end{align*}
$$

The last line converges to 0 due to $\Delta t_i \to 0$ as the partition gets finer when $n\to\infty$. This proves part 2, so that $\E(I_n^2(T)) \to 0$ thereby letting $I_\infty(T) = 0$. Since $I_\infty(T) = 0$, we have:

$$
\frac{1}{2} \sum_{i=0}^{n-1} f_{xx}(W_j, t_j)(W_{j+1} - W_j)^2 \sim \frac{1}{2} \sum_{i=0}^{n-1} f_{xx}(W_j, t_j)(t_{j+1} - t_j) \stackrel{n\to\infty}{\to} \frac{1}{2}\int_0^T f_{xx}(W(t), t)dt
$$

To make things faster, **Box calculus** is introduced, which simply provides a heuristic of how to handle cases such as $(\partial W_t)^2$. These are not well defined terms, but exist as a heuristic for algebraic convenience. 

Heuristic | $dW$ | $dt$|
----|------|-----|
$dW$| $dt$ | 0 |
$dt$| 0    | 0 |

As mentioned, $\sum f(t_i, W(t_i))(W(t_{i+1})-W_{t_i})^2$ will heuristically converge to $\int f(t, W(t))(dW(t))^2$. So by the above heuristic, $\sum f(t_i, W(t_i))(W(t_{i+1})-W_{t_i})^2 \to \int f(t, W(t))dt$. Remember that this heuristic exists solely due to the quadratic variation of Brownian motion.

