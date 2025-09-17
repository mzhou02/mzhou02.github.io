---
layout: page
title: Szemerédi's Regularity Lemma
description: 
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

One of the guiding principles in extremal combinatorics is that large, complicated objects often admit useful approximate descriptions. For graphs, a natural approach is to partition the vertex set into a moderate number of pieces and ask whether the edge distribution between these pieces has some uniformity. 

The Regularity Lemma asserts that this can always be done: every large graph can be approximated, at a coarse scale, by a partition in which almost all pairs of vertex classes behave in a pseudorandom fashion. That is, the edge density between two classes is essentially stable, in the sense that it does not fluctuate significantly when one restricts to large subsets. This allows one to replace the original graph by a simpler "reduced graph'' on the partition classes, losing only a small amount of information. 

<h3>Basic Definitions</h3>

**Edge density.** Let $$G=(V,E)$$ be a graph and let $$A,B \subseteq V$$ be disjoint nonempty sets.  
The *edge density* between $$A$$ and $$B$$ is defined as  

$$d(A,B) := \frac{e(A,B)}{|A|\,|B|},$$  

where $$e(A,B)$$ is the number of edges with one endpoint in $$$A$$ and one in $$B$$.

Now, how should we define what it means for the edges between two vertex sets to look random? A natural way to define randomness between two vertex sets is to ask whether their edges are distributed approximately uniformly.  To capture this, we require that no large subset of either side reveals a very different density of edges than the whole pair.  

**$$\varepsilon$$-regular pair.** Given $$\varepsilon > 0$$, a pair $$(A,B)$$ of disjoint vertex sets is said to be $$\varepsilon$$-regular if for all subsets $$X \subseteq A$$ and $$Y \subseteq B$$ with $$|X| \geq \varepsilon |A|$$ and $$|Y| \geq \varepsilon |B|$$$, one has  

$$\bigl|\, d(X,Y) - d(A,B) \,\bigr| \leq \varepsilon.$$

In other words, the density between $$A$$ and $$B$$ is essentially uniform across all large enough sub-pairs.

We now define what it means for a partition to be random-like.

**$$\varepsilon$$-regular partition.** For $$\varepsilon > 0$$, a partition of the vertex set  

$$V = V_0 \cup V_1 \cup \cdots \cup V_k$$

is called an $$\varepsilon$$-regular partition if:  

1. $$\#V_0\# \leq \varepsilon \#V\#$$ (an exceptional set of negligible size) 
2. $$\#V_1\# = \#V_2\# = \cdots = \#V_k\#$$ (the non-exceptional classes are balanced)
3. all but at most $$\varepsilon k^2$$ of the pairs $$(V_i,V_j)$$ with $$1 \leq i < j \leq k$$ are $$\varepsilon$$-regular.  

Having defined $$\varepsilon$$-regular partitions, a natural question arises: how many parts must we allow in order to guarantee that such a partition exists?  Remarkably, Szemerédi proved that the number of parts required depends not on the size of the graph, but only $$\varepsilon$$ (and a chosen lower bound for the number of parts).
This Ramsey-like phenomenon is the content of the Regularity Lemma.  

**Szemerédi’s Regularity Lemma.**  For every $$\varepsilon>0$$ and integer $$m_0 \ge 1$$ there exists $$M=M(\varepsilon,m_0)$$ such that every finite graph $$G=(V,E)$$ admits a partition

$$V \;=\; V_0 \cup V_1 \cup \cdots \cup V_k, \qquad m_0 \le k \le M,$$

with

$$|V_0| \le \varepsilon |V| \quad\text{and}\quad |V_1|=\cdots=|V_k|,$$

for which all but at most $$\varepsilon k^2$$ of the pairs $$(V_i,V_j)$$, $$1\le i<j\le k$$$, are $$\varepsilon$$-regular.

<h3>Proof</h3>
At first sight, the Regularity Lemma looks like a forbiddingly difficult statement to prove: one has to somehow enforce uniform edge distributions across almost all pairs in a partition of arbitrary size. The key insight is the so-called **energy** increment argument*. One defines a simple quantitative measure of how well a partition captures the structure of a graph—often called the *energy* of the partition—by averaging squared edge densities across the pairs. If a partition is not yet $$\varepsilon$$-regular, then one can refine it in such a way that this energy strictly increases by a definite amount. Since the energy is bounded above, this process cannot continue indefinitely, and so after finitely many steps one arrives at an $\varepsilon$-regular partition. The elegance of this method lies in turning what seems an intractable global requirement into the repeated local act of improving a potential function.  

**Definition (Energy of a pair).**  Let $$U, W \subseteq V$$ with $$\#V\# = n$$. The energy of the pair $$(U,W)$$ is defined by  

$$q(U,W) := \frac{|U|\,|W|}{n^{2}} \, d(U,W)^{2}.$$

For partitions  $$\mathcal{P}_U = \{ U_1, \ldots, U_k \}$$ of $$U$$ and $$\mathcal{P}_W = \{ W_1, \ldots, W_\ell \}$$ of $$W$$, we define the energy to be the sum of the energies between each pair of parts:  

$$q(\mathcal{P}_U, \mathcal{P}_W) := \sum_{i=1}^{k} \sum_{j=1}^{\ell} q(U_i, W_j).$$

Finally, for a partition  $$\mathcal{P} = \{ V_1, \ldots, V_k \}$$ of $$V$$, define the energy of $$\mathcal{P}$$ to be $$q(\mathcal{P}) := q(\mathcal{P}, \mathcal{P}).$$

Specifically,  

$$q(\mathcal{P}) = \sum_{i=1}^{k} \sum_{j=1}^{k} q(V_i, V_j) = \sum_{i=1}^{k} \sum_{j=1}^{k} \frac{|V_i|\,|V_j|}{n^{2}} \, d(V_i, V_j)^{2}.$$

Note that energy is always between $$0$$ and $$1$$, since edge density is at most $1$:  

$$q(\mathcal{P}) = \sum_{i=1}^{k} \sum_{j=1}^{k} \frac{|V_i|\,|V_j|}{n^{2}} \, d(V_i, V_j)^{2} \;\;\leq\;\; \sum_{i=1}^{k} \sum_{j=1}^{k} \frac{|V_i|\,|V_j|}{n^{2}} = 1.$$

To be continued.