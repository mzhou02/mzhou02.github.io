---
layout: post
title: Diophantine Approximation and Transcendental Numbers  
date: 2025-04-12 22:48:00
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

Many numbers we encounter in everyday life–even those that seem impossibly complex–fall into categories we've known for centuries: rational numbers, which can be written in the form $$p/q$$, and algebraic numbers, which can be written as the root of some rational polynomial. But beyond these well-behaved figures lies a stranger world: the realm of transcendental numbers. 

Transcendental numbers are numbers that cannot be expressed as solutions to polynomial equations with integer coefficients. Up until the mid-19th century, it was unresolved whether any transcendental numbers existed. In fact, it is extremely hard to even prove that a number may even be transcendental. While Liouville had proven the existence of transcendental numbers in 1844, it wasn't until 1873 that Hermite proved $$e$$ is transcendental, followed by Lindemann's proof of $$\pi$$'s transcendence in 1882. Liouville's approach, centered on approximation properties of numbers, provided the first concrete examples of transcendental numbers and established a powerful criterion for transcendence.

To understand the intuition of Louiville's approach, let's first walk through Cantor's proof of the existence of transcendental numbers. To understand why transcendental numbers must exist, we begin with a key concept from set theory: the idea of countable and uncountable sets.

A set is said to be $$\emph{countable}$$ if its elements can be placed into a one-to-one correspondence with the natural numbers $$ \mathbb{N} = \{1, 2, 3, \dots\} $$. That is, we can enumerate the elements in a list: $$ x_1, x_2, x_3, \dots $$. Examples of countable sets include:

- The set of natural numbers $$ \mathbb{N} $$
- The set of integers $$ \mathbb{Z} $$
- The set of rational numbers $$ \mathbb{Q} $$

For example, $$ \mathbb{Q} $$ is in one one-to-one correspondence to $$ \mathbb{N} $$ by "counting" rationals in order by the sum of their numerator and denominator (ie. $$1 / 1$$ goes first, $$1 / 2$$ goes second, $$2 / 1$$ goes third, $$1 / 3$$ goes fourth, $$2 / 2$$ goes fifth, $$3 / 1$$ goes sixth, etc). Similar to the proof that $$ \mathbb{Q} $$ is countable, the set of all integer polynomials is also countable, meaning that the set of all algebraic numbers is countable.

Since all algebraic numbers are countable, it would suffice to show that the reals are uncountable in order to demonstrate the existence of transcendental numbers. To do this, Cantor supposed for contradiction that all real numbers in the interval $$ (0, 1) $$ can be listed:

$$r_1 = 0.a_{11}a_{12}a_{13}\dots \\ r_2 = 0.a_{21}a_{22}a_{23}\dots \\ r_3 = 0.a_{31}a_{32}a_{33}\dots \\ \vdots $$

Now construct a new number $$ r \in (0,1) $$ by altering the diagonal digits:

$$b_i =
\begin{cases}
    1 & \text{if } a_{ii} \neq 1 \\
    2 & \text{if } a_{ii} = 1
\end{cases}
$$

and define

$$r = 0.b_1 b_2 b_3 \dots$$

By construction, $$ r $$ differs from each $$ r_i $$ in at least the $$ i$$-th decimal place. Therefore, $$ r$$ is not in the list, contradicting our assumption that all real numbers in $$ (0,1) $$ were listed.

From this result, we can make a clear observation. While the rational and algebraic numbers are scattered densely across the real line, they form only a countable skeleton. The transcendental numbers, in contrast, fill in the uncountable “gaps” between them, making up almost all of the real number continuum. In essence, because transcendental numbers fill in these gaps between rationals, they can reveal themselves through how well they can be approximated by rationals. Roughly speaking, an irrational numebr that can be approximated by rationals better than any other number must be transcendental.

<h2>Liouville's Insight and Transcendental Numbers</h2>
Liouville’s crucial insight, dating back to 1844, was exactly this: to understand just how well irrational algebraic numbers can be approximated by rationals—and how that imposes deep structural limitations. To demonstrate this, he constructed what is now known as the <b>Liouville constant</b>:

$$L = \sum_{n=1}^{\infty} \frac{1}{10^{n!}} = 0.110001000000000000000001000\ldots$$

This number is famous not only for its curious decimal expansion, but also for being the first number proved to be **transcendental**—that is, not the root of any non-zero polynomial with integer coefficients.

Let’s begin with the key result:

**Theorem 1 (Liouville).** *Let* $$\alpha$$ *be an irrational algebraic number of degree* $$n > 1$$. *Then there exists a positive constant* $$A$$ *such that for all integers* $$p, q$$ *with* $$q > 0$$, we have:  
$$\left|\alpha - \frac{p}{q}\right| > \frac{A}{q^n}$$

In other words, irrational algebraic numbers can’t be too well approximated by rational numbers, and any number that can be approximated well by a rational of small denominator must be transcendental.

*Proof.* Let $$f(x) = \sum a_k x^k$$ be the minimal polynomial of $$\alpha$$ with integer coefficients. Since $$f(\alpha) = 0$$, we’ll examine how $$f$$ behaves near $$\alpha$$.

By the fundamental theorem of algebra, $$f$$ has at most $$n$$ roots. This means there exists some $$\delta_1 > 0$$ such that if $$0 < |x - \alpha| < \delta_1$$, then $$f(x) \neq 0$$.

Also, since $$f'(\alpha) \neq 0$$ and $$f'$$ is continuous, we can find $$\delta_2 > 0$$ and a bound $$M > 0$$ such that if $$|x - \alpha| < \delta_2$$, then  

$$0 < |f'(x)| \le M$$

Let $$\delta = \min(\delta_1, \delta_2)$$. If a rational number $$\frac{p}{q}$$ lies within $$\delta$$ of $$\alpha$$, then by the Mean Value Theorem, there exists $$x_0$$ between $$p/q$$ and $$\alpha$$ such that

$$f'(x_0) = \frac{f(\alpha) - f(p/q)}{\alpha - p/q}$$

Since $$f(\alpha) = 0$$ and $$f(p/q) \neq 0$$, we rearrange:

$$|\alpha - p/q| = \frac{|f(p/q)|}{|f'(x_0)|}$$

Now consider the value of $$f(p/q)$$. Since $$f$$ has integer coefficients,

$$f(p/q) = \frac{1}{q^n} \sum_{k=0}^{n} a_k p^k q^{n-k}$$

This sum is an integer, and if $$f(p/q) \ne 0$$, its absolute value is at least 1. So,

$$|\alpha - p/q| \ge \frac{1}{|f'(x_0)|} \cdot \frac{1}{q^n} \ge \frac{1}{M} \cdot \frac{1}{q^n}$$

Setting $$A = 1/M$$, we get the desired bound.

We then define a **Liouville number** as an irrational number $$x$$ such that, for every integer $$n > 0$$, there are infinitely many rational numbers $$p/q$$ satisfying:

$$\left|x - \frac{p}{q} \right| < \frac{1}{q^n}$$

These numbers are, in a sense, *too well* approximated by rationals to be algebraic, and are better approximated by rationals than any other algebraic number.

**Theorem 2.** *Every Liouville number is transcendental.*

*Proof.* Suppose not—suppose there’s a Liouville number $$x$$ that is algebraic of degree $$d > 1$$. Then by Theorem 1, there is a constant $$A > 0$$ such that:

$$\left|x - \frac{p}{q} \right| > \frac{A}{q^d}$$

for all rational numbers $$p/q$$.

But the definition of Liouville number tells us that for any $$n > d$$, we can find infinitely many $$p/q$$ such that:

$$\left|x - \frac{p}{q} \right| < \frac{1}{q^n}$$

Since $$n > d$$, for sufficiently large $$q$$, the inequality

$$\frac{1}{q^n} < \frac{A}{q^d}$$

holds—contradicting the lower bound. Therefore, $$x$$ cannot be algebraic.

Returning to Liouville’s original construction:

$$L = \sum_{n=1}^{\infty} \frac{1}{10^{n!}}$$

We’ll show that this specific number satisfies the Liouville condition.

**Theorem 3.** *The Liouville constant* $$L$$ *is transcendental.*

*Proof.* For any $$m > 0$$, define a rational approximation:

$$L_m = \sum_{n=1}^{m} \frac{1}{10^{n!}}$$

This is a rational number with denominator $$10^{m!}$$. The error between $$L$$ and $$L_m$$ is:

$$|L - L_m| = \sum_{n=m+1}^{\infty} \frac{1}{10^{n!}} < \frac{1}{10^{(m+1)!}}$$

Since $$(m+1)! > m!(m+1)$$, we can write:

$$|L - L_m| < \frac{1}{10^{m!(m+1)}} = \frac{1}{(10^{m!})^{m+1}} = \frac{1}{q^{m+1}}$$

where $$q = 10^{m!}$$.

Given any $$k > 0$$, we can choose $$m$$ so that $$m+1 > k$$. Then:

$$|L - L_m| < \frac{1}{q^k}$$

This shows that $$L$$ can be approximated by rationals with error smaller than $$1/q^k$$ for every $$k$$—precisely the definition of a Liouville number. Hence, by Theorem 2, $$L$$ is transcendental.

<h2>Concluding Remarks: Roth’s Theorem on Diophantine Approximation</h2>

Liouville’s theorem was eventually sharpened in 1955 by Klaus Roth, who proved a striking improvement:

**Theorem 4 (Roth).** *Let* $\alpha$ *be an irrational algebraic number. Then for every* $\epsilon > 0$*, there exist only finitely many rational numbers $p/q$ such that*

$$\left| \alpha - \frac{p}{q} \right| < \frac{1}{q^{2 + \epsilon}}$$

This result shows that the exponent $$2$$ is essentially optimal for algebraic irrationals: they cannot be approximated better than order $$1/q^2$$, no matter how high their degree.

This study of how well real numbers can be approximated by rational ones is known as Diophantine Approximation. At first glance, this seems like a simple analytical question—how close can a fraction get to a given real number? But as it turns out, the answer is tightly constrained by number-theoretic structure. It forms in essence a conceptual bridge between analysis and number theory, revealing how the seemingly soft idea of “closeness” is secretly governed by hard algebraic facts. For instance, while any real number can be approximated by rationals, not all can be approximated equally well—and that difference can be telling.

Transcendental numbers—those that satisfy no algebraic equation with integer coefficients—can often be detected by their approximation behavior. Liouville’s insight was not just a clever construction—it was the beginning of a powerful philosophy: that transcendence can be *felt* in how a number dances around the rationals. And in that dance, some steps are too intricate for algebra to choreograph.

