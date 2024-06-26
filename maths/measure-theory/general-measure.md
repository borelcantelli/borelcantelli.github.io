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


# Measures

## TLDR

1. A measure function is endowed upon a measure space and must assign the empty set to a measure of 0, and must be countably additive.
2. A measure space must satisfy monotonicity, subadditivity, and continuity from above and below.
3. Complete measures deal with null sets, which are sets with measure 0. This gives rise to the term $P$-almost everywhere, which means that proposition $P$ holds for all sets except for the null sets.
4. Outer measures do not satisfy requirements of measures - by definition, but they induce a measurable space
5. This induction of a measurable space allows extensions to be made on measures defined on simpler spaces. Most importantly, Caratheodory's extension theorem.
6. Borel measures are constructed by defining a premeasure on the algebra of half open sets, then invoking Caratheodory to assert that a measure, and a complete measure extends from the premeasure on a $\sigma$-algebra.
7. The Borel measure when defined by function $F$, that is, $\mu((a,b]) = F(b)-F(a)$, is called the Lebesgue-Stieltjes measure. When $F(x)=x$, this measure is called the Lebesgue measure.

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

# Outer Measure

In integration, we require that the upper Riemann sum and the lower Riemann sum converge to be integrable. That is, as partitions get finer, the upper sum descends down to the true area, while the lower sum ascends to the true area. The **outer measure** is inspired by the upper sum. 

The **outer measure** $\mu^O$ is any measure function that satisfies:

1. $\mu^O(\varnothing) = 0$
2. Monotonicity: $\mu^O(A) \leq \mu^O(B)$ if $A \subseteq B$
3. Subadditivity: $\mu^O(\cup_i A_i) \leq \sum_i \mu^O(A_i)$

A particular outer measure is the **Lebesgue outer measure** defined on the space $\Omega$. It is defined as:

$$\mu^O(A) = \inf\left\{ \sum_{i=1}^\infty \lambda(E_i) : A \subseteq \bigcup_{i=1}^\infty E_i;\; \forall E_i \in \mathcal{E}\right \}$$

Here $\lambda$ is the Lebesgue measure. For now we define this as $\lambda: \mathcal{E} \to [0, \infty]$ for $\mathcal{E} \subseteq 2^\Omega$. $\lambda(\varnothing) = 0$. From the definition, there is nothing from stopping us to restrict $A$ to any $\sigma$-algebra. In fact, $A$ can be anything in $2^\Omega$. 

Intuitively, this is the "smallest" measure of all measurements made on coverings of $A$, the set of interest.

**Exercise**: Prove that $\mu^O$ satisfies monotonicity

For subadditivity:

*Proof*: Given a countable collection of $A_1,...$, define a covering made by $E_i^j$. Specifcally, we cover each $A_i$ with a set of $\{E_{i,j}^k\}_{j>0}$ such that the covering is slightly larger than $A_i$. Specifically, we fulfill the condition $\sum_j \lambda(E_{i,j}^k) \leq \mu^O(A_k) + \frac{\varepsilon}{2^k}$ for some $\varepsilon > 0$.

Then,

$$\sum_{k=1}^\infty \sum_{j=1}^\infty \lambda(E_{i,j}^k) \leq \sum_{k=1}^\infty \mu^O(A_k) + \frac{\epsilon}{2^k} = \sum_{k=1}^\infty \mu^O(A_k) + \varepsilon$$

As $\mu^O(\cup A_i)$ is the infimum of a set that contains $\sum_{k=1}^\infty \sum_{j=1}^\infty \lambda(E_{i,j}^k)$, then 

$$\mu^O(\cup A_i) \leq \sum_{k=1}^\infty \sum_{j=1}^\infty \lambda(E_{i,j}^k) \leq  \sum_{k=1}^\infty \mu^O(A_k) + \varepsilon$$

As $\varepsilon > 0$ is arbitrary, then we have:

$$\mu^O(\cup A_i) \leq \sum_{k=1}^\infty \mu^O(A_k)$$

satisfying subadditivity. **QED**

It is important to note that we are covering each $A_k$ with a covering that is larger than $A_k$ by $\frac{\varepsilon}{2^k}$. So as $k$ increases, the covering of $A_k$ gets arbitrarily closer to $A_k$ in outer measure. It is a relatively useful technique in measure theoretic proofs.

Again, the outer measure $\mu^O$ is not defined on sets to a restricted to a $\sigma$-algebra. In fact, it is not even a measure since it is defined on all subsets of $\mathbf{R}^n$. Clearly, we run into the issue of mutual consistency as seen before. We must restrict the definition to sets of $2^{\mathbf{R}^n}$ that are $\mu^O$-measurable. 

**Definition**: A set $A$ is $\mu^O$-measurable if 

$$\mu^O(E) = \mu^O(E\cap A) + \mu^O(E\cap A^C);\; \forall E \in 2^\Omega$$

Note that the outer measure did not require additivity on disjoint sets. This definition provides that restriction. This definition can be interpreted with the following logic:

1. $\mu^O(E) \leq \mu^O(E\cap A) + \mu^O(E\cap A^C)$ by subadditivity
2. If we also can show the above is true when $\leq$ is changed to $\geq$, then we have equivalence. (for $E$ where $\mu^O(E) < \infty$)

That is, the idea of the "lower" outer measure and "upper" outer measure need to agree to be $\mu^O$-measurable.

So if set $A$ does not satisfy this property, it is not $\mu^O$ measurable. Why did we bother with $\sigma$-algebras and complete measures if we could reduce measurability down to this definition? It turns out that $\mu^O$-measurable sets form a $\sigma$-algebra as well, and is a complete measure.

# Caratheodory's Theorem

This is the statement of the theorem:

**Theorem** (Caratheodory) Let $\mu^O$ be an outer measure on $\Omega$. Then $\mathcal{A}^O := \{\mu^O \text{-measurable sets}\}$ forms a $\sigma$-algebra. Also, $\mu^O$ defined on sets in $\mathcal{A}^O$ is a complete measure.

The significance of this theorem is that it allows us to define a measure that is valid on a simpler algebraic structure, in this case, the set of elements that satisfy the definition of $\mu^O$-measurable. Then invoking this theorem allows us to define an agreeing *complete* measure on this simpler structure, that generalizes to a $\sigma$-algebra. That is, if we can define a measure, we can extend the measure to be complete. 

A variant of this theorem exists in probability theory, allowing us to define a probability measure on an algebra $A$, which extends to another probability measure on $\sigma$-algebra $\mathcal{F}-sets$, that agrees with the measure on $A$-sets.

While we can try to use the definition of an outer measure and its $\mu^O$-measurable sets to demonstrate the properties of a $\sigma$-algebra, we can also do so more conveniently with the monotone class theorem or an intermediary step of Dynkin's $\pi-\lambda$ theorem.

*Proof of Caratheodory*: First, $A, A^C$ are $\mu^O$ measurable is trivial by the definition. So if $A \in \mathcal{M}^O$ then so is $A^C$. $\varnothing \in \mathcal{M}^O$ is free too. 

This satisfies the $\lambda$-system requirement and allows us to demonstrate closure under finite unions for the $\pi$-system requirement of finite intersections.


Now we demonstrate that $\mathcal{M}^O$ is closed under finite unions. That is, if $A, B \in \mathcal{M}^O$ then $A \cup B \in \mathcal{M}^O$. Indeed:

$$\mu^O(E) = \mu^O(E \cap A) + \mu^O(E \cap A^C)$$
$$= \mu^O(E \cap A \cap B) + \mu^O(E \cap A \cap B^C) + \mu^O(E \cap A^C \cap B) + \mu^O(E \cap A^C \cap B^C)$$

$$\leq \mu^O(E \cap (A \cup B)) + \mu^O(E \cap (A \cup B)^C)$$

Combining this with subadditivity, we have that $A \cup B$ satisfies the definition of being $\mu^O$ measurable. As finite unions and complementation closed in $\mathcal{M}^O$, we have that it forms an algebra, and also is a $\pi$-system. (Note it is more than a $\pi$-system too since it is closed under complements.)

The last step to show $\mathcal{M}^O$ is a $\sigma$-algebra is to show closure under disjoint countable unions. Let $A_1,...$ be a sequence of disjoint sets which are in $\mathcal{M}^O$.

As $\mathcal{M}^O$ is an algebra, then:

$$\mu^O(E) = \mu^O(E \cap (\sqcup^n_i A_i)) + \mu^O(E \cap (\sqcup^n_i A_i)^C)$$

Note that $E\cap (\sqcup_i^n A_i) \supseteq E \cap (\sqcup_i^\infty A_i)$, so we can rewrite the above with and inequality:

$$\mu^O(E) \geq \mu^O(E \cap (\sqcup^n_i A_i)) + \mu^O(E \cap (\sqcup^n_i A_i)^C)$$

To proceed to the next step, show that:

**Exercise**: Show $\mu^O(E \cap (\sqcup_{i=1}^n A_i)) = \sum_{i=1}^n \mu^O(E \cap A_i)$. *Hint: Do this by induction. The key is that $\mu^O(E \cap (\sqcap_{i=1}^n A_i)) = \mu^O(E \cap A_n) + \mu^O(E \cap (\sqcup_{i=1}^{n-1}A_i))$.*

Sending $n\to\infty$:

$$\mu^O(E) \geq \sum_{i=1}^\infty \mu^O(E \cap A_i) + \mu^O(E \cap (\sqcup^n_i A_i)^C)$$

and by subadditivity of the first term on the RHS,

$$\mu^O(E) \geq \mu^O(E \cap (\sqcup_{i=1}^\infty A_i)) + \mu^O(E \cap (\sqcup_{i=1}^\infty A_i)^C)$$

So countable disjoint unions are closed in $\mathcal{M}^O$ making it a $\lambda$-system as well. As $\mathcal{M}^O$ is a $\pi$-system and a $\lambda$-system, it is a $\sigma$-algebra.

$\mu^O$ is a measure on $\mathcal{M}^O$ since it is is closed under disjoint unions as well. That is:

$$\mu^O(\sqcup^n_i A_i) \geq \sum_{i=1}^\infty \mu^O(\sqcup^n_i A_i \cap A_i) + \mu^O(\sqcup^n_i A_i \cap (\sqcup^n_i A_i)^C)$$

$$\mu^O(\sqcup^n_i A_i) \geq \sum_{i=1}^\infty \mu^O(A_i)$$

And combining this with subadditivity, we have equality. Thus $\mu^O$ is a measure. 

It is also a complete measure. For $A \subset N$ such that $\mu^O(N) = 0$:

$$\mu*(E) \geq \mu^O(E \cap A) + \mu^O(E \cap A^C)$$

**Exercise**: Prove the above statement. *Hint: $E \cap A \subseteq A$ and $E \cap A^C \subseteq E$. Then argue by subadditivity to show that null sets are in $\mathcal{M}^O$.

Thus we shown that $\mathcal{M}^O$ is a $\sigma$-algebra, and the outer measure $\mu^O$ defined on sets of $\mathcal{M}^O$ is a complete measure. **QED**

## Premeasures and Application of Caratheordory

Caratheodory's theorem provides a good way for use to contruct a measure on a simpler set and insist that the same measure can be validly extended to other more complex environments, as discussed before. We introduce the notion of a **premeasure**. 

**Definition**: A **premeasure** is a function $\mu_0: L \to [0, \infty]$ for algebra $\mathcal{A}$. It satisfies the following:

1. $\mu_0(\varnothing) = 0$
2. $\mu_0(\sqcup_{i=1}^\infty A_i) = \sum_{i>0}\mu_0(A_i)$

We say that a premeasure $\mu_0$ defined on algebra $\mathcal{A}$ *induces* an outer measure $\mu^O$ if:

$$\mu^O(A) := \inf\left\{\sum_i \mu_0(E_i) : A \subseteq \cup_i E_i;\; E_i \in \mathcal{A}\right\}$$

We can provide a property of premeasures that leverages Caratheodory and demonstrates the usefulness of defining a premeasure.

**Theorem**: On a space $\Omega$, if we have an algebra $\mathcal{A}$ equipped with a premeasure $\mu_0$, its induced outer measure $m^O$ is agrees on $\mathcal{A}$-sets, and $\mathcal{A}$ is $m^O$-measurable.

*Proof*: First, suppose $A \subseteq \cup E_i$. Then define $B_i$ as $A \cap E_i\setminus \cup^{i-1}_j E_j $. Therefore, we know that at least $B_i \subseteq E_i$ and is in $\mathcal{A}$, so $\sum m_0(B_i) \leq \sum m_0(E_i)$. Thus $m_0(A) \leq m^O(A)$. ($\sum m_0(E_i)$ is any element of the outer measure set).

Also $A \subseteq \cup B_i $, so $\sum m_0(B_i)$ is in the set $\{\sum_i \mu_0(E_i) : A \subseteq \cup_i E_i;\; E_i \in \mathcal{A}\}$, thus $m^O(A) \leq m_0(A)$. Therefore, $m^O(A) = m_0(A)$ for $A \in \mathcal{A}$, that is, measures agree.

Now we can just show that $A \in \mathcal{A}$-sets are $m^O$-measurable. To do so, we just need to show:

$$m^O(E) \geq m^O(E \cap A) + m^O(E \cap A^C);\; \forall A \in \mathcal{A},\, \forall E \in 2^\Omega$$

Indeed, there exists some $E\subseteq \sqcup_i B_i$ with $B_i \in \mathcal{A}$ such that $m^O(E) + \varepsilon \geq \sum m_0(B_j)$. This is by definition of the outer measure. Now $A_j$ are all $m^O$ measurable and $m_0$ agrees with such measure on that domain, so:

$$m^O(E) + \varepsilon \geq \sum m^O(B_j) = \sum m^O(B_j \cap A) + m^O(B_j \cap A^C)$$

which actually holds for any $B \subset 2^\Omega$. Then since $m^O(E) \leq m^O(\sqcup B_i) \leq \sum m^O(B_i)$, by sub-additivity of $m^O$,

$$\sum m^O(B_j \cap E) + m^O(B_j \cap A^C) \geq m^O(E \cap A) + m^O(E \cap A^C)$$

Therefore $m^O(E) = m*(E \cap A) + m^O(E \cap A^C)$, so $A$ is $m^O$-measurable, for any $A \in \mathcal{A}$. **QED**

This theorem tells us that the outer measure, induced by the premeasure on an algebra, is defined on a $\sigma$-algebra, namely the set of all outer-measurable sets. Furthermore, this outer measure is an extension of the pre-measure, and all sets in the algebra is also outer-measurable.

This next theorem is commonly accepted as the most powerful application of Caratheodory's Theorem. It is what gives it the name, *Caratheodory's extension thoerem* used in probability theory - in particular, to prove Fubini's theorem on joint probability spaces. 

**Theorem**: Let $m_0$ be a premeasure defined on algebra $A$ and $\mathcal{F} := \sigma(A)$. Then there exists a measure $\mu$ on $\mathcal{F}$, such that $\mu = m^O\|_{\mathcal{F}}$ and $m\|_{A} = m_0$. Furthermore, if $\nu$ is another measure defined on $\mathcal{F}$ that extends from $m_0$, then $\nu(E) \leq \mu(E)$ for all $\mathcal{F}$-measurable sets, with equality when $\mu(E)$ is finite.

*Proof*: The existence of such a measure with said conditions is a consequence of the theorem above. Now onto uniqueness and $\nu$.

Let $E \in \mathcal{F}$. Given some sequence of $A_i \in A$ such that $E \subseteq \sqcup_i A_i$:

$$\nu(E) \leq \sum \nu(A_i) = \sum \mu_0(A_i) \leq \mu(E)$$

Thus $\nu(E) \leq \mu(E)$. Now suppose we restrict $\mu(E) < \infty$. Then it suffices to show $\nu(E) \geq \mu(E)$ to demonstrate equality after combining it with the immediately above result.

Select some $E \subseteq \sqcup_i A_i$ with each $A_i \in \mathcal{A}$, such that:

$$\mu(E) < \sum m_0(A_i) = \mu(\sqcup_i A_i) \text{ equivalently } \mu(E) + \varepsilon \geq \mu(\sqcup_i A_i)$$

(Remember that $m_0 = \mu$ on $\mathcal{A}$ and $A_i$ are disjoint.) As $E \subseteq \sqcup A_i$, $\mu(\sqcup A_i \setminus E) = \mu(\sqcup_i A_i) - \mu(E) \leq \varepsilon$ from above. We can do so since $\mu(E) < \infty$. Thus, $\nu$ which is always at least bounded by $\mu$ from the result above, we get $\nu(\sqcup A_i \setminus E) = \nu(\sqcup A_i) - \nu(E) \leq \varepsilon$. Then:

$$\nu(E) \geq \nu(\sqcup A_i) - \varepsilon \geq \mu(E) - \varepsilon$$

Since $\varepsilon$ is arbitrary, then $\nu(E) \geq \mu(E)$. Combining with $\nu(E) \leq \mu(E)$, we arrive at $\nu(E) = \mu(E)$ once we require $\mu(E)<\infty$. **QED**

So this theorem is proven by first, establishing existence via the previous theorem, and demonstrating uniqueness of measures by showing the measures agree. This mechanism works by using some disjoint covering of $E$ which is possible from the definition of the premeasure and induced outer measure. Then using properties of subadditivity, and an epsilon room argument, we arrive at the conclusion. It allows us to state that a single measure exists from the extension of a premeasure on an algebra. 

# Borel Measures

A measured defined on $\mathcal{B}(\mathbf{R}^n)$ is called a **Borel Measure**. Due to properties of the real numbers, there are a few properties that can be derived from the Borel measures. Namely, if $\mu$ is a finite Borel measure, and we define $\mu((-\infty, x]) := F(x)$, then $F$ is:

1. Increasing, i.e. $F(x) \leq F(y)$ for $x < y$
2. Right continuous, i.e. $F(x^+) = F(x)$ for all $x$. 

**Exercise**: Prove the statements above. Assume $\mu$ is already a valid measure. *Hint: 1) is by monotonicity, and 2) is by continuity from above.*

## Building Borel Measures

From the previous statements, it is clear that we define the Borel measure on Borel sets, namely sets of form $(-\infty, x]$ and $(x, \infty)$ as these are complements of each other. To discuss measurability of such sets, we consider how to construct a $\sigma$-algebra from these sets and also how to define a measure on 1) the generating sets $(-\infty, x]$ and $(x, \infty)$  and 2) its null sets to form a complete measure.

### Create an Algebra on $\mathbf{R}$
First, we introduce the term *h-interval*. This is any interval of the form $(-\infty \leq x < y < \infty)$:

1. $(x,y]$
2. $(x, \infty)$
3. $\varnothing$

The set of h-intervals and its finite unions form an algebra $\mathcal{H}$, but not a $\sigma$-algebra.

*Proof Sketch*: $\varnothing$ is free, and so is closure under finite unions. The complements can be formed by:

1. $(x, y]^C = (y, \infty) \cup (-\infty, x]$
2. $(x, \infty)^C = (-\infty, x]$
3. $\varnothing^C = (-\infty, \infty)$

which are all elements in $\mathcal{H}$. **QED**

### Define a Premeasure on the Algebra

Now that we have an algebra, we can define a premeasure on $\mathcal{H}$. Recall $F(x) = \mu((-\infty, x])$. Now we define $\mu_0(\sqcup_i^n (a_i, b_i]) = \sum_{i}^n F(b_i)-F(a_i)$. Clearly $\mu_0$ is a premeasure, as we also have $\mu_0(\varnothing) = \sum_i^n F(a)-F(a) = 0$.

This premeasure has three important properties:

1. The premeasure is well defined, i.e. any two equivalent sets have the same premeasure.
2. $F(\infty) = \sup_x F(x)$ and $F(-\infty) = \inf_x F_x$
3. If $F(x) = x$ then $\mu_0((a, b]) = b - a$

*Proof*: For 1) suppose we have two equivalent sets $\sqcup_i^n (a_i, b_i] = \sqcup_j^m (c_j, d_j]$. Then:

$$\mu_0(\sqcup_i^n (a_i, b_i]) = \sum_i^n \mu_0((a_i, b_i])$$

Take the common refinement of $(a_i, b_i]$ and $(c_j, d_j]$. There are $n\times m$ intervals, i.e. $\sqcup_i^n (a_i, b_i] = \sqcup_j^m (c_j, d_j] = \sqcup_j^m\sqcup_i^n (g_{ij}, h_{ij}]$. By definition of a common refinement:

1. $\sqcup_j^m (g_{ij}, h_{ij}] = (a_i, b_i]$
2. $\sqcup_i^n (g_{ij}, h_{ij}] = (c_j, d_j]$

Thus we have:

$$\mu_0(\sqcup_i^n (a_i, b_i]) = \sum_i^n \mu_0((a_i, b_i]) = \sum_i^n \mu_0(\sqcup_j^m (g_{ij}, h_{ij})) = \sum_i^n \sum_j^m F(h_{ij})-F(g_{ij})$$

$$\equiv\sum_i^n \sum_j^m F(h_{ij})-F(g_{ij}) = \sum_j^m F(d_j) - F(c_j) = \mu_0(\sqcup_j^m (c_j, d_j])$$

2) is free, by monotonicity of $F$

3) is more involved. We prove only the finite $(a,b]$ case. We wish to find the premeasure on half interval $I := (a,b]$. Suppose there is a partition such that $I = \sqcup_j^\infty I_j$. Then:

$$\mu_0(I) = \mu_0(\sqcup_j^n I_j) + \mu_0(I \setminus \sqcup_{j}^{n} I_j) \geq \mu_0(\sqcup_j^n I_j) = \sum_j^n \mu_0(I_j)$$

Sending the limit $n\to\infty$, we get $\mu_0(I) \geq \sum_j \mu_0(I_j)$. Now it remains to show $\sum_j \mu_0(I_j) \leq \mu_0(I)$. To do so, we construct a compact set that will cover $b$, and approach $a$ from above. Namely, we will build something like so:

$$[a+\delta, b] \subseteq \sqcup_i (a_i,b_i] \subseteq \cup (a, b+\delta_j)$$

Thus we have a largest set that is approaching $(a,b]$ by becoming smaller, and another set that is approaching $(a, b]$ by enlarging. 

We can do so since $F$ is right continuous, so there exists some $\delta$ such that $F(a+\delta) - F(a) < \varepsilon$ for some $\varepsilon > 0$. Thus we can construct the set $[a+\delta, b] \subseteq I$. Furthermore, there is some sequence of $\delta_j$ such that $F(b_j + \delta_j) - F(b_j) < \varepsilon/2^j$. Again, this is possible by the right continuity of $F$.

Now, $[a + \delta, b]$ is a compact set in $\mathbf{R}^1$, so there exists a finite subcover for any open covering of this set by the Heine-Borel theorem. $\cup_i (a_i ,b_i + \delta_i)$ is an open cover, and by HB Theorem, $[a+\delta, b] \subseteq \cup_i^n (a_i, b_i + \delta_i)$. Now:

1. $F(b)-F(a) = F(b)-F(a+\delta)+F(a+\delta)-F(a)$. Telescope the sum
2. $= F(b)-F(a+\delta) + \varepsilon$. Last two terms reduce to $\varepsilon$ by right continuity
3. $\leq\sum_i^n F(b_i+\delta_i) - F(a_i) + \varepsilon$. Heine Borel covering measure
4. $\leq \sum_i^\infty F(b_i)-F(a_i) + 2\varepsilon$. Right continuity on $F(b_i + \delta_i) - F(b_i) < \epsilon/2^i$

Since $\varepsilon$ is arbitrary, $F(b)-F(a) \leq \sum_i^\infty F(b_i)-F(a_i) = \sum_j^\infty \mu(I_j) = \mu_0(I)$. Thus combining with the previous result, $F(b)-F(a) = \mu_0(I)$. **QED**

### Invoke Caratheodory to Extend the Premeasure
So now we have a premeasure $\mu_0$ defined on an algebra $\mathcal{H}$. It has an induced outer measure $\mu^O$. By Caratheodory, we have $\mu$ defined on $\sigma(\mathcal{H})$ - the Borel $\sigma$-algebra, where $\mu \equiv \mu^*\|_{\sigma(\mathcal{H})}$. We say that $\mu$ extends $\mu_0$. 

The significance of the property above is that the premeasure $\mu_0$ can be created by any increasing right continuous function $F: \mathbf{R} \to \mathbf{R}$. This premeasure can then be extended to a $\sigma$-algebra and is a measure on that $\sigma$-algebra, namely the Borel $\sigma$-algebra. Because of this, we can define any probability measure (distribution function) the same way. Recall in probability, we have $P(X \leq x) = \int_{\omega:X(\omega) \leq x} d\mathbf{P}(\omega)$. Here $\mathbf{P}$ is a probability measure, and we can change its basis to give the equivalent integral $\int_0^x dF(x)$, where $F$ is the CDF of $X$. 

So we have a measure space $(\mathbf{R}, \mathcal{B}, \mu)$ that is induced and extended from the premeasure on the algebra of half open intervals. The last step is to complete the measure, which we already know how - by considering all sets and subsets of sets where the measure is 0. This is denoted as $(\bar{\mathbf{R}}, \bar{\mathcal{B}}, \bar{\mu})$. From here, we let $\mu$ denote the complete measure. Recall that $\mu((a,b]) = F(b)-F(a)$. This as defined on the completed measure space is called the **Lebesgue-Stieltjes** measure associated with $F$. If $F(x) = x$, then this $\mu$ is called the **Lebesgue** measure on the measurable space. 




