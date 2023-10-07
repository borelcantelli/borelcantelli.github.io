---
layout: default
title: Typography
parent: Measure Theory
nav_order: 1
---

# What is a Measure

A measure is a function $\mu$ that takes a set $A$ (typically in $\mathbf{R}^n$) and maps it to the range $[0,\infty]$. We would ideally hope that this function $\mu$ satisfies the following:

1. $\mu$ should be defined for any subset of $\mathbf{R}^n$
2. The measure of the empty set should be 0, i.e. $\mu(\empty) = 0$
3. The measure of any two disjoint sets $A, B$ should be $\mu(A \cup B) = \mu(A) + \mu(B)$