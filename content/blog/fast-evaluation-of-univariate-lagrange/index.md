---  
title: Fast Evaluation of Multilinear Lagrange Interpolating Polynomials  
date: "2024-07-13T06:26:45.934Z"  
description: "Streaming based evaluation of the multilinear extension of a function over the v-dimensional hypercube at any field element"
---
- Let $f: \{0,1\}^v \rightarrow \mathbb{F}$ and suppose a verifier has all evaluations $f(x)$ for $x \in \{0,1\}^v$.
  - The domain of $f$ has size $|\{0,1\}^v| = 2^v$  and we'll call the domain size $n$ for easy time/space analysis
- Goal: The verifier aims to evaluate the multilinear extension of $f$, $\tilde{f}(x)$ for any $x \in \mathbb{F}_p$
- Given $f(w)$ for all $w \in \{0,1\}^v$ and a $v$-dimensional vector $r \in \mathbb{F}^v$, the verifier must efficiently evaluate $\tilde{f}(r)$
# Streaming Interactive Proofs
- See the paper [Verifying Computations with Streaming Interactive Proofs](https://arxiv.org/pdf/1109.6882)
  - $O(n\cdot log(n))$ runtime
  - $O(log(n))$ space - $v$ + 1 field elements are stored
- The verifier can compute $\tilde{f}(r)$ in $O(n\cdot log(n))$ time and $O(log(n))$ space by streaming the inputs $f(w)$ for all $w \in \{0,1\}^v$.
  - The order of the inputs does not matter.
- $V$ can calculate the sum
  $$
  \tilde{f}(r) = \sum_{w\in \{0,1\}^v} f(w)\cdot L_w(x_1,\dots,x_v)
  $$
  with a recursive algorithm.
- Begin by initializing the accumulator to 0: $\tilde{f}(r) \leftarrow 0$. Then with each entry, add the next term to the accumulator
  $$
  \tilde{f}(r) \leftarrow \tilde{f}(r) + f(w) \cdot L_w(r)
  $$
- With this approach, the verifier only needs to store the value $r$ and the current value of $\tilde{f}(r)$ as it iterates through values of $w \in \{0,1\}^v$. $r$ is a $v$-dimensional vector of elements of $\mathbb{F}$ and $\tilde{f}(r) \in \mathbb{F}$, so the algorithm's space usage is $v+1$ field elements or $O(v ) = O(log_2(n))$
- The verifier runs through each of the $2^v = n$ values in $w \in \{0,1\}^v$, each time evaluating the $v$-term Lagrange basis polynomial $L_w(r)$
  $$
  L_w(r_1,\dots,r_v) := \prod^{v}_{i=1} r_i\cdot x_i + (1-r_i)(i-x_i)
  $$
  so $2^v$ operations in $O(v)$ are run to compute the full sum, resulting in a runtime of $O(v \cdot 2^v)$ or $O(n \cdot log(n))$