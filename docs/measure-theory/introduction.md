---
layout: default
title: Measure Theory Introduction
parent: Measure Theory
nav_order: 1
---

# What is a Measure

A measure is a function $\mu$ that takes a set $A$ (typically in $\mathbf{R}^n$) and maps it to the range $[0,\infty]$. We would ideally hope that this function $\mu$ satisfies the following basic requirements for any subset of $\mathbf{R}^n$:

1. For any collection of countable disjoint sets $A_i$, we want $\mu(\cup A_i) = \sum \mu(A_i)$.
2. If $E$ and $F$ are sets that are invariant under rotation, translation, or reflection, then $\mu(E) = \mu(F)$
3. $\mu((0,1]^n) = 1$ for any $n < \infty$

In plain words, (1) requires that measuring a set that can be decomposed as a collection of disjoint sets, can be measured by adding the measures of each disjoint value in the collection. (2) requires that if a set undergoes a transformation that does not alter its size, it should have the same measure as before. (3) says that any unit simplex should have measure 1 in any finite dimension.

## Failure of Requirements

The requirements do not hold simultaneously all the time, that is, there exist cases in which these three conditions are not *mutually consistent*. We provide a classic counterexample.

Suppose we have an equivalence relation $x\sim y$ if and only if $\|x-y\| = r$ for any $x \in [0,1]$. Here $r$ is a rational number in the unit simplex of $\mathbf{R}^1$, i.e. $r \in \mathbf{Q}\cap [0,1]$. Let $N \subseteq [0,1)$ be the set that contains one element from each equivalence class (invoking axiom of choice). 

For each rational in the unit simplex, define $N_r$,

$$N_r=\{x+r : x \in N\cap[0,1-r)\} \cup \{x+r-1: x\in N\cap[r-1, 1)\}$$

That is, $N_r$ is exactly a shifted $N$ (by some rational), and for the part that extends past 1, chop off and prepend to the shifted interval, so that $N$ is constrained to be inside of $[0,1)$. Thus $\mu(N_r) = \mu(N)$ due to translational invariance.

For each $x \in [0, 1)$, there exists a unique $r$ such that $x \in N_r$. This is true since $N$ and $N_r$ contain members within each equivalence class. That is, indeed there exists a unique $y$ in $N$ such that $x\sim y$ (forms an equivalence class with $x$). If $y\leq x$ then $x \in N_r = N_{x-y}$ otherwise, $x\in N_r = N_{1+x-y}$. So $x$ must be in some $N_r$. 

This also means the disjoint union $\cup_r N_r$ since all $x \in [0, 1)$ is in $N_r$ for any $r$. Now use the three properties of the measure. First,

$$1 \stackrel{(3)}{=} \mu([0,1)) \stackrel{\cup_r N_r = [0, 1)}{=} \mu(\cup_r N_r) \stackrel{(1)}{=} \sum_r \mu(N_r) \stackrel{N_r \sim N \oplus r}{=} \sum_r \mu(N) = \infty$$

Of course, the last equality holds since $\mu(N)$ is a constant over all $r$, and $r$ is a countable sum. $N, N_r$ are all subsets of $\mathbf{R}$, but clearly, the requirements of measure are not consistent with these sets. Our requirements are too restrictive, and we must give up some requirements if we want to measure all sets. Or we can decide to only focus on sets in which measure rules are mutually consistent. This gives us the **sigma algebra** or $\sigma$-algebra. 

Our remedy is to not require measure laws to hold on all subsets, but specifically on a subset called the measureable sets. We can also denote this as $\mathcal{L}^n$ which is a subset of $2^{\mathbf{R}^n}$ (power set of reals). $\mathcal{L}^n$ is specifically, the set of all measurable sets in $\mathbf{R}^n$. 

(It is of tangential importance to note that we must accept the axiom of choice to produce this counter example)
