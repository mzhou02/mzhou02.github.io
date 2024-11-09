---
layout: post
title: Hardy-Ramanujan with probabilistic method
date: 2024-10-21 13:57:00
description:
tags: combinatorics number-theory
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

One of my favorite applications of the probabilistic method is perhaps the Hardy-Ramanujan theorem in number theory, which roughly states that almost all numbers $$x$$ have roughly $$\text{log}(\text{log}(x))$$ number of prime divisors, denoted as $$\omega(x)$$, with an error bound that approaches $$O\left( \sqrt{\text{log}(\text{log}(x))} \right)$$ (ie. raise it to a $$1 + \epsilon$$ power). Though complex sounding, we can use simple tools like linearity of expectation to find the expectation and variance of prime divisors; and, if they are close enough, use Chebyshev's inequality to show that almost all cases of this random variable are assymptotically equal to its expectation with only a vanishing number of exceptions, providing a clever and elementary proof to a seemingly difficult number theoretic problem.

<b>Theorem (Ramanujan).</b> Let $$\phi$$ be any function that grows arbitrarily slowly to infinity. For large enough $$N$$, we have that $$ \vert  \{x \leq N :\vert \omega(x) - \text{log}(\text{log}(x))  \vert  > \phi(x)\sqrt{\text{log}(\text{log}(x))} \} \vert  = o(N)$$ 

To prove this statement, it suffices to show that $$\textbf{P}(  \vert \omega(x) - \text{log}(\text{log}(x)) \vert  > \phi(x)\sqrt{\text{log}(\text{log}(x) )}) = o(1)$$. This looks like an application of Chebyshev's inequality.

We can start by defining a random variable $$X$$ denoting the number of prime divisors a randomly selected $$x \leq N$$ has that are each less than $$N^{\frac{1}{3}}$$ (notice that $$x$$ can only have at most 3 prime factors larger than $$N^{\frac{1}{3}}$$, so $$X$$ will only be off by a constant additive factor). For a technical detail we will see later, it will be easier to analyze prime factors less than $$N^{\frac{1}{3}}$$ rather than $$N$$. 

Since the probability that a prime $$p$$ divides $$x$$ is $$\frac{1}{p} + O\left(\frac{1}{N}\right)$$, we have that

$$\textbf{E}[X] = \sum_{p \leq N^{\frac{1}{3}}} \frac{1}{p} + O\left(\frac{1}{N}\right)$$

by simply letting $$X$$ be the sum of indicator variables representing whether a prime divides $$x$$ or not. Since each indicator variable is a Bernoulli random variable, their individual variances are $$\textbf{P}(p \mid x) - \textbf{P}(p \mid x)^2$$, meaning that

$$\textbf{Var}(X) = \left(\sum_{p \leq N^{\frac{1}{3}}} \frac{1}{p} - \frac{1}{p^2} + O\left( \frac{1}{N} \right) \right) - 2\left(\sum_{p < q \leq N^{\frac{1}{3}}} \textbf{E}[(\mathbb{I}(p \mid x))(\mathbb{I}(q \mid x))] - \textbf{E}[(\mathbb{I}(p \mid x))]\textbf{E}[(\mathbb{I}(q \mid x))] \right).$$

Notice that $$(\mathbb{I}(p \mid x))(\mathbb{I}(q \mid x)) = 1$$ if and only if both $$p$$ and $$q$$ divide $$x$$. This is the same as if $$pq$$ divides $$x$$, which has probability $$\frac{1}{pq} + O\left(\frac{1}{N}\right)$$. Thus,
<br>
<br>

$$\textbf{E}[(\mathbb{I}(p \mid x))(\mathbb{I}(q \mid x))] - \textbf{E}[(\mathbb{I}(p \mid x))]\textbf{E}[(\mathbb{I}(q \mid x))] $$

<br>

$$= \frac{1}{pq} + O\left(\frac{1}{N}\right) - \left(\frac{1}{p} + O\left(\frac{1}{N}\right)\right) \left(\frac{1}{q} + O\left(\frac{1}{N}\right) \right) = O\left(\frac{1}{N}\right)$$

and

$$\textbf{Var}(X) = \left(\sum_{p \leq N^{\frac{1}{3}}} \frac{1}{p} - \frac{1}{p^2} + O\left( \frac{1}{N}\right) \right) + 2\left(\sum_{p < q \leq N^{\frac{1}{3}}} O\left( \frac{1}{N}\right)\right).$$

Since we are summing at most $$N^{\frac{2}{3}}$$ number of terms (the technical detail we discussed earlier), we have that each $$O(\frac{1}{N})$$ vanishes. We can then use Merten's estimates and the fact that the sum of reciprical of squares converges to get that

$$\textbf{E}[X] = \text{log}(\text{log}(N)) + O(1)$$

$$\textbf{Var}(X) = \text{log}(\text{log}(N)) + O(1)$$

and then by Chebyshev's inequality and the fact that $$\text{log}(\text{log}(x)) = \text{log}(\text{log}(N)) + O(1)$$ with probability $$1 - o(1)$$, we have that

$$\textbf{P}\left(  \vert \omega(x) - \text{log}(\text{log}(x)) \vert  > \phi(x)\sqrt{\text{log}(\text{log}(x) )}\right) \ll o(1) + \frac{\text{log}(\text{log}(N))}{\phi(x)^2\text{log}(\text{log}(N))} = o(1)$$

meaning that

$$ \vert  \{x \leq N :\vert \omega(x) - \text{log}(\text{log}(x))  \vert  > \phi(x)\sqrt{\text{log}(\text{log}(x))} \} \vert  = o(N).$$

<br>

<h3> Note on Merten Estimates </h3>
Merten's estimates can be derived using elementary techniques. All you really need is that $$\text{log}(n!) = n\text{log}(n) + O(n)$$, as

$$\text{log}(n!) = \sum_{p \leq n} \text{log}(p) \lfloor \frac{n}{p} \rfloor = O(n) + n\sum_{p \leq n} \frac{\text{log}(p)}{p}$$

$$\sum_{p \leq n}\frac{\text{log}(p)}{p} = \text{log}(n) + O(1)$$

Then, using summation by parts on $$\frac{\text{log}(p)}{p}$$ and $$\frac{1}{\text{log}(p)}$$, we have that

$$\sum_{p \leq n} \frac{1}{p} = \text{log}(\text{log}(n)) + O(1)$$