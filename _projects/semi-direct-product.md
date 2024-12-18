---
layout: page
title: Semidirect Product
description: An abstraction of the direct product
img: 
importance: 1
category: algebra
mathjax: true
---
<script async src="https://www.googletagmanager.com/gtag/js?id=G-0823RLC0T3"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-0823RLC0T3');
</script>

<h3> A Less "Direct" Product </h3>
Let $$G$$ be the Cartesian product of two groups $$N$$ and $$H$$. We can define a group relation on $$G = N \times H$$ pointwise using the operations from $$N$$ and $$H$$. In other words, $$(n_1, h_1)(n_2, h_2) = (n_1n_2, h_1h_2)$$. Because the group operations of $$N$$ and $$H$$ are both associative and closed, this operation is also associative and closed. There exists an identity, namely $$(0, 0)$$, and every element has an inverse. Thus, this "Cartesian" product also has a group structure when defined pointwise. We call this the direct product.

A natural question that arises is when a group be expressed as a direct product of subgroups. To investigate, let $$N \times H$$ be the direct product of groups $$N$$ and $$H$$. Notice how for $$(n, h) \in N \times H$$ and $$n' \in N$$, $$(n, h)(n', 0)(n^{-1}, h^{-1}) = (nn'n^{-1}, 0) \in  N$$. Thus, $$N \trianglelefteq N \times H$$. Similarly, $$H \trianglelefteq N \times H$$. In other words, if $$G$$ can be written as a direct product $$N \times H$$ for subgroups $$N$$ and $$H$$, both subgroups must be normal.

These are quite substantial demands for non-abelian groups. If we wanted to construct groups that contain $$N$$ and $$H$$ as subgroups, our scope is quite limited with just the direct product. Naturally, we would like to "relax" this requirement by only requiring one subgroup to be normal. We wouldn't be able to write $$G$$ as a direct product of two of its subgroups in this case. But let us see how far we can go down this route.

Let $$N$$ be normal in $$G$$, and $$H$$ be a subgroup of $$G$$ such that $$N \cap H = \{0\}$$ and $$NH$$ (defined to be $$\{nh \mid n \in N, h\in H \}$$) equals $$G$$. Since the intersection is only the identity, we can write each element of $$g$$ uniquely as the product $$nh$$ for $$n \in N$$ and $$h \in H$$ (proof left as an exercise to the reader). Given $$n_1, n_2 \in N$$ and $$h_1, h_2 \in H$$, we have that

$$(n_1h_1)(n_2h_2) = n_1(h_1n_2h_1^{-1})h_1h_2 = n_3h_3$$

where $$n_3 = n_1(h_1n_2h_1^{-1})$$ and $$h_3 = h_1h_2$$ (note normality of $$N$$, so that $$h_1n_2h_1^{-1} \in N$$). In a way, $$G$$ is sort of a "product" between these subgroups. If we were to define its elements of $$G$$ as ordered pairs of elements from $$N$$ and $$H$$, our "group operation" would be that $$(n_1, h_1)(n_2, h_2) = (n_1(h_1n_2h_1^{-1}), h_1h_2)$$ (you can verify that this operation follows group axioms). This is called the <b> internal semidirect product </b>, denoted as $$N \rtimes H$$.

<h3> An External Product </h3>
We can draw inspiration from this example of $$G$$ to construct something called the <b> external semidirect product </b>. Rather than rewriting $$G$$ as an internal semidirect product, why not see what class of groups we can construct from $$N$$ and $$H$$?

The key idea from our example is that because $$N$$ is normal, $$H$$ acts upon $$N$$ by conjugation, which is a map in the automorphism group of $$N$$. This group action induces a homomorphism $$\phi: H \rightarrow \text{Aut}(N)$$ (as given $$h_1, h_2 \in H$$ and $$n \in N$$, conjugating $$n$$ by $$h_2$$ then by $$h_1$$ yields $$h_1h_2nh_2^{-1}h_1^{-1}$$, which is the same as conjugating by $$h_1h_2$$), which defines a proper group operation for our ordered pairs. Thus, the group operation in this "product" for $$G$$ is based mainly upon the automorphism that $$H$$ induces upon $$N$$, as $$(n_1, h_1)(n_2, h_2) = (n_1 \phi_{h_1}(n_2), h_1h_2)$$.

So given groups $$N$$ and $$H$$ and a homomorphism $$\phi: H \rightarrow \text{Aut}(N)$$, we can construct a group $$G$$ which is the semidirect product of these groups as follows: let $$\cdot$$ be the left action of $$H$$ on $$N$$ determined by $$\phi$$. $$G$$ will be the set of ordered pairs $$(n, h)$$ for $$n \in N$$ and $$h \in H$$ with the following group operation:

$$(n_1, h_1)(n_2, h_2) = (n_1 (h_1 \cdot n_2), h_1h_2)$$

We denote this as $$N \rtimes_{\phi} H$$.

<b> Example. </b> Let $$p$$ and $$q$$ be distinct primes. Classify all groups of order $$pq$$.

Let $$G$$ be an arbitrary group of order $$pq$$. Without the loss of generality, let $$q < p$$. By Cauchy's theorem, there exists a subgroup of order $$p$$, denote this as $$P$$, and a subgroup of order $$q$$, denote this as $$Q$$. Because $$p$$ and $$q$$ are distinct primes, $$q \nmid p$$, meaning that, by Lagrange's theorem, there cannot exist a subgroup of order $$q$$ in $$P$$. Thus, $$P \cap Q = \{0\}$$, meaning that $$ \vert PQ \vert  =  \vert P \vert  \vert Q \vert  = pq$$ and thus equals $$G$$. By the Sylow theorems, the number of Sylow $$p$$-subgroups of $$G$$, $$n_p$$, is congruent to $$1 (\text{mod } p)$$. Assume $$n_p > 1$$. Then, the number of elements in $$G$$ is greater than or equal to $$n_p(p - 1) + 1 \geq (p + 1)(p - 1) + 1 = p^2 > pq$$, which is a contradiction. Thus, $$n_p = 1$$, and since all Sylow $$p$$-subgroups are conjugate to each other, $$P$$ is normal in $$G$$, meaning that $$G = P \rtimes Q$$. Since $$P$$ is cyclic, there is a one-to-one correspondance between automorphisms of $$P$$ and mappings from $$g$$ to $$g^k$$, where $$g$$ is a generator of $$P$$ and $$k$$ is a relatively prime number to $$p$$ between $$1$$ and $$p$$. Thus, $$ \vert  \text{Aut}(P) \vert  = p - 1$$.

<b> Case 1: </b> $$q \nmid p - 1$$. Let $$\phi: Q \rightarrow \text{Aut}(P)$$. Then, by the first isomorphism theorem, $$\text{Im}(Q) \cong Q' \leq Q$$. Thus, $$ \vert \text{Im}(Q) \vert  \mid q$$, meaning that $$ \vert \text{Im}(Q) \vert  = 1$$ or $$ \vert \text{Im}(Q) \vert  = q$$. Because it is also a subgroup of $$\text{Aut}(P)$$, $$ \vert \text{Im}(Q) \vert   \mid  \vert \text{Aut}(P) \vert $$, meaning that it is equal to 1. Thus, every $$Q$$ acts on $$P$$ trivially, meaning that $$G = P \times Q$$. Thus, $$G$$ is a cyclic group of order $$pq$$.

<b> Case 2: </b> This is left as an exercise to the reader.

Naturally, one would want to know if every non-simple group is isomorphic to some semidirect product of subgroups. An simple counterexample is $$\mathbb{Z}/4\mathbb{Z}$$, as its only normal subgroup is $$\mathbb{Z}/2\mathbb{Z}$$, meaning that it would have to be the semidirect product of $$\mathbb{Z}/2\mathbb{Z}$$ and $$\mathbb{Z}/2\mathbb{Z}$$ which has elements of order at most 2. Ok, so not all groups are isomorphic to some semidirect product. But is there a condition that is related?

To be continued.