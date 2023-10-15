---
layout: default
title: Transformers
parent: Sequential Models
grand_parent: Basics of Machine Learning
nav_order: 3
has_children: true
permalink: ml/sequential-models/transformers
---

# Transformers

Transformers are the next advancement in sequential models. They are primarily used in language modeling, but can be used in various other sequence-to-sequence applications. To understand transformers, one needs to understand the attention mechanism.

## Attention

Put aside the goal of developing sequence modeling for now. We focus on a mechanism used by transformers, the same way LSTMs use memory cells. Consider a sequence of embedded tokens $[x_1,...,x_n] \in \mathbf{R}^{d\times n}$ in a database and we wish to do a soft match with a query $y \in \mathbf{R}^{d\times 1}$. That is, we should associate each $x_i$ with a key and value, and the query $y$ will attempt to match the key as close as possible.

To do so, we need to convert $x_i$'s into a learned embedding space for key and value, which is done by $(W_{K}x_i : W_{V} x_i)$. Likewise, we cast $y$ into a learned embedding space $W_Q y$. As stated, $W_K, W_V, W_Q \in \mathcal{w \times d}$ are trainable and are to be trained upon initialization.

They key and query is thus $W_K x_i$ and $W_Q y$ which are $w\times 1$ vectors. So its dot product, representing similarity, can be evaluated

$$(W_k x_i)^T(W_Q y) = x_i^T Q_k^T W_Q y$$

Yielding the score for the $i$-th embedding $s_i$. We can take the score over all $n$ embeddings to get $s_1,...,s_n$. The scores each be transformed via softmax to be in $(0,1)$, which gives $[p_1,...,p_n]$.

Then attention is:

$$\sum_{i=1}^n p_i \times W_V x_i$$

In summary:

We wish to calculate attention for $y$ given a set of $x_1,...,x_n$ embeddings.

1. We take some projection matrices $W_V, W_K, W_Q$ that casts embeddings into another $w$-dimensional space.
2. Cast $y$ to the query space with $W_Q$, and each $x_i$ to the key and value space with $W_K, W_V$ respectively.
3. Evaluate similarity between $y$ and each $x_i$ keys in the $w$-space to get $[s_1,...,s_n]$
4. Take the softmax, and divide by $\sqrt{d}$ for each $s_i$ to get $[p_1,...,p_n]$
5. Calculate attention as a weighted sum of the value space for each $x_i$. Specifically, the sum is weighted by the $p_i$'s.

### Mutlihead Attention

The previous section presented a single mechanism which is called an **attention head**. Mutlihead attention is simply repeating the process concurrently several times, $h$ times for $h$ heads. Thus in the end, we can compute multiattention by creating $h$ many of $W_k, W_Q, W_V$ matrices and repeating the process for each $h$ heads. This yields $h$ attention scores for $y$ on $x_1,...,x_n$.

### Self Attention

Self attention compares each $x_i$ to each $x_1,...,x_n$, yielding attention scores for $n$ attention scores. Define $x_{1,...,n}\sim y$ as attending $y$ to $x_{1,...,n}$ which results in the attention score. Then self attention produces the following:

$$[x_{1,...,n}\sim x_1,...,x_{1,...,n}\sim x_n]$$

Self attention gives no information on the order, i.e. any permutation of the self attention scores could be valid. So we include positional embeddings, which are essentially numbers that give information on the ordering of tokens. These values are typically generated with trigonometric functions.

## Transformer Block

Now that attention is discussed, the transformer block is ready to be built. Transformers as it is now, can be difficult to train due to the several parameters that need to be updated. Additionally, we can stack transformer blocks, which makes the network deeper.

To mitigate these gradient issues arising from training, we can add skip connections and normalizations. One way to mitigate gradient vanishing is to add the pre-attended vectors to the attention scores, and layer norm.

Then they are fed into an MLP and then added and layer-normed again.

So a tranformer block is the following:

1. Take input token sequence and self attend each token to the whole token sequence
2. Add the pre sequence to the attention scores and layer norm the result
3. Pass the result to an MLP and add the pre-MLP to the post-MLP results, then layer-norm

This forms one transformer block, in the encoding step.

After attention encoding, the decoder blocks take the $W_K^0$ and $W_V^0$ from the final encoder layer. The decoder block is as follows:

1. Use the $W_K^0$ and $W_V^0$ in block. An embedded start token serves as a query at this point.
2. The self attention mechanism happens again, and the output of attention scores is passed through an MLP and softmaxed to give probabilities of the most likely next word (a token that is then converted back to a word). Call it $t_1$.
3. The predicted word $t_1$ is then embedded and used as the next query for the decoder attention layer.
    - The decoder self-attention layers use the query from only previously predicted words. These query matrices are produced by the first decoder layer, and passed on to the next decoder layer. This gives autoregressive nature of the model.

This is how GPT-3 works. As it creates no embeddings for the vocabulary, it is a decoder only model, namely the decoder is the entire attention mechanism in the (encoder-decoder attention blocks).

## Training Critieria

Suppose we provide the supervised label output "Thank you" to some input sequence. In token space, it is "[34, 23, 9, 0]" representing "[Th, ank, you, <>]". Then we would place a probability distribuion around $P(o_1=34)=1, P(o_2=34)=1, P(o_3=34)=1, P(o_4=0)=1$, that is, 4 random variables, one for each position.

Then the softmax layer will provide probabilities in training that will optimize to mimic the above distribution.

One way would be to pick the highest probability for $o_1$, then for $o_2$ given the previous, and so on, which is called greedy decoding. A more common method is beam search. Suppose we do beam search with beam size of $2$. This means that at $o_1$, we select the top 2 most likely tokens. We run the model forward twice, assuming $o_1$ was each of the top 2, and predice the next $o_2$. Then check which sequence of $(o_1, o_2)$ yielded the lower KL divergence error, which we keep the best $o_1$. Then we procede to select $o_2$ by picking the top 2 and evaluating loss for a generated $o_3$. We repeat this process until the stop token. In summary:

1. Select $k$ candidates for position $i$ i.e. $o_i = c_1,..., c_k$
2. Run the model $j=1,...,k$ times, each time supposing $o_i = c_j$ and generating $\hat{o}_{i+1}$
3. Check which $(c_j, \hat{o}_{i+1})_{j=1}^k$ yields the lowest error compared to the truth
4. Set $o_i := c_j$ for the optimal $j$ that yielded the lowest error in part (3)
5. Move to $o_{i+1}$, and generate $k$ candidates. Repeat from (1) until exhausted sequence (EOS token)



