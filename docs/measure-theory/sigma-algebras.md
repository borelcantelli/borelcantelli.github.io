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

## Why Sigma Algebras and Smallest Sigma Algebras

At this point, we should wonder why we care about $\sigma$-algebras. $\sigma$-algebras have the unique property in that they are closed under countable unions. Futhermore, we can think of this as related to measure theory in that:

1. The measure of nothing should be 0, and the measure of the entire set can be $\infty$ (or 1 in the case of probability). In fact, the measure of the entire set of interest can be any real number.

2. If we can measure one object, we should also be able to *collectively* measure everything but the object.

3. If we can measure each object in a potentially infinite and countable set, then we should be able to measure the combination of all of items in the set collectively.

Dealing with the smallest $\sigma$-algebra provides a notion of how much information is in a set. For example, suppose I draw random integers from $\{1,2,3,4\}$. I can generate the smallest $\sigma$-algebra from the set $\{(1,2), (3,4)\}$. This $\sigma$-algebra will be:

$$\mathcal{F} := \{\varnothing, \{1,2\}, \{3,4\}, \{1,2,3,4\} \}$$

Now suppose I generate the smallest $\sigma$-algebra from $\{\{1\}, \{2\}, \{3\}, \{4\}\}$. That is, I want the smallest $\sigma$-algebra that contains each possible outcome. Then the $\sigma$-algebra will be"

$$\mathcal{G} := \{\varnothing, \{1\}, \{2\}, \{3\}, \{4\}, \{1,2\}, \{2, 3\}, \{1,4\}, \{3,4\}, ...,  \{1,2,3,4\} \}$$

It will have $2^4$ elements in it, and note that this $\sigma$-algebra is contains the previous one, i.e. $\mathcal{F} \subset \mathcal{G}$. We say that $\mathcal{G}$ is *finer* than $\mathcal{F}$. We also say that $\mathcal{G}$ provides more information that $\mathcal{F}$. 

To see this, imagine someone stuck in a room and is drawing random numbers. You this person a set of index card, and each index card corresponds to an element of $\mathcal{F}$. They must return you the index card corresponding to the smallest element of $\mathcal{F}$ that contains the number from their draw. So if they draw a 2, they return the index card corresponding to $\{1,2\}$. 

Now imagine there is another person stuck in another room, and they are playing the same random game with the same rules, except this time, they are given index card, where each card corresponds to an element of $\mathcal{G}$. They must return you the index card corresponding to the smallest element of $\mathcal{G}$ that contains the number of their draw. 

In both rooms, the same game is being played, but you receive index cards with differing information. Clearly, the index cards from the $\mathcal{G}$ regime is more informative, as when a 2 is drawn, you will receive the card corresponding to $\{2\}$. 

It is obvious that for any finite set, its power set is the finest $\sigma$-algebra. If all sets we want to learn about were finite, there would be no need to study measure theory. But obviously, real numbers are uncountably infinite, and rational numbers of countably infinite, so if we want to measure something as simple as an interval of these objects, we would need to study measure theory, and consider the smallest $\sigma$-algebras generated by sets of interest, i.e. $\sigma((0,1])$. 
