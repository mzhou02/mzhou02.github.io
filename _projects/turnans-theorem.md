---
layout: page
title: Turan's Theorem
description: This article is still in the works
img: 
importance: 1
category: combinatorics
mathjax: true
---
Turan's theorem states that a graph without $$K_{r + 1}$$ on $$n$$ vertices can have no more than $$T(n, r) = \left(1 - \frac{1}{r}\right)\frac{n^2}{2}$$ number of edges. Here, we present some proofs and applications.

<h3> Proof #1: Induction </h3>
We begin with a base case. If $$n \leq r$$, we have that we can have we can have a complete graph. Since

$$n^2r - n^2 \geq n^2r - nr$$
$$\left(r - 1\right)\frac{n^2}{2} \geq \binom{n}{2}r$$
$$\left(1 - \frac{1}{r} \right)\frac{n^2}{2} \geq \binom{n}{2}$$

and we are done.

We continue to our inductive case. Since this graph has a maximal number of edges and no $$r + 1$$ clique, there must exist an $$r$$-clique. Let us take this $$r$$-clique out. There are exactly $$\binom{r}{2}$$ edges in this $$r$$-clique, and at most $$(r - 1)(n - r)$$ edges between the $$r$$-clique and the rest of the graph. In addition, since the rest of the graph doesn't have an $$r$$-clique either, it has at most $$T(n - r, r)$$ edges, meaning that the number of edges is upper bounded by:
$$\frac{r(r - 1)}{2} + (r - 1)(n - r) + \left(1 - \frac{1}{r}\right)\frac{(n - r)^2}{2} = \left(1 - \frac{1}{r}\right)\frac{n^2}{2}$$

<h3> Proof #2: Maximizing Lagrangian </h3>
This one is perhaps my favorite proof of the theorem. Given a graph $$G$$ with no $$K_{r + 1}$$ subgraph but maximum number of edges, let us assign weights to each vertex such that $$\sum_{v \in V} w(v) = 1$$, and assign weights to edges as the product of the weights of its vertices. This sum of edge weights is kown as the Lagrangian of the graph. We can obviously assign uniform weights, so that each edge has weight $$\frac{1}{n^2}$$. In this case, the Lagrangian is equal to $$\frac{T(n, r)}{n^2}$$. To upper bound $$T(n, r)$$, it thus then suffices to find an upper bound on the Lagrangian of this graph and multiply by $$n^2$$.

Notice that if we have two non adjacent vertices $$v_i$$ and $$v_j$$ of positive weight with $$s_u$$ and $$s_j$$ as the sum of the weights of their respective neighbors, we have a procedure that can increase the Lagrangian of the graph if one of these vertices has weight 0 and the other has weight $$w(v_i) + w(v_j)$$. Indeed, if $$s_i > s_j$$, we can shift all the weight from $$v_j$$ to $$v_i$$ to have a change in the Lagrangian equal to $$s_i(w(v_i) + w(v_j)) - (s_iw(v_i) + s_jw(v_j))  = v_j(s_i - s_j) > 0$$. We can keep shifting all the weights like this until there exists no non-adjacent vertices of positive weight. In this case, all edges with non-zero weights lie on a clique.

Let this clique be of size $$k$$. If we have vertices $$v_i$$, $$v_j$$ such that $$w(v_i) > w(v_j)$$, let us select $$\epsilon < w(v_i) - w(v_j)$$. If we shift $$\epsilon$$ weight from $$v_i$$ to $$v_j$$, we have that the change of the Lagrangian is equal to $$\epsilon(w(v_i) - w(v_j)) - \epsilon^2 > 0$$. Thus, we can keep shifting weight like this, where the maximum is obtainable when all weights are equal. Since we have $$\frac{k(k - 1)}{2}$$ edges, we have that the maximum weight of the graph is equal to $$\frac{k - 1}{2k} = (1 - \frac{1}{k})\frac{1}{2}$$.

This function is monotonically decreaing, meaning that it would be optimal to put weights on the largest clique in the graph, which is of size $$r$$. Thus, the Lagrangian of the graph is equal to $$(1 - \frac{1}{r})\frac{1}{2}$$, and we have that $$T(n, r) \leq \left(1 - \frac{1}{r}\right)\frac{n^2}{2}$$.