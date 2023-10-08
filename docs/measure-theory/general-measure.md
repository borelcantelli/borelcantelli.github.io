---
layout: default
title: General Measure Functions
parent: Measure Theory
nav_order: 3
has_toc: true
---

# Navigation Structure
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


# General Measure Functions

## TLDR

1. A measure function is endowed upon a measure space and must assign the empty set to a measure of 0, and must be countably additive.
2. A measure space must satisfy monotonicity, subadditivity, and continuity from above and below.
3. Complete measures deal with null sets, which are sets with measure 0. This gives rise to the term $P$-almost everywhere, which means that proposition $P$ holds for all sets except for the null sets.

## Measure Functions and its Types

A measure function is a type of function that is defined on measurable sets. Any $\sigma$-algebra generated on $\Omega$ is a measurable subset of $2^\Omega$. Thus, for $\sigma$-algebra $\mathcal{A}$ endowed upon $\Omega$, the measure function $\mu: \mathcal{A} \to [0,\infty]$ is a **measure function**. It satisfies two criteria:

1. $\mu(\varnothing) = 0$
2. Countable additivity. For disjoint $A_1, A_2,...$, $\mu(\sqcup_{i>0} A_i) = \sum_{i>0} \mu(A_i)$.

*Notation*: Instead of writing $\mathcal{A}$ endowed upon $\Omega$, with a measure function $\mu: \mathcal{A} \to [0, \infty]$, we typically express this as $(\Omega, \mathcal{A})$ or $(\Omega, \mathcal{A}, \mu)$. The former indicates a *measurable space*, while the latter indicates a *measure space*, which is just a measurable space, with a measure function defined on the space. Sets in $\mathcal{A}$ are called *measurable sets*.

Criteria 2 also implies finite additivity, i.e. for disjoint $A_1,...,A_n$, $\mu(\sqcup_{i=1}^n A_i) = \sum_{i=1}^n \mu(A_i)$. If $(\Omega,\mathcal{A}, \mu)$ only satisfies the finite additivity criteria, and is not countably additive, then we say $\mu$ is a **finitely additive measure**.

A few more definitions given $(\Omega, \mathcal{A}, \mu)$:

1. If $\mu(\Omega) < \infty$ them $\mu$ is a **finite measure**
2. $\mu$ is **$\sigma$-finite** if $X = \cup_{i> 0} E_i$ with $\mu(E_i) < \infty$ for all $E_i \in \mathcal{A}$.
3. If for each $A \in \mathcal{A}$ that has $\mu(A) = \infty$, there exists a subset $F \subseteq A \in \mathcal{A}$ where $\mu(F) < \infty$, then $\mu$ is **semi-finite**.

**Exercise**: Show that the counting measure is a measure. *Hint: The counting measure is a mapping between a set and its cardinality, and takes the value $\infty$ when a set is at least countably infinite.*

**Exercise**: If $\mu$ is a measure on $(\Omega, \mathcal{F})$ then define $\mu_B(\cdot) = \mu(\cdot \cap B)$. Show that $\mu_B$ is also a measure on $(\Omega, \mathcal{F})$.

## Measure Space

As defined previously, a measure space is a measurable space that has a measure function defined on it. A measure space $(\Omega, \mathcal{A}, \mu)$ must have the following properties:

1. Monotonicity. If $E, F \in \mathcal{A}$ and $E \subseteq F$ then $\mu(E) \leq \mu(F)$
2. Subadditivity. If $E_1,E_2,... \in \mathcal{A}$, then $\mu(\cup_{i>0}E_i) \leq \sum_{i>0} \mu(E_i)$
3. Continuity from below. If $E_1\subseteq E_2 \subseteq ... \in \mathcal{A}$ then $\mu(\cup_{i>0}E_i) = \lim_i \mu(E_i)$.
4. Continuity from above. If $E_1 \supseteq E_2 \supseteq ... \in \mathcal{A}$ and $\mu(E_1) < \infty$, then $\mu(\cap_{i>0}E_i) = \lim_i \mu(E_i)$.

Note that in (4), the finiteness assumption of the largest superset is not present in (3). The finiteness assumption is necessary since it is not always true when there is infinite measure. Here is an example.

Consider measure space $(\Omega, \mathcal{F}, \mu)$ where $\mu$ is the counting measure. Let $E_i := \{i, i+1,...\}$. Of course, $E_i \supset E_{i+1}$, but $\cap E_i = \varnothing$ since any given finite number will no longer exist in $E_i$ for $i$ large enough. Thus $\mu(\cap E_i) = 0$. However, $\lim_i \mu(E_i) = \infty$ since $E_\infty$ is still an infinite set.

*Proof of properties*: 

1. Monotonicity: suppose $E \subseteq F$. Then $E \cup F \setminus E = E \cup F \cap E^C = F$. Now $\mu(F) = \mu(E) + \mu(F \cap E^C)$. As $\mu \geq 0$, then $\mu(F) \geq \mu(E)$.

2. Let $B_k = A_k \setminus \cup_{i=1}^{k-1} A_k$ and $B_1 = A_1$. Then $\cup B_k = \cup A_k$ bu $\{B_k\}_{k>0}$ are all disjoint.

$$\mu(\cup_{i>0} A_i) = \mu(\sqcup_k B_k) = \sum \mu(B_k) = \sum_k \mu(A_k \setminus \cup_{i<k} A_i) \leq \sum \mu(A_k)$$

3. If $E_i \subseteq E_{i+1}$, then $B_i = E_i \setminus E_{i-1}$ with $B_1 = E_1$ form disjoint sets and $\cup B_i = \cup E_i$. Then:

$$\mu(\cup_{i>0}E_i) = \mu(\sqcup_{i>0} B_i) = \sum_{i>0} \mu(B_i) = \sum_{i > 1} \mu(E_i) - \mu(E_{i-1}) + B_1 = \lim_i \mu(E_i)$$

4. If $E_i \supseteq E_{i+1}$ and $\mu(E_1) < \infty$, then let $B_i = E_1 \setminus E_i$. Thus $F_i \subseteq F_{i+1}$ is increasing so by (3), $\cup F_i = E_1$. Also, note that $\cup F_i = \cup E_1 \cap E_i^C = E_1 \setminus \cap E_i$

$$\mu(E_1) = \mu(\cup F_i) = \mu(E_1 \setminus \cap E_i) = \mu(E_1) - \mu(\cap E_i)$$

$$\implies \mu(\cup F_i) +\mu(\cap E_i) = \mu(E_1)$$

$$\implies \lim_i \mu(F_i) = \lim_j \mu(E_1) - \mu(E_j) + \mu(\cap E_i)$$

$$\implies \lim_i \mu(E_i) = \mu(\cap E_i)$$

**Exercise**: Prove that $\mu(\liminf A_n) \leq \liminf \mu(A_n) \leq \limsup \mu(A_n) \leq \mu(\limsup A_n)$. *Hint: $\liminf A_n \equiv \cup_{N> 0}\cap_{n > N} A_n$ and $\limsup A_n \equiv \cap_{N>0}\cup_{n > N}A_n$. Use the above properties to show this.*

# Null Sets and Complete Measures

Given a measure space, $(\Omega, \mathcal{A}, \mu)$, a **null set** is a set $A\in \mathcal{A}$ such that $\mu(A) = 0$. A statement is true **almost everywhere** if the statement is true on all sets, but not necessarily the null sets. $\mu$ is a **complete measure** if $\mathcal{A}$ includes all subsets of null sets.

