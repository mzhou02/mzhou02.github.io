---
layout: post
title: Almost all numbers $$x$$ have log(log$$(x)$$)) prime factors
date: 2024-10-21 13:57:00
description:
tags: combinatorics
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

<b>Theorem (Ramanujan).</b> Let $$\phi$$ be any function that grows arbitrarily slowly to infinity. For large enough $$N$$, we have that $$| \{x \leq N : |\omega(x) - \phi(x)\sqrt{\text{log}(\text{log}(x))} | > \text{log}(\text{log}(x))  \}| = o(n)$$