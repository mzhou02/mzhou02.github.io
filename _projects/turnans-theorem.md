---
layout: page
title: Turan's Theorem
description: This article is still in the works
img: 
importance: 1
category: combinatorics
mathjax: true
---
<script async src="https://www.googletagmanager.com/gtag/js?id=G-0823RLC0T3"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-0823RLC0T3');
</script>

Turan's theorem states that a graph without $$K_{r + 1}$$ on $$n$$ vertices cannot have more than the number of edges than the Turan graph on $$n$$ vertices: the complete $$r$$-partite graph $$n$$ vertices such that the size of each partition differs from each other by at most 1 vertex. We denote the Turan graph on $$n$$ vetices without $$K_{r + 1}$$ as $$T(n, r)$$. Here, we present some proofs and applications. The first is that each the edge maximal graph on $$n$$ vertices without $$K_{r + 1}$$ has no more than $$\left(1 - \frac{1}{r}\right)\frac{n^2}{2}$$ number of edges, the second is that $$T(n, r)$$ is edge maximal, and the third that $$T(n, r)$$ is the unique maximal example.

<h3> Proof #1: Maximizing Lagrangian </h3>
This one is perhaps my favorite proof of the theorem. Given a graph $$G$$ with no $$K_{r + 1}$$ subgraph but maximum number of edges, let us assign weights to each vertex such that $$\sum_{v \in V} w(v) = 1$$, and assign weights to edges as the product of the weights of its vertices. This sum of edge weights is kown as the Lagrangian of the graph. We can obviously assign uniform weights, so that each edge has weight $$\frac{1}{n^2}$$. In this case, the Lagrangian is equal to $$\frac{T(n, r)}{n^2}$$. To upper bound $$T(n, r)$$, it thus then suffices to find an upper bound on the Lagrangian of this graph and multiply by $$n^2$$.

Notice that if we have two non adjacent vertices $$v_i$$ and $$v_j$$ of positive weight with $$s_u$$ and $$s_j$$ as the sum of the weights of their respective neighbors, we have a procedure that can increase the Lagrangian of the graph if one of these vertices has weight 0 and the other has weight $$w(v_i) + w(v_j)$$. Indeed, if $$s_i > s_j$$, we can shift all the weight from $$v_j$$ to $$v_i$$ to have a change in the Lagrangian equal to $$s_i(w(v_i) + w(v_j)) - (s_iw(v_i) + s_jw(v_j))  = v_j(s_i - s_j) > 0$$. We can keep shifting all the weights like this until there exists no non-adjacent vertices of positive weight. In this case, all edges with non-zero weights lie on a clique.

Let this clique be of size $$k$$. If we have vertices $$v_i$$, $$v_j$$ such that $$w(v_i) > w(v_j)$$, let us select $$\epsilon < w(v_i) - w(v_j)$$. If we shift $$\epsilon$$ weight from $$v_i$$ to $$v_j$$, we have that the change of the Lagrangian is equal to $$\epsilon(w(v_i) - w(v_j)) - \epsilon^2 > 0$$. Thus, we can keep shifting weight like this, where the maximum is obtainable when all weights are equal. Since we have $$\frac{k(k - 1)}{2}$$ edges, we have that the maximum weight of the graph is equal to $$\frac{k - 1}{2k} = (1 - \frac{1}{k})\frac{1}{2}$$.

This function is monotonically decreaing, meaning that it would be optimal to put weights on the largest clique in the graph, which is of size $$r$$. Thus, the Lagrangian of the graph is equal to $$(1 - \frac{1}{r})\frac{1}{2}$$, and we have that $$T(n, r) \leq \left(1 - \frac{1}{r}\right)\frac{n^2}{2}$$.

<h3> Proof #2: Induction </h3>
We begin with a base case. If $$n \leq r$$, we have that the Turan graph is the complete graph. This is obviously edge maximal and has no $$K_{r + 1}$$. If $$n \geq r + 1$$ and is edge maximal without an $$r + 1$$ clique, there must exist an $$r$$-clique (or else we can always add more edges until we get an $$r$$-clique). Let us take this $$r$$-clique out. First, notice that regardless of how the $$r$$-clique is connected back to the graph on $$n - r$$ vertices, there are at most most $$(r - 1)(n - r)$$ edges in between them. If not, then there exists some vertex that is adjacent to the entire $$r$$ clique, and we have n $$r + 1$$-clique. Notice that the Turan graph on $$T(n, r)$$ edges is made up of an $$r$$ clique connected to $$T(n - r, r)$$ with $$(r - 1)(n - r)$$ number of edges. By the inductive hypothesis, $$T(n - r, r)$$ is edge maximal. Thus, $$T(n, r)$$ is edge maximal.

<h3> Example: Efficient Testing</h3>
<b>Problem 1:</b> You have a flashlight and $$n$$ batteries. The flashlight is powered by 2 batteries, and only works if both of them are functioning. However, only $$k$$ of these batteries work. To check which batteries work, you can conduct a test by putting two batteries in the flashlight and checking if it works. What is the least number of checks you must do to find a pair of batteries that works?

We can make a graph describing our tests by having $$n$$ vertices representing each battery and an edge between batteries for batteries that we tested. Our goal is to just draw an edge in between 2 of the $$k$$-vertices. An optimal testing algorithm cannot output an independent set of size at least $$k$$ or else there will be an adversarial example against your tests; namely, $$k$$ of those vertices will be the functioning batteries. Thus, every optimal algorithm cannot output an independent set of size $$k$$, which means that the optimal algorithm must conduct at least $$\binom{n}{2} - T(n, r) + 1$$ number of tests. Since this number of tests would include no independent set of size $$k$$, all groups of $$k$$ batteries have at least one pair that was tested, meaning at least one pair of working batteries has been tested. Thus, any optimal algorithm will conduct at most $$\binom{n}{2} - T(n, r) + 1$$ number of tests.