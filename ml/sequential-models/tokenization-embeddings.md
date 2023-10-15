---
layout: default
title: Tokenization and Embeddings
parent: Sequential Models
nav_order: 2
has_children: true
---

# Tokenization and Embeddings

One question in sequential models that is not clear yet is the abiltiy for the models to read language data. To solve this issue, we introduce the idea of tokenization and embeddings.

## Tokenization

Tokenization is how words get converted to something parsable by the network or module. This process can also be called **encoding**. It could be as simple as listing every word in alphabetical order and assigning it a number or token index. The problem with this approach is that the lookup for word to token index can grow very large in large corpuses. In language such as German, there exists many compound words that are really just smaller words combined. This schema of tokenization would involve creating 3 tokens for the word "wiedersehen", "weider" and "sehen".

To deal with this issue, we consider the BPE byte pair encoding scheme. This scheme does the following:

1. Initialize a corpus $C$ of a sequence of characters
2. Initialize $V$ to be the set of characters
3. While $V$ can still be increased
    - Find two characters $xx'$ that occur together the most commonly in $C$
    - Add the element $\{xx'\}$ to $V$

We can do this with ascii, but also with unicode characters.

There are other methods as well, such as Unigram. Unigram works by the following.

1. Get every possible subset of characters from a corpus, or any reasonably big corpus to form a vocabulary $V$
2. Evaluate the number of occurence of $v \in V$ in the corpus and divide by the sum of each occurence giving a probability $P(v)$
3. $\ell = - \sum_i^k \log(P(v_i))$ is the log likelihood of the vocabulary
4. Determine tokens that contribute the least to the likelihood, and remove them. E.g. skim off the bottom 20\% contributors to the log likelihood.
5. Repeat until $V$ is small enough.

This tokenization strategy does the opposite of what BPE does - it shinks a vocabulary. 

## Embedding

A sequential model takes a sequence and has to embed it into some space in order to determine the next few sequences. It is easier to talk about embeddings in the context of language models, so we use the concept of tokenization as discussed above. The model must be able to represent the sequence in a metric space, and to do so, it takes the encoded tokens and casts it into an embedding space. 

Define the function $\phi : V^{L} \to \mathbf{R}^{d\times L}$. That is, $\phi$ takes a vocabulary of size $L$ and maps it to an embedding space of $L$-column vectors, each of dimension $d$. So a sequence $x_{1,...,L}$ can be mapped with $\phi$ to get $E\in\mathbf{R}^{d\times L}$. If we have a batch of $n$ sequences, i.e. $x_{1,...,L}^{i=1,...,n}$, then we can have $\phi$ map it to batch embeddings $E \in \mathbf{R}^{n\times d \times L}$.

Embeddings give rise to a classification of language models. 

### Encoding Only Models

These models learn an embedding given a sequence of tokens. Thus tokens that come before and after a token of interest will influence the token of interest. That is, given the following tokens:

$$[a, b, c] \to (x_1, x_2, x_3);\;\; x_1, x_2, x_3 \in \mathbf{R}^{d\times 3}$$


If $b$ is a token of interest, for example we are interested in predicting the next token after $b$, the encoding model will not be good at this tasks since $x_2$ was formed based on $x_1$ and $x_3$. It needs information on $x_3$ to determine the $x_2$ features. Thus treating this model as a next token prediction task will lead to severe overfitting. One would need to use masked language training to do next token prediction with this model.

### Decoding Only Models

This model generates a contextual embedding but also learns a probability distribution over the the next $k$ tokens - autoregressively. That is:

$$x_{1,...,L} \to \phi(x_{1,...,L});\text{ and also } P(x_{L+1,...,L+k}|\phi(x_{1,...,L}))$$

It can use likelihood based criteria to train, and can handle text completion. However, the embeddings may be of lower quality since the embeddings can only depend on the preceding tokens. RNNs or LSTMs can be appropriate components of decoding only models.

### Encoding-Decoding Models

These models produce encodings, and also probability distributions for later tokens. The training takes longer, since it is training an embedding, and also a decoding that needs to learn probability distributions. Furthermore, it requires further ad-hoc training such as masked token prediction.



