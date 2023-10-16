---
layout: default
title: Probability
nav_order: 1
has_children: true
permalink: /stats/probability
---

Probability theory was not formalized until [A.N. Kolmogorov](https://en.wikipedia.org/wiki/Andrey_Kolmogorov), a Soviet mathematician, set out to view probability as a measure - and this viewpoint drew upon the work of [Henri Lebesgue](https://en.wikipedia.org/wiki/Henri_Lebesgue) who generalized the field of measure theory via integration. From calculus-based probability, we saw that a probability was either a sum or an integral over the density or mass function, and at the time of studying it, we did not necessarily need to connect the ideas of "measure" and "integration" together, but in order to better understand properties of probability, it is crucial to visit measure theory and to view probability as a measure. To this end, we will develop some rigor in understanding basic measure theory and apply it into probability.

Additionally, one of the core motivations for studying measure theoretic probability is to understand the concept of limits and infinities - which is where analysis comes into play. An outcome space with a finite number of outcomes is easy to analyze, and we are able to enumerate each event with a probability. However in the case of infinite outcome spaces or even uncountable outcome spaces, this is no longer so simple. Throughout the study of measure theoretic probability, it is critical to understand the notion of what it truly means when considering the "limit."

Once probability theory is understood, many applications of probabilitic modelling and thinking can be developed. Statistical inference draws upon probability theory to help us discern what is expected of our data and compare it to what has been observed. Probability also provides us with a tool to model processes that are not necessarily independent over time, so that we can study its evolution - giving rise to stochastic processes. Finally, we can blend ODE and PDE theory together with probability to model natural phenomena in a probabilistic manner, rather than a deterministic one. 