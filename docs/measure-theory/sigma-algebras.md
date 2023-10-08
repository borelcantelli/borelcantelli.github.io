---
layout: default
title: Sigma Algebras
parent: Measure Theory
nav_order: 2
---

# Sigma Algebra

## Introducing the Sigma Algebra

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

**Exercise**: Prove that $\sigma$-algebras are closed under countble intersections. *Hint: Define an arbitrary $\sigma$-algebra and use Demorgans law to show that if countable sets $X_1,...$ are in the $\sigma$-algebra, then its intersection must also be in the $\sigma$-algebra*.

## Special Sigma Algebras

The power set of any set is clearly a $\sigma$-algebra. The trivial $\sigma$-algebra on any set $\Omega$ is $\{\varnothing, \Omega\}$. Another trivial $\sigma$-algebra is $\{\varnothing, X, X^C, \Omega\}$ for any $X \in \Omega$. 

We can even generate $\sigma$-algebras by taking intersections. As seen in the previous exercise, any two $\sigma$-algebras endowed on $\Omega$ is still a $\sigma$-algebra. Suppose $X$ is an element in $\Omega$. What is the smallest $\sigma$-algebra that still contains $X$? 

This is a question of interest since we would ideally like to consider $\sigma$-algebras that contain $X$, which means it must contain $X^C$ and all countable unions that involve $X$ or $X^C$. We denote this as $\sigma(X)$. 

**Definition**: Let $\mathcal{G}$ be the collection of all possible $\sigma$-algebras that can be made on $\Omega$. Let $\mathcal{F}$ be the collection of all $\sigma$-algebras that contain $X\in\Omega$. The smallest $\sigma$-algebra endowed on $\Omega$ that contains a set $X\in\Omega$ is:

$$\sigma(X) = \bigcap_{\mathcal{F} : \mathcal{F} \in \mathcal{G}} \mathcal{F}$$

In otherwords, it is the intersection of all $\sigma$-algebras that contain $X$. 

## Why Sigma Algebras

At this point, we should wonder why we care about $\sigma$-algebras. $\sigma$-algebras have the unique property in that they are closed under countable unions. Futhermore, we can think of this as related to measure theory in that:

1. The measure of nothing should be 0, and the measure of the entire set can be $\infty$ (or 1 in the case of probability). In fact, the measure of the entire set of interest can be any real number.

2. If we can measure one object, we should also be able to *collectively* measure everything but the object.

3. If we can measure each object in a potentially infinite and countable set, then we should be able to measure the combination of all of items in the set collectively.


