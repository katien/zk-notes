---
title: Fast Evaluation of Lagrange Interpolating Polynomials
date: "2024-08-08T06:26:45.934Z"
description: "Optimization that reduces evaluation of a low degree extension at a field element from $O(n^2)$ to $O(n)$ time"
---
# Naive Evaluation
- Given a vector $m \in \mathbb{F}_p^n$ with univariate low degree extension $q_m$, evaluating $q_m(r)$ for $r \in \mathbb{F}_p$ naively would require $O(n^2)$ time.
- The interpolating polynomial $q_m$ has n terms
  $$
  q_m(r) = \sum^{n-1}_{0}m_{i+1}\cdot L_i(r)
  $$
  but each Lagrange basis polynomial $L_i(r)$ has another $n-1$ terms
  $$
  L_i(r) = \prod^{n-1}_{j=0;j\neq i} \frac{x-x_j}{x_i-x_j}
  $$
- To compute $q_m(r)$ according to these definitions, we would have $O(n\cdot(n-1)) \approx O(n^2)$ operations to perform
# Linear Time Shortcut
- We can eliminate most of the work associated with computing each Lagrange basis polynomial by computing the basis polynomials recursively
- We rely on the fact that since the nodes over which our Lagrange basis is defined are the same as their indices ($x_i = i$), the $i$th Lagrange basis polynomial can be expressed as a function of the $i-1$th basis polynomial which can be computed in constant time.
- We begin by computing the first basis polynomial $L_0$ according to its definition which still takes $O(n)$ time
- Then for any $i>0$, for an interpolating set where all nodes $x_i = i \in \{0,\dots,n-1\}$, we can compute in constant time

$$
L_i(r) = \frac{L_{i-1}(r) \cdot (r-(i-1))\cdot(-(n-i))}{(r-i)\cdot i}
$$
- Since the Lagrange basis polynomials are a constant time calculation with this algorithm, the full evaluation of $q_m(r)$ is a sum of $n$ constant time terms, so the runtime is reduces from $O(n^2)$ to O(n).

