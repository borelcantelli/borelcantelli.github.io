---
layout: default
title: Sequential Models
parent: Sequential Models
grand_parent: Basics of Machine Learning
nav_order: 1
has_children: true
permalink: /ml/sequential-models/first-models
---

# First Sequential Models

A sequential model is one that can do either:

1. Take a sequence and give a scalar output
2. Take a sequence and give a sequence output

The key unifying trait of either one of these types is that the sequential nature of the training data is preserved and accounted for when producing outputs. Below we provide the basics on how each sequential model works.

The most basic sequential model is the RNN, 

## Recurrent Neural Networks

A recurrent neural network involves hidden states, which is what seperates them from other models at the time. These hidden states are sequentially updated by each sequential data point, and each output is a function of the hidden state and the input vector.

Given some time series $X_1^T$, $Y_1^T, Z_1^T$ that may have underlying correlation structure, we can create an RNN to output next predictions $X_{t+1}, Y_{t+1}, Z_{t+1}$. To do so, we use the following algorithm (there are other variants as well).

The uses the following parameters:

- $h_0$ is the initialized hidden state
- $W_h$ is the update matrix for the hidden state
- $W_I, W_0$ is the update matrix to take the input space to the hidden space, and then from the hidden space to the output space
- $b_h, b_O$ are bias terms associated with the weights $W_h$ and $W_O$. 

### Forward Pass

We have inputs which are batches of size $n$, i.e. $(x_1,...,x_n)$ sequences. We can evaluate each sequence at time $t$, to get $(x_{1t}, ...,x_{nt})$. Now also assume each $x_{ij}$ is $d$-dimensional. 

The forward pass involves taking each time step $t$, i.e. $(x_{1t},...,x_{nt})$, and encoding this batch of sequences into a hidden state $h_t$ by using the previous hidden state information. Then, we can find the output by decoding the hidden state i.e. $(\hat{x}_{1t},...,\hat{x}_{nt})$.

0. Take a batch of $n$ sequences over some time span $T$. $v_t = (x_1,...,x_n)_t$. So $v_t$ is a batch of $n$ sequences at time $t$, each one is $d$-dimensional. $v_t \in \mathbf{R}^{n\times 1 \times d}$, and $(x_1,...,x_n)_{t=1}^T \mathbf{R}^{n\times T\times d}$
1. Initialize hidden state vector $h_0 \in \mathbf{R}^{n \times 1 \times d}$, and its update matrix $W_h \in \mathbf{R}^{d \times d}$ and its bias vector $b_h ^{n\times 1 \times d}$. Furthermore, we initialize $W_I^{d\times d}$ which maps the input state into the hidden state, and $W_O^{d \times n}$ which maps the hidden state to an ouput state $(\hat{x},\hat{y},\hat{z})_t \in \mathbf{R}^{n\times 1 \times d}$, with a bias term.
2. For each time step, update the hidden state like so:
    - $h_{t+1} \leftarrow W_I v_{t+1} + W_h h_t + h_b$
    - $h_{t+1} \leftarrow \sigma(h_{t+1})$ element-wise
3. Output is calculated as $\hat{v}_{t+1} = W_O h_{t+1} + b_O$
4. Repeat these steps until sequence is exhausted, and collect $(x,y,z)_{i=1}^T$ and outputs $(\hat{x},\hat{y},\hat{z})_{i=1}^T$. 



### Backward Propogration

Backpropogration can become costly here, so we introduce the notion of backpropogration through time BPTT. Note that the only thing being updated in each step is the sequence of $h_t$'s. BPTT. So we can take the loss of the outputs and inputs and calculate the gradients of the loss with respect to the $h_t$. We can also do one single calculation for each of $W_h$, $W_I$ and $W_O$. Then we can update these parameters for each backpropogration step.

Consider a loss $l(\hat{v}_i^T , v_i^T)$. In backpropogration, we are concerned first with $\nabla_{\hat{v}} l$. Now:

- $\nabla_{W_O} l = \nabla_{\hat{v}}l \times \nabla_{W_O}\hat{v} = \nabla_{\hat{v}}l \times h_{\text{final hidden layer}}$

- $\nabla_{h_{\text{final hidden layer}}} l = \nabla_{\hat{v}}l \times \nabla_{h_{\text{final hidden layer}}}\hat{v} = \nabla_{\hat{v}}l \times W_O$

- $\nabla_{h_{t}} l = \nabla_{\hat{v}}l \times W_O \times \sum_{k=\text{last layer}-1}^t \nabla_{h_{k}} h_{t+1} = \sum_{k=\text{last layer}-1}^t W_h \times \nabla_{\hat{v}}l \times W_O$

You can use the chain rule to see how $\nabla_W l$ are all evaluated. 

Notice how as the grad of $h_t$ gets closer to $t=0$, the sum becomes large. This can lead to gradient explosion, and as we have a lot of multiplication terms, we may even see gradient vanishing. For exploding gradients, we can use gradient clipping, or using truncation of the sum above so we do not use the whole history.

### Short-comings of RNNs

The major fallback of RNNs is that they are unable to capture long range dependencies. This is important in language modeling in particular, for example if we begin a long paragraph with some facts, the RNN typically will forget this by the end of the paragraph. 

Furthermore, due to exploding gradients, we can use truncation. This makes long range dependencies issues especially pronounced since we are no longer updating based on sufficiently early errors.

Whilfe gradient clipping can help with exploding gradients, it is not clear how to handle vanishing gradients.

## LSTM

To handle the issue of vanishing gradients and to capture long range dependencies, the LSTM was developed. This was an improvement on the RNN. At the very high level, it included objects called "gated units" that can decided to forget or update a certain input module. LSTMs replaced a recurrent layer, i.e. module generating the hidden layers with module called a memory cell. Each memory cell has an internal state designed to allow gradients to pass through without vanishing or exploding.

### Memory Cells

Each memory cell contains a gate, that is trained to allow inputs to affect its internal state. The memory cell then pushes information to an output gate.

The memory cell must allow the input through an input node first. The input node value is calculated as follows:

- $C_t = \tanh(X_tW_{\text{in}} + H_{t-1}W_{H\text{in}} + b_\text{in})$

Here $W_{\text{in}} \in \mathbf{R}^{d\times h}$ and $H_{t-1} \in \mathbf{R}^{h\times h}$ and $W_{H\text{in}} \in \mathbf{R}^{h\times h}$ and the bias is $\mathbf{R}^{h\times 1}$. This $C_t$ specifies the value of the input node which is then used in computations along with the input, forget, and output gate.

- Input gate: how much of the input at the given should be added to the current memory cell value

- Forget gate: whether the input at the given should trigger a cell to foget its cell value or maintain it

- Output gate: whether the input at the given time affect the output

These gates take on values in the unit interval and are activate via activation functions.

- Input gate: $I_t = \sigma(X_tW_{I} + H_{t-1}W_{HI} + b_I)$
- Forget gate: $G_t = \sigma(X_tW_{F} + H_{t-1}W_{FI} + b_F)$
- Output gate: $O_t = \sigma(X_tW_{O} + H_{t-1}W_{OI} + b_O)$

So for input data $X \in \mathbf{R}^{n\times 1 \times d}$ and a hidden state of size $H\in \mathbf{R}^{n\times h}$, $W_I, W_F, W_O \in \mathbf{R}^{d \times h}$, $W_{HI}, W_{HF}, W_{HF} \in \mathbf{R}^{h \times h}$, and $b_I \in \mathcal{h\times 1}$, we get $I_t, G_t, I_t \in \mathbf{R}^{h\times 1}$


### Memory Cells in Action

To actually leverage all of these parameters, we can update the memory gate cell as follows:

$C_{t+1} = F_t \times C_{t-1} + I_t \times C_{t}$ where these are elementwise products.

This allows the state of each memory cell to be modified depending on:

1. How much of the old memory cell state $C_{t-1}$ be retained $F_t$ into the next cell state
2. How much of the current cell state $C_t$ be retained $I_t$ into the the next cell state

Another parameter that should be updated is the hidden state $H$. 

$H_{t+1} = O_t \times \tanh(C_t)$

This allows for the hidden state to be updated by

1. How much of the current cell state should $C_t$ contribute to the next hidden state $H_{t+1}$.

That is, the output gate determines how much to update the hidden state using the cell memory. As time step inputs are sequentially passed through, the memory gate cell is updated and the hidden states are updated. The rest of the LSTM will procede in the same way as the RNN.

This architecture alleviates the gradient vanishing problem since it can learn to forget and preturb or leave its states untouched depending on input. As such, we are able to train over longer sequences and learn long range dependencies.
