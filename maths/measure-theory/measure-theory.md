---
layout: default
title: Measure Theory
nav_order: 1
has_children: true
permalink: /maths/measure-theory
---

# Measure Theory

Measure theory is the study of the **measure**. It is an important basis of probability theory, and in general, integration theory. It is also a natural extension from mathematical analysis which typically finishes with basic integration theory. As measure theory is an important basis for probability theory, the following pages will be dedicated to understanding properties of the measure, as well as motivations for why we study measures.

# Word on Notation

This was all made in markdown and mathjax. In an ideal world, full latex support would be the most appropriate to write on measure theory. Due to convenience, I take several liberal shortcuts when it comes to notation. I list a few here:

- $\sqcup$ means disjiont union, but sometimes $\cup$ is used to denote the same thing. Unless explicitly stated, $\cup$ will be used to denote a union. 
- $\cup_i, \cap_i$ or $\cup, \cap$ all denote countable unions and intersections. If there is an $n$ like $\cup_i^n$ or $\cap_i^n$, it denotes a finite union/intersection. The same distinction applies to sums being countable or finite.
- Typical books use $\mathcal{A}_0$ to denote an algebra and $\mathcal{A}$ to denote a $\sigma$-algebra. The mathematical structure of whatever variable used will be explicitly defined here. This means there can be even $\pi$-systems denoted with $\mathcal{A}$ - but this will be explicitly stated.
- $\mathcal{B}$ denotes the Borel set on $\mathbf{R}$, unless explicitly stated that it is defined on one or any arbitrary topological space.
- If a function $f: X \to \mathbf{R}$ and measurable, it is assumed that the associated measurable spaces are $(X, \mathcal{X})$ and $(\mathbf{R}, \mathcal{B})$. If $g: X \to Y$ is measurable, it is assumed that $(X, \mathcal{X})$ and $(Y, \mathcal{Y})$ are the associated measurable spaces.



{: .fs-6 .fw-300 }
