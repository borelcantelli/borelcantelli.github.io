---
layout: default
title: Machine Learning
nav_order: 1
has_children: true
permalink: /ml
---

# Machine Learning

Machine learning, and in general, artificial intelligence involves formulating algorithms that a computer can execute in reasonable time to solve tasks, mainly prediction or description oriented tasks.

Popular applications of ML are in robotics, computer vision, NLP, and much more. However, machine learning also encompasses algorithms such as least squares, GLMs, kernel based non-parametric methods, fitting stochastic processes, and methods based on some defined metric space such as SVD or KNN. 

This introduces the divergence of modern and classical ML. Many statistical and mathematical theories have been used to provide rigorous lower and upper bounds on error for the classical methods. However, for modern methods, the theory is not as well-known, nor is the theory entirely well developed. 

Much of the classical methods are well studied, and with enough measure theory and functional analysis (see math section), the proofs are all accesible. so we focus machine learning on the modern methods. 

## Modes of Learning

The three primary modes of learning in Machine Learning are:

Supervised Learning: In this mode, the model is trained on a labeled dataset. That is, the correct answers (labels) are provided along with the input data. The model learns to predict the output from the input data during the training process.

Unsupervised Learning: In this mode, the model is trained on an unlabeled dataset. The model learns to identify patterns and relationships in the input data without any prior labels.

Reinforcement Learning: In this mode, the model learns to make decisions by performing certain actions and receiving rewards. The model learns to perform the best action that maximizes the reward over time. That is, model learns by interacting with the environment, and receives rewards. These rewards are used to iterate the model as it continues to interact and collect experience.