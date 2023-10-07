---
layout: default
title: Typography
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



