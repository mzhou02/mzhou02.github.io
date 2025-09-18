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
  window.dataLayer = window.dataLayer ||  [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-0823RLC0T3');
</script>

One of the guiding principles in extremal combinatorics is that large, complicated objects often admit useful approximate descriptions. For graphs, a natural approach is to partition the vertex set into a moderate number of pieces and ask whether the edge distribution between these pieces has some uniformity. 

The Regularity Lemma asserts that this can always be done: every large graph can be approximated, at a coarse scale, by a partition in which almost all pairs of vertex classes behave in a pseudorandom fashion. That is, the edge density between two classes is essentially stable, in the sense that it does not fluctuate significantly when one restricts to large subsets. This allows one to replace the original graph with a bounded “reduced graph,” where the vertices correspond to classes and the edges correspond to regular pairs of sufficient density.

From this point of view, many difficult problems about embedding or counting subgraphs are reduced to finite, manageable questions on the reduced graph. Once the graph is approximated by a partition in which most pairs look uniform, one can analyze its global behaviour using only this coarse description. Uniformity ensures that the edge counts between large subsets align closely with what the densities predict, so patterns identified in the reduced graph can be faithfully realized in the original graph. This strategy—simplify, analyze at a reduced scale, and then lift back—has become one of the central tools of modern combinatorics, allowing local randomness to be systematically converted into global structure.

<h3>Basic Definitions</h3>

Now, how should we define what it means for the edges between two vertex sets to look random? We begin by defining what edge density means. A natural way to define randomness between two vertex sets is to ask whether their edges are distributed approximately uniformly. To capture this, we require that no **large** subset of either side reveals a very different density of edges than the whole pair.

<div class="cv">
  <div class="card mt-3 p-3">
    <h3 class="card-title font-weight-medium">Edge Density</h3>
    <div class="card-text">
      <p>Let \(G=(V,E)\) be a graph and let \(A,B \subseteq V\) be disjoint nonempty sets. The <em>edge density</em> between \(A\) and \(B\) is defined as</p>
      
      <p>$$d(A,B) := \frac{e(A,B)}{\vert A\vert \,\vert B\vert },$$</p>
      
      <p>where \(e(A,B)\) is the number of edges with one endpoint in \(A\) and one in \(B\).</p>
    </div>
  </div>
  
  <div class="card mt-3 p-3">
    <h3 class="card-title font-weight-medium">\(\varepsilon\)-Regular Pair</h3>
    <div class="card-text">
      <p>Given \(\varepsilon > 0\), a pair \((A,B)\) of disjoint vertex sets is said to be \(\varepsilon\)-regular if for all subsets \(X \subseteq A\) and \(Y \subseteq B\) with \(\vert X\vert \geq \varepsilon \vert A\vert\) and \(\vert Y\vert \geq \varepsilon \vert B\vert\), one has</p>
      
      <p>$$\bigl\vert \, d(X,Y) - d(A,B) \,\bigr\vert \leq \varepsilon.$$</p>
      
      <p>In other words, the density between \(A\) and \(B\) is essentially uniform across all large enough sub-pairs.</p>
    </div>
  </div>
</div>

We now define what it means for a partition to be random-like.

<br>
<div class="card mt-3 p-3">
  <h3 class="card-title font-weight-medium">\(\varepsilon\)-Regular Partition</h3>
  <div class="card-text">
    <p>A partition of \(V\) into \(k\) classes</p>
    
    <p>$$\mathcal{P} = \{ V_1, V_2, \ldots, V_k \}$$</p>
    
    <p>is called an \(\varepsilon\)-regular partition if</p>
    
    <p>$$\sum_{\substack{(V_i,V_j) \ \text{not } \varepsilon\text{-regular}}} |V_i|\,|V_j| \;\;\leq\;\; \varepsilon \, |V|^2.$$</p>
  </div>
</div>
<br>

<figure style="max-width: 90%; margin: 0; text-align: center;">
  <img src="/assets/img/epsilon_regular_partition.png" 
       alt="Epsilon Regular Partition" 
       style="max-width: 100%; height: auto; display: block; margin: 0 auto;">
</figure>

We have previously mentioned how every graph has a partition in which almost all pairs of vertex classes behave in a pseudorandom fashion. Having introduced the notion of an $\varepsilon$-regular partition, one might naturally ask how large the partition must be, as a function of the graph size and $\varepsilon$. One of the most striking and beautiful features of combinatorics is that the answer depends only on $\varepsilon$, and is in fact **independent** of the size of the graph. This Ramsey-like phenomenon is the content of Szemerédi’s Regularity Lemma.

<br>
<div class="card mt-3 p-3">
  <h3 class="card-title font-weight-medium">Szemerédi's Regularity Lemma</h3>
  <div class="card-text">
    <p>For every \(\varepsilon > 0\) and every positive integer \(m\), there exists an integer \(M = M(\varepsilon, m)\) such that if \(G\) is a graph with at least \(M\) vertices, then there exists an integer \(k\) with \(m \leq k \leq M\) and an \(\varepsilon\)-regular partition of the vertex set of \(G\) into \(k\) classes.</p>
  </div>
</div>  
<br>

<h3>Proof</h3>
At first sight, the Regularity Lemma looks like a forbiddingly difficult statement to prove: one has to somehow enforce uniform edge distributions across almost all pairs in a partition of arbitrary size. The key insight is the so-called **energy increment argument**. One defines a simple quantitative measure of how well a partition captures the structure of a graph—often called the *energy* of the partition—by averaging squared edge densities across the pairs. If a partition is not yet $$\varepsilon$$-regular, then one can refine it in such a way that this energy strictly increases by a definite amount. Since the energy is bounded above, this process cannot continue indefinitely, and so after finitely many steps one arrives at an $\varepsilon$-regular partition. The elegance of this method lies in turning what seems an intractable global requirement into the repeated local act of improving a potential function.  

**Definition (Energy of a pair).**  Let $$U, W \subseteq V$$ with $$\vert V\vert  = n$$. The energy of the pair $$(U,W)$$ is defined by  

$$q(U,W) := \frac{\vert U\vert \,\vert W\vert }{n^{2}} \, d(U,W)^{2}.$$

For partitions  $$\mathcal{P}_U = \{ U_1, \ldots, U_k \}$$ of $$U$$ and $$\mathcal{P}_W = \{ W_1, \ldots, W_\ell \}$$ of $$W$$, we define the energy to be the sum of the energies between each pair of parts:  

$$q(\mathcal{P}_U, \mathcal{P}_W) := \sum_{i=1}^{k} \sum_{j=1}^{\ell} q(U_i, W_j).$$

Finally, for a partition  $$\mathcal{P} = \{ V_1, \ldots, V_k \}$$ of $$V$$, define the energy of $$\mathcal{P}$$ to be $$q(\mathcal{P}) := q(\mathcal{P}, \mathcal{P}).$$

Specifically,  

$$q(\mathcal{P}) = \sum_{i=1}^{k} \sum_{j=1}^{k} q(V_i, V_j) = \sum_{i=1}^{k} \sum_{j=1}^{k} \frac{\vert V_i\vert \,\vert V_j\vert }{n^{2}} \, d(V_i, V_j)^{2}.$$

Note that energy is always between $$0$$ and $$1$$, since edge density is at most $1$:  

$$q(\mathcal{P}) = \sum_{i=1}^{k} \sum_{j=1}^{k} \frac{\vert V_i\vert \,\vert V_j\vert }{n^{2}} \, d(V_i, V_j)^{2} \;\;\leq\;\; \sum_{i=1}^{k} \sum_{j=1}^{k} \frac{\vert V_i\vert \,\vert V_j\vert }{n^{2}} = 1.$$

To be continued...