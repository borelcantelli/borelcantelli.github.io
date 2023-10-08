---
layout: default
title: Sigma Algebras
parent: Measure Theory
nav_order: 1
---

# Sigma Algebra

We should keep in mind the type of subsets of real numbers that we can measure. To do so, we define a logic on objects that can be measured, i.e. $\mathcal{L}^n$. Special types of sets impose a logic on the objects in the collection. As such, the first "logic" to be defined is the concept of an *algebra*.

**Definition**: Given an ambient space $\Omega$, an **algebra** $A$ endowed on the space satisfies the following constraints:

1. $\varnothing \in A$
2. If $X \in A$ then $X^C \in A$
3. If $X_1,...,X_n$ is a collection of finite countable sets in $A$, then its union $\cup_{i=1}^n X_i \in A$

If we further relax the condition (3) to being just countable sets (finite or infinite), $A$ becomes a $\sigma$-algebra and is usually denoted with script $\mathcal{A}$. That is:

**Definition**: Given an ambient space $\Omega$, a $\sigma$**-algebra** $\mathcal{A}$ endowed on the space satisfies the following constraints:

1. $\varnothing \in \mathcal{A}$
2. If $X \in \mathcal{A}$ then $X^C \in \mathcal{A}$
3. If $X_1,...,X_n$ is a collection of finite countable sets in $\mathcal{A}$, then its union $\cup_{i=1}^n X_i \in \mathcal{A}$

Note that any algebra is a $\sigma$-algebra but not all $\sigma$-algebras as algebras.

**Exercise**: Prove that the intersection of two $\sigma$-algebras, $\mathcal{E}, \mathcal{F}$ both endowed on $\Omega$, forms a $\sigma$-algebra. *Hint: show that property (1), (2), and (3) all hold in $\mathcal{E}\cap \mathcal{F}$.*