---
layout: post
title: Hardy-Ramanujan with probablistic method
date: 2024-10-21 13:57:00
description:
tags: combinatorics, number theory
categories: math
featured: false
---
<script async src="https://www.googletagmanager.com/gtag/js?id=G-0823RLC0T3"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-0823RLC0T3');
</script>

One of my favorite applications of the probablistic method is perhaps the Hardy-Ramanujan theorem in number theory, which roughly states that almost all numbers $$x$$ have about less than $$\text{log}(\text{log}(x))$$ number of prime divisors, $$\omega(x)$$.

<b>Theorem (Ramanujan).</b> Let $$\phi$$ be any function that grows arbitrarily slowly to infinity. For large enough $$N$$, we have that

$$ \vert  \{x \leq N : \vert \omega(x) - \text{log}(\text{log}(x))  \vert  > \phi(x)\sqrt{\text{log}(\text{log}(x))  \} \vert  = o(N)$$ 

To prove this statement, it suffices to show that $$P(  \vert \omega(x) - \text{log}(\text{log}(x)) \vert  > \phi(x)\sqrt{\text{log}(\text{log}(x) )}) = o(1)$$. This looks like an application of Chebyshev's inequality.

To begin, let $$X$$ be the random variable equal denoting the number of prime divisors a randomly selected $$x \leq N$$ has that are each less than $$N^{\frac{1}{3}}$$ (notice that $$x$$ can only have at most 3 prime factors larger than $$N^{\frac{1}{3}}$$, so $$X$$ will only be off by a constant additive factor). Since the probability that a prime $$p$$ divides $$x$$ is $$\frac{1}{p} + O\left(\frac{1}{N}\right)$$, we have that

$$\textbf{E}[X] = \sum_{p \leq N^{\frac{1}{3}}} \frac{1}{p} + O\left(\frac{1}{N}\right)$$

by simply letting $$X$$ be the sum of indicator variables representing whether a prime divides $$x$$ or not. Since each indicator variable is a Bernoulli random variable, their individual variances are $$\frac{1}{p} - \frac{1}{p^2}$$, meaning that

$$\textbf{Var}(X) = \left(\sum_{p \leq N^{\frac{1}{3}}} \frac{1}{p} - \frac{1}{p^2} \right) - 2\left(\sum_{p < q \leq N^{\frac{1}{3}}} \textbf{E}[(\mathbb{I}(p \mid x))(\mathbb{I}(q \mid x))] - \textbf{E}[(\mathbb{I}(p \mid x))]\textbf{E}[(\mathbb{I}(q \mid x))] \right)$$

Notice that $$(\mathbb{I}(p \mid x))(\mathbb{I}(q \mid x)) = 1$$ if and only if both $$p$$ and $$q$$ divide $$x$$. This is the same as if $$pq$$ divides $$x$$, which has probability $$\frac{1}{pq} + O\left(\frac{1}{N}\right)$$. Thus,

$$\textbf{E}[(\mathbb{I}(p \mid x))(\mathbb{I}(q \mid x))] - \textbf{E}[(\mathbb{I}(p \mid x))]\textbf{E}[(\mathbb{I}(q \mid x))] $$
$$= \frac{1}{pq} + O\left(\frac{1}{N}\right) - \left(\frac{1}{p} + O\left(\frac{1}{N}\right)\right) \left(\frac{1}{q} + O\left(\frac{1}{N}\right) \right) = O\left(\frac{1}{N}\right)$$

Thus,

$$\textbf{Var}(X) = \left(\sum_{p \leq N^{\frac{1}{3}}} \frac{1}{p} - \frac{1}{p^2} \right) + 2\left(\sum_{p < q \leq N^{\frac{1}{3}}} \frac{1}{N}\right)$$

Using Merten estimates and the fact that the sum of reciprical of squares converges, we get that

$$\textbf{E}[X] = \text{log}(\text{log}(N)) + O(1)$$
$$\textbf{Var}(X) = \text{log}(\text{log}(N)) + O(1)$$

and then by Chebyshev's inequality, we have that

$$P(  \vert \omega(x) - \text{log}(\text{log}(x)) \vert  > \phi(x)\sqrt{\text{log}(\text{log}(x) )}) \ll \frac{\text{log}(\text{log}(N))}{\phi(x)^2\text{log}(\text{log}(x)} = o(1)$$

and we are done.