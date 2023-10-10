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
4. Outer measures 

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

**Exercise**: If $\mu$ is a measure on $(\Omega, \mathcal{F})$ then define $\mu_B(\cdot) = \mu(\cdot \cap B)$ where $B \in \mathcal{F}$. Show that $\mu_B$ is also a measure on $(\Omega, \mathcal{F})$.

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

We can extend a measure to be complete. That is, given $(\Omega, \mathcal{F}, \mu)$, we can extend it to $(\Omega, \bar{\mathcal{M}}, \bar{\mu})$, the completion of the measure space.

Let $(\Omega, \mathcal{M}, \mu)$ be a measure space. Let $\mathcal{N} := \{N \in \mathcal{M} : \mu(N) = 0\}$. Define $\bar{\mathcal{M}} = \{E \cup F : E\in\mathcal{M},\, F\subset N, \forall N \in \mathcal{N}\}$. Then $\bar{\mathcal{M}}$ is a $\sigma$-algebra and there exists a unique $\bar{\mu}$ that extends the measure $\mu$ onto $\bar{\mathcal{M}}$. That is $\bar{\mathcal{M}}$ is the completion of $\mathcal{M}$ with respect to $\mu$.

*Proof*: First, we can show that $\bar{\mathcal{M}}$ is a $\sigma$-algebra by showing closure under complements and countable unions. 

Consider a countable sequence of $M_1,... \in \bar{\mathcal{M}}$. Then for each $i$, we can decompose $M_i = E_i \cup F_i$, and 

$$\cup_{i > 0} M_i = \cup_{i> 0} E_i \cup F_i = \cup_{i>0}E_i \bigcup \cup_{i>0} F_i$$

Since $\cup_{i>0}E_i \in \mathcal{M}$ and $\cup_{i>0} F_i \in \mathcal{N}$ (indeed, $F_i \subset N$ so $\mu(F_i) = 0$, and $\mu(\cup F_i) = 0$, thus it is in $\mathcal{N}$). So we have closure under countable unions.

To show closure of complementation, we need to note that $E \cap F = \varnothing$. This is implied by construction of our sets. Indeed, let $E \cup F = E \sqcup F \setminus E$. Now $F \setminus E \subseteq N \setminus E$ since $F \subseteq N$.  And $N \setminus E$ still has measure 0, so $N \setminus E \in \mathcal{N}$. WLOG, $E \cap \mathcal{N} = \varnothing$, that is $E$ is disjoint from any element in $\mathcal{N}$.

Now let $M \in \bar{\mathcal{M}}$, so there exists an $E\in\mathcal{M}$ and a $F \in \mathcal{M}$ s.t. $E \cup F \in \mathcal{M}$. We want to show closure of $(E \cup F)^C$ or $E^C \cap F^C$.

$$E^C \cap F^C = E^C \cap (N^C \cup N \cap F^C) = (E^C \cap N^C) \cup (N \cap F^C)$$

$E^C \in \mathcal{M}$ is obvious. $N^C \in \mathcal{M}$ is true since $N$ is measurable and thus is in $\mathcal{M}$. $N \cap F^C$ is still a measure 0 set, so it is still in $\mathcal{N}$. Thus $(E^C \cap N^C) \in \mathcal{M}$ and $(N \cap F^C) \in \mathcal{N}$ so its union must be in $\bar{\mathcal{M}}$.

$\varnothing \in \bar{\mathcal{M}}$ is free.

Second, we show the completion of the measure exists. We do this by defining the completion of $\mu$ on the sets in $\bar{\mathcal{M}}$, and dictate that the measure and its completion agree on $\mathcal{M}$-sets. For $E \in \mathcal{M}$ and $F \in \mathcal{N}$, 

$$\mu(E) = \bar{\mu}(E) \leq \bar{\mu}(E \cup F) \leq \bar{\mu}(E \cup N) \leq \bar{\mu}(E) + \bar{\mu}(N) = \mu(E)$$

Therefore, we can deduce that $\bar{\mu}(E\cup F) = \mu (E)$. That is, any set in $\bar{\mathcal{M}}$ can be measured with $\bar{\mu}$ as defined by the previous.

Now for any two equal sets in $\bar{\mathcal{M}}$, namely of the forms. $E_1 \cup F_1 = E_2 \cup F_2$, we aim to show that $\mu(E_1) = \mu(E_2)$ (this is simply by definition of $\bar{\mu}$).

$$\mu(E_1) \leq \mu(E_1 \cup F_1) = \mu(E_2 \cup F_2) \leq \mu(E_2  \cup N) = \mu(E_2)$$

$$\mu(E_2) \leq \mu(E_2 \cup F_2) = \mu(E_1 \cup F_1) \leq \mu(E_1 \cup N) =\mu(E_2)$$

As $\mu(E_1) = \mu(E_2) \iff \bar{\mu}(E_1 \cup F_1) = \bar{\mu}(E_2 \cup F_2)$, then it must be that $\bar{\mu}$ agrees on equivalent sets, hence unique. Furthermore, $\bar{\mu}$ is a complete measure of $\bar{\mathcal{M}}$ since $\bar{\mathcal{M}}$ contains all subsets of null sets by contruction on which $\bar{\mu}$ is defined.







