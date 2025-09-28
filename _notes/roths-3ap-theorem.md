---
layout: page
title: Roth's Theorem
description: 
img: 
importance: 2
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

<div class="cv">
  <div class="card mt-3 p-3" style="width: fit-content; max-width: 100%;">
    <h2 class="card-title font-weight-medium">Contents</h2>
    <div class="card-text">
      <ol style="margin: 0; padding-left: 1.5rem;">
        <li><a href="#density-increment-argument">Density Increment Argument</a></li>
        <li><a href="#strategy">Strategy and Pre-Requisites</a></li>
        <li><a href="#non-uniformity">3AP-free $\Rightarrow$ large Fourier coefficient</a></li>
        <li><a href="#density-increment">Large Fourier Coefficient $\Rightarrow$ Density Increment</a></li>
        <li><a href="#wrap-up">Wrap Up</a></li>
      </ol>
    </div>
  </div>
</div>

<h1 id="density-increment-argument">Density Increment Argument</h1>
<br>

Roth’s celebrated theorem asserts that any subset of the natural numbers of positive upper density necessarily contains a three-term arithmetic progression (3AP). The proof is based on what is now called the *density increment argument*. Roughly speaking, one assumes that a set $A$ of natural numbers is free of three-term progressions, and then shows that this assumption forces $A$ to be “more dense than expected” on some structured subset of the integers. Concretely, if $A \subset [N]$ has density $\alpha$, then one can locate a subprogression $P \subset [N]$ on which the density of $A$ is not merely $\alpha$, but rather, let's say $\alpha + c\alpha^{2}$ for some absolute constant $c>0$:

$$\alpha = \frac{\vert A \cap [N]\vert }{N} \leq \frac{\vert A \cap P\vert }{\vert P\vert } - \Omega(\alpha^2)$$

(note that we chose $\alpha^2$ as this works out via the proof method. In reality, any diverging series will do).

This iteration can be continued: once a progression $P$ has been identified, one may then restrict attention to $A \cap P$ and repeat the argument, obtaining a further density increment on a subprogression of $P$. Since density is bounded above by $1$, this procedure cannot continue indefinitely. Eventually, one reaches a contradiction once $N$ becomes too large, thereby establishing the theorem.

To see why such density increments arise upon restriction, it is helpful to examine the inequality more carefully. What it tells us is that whenever $A$ avoids three-term progressions, one can always locate a subprogression on which the density of $A$ increases by an amount comparable to $\alpha^{2}$. Thus, each step of the iteration enforces a definite gain in density. The key question, then, is to identify the mechanism responsible for this gain: what structural features of $A$ make such an increment possible? It is this mechanism that we now turn to analyze. Let's start with,

$$(\alpha + \Omega(\alpha^2)) \ \vert P\vert  \leq \vert A \cap P\vert $$

In order to establish such an inequality, it suffices to show that

$$\sum_{i=1}^k \Omega(\alpha^2) \vert P_i\vert  \leq \sum_{i=1}^k \left\vert \vert A \cap P_i\vert  - \alpha P_i\right\vert $$

The right-hand side can be rewritten as

$$\sum_{i=1}^k \left\vert  \sum_{x \in P_i} (\mathbb{I} - \alpha)(x)\right\vert  \text{   (1)}$$

which bears a striking resemblance to a Fourier coefficient:

$$\widehat{\mathbb{I} - \alpha}(\theta) = \sum_{x \in [N]} (\mathbb{I} - \alpha)(x) e^{-2\pi i x\theta}. \text{   (2)}$$

By the triangle inequality, (1) provides an upper bound for the Fourier coefficient in (2), provided that \(e^{-2\pi i x\theta}\) remains approximately constant on each subset \(P_i\). Therefore, if we can demonstrate the existence of a large Fourier bias and partition \([N]\) into subsets where \(e^{-2\pi i x\theta}\) behaves uniformly, we can work backward to obtain another density increment.

<br>
<h1 id="strategy">Strategy and Pre-Requisites</h1>
<br>

<h3>Strategy</h3>

<ol>
  <li>3AP-free $\Rightarrow$ large Fourier coefficient</li>
  <li>Large Fourier coefficient $\Rightarrow$ density increment</li>
  <li>Iterate density increment</li>
</ol>

<h3>Key Identities</h3>
<ul>
  <li>(Fourier Transform) $\widehat{f}(\theta) = \sum_{x \in \mathbb{Z}} f(x)\text{e}(-x\theta)$ where $\text{e}(t) = \exp{(2\pi i t)}$</li>
  <li>$\widehat{f}(0) = \sum_{x \in \mathbb{Z}} f(x)$</li>
  <li>(Parseval) $\sum_{x \in \mathbb{Z}} f(x)\overline{g(x)} = \int_0^1 \widehat{f}(\theta)\overline{\widehat{g}(\theta)} \, d\theta$</li>
  <li>(Inversion) $f(x) = \int_0^1 \widehat{f}(\theta) \text{e}{(x\theta)} \, d\theta$</li>
</ul>

<br>
<h3>Counting Function</h3>

Since our goal is to prove that we can find a large Fourier coefficient, it might be a good idea to relate the number of 3APs within a set $A$ to its Fourier coefficients. More generally, we'll define a counting function as follows:

$$\Lambda(f, g, h) = \sum_{x, y} f(x)g(x + y)h(x + 2y)$$

As you may have guessed, we're interested particularly in the value $\Lambda(\mathbb{I}, \mathbb{I}, \mathbb{I})$.

<br>
<div class="card mt-3 p-3">
  <h4 class="card-title font-weight-medium">Lemma 1.</h4>
  <div class="card-text">
    <p>$$\Lambda(f, g, h) = \int_0^1 \widehat{f}(\theta) \widehat{g}(-2\theta) \widehat{h}(\theta) d\theta$$</p>
  </div>
</div>
<br>

*Proof*. Using Fourier Inversion,

$$\Lambda(f, g, h) = \sum_{x, y} 
\int_0^1 \widehat{f}(\theta) \text{e}{(x\theta)} \ d\theta 
\int_0^1 \widehat{g}(\theta) \text{e}{((x + y)\theta)} \ d\theta 
\int_0^1 \widehat{h}(\theta) \text{e}{((x + 2y)\theta)} \ d\theta $$

$$= \int_0^1 \int_0^1 \int_0^1 \widehat{f}(\theta_1) \widehat{g}(\theta_2) \widehat{h}(\theta_3) \left(\sum_{x, y} \text{e}{(x(\theta_1 + \theta_2 + \theta_3))}\text{e}{(y(\theta_2 + 2\theta_3))} \right)\ d\theta_1 d\theta_2 d\theta_3  $$

Since $\int_0^1 e(xt) dt = 0$ for integers $x$, we have that the integrals are 0 if $\theta_1 + \theta_2 + \theta_3 \neq 0$ or $\theta_2 + 2\theta_3 \neq 0$. Thus, the only contributing terms in the integral are those where $\theta_1 = -\frac{1}{2}\theta_2 = \theta_3$, and we have that

$$\int_0^1 \int_0^1 \int_0^1 \widehat{f}(\theta_1) \widehat{g}(\theta_2) \widehat{h}(\theta_3) \left(\sum_{x, y} \text{e}{( (\theta_1 + \theta_2 + \theta_3))}\text{e}{(y(\theta_2 + 2\theta_3))} \right)\ d\theta_1 d\theta_2 d\theta_3  $$

$$= \int_0^1 \widehat{f}(\theta) \widehat{g}(-2\theta) \widehat{h}(\theta) \ d\theta \square$$

We denote $\Lambda(f, f, f)$ as $\Lambda_3(f)$.

<br>
<h1 id="non-uniformity">3AP-free $\Rightarrow$ large Fourier coefficient</h1>
<br>

Notice that the quantity we are interested, $(\mathbb{I} - \alpha)(x)$, is equal to $(\mathbb{I} - \alpha \mathbb{I}_{[N]})(x)$ (since we are only summing over elements of $[N]$). The number of 3APs in $\mathbb{I}_{[N]}$ is much larger than the number of 3APs in $A$ if $A$ were 3AP free. So if we want to bound the density of $A$ if $A$ is 3AP free, we naturally want to find a way to relate the number of 3APs to the Fourier coefficients of their functions. 

<br>
<div class="card mt-3 p-3">
  <h4 class="card-title font-weight-medium">Lemma 2 (Counting Lemma).</h4>
  <div class="card-text">
    <p>Let $f, g : \ \mathbb{Z} \rightarrow \mathbb{C}$ and $\sum_{n \in \mathbb{Z}} \vert f(n)\vert ^2 \leq M, \sum_{n \in \mathbb{Z}} \vert g(n)\vert ^2 \leq M$, for some $M$. Then, we have that:</p>

    $$\left\vert  \Lambda_3(f) - \Lambda_3(g) \right\vert  < 3M \Vert \widehat{f - g}\Vert_{\infty}$$
  </div>
</div>
<br>


*Proof*. Notice that

$$\Lambda(f - g, f, f) + \Lambda(g, f - g, f) - \Lambda(g, g, f - g) = \Lambda_3(f) - \Lambda_3(g)$$

Using Lemma 1, we have that

$$\lvert \Lambda(f - g, f, f) \rvert \leq \int_0^1 \mid \widehat{f - g}(\theta) \widehat{f}(-2\theta) \widehat{f}(\theta) \mid d\theta \leq \Vert \widehat{f - g}\Vert_{\infty} \int_0^1 \lvert \widehat{f}(-2\theta) \rvert \lvert \widehat{f}(\theta) \rvert d\theta $$
    
By Cauchy-Schwarz,

$$\leq \Vert \widehat{f - g}\Vert_{\infty} \left(\int_0^1 \widehat{f}(-2\theta)^2 \right)^{1/2} \left(\int_0^1 \widehat{f}(\theta)^2 \right)^{1/2} d\theta $$

Using Parseval's Identity,

$$=  \Vert \widehat{f - g}\Vert_{\infty} \left(\sum_{x \in \mathbb{Z}} f(2x)^2 \right)^{1/2}\left(\sum_{x \in \mathbb{Z}} f(x)^2 \right)^{1/2}$$
    
$$\leq M \Vert \widehat{f - g}\Vert_{\infty}$$

Repeat for the rest, and we have that

$$\vert \Lambda_3(f) - \Lambda_3(g)\vert  \leq \vert \Lambda(f - g, f, f)\vert  + \vert \Lambda(g, f - g, f)\vert  + \vert \Lambda(g, g, f - g)\vert  \leq 3M \Vert \widehat{f - g}\Vert_{\infty} \square$$

Now, we continue with finding our large Fourier coefficient.

<br>
<div class="card mt-3 p-3">
  <h4 class="card-title font-weight-medium">Lemma 3 (No 3AP $\Rightarrow$ Non-uniformity).</h4>
  <div class="card-text">
    <p>Let $A \subset [N]$ be 3-AP free with $\vert A\vert  = \alpha N$ and $N \geq 5/\alpha^2$. Then there exists some $\theta$ such that </p>

    $$\left\vert \sum _{x \in \mathbb{Z}}^{N}(\mathbb{I}_{A}-\alpha )(x)e(-x\theta)\right\vert \geq {\frac {\alpha ^{2}}{10}}N$$.
  </div>
</div>
<br>

Let $\mathbb{I}_{[N]}$ represent the indicator function for whether an element $x$ is in $[N]$ and $\mathbb{I}_{A}$ represent the indicator function for whether an element $x$ is in $A$. Then, we have that $\Lambda_3(\mathbb{I}_{[N]})$ counts the number of 3-term arithmetic progressions from $[N]$, which is more than $\frac{N^2}{2}$. Since we are assuming that $A$ has no nontrivial 3-term arithmetic progressions, we have that $\Lambda_3(\mathbb{I}_A) = \alpha N$. Thus, we have that

$$\frac{\alpha^3N^2}{2} - \alpha N \leq \Lambda_3(\alpha \mathbb{I}_{[N]}) - \Lambda_3(\mathbb{I}_A)$$

By the counting lemma, we have that

$$\Lambda_3(\alpha \mathbb{I}_{[N]}) - \Lambda_3(\mathbb{I}_A) \leq 3\alpha N \Vert \widehat{f - g}\Vert_{\infty} $$

$$\frac{\alpha^3N^2}{2} - \alpha N \leq 3\alpha N \Vert \widehat{f - g}\Vert_{\infty}$$

$$\Vert \widehat{f - g}\Vert_{\infty} \geq \frac{\alpha^2N}{6} - \frac{1}{3} \geq \frac{\alpha^2N}{10} \square$$

<br>
<h1 id="density-increment">Large Fourier Coefficient $\Rightarrow$ Density Increment</h1>
<br>

<h3>Subset Division</h3>

Now that we have our large Fourier coefficient, we just now need to be able to divide $[N]$ into subsets where $e(-x\theta)$ stay roughly constant. To do so, we first introduce a lemma by Dirichlet.

<br>
<div class="card mt-3 p-3">
  <h4 class="card-title font-weight-medium">Lemma 4 (Dirichlet).</h4>
  <div class="card-text">
    <p>Let $\theta, \delta \in \mathbb{R}$, there is $x \leq \frac{1}{\delta}$ such that the distance of $x\theta$ to the closest integer is at most $\delta$. </p>
  </div>
</div>
<br>

*Proof*. Select $m = \lfloor \frac{1}{\delta}\rfloor$, and let us have an arithemtic sequence $0, \theta, 2\theta, ..., m\theta$. Let us divide the unit interval into $\lceil \frac{1}{\delta} \rceil$ intervals: if the $\frac{1}{\delta}$ is am integer, divide it equally. Otherwise, make the first $m$ sections length $\delta$, and the last the remainder (which is less than $\delta$). In case 1, by pigeonhole, we have that two $i\theta$ and $j\theta$ must be in the same section. In the second case, if the last section contained an $i\theta$, we just use that one. Otherwise, there will also be two $i\theta$ and $j\theta$ must be in the same section. The difference $\vert i - j\vert $ the suffices. $\square$

The reason why this is so useful is because if we can find a $d$ where $d\theta$ is close to an integer, then $e(d\theta)$ is roughly equal to 1, meaning that the arithmetic progression $e(x\theta), e((x + d)\theta), e((x + 2d)\theta), ...$ is roughly constant. We continue this idea as follows.

<br>
<div class="card mt-3 p-3">
  <h4 class="card-title font-weight-medium">Lemma 5 (Dirichlet).</h4>
  <div class="card-text">
    <p>Let $ 0<\eta <1,\theta \in \mathbb {R} $. Assume that $ N>C\eta ^{-6}$ for a universal constant $C$. Then it is possible to partition $[N]$ into arithmetic progressions $ P_{i}$ with length $ N^{1/3}\leq \vert P_{i}\vert \leq 2N^{1/3}$ such that $ \sup _{x,y\in P_{i}}\vert e(x\theta )-e(y\theta )\vert <\eta $ for all $ i$. </p>
  </div>
</div>
<br>
    
*Proof*. We first bound $\vert e(x\theta) - e(y\theta)\vert $ for $x, y \in P_i$. Because they lie in am arithmetic progression with length $d$, let $x = y + kd$ for some $k$. We can start by rewriting this difference as a telescoping sum

$$\vert e(x\theta) - e(y\theta)\vert  = \left\vert \sum_{i = 1}^k e((y + id)\theta) - e((y + (i - 1)d)\theta)\right\vert  \leq \sum_{i = 1}^k  \vert e((y + id)\theta) - e((y + (i - 1)d)\theta)\vert $$

$$ = \sum_{i = 1}^k \vert e(d\theta) - 1\vert  \leq \vert P_i\vert  \  \vert e(d\theta) - 1\vert $$

Thus, it suffices to pick a common difference $d$ that makes $e(d\theta)$ arbitrarily close to 1. We use the above lemma to choose a $d$ such that $d\theta \leq \frac{\eta}{3\pi N^{1/3}}$ where $d \leq \frac{4\pi N^{1/3}}{\eta}$. First, notice that $\vert e(d\theta) - 1\vert $ is bounded by the arc length of $d\theta$ degrees. Thus,

$$\vert e(d\theta) - 1\vert  \leq 2\pi \left( \frac{\eta}{4 \pi N^{1/3}}\right) = \frac{\eta}{2N^{1/3}} $$
Thus,

$$\sup _{x,y\in P_{i}}\vert e(x\theta )-e(y\theta )\vert  \leq \vert P_i\vert  \ \vert e(d\theta) - 1\vert  \leq 2N^{1/3} \left( \frac{\eta}{2 N^{1/3}}\right) = \eta$$

Since $N > C\eta^{-6}$, we have that $d \leq \frac{4\pi N^{1/3}}{CN^{-1/6}} = O(\sqrt{N})$, which is small enough for us to divide our subprogressions with lengths between $N^{1/3}$ and $2N^{1/3}$. $\square$


Using this lemma, we now find a subprogression that we can restrict the original set to and find a density increment.

<h3>Density Increment</h3>
<div class="card mt-3 p-3">
  <h4 class="card-title font-weight-medium">Lemma 6 (Non-uniformity $\Rightarrow$ Density Increment).</h4>
  <div class="card-text">
    <p>Let $A$ be a 3-AP-free subset of $[N]$, with $\vert A\vert =\alpha N$ and $N > C\alpha ^{-12}$. Then, there exists a sub progression $P\subset [N]$ such that $\vert P\vert \geq N^{1/3}$ and $\vert A\cap P\vert \geq (\alpha +\alpha ^{2}/40)\vert P\vert $. </p>
  </div>
</div>
<br>

*Proof*. Using the large Fourier coefficient we calculated before, we have that

$$\left\vert \sum _{x \in \mathbb{Z}}^{N}(\mathbb{I}_{A}-\alpha )(x)e(-x\theta)\right\vert \geq {\frac {\alpha ^{2}}{10}}N$$

Since $N > C\alpha^{-12}$, we can select $\eta = \frac{\alpha^2}{20}$ to divide our sequence into subprogressions

$$\frac{\alpha^2}{10}N \leq \left\vert \sum _{x \in \mathbb{Z}}^{N}(\mathbb{I}_{A}-\alpha )(x)e(-x\theta)\right\vert  = \left\vert \sum_{i = 1}^k \sum _{x \in P_i}(\mathbb{I}_{A}-\alpha )(x)e(-x\theta)\right\vert  \leq \sum_{i = 1}^k \left\vert  \sum _{x \in P_i}(\mathbb{I}_{A}-\alpha )(x)e(-x\theta)\right\vert $$

and because $\sup _{x,y\in P_{i}}\vert e(x\theta )-e(y\theta )\vert <\frac{\alpha^2}{20}$,

$$\leq \sum_{i = 1}^k \left(\left\vert  \sum _{x \in P_i}(\mathbb{I}_{A}-\alpha )(x)e(-y_i\theta)\right\vert  + \left\vert  \sum _{x \in P_i}(\mathbb{I}_{A}-\alpha )(x)(e(-x\theta) - e(-y_i\theta))\right\vert \right)$$

$$\leq \sum_{i = 1}^k\left( \left\vert  \sum _{x \in P_i}(\mathbb{I}_{A}-\alpha )(x)\right\vert  + \frac{\alpha^2}{20}\vert P_i\vert \right)$$

for some fixed $y_i \in P_i$. And we have that

$$\frac{\alpha^2}{20} \sum_{i = 1}^k \vert P_i\vert  = \frac{\alpha^2}{20}N \leq \sum_{i = 1}^k \left\vert  \sum _{x \in P_i}(\mathbb{I}_{A}-\alpha )(x)\right\vert  = \sum_{i = 1}^k \left\vert  \vert A \cap P_i \vert  - \alpha P_i \right\vert $$

$$=\sum_{i = 1}^k \left\vert  \vert A \cap P_i \vert  - \alpha P_i \right\vert  + \vert A\cap P_i\vert  - \alpha P_i$$

Notice that $\left\vert  \vert A \cap P_i \vert  - \alpha P_i \right\vert  + \vert A\cap P_i\vert  - \alpha P_i$ is either 0 or $2\vert A\cap P_i\vert  - 2\alpha P_i$. Because the right hand side is larger than the left, at least one term on the right is at least as large as a term on the left, and since all terms on the left sum are greater than 0, we have that there is some $P_i$ such that

$$\frac{\alpha^2}{20} \vert P_i\vert  \leq 2\vert A\cap P_i\vert  - 2\alpha P_i$$

$$\vert A\cap P_i\vert \geq (\alpha +\alpha ^{2}/40)\vert P_i\vert  \square$$

<br>
<h1 id="wrap-up">Wrap Up</h1>
<br>

After at most $40/\alpha + 1$ iterations of restricting to a subprogression and finding the new density of that set restricted to this subprogression, we will double our density in that subset. Then, after $20/\alpha + 1$, we will double again. We can double at most $-\text{log}(\alpha)$ number of times before getting a density greater than 1, meaning that we will have at most $O(\frac{1}{\alpha})$ number of iterations. We can continue iterating until $N_i \leq C\alpha_i^{-12}$, where $N_i$ is size of the progression we are restricting to and $\alpha_i$ is the density of the set in this progression. Let us continue iterating until this condition. Then $N_i \leq C \alpha_i^{-12} \leq C\alpha^{-12}$, and since our subprogression is of size at least $N^{1/3}$,

$$N \leq N_i^{3^i} \leq (C\alpha^{-12})^{3^i}$$

Since we have at most $O(\frac{1}{\alpha})$ iterations before achieving a density over 1,

$$\leq (C\alpha^{-12})^{3^{O(1/\alpha)}} = e^{e^{O(1/\alpha)}}$$

This means that

$$\alpha = O\left( \frac{1}{\text{log(log(}N))} \right)$$