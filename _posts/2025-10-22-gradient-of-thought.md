---
layout: post
title: "The Gradient of Thought: Reasoning as a Natural Objective for Alignment"
date: 2025-10-22 18:34:00
description:
tags: nlp
categories: ai
featured: false
---
<script async src="https://www.googletagmanager.com/gtag/js?id=G-0823RLC0T3"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-0823RLC0T3');
</script>

Large language models (LLMs) learn in a remarkably simple way: by predicting the next token in a sequence. During pre-training, they process vast corpora and adjust parameters $\theta$ so that the probability assigned to the correct next token,

$$p_\theta(x_t \mid x_{<t}),$$

steadily rises. This is the familiar cross-entropy language-modeling objective.

At a high level, an LLM learns a map from *context*, what has already been seen, to *expectation*, a distribution for what should come next,. Fine-tuning, or post-training, refines this map for specific tasks: multi-step reasoning, following instructions, calibration, and so on.

<p>A by-now standard empirical observation is that prompting models to produce Chain-of-Thought (CoT), step-by-step intermediate explanations, often yields striking improvements on reasoning tasks<sup><a href="#ref1">[1]</a></sup>. Appending "Let’s think step by step" induces the model to unfold a short sequence of intermediate tokens that articulate a path:</p>

<blockquote style="margin-left: 2.5em; margin-right: 2.5em; font-size: 0.9em; color: #444; font-style: italic;"> "To solve this, first note the number is even; therefore dividing by two gives..." </blockquote>

Why should such intermediate text help?

<br>
<h1> An Information-Theoretic Perspective </h1>
<br>

At bottom, next-token prediction is an exercise in uncertainty management. The *surprisal* of a token $x_t$ given its context $x_{<t}$ is

$$\text{surp}(x_t \mid x_{<t}) \;=\; -\log p_\theta(x_t \mid x_{<t}),$$

measured in bits. Surprisal counts how many "yes/no" questions one would need, on average, to identify the correct token under the model's own distribution. High surprisal signals a difficult prediction (the model did not expect this token); low surprisal signals an easy one.

When a question is hard, we rarely vault straight to the answer. We instead lay down stepping stones: small statements that are easy to verify, each making the next step easier. CoT plays the same role for a model. Rather than a single long, unlikely jump $q \to a$, we nudge the model to traverse a short path

$$q \;\to\; r_1 \;\to\; r_2 \;\to\; \cdots \;\to\; r_k \;\to\; a,$$

so that each conditional $p_\theta(r_i \mid q, r_{<i})$ and $p_\theta(a \mid q, r_{1:k})$ is higher than it would have been without the preceding scaffolding. The informational load that would otherwise concentrate in a single cliff-edge step becomes distributed across the sentence in several gentler slopes.

<br>
<h1> Chain-of-Thought and Post Training Gradients </h1>
<br>

One can think of post-training not as the acquisition of new facts, but as the adjustment of geometry within a model that already knows much. Because language models are optimized under a cross-entropy objective, **surprisal and gradient behavior are intimately linked**: when the model’s predictions are more evenly distributed, its updates become steadier. Fine-tuning on Chain-of-Thought (CoT) prompt-answer pairs achieves precisely this effect. The reasoning prefix redistributes surprisal across tokens, smoothing the loss surface and dampening abrupt fluctuations in the gradient with respect to the logits, what we may call gradient spikes. In essence, CoT supervision transforms learning from a sequence of sharp corrections into a flow of gentle, coherent updates.

<br>
<div class="card mt-3 p-3">
  <h6 class="card-title font-weight-medium">Lemma 1</h6>
  <div class="card-text">
    <p>The partial derivative of the loss with respect to $y_k$ is simply the model’s predicted probability of that token minus the true label. In other words,
    
    $$\frac{\partial \mathcal{L}}{\partial y_k} = p_\theta(y_k \mid y_{<t}) - \mathbb{I}\{y_k = x_t\}, $$
    
    where $x_t$ is the correct next token in the training data. </p>
  </div>
</div>
<br>

*Proof.* Write $Z = \sum_j e^{y_j}$ and $p_i = e^{y_i}/Z$. 

For each $i$,

$$\log p_i = y_i - \log Z,$$

so

$$\frac{\partial}{\partial y_k} \log p_i = \frac{\partial y_i}{\partial y_k} - \frac{\partial \log Z}{\partial y_k} = \mathbb{I}\{i = k\} - \frac{1}{Z} \frac{\partial Z}{\partial y_k}$$

<br>

$$ = \mathbb{I}\{i = k\} - \frac{e^{y_k}}{Z} = \mathbb{I}\{i = k\} - p_k.$$


Therefore,

$$\frac{\partial \mathcal{L}}{\partial y_k} = -\sum_{i=1}^V q(i) \frac{\partial}{\partial y_k} \log p_i = -\sum_i q(i)\bigl(\mathbb{I}\{i = k\} - p_k\bigr)$$

<br>

$$= -q(k) + p_k \sum_i q(i) = p_k - q(k),$$

since $\sum_i q(i) = 1$. 

Specializing to the one-hot case <p>$q = \mathbb{I}\{k = x_t\}$</p> gives

$$\frac{\partial \mathcal{L}}{\partial y_k} = p_k - \mathbb{I}\{k = x_t\},$$

as claimed. $\square$

<br>
We now use this to connect the size of the gradient to the model’s probability distribution.


<br>
<div class="card mt-3 p-3">
  <h6 class="card-title font-weight-medium">Lemma 2</h6>
  <div class="card-text">
    <p>Let $g$ be the loss gradient in respect to model logits. If $\|g\| > \tau$, then 
    
    $$ 1 - \sqrt{\tau} < \theta(x_t \mid y_{<t}) < 1 - \sqrt{\tfrac{\tau}{2}}. $$
    
    </p>
  </div>
</div>
<br>

*Proof.* Let $p$ denote the probability vector $p_\theta(\cdot \mid y_{<t})$ and $p_x$ represent $p_\theta(x_t \mid y_{<t})$. From Lemma 1, the gradient with respect to the logits satisfies

$$ \|g\|^2 = (1 - p_x)^2  + \sum_{y_i \neq x_t} p_\theta(y_i \mid y_{<t})^2. $$

Rewriting,

$$\|g\|^2 = \|p\|_2^2 - 2p_x + 1.$$

Since the entries of $p_\theta$ are nonnegative and sum to one,

$$\|p\|_2^2 \leq p_x^2 + (1 - p_x)^2.$$

Substituting this bound gives

$$\|g\|^2 \leq 2(1 - p_x)^2.$$

Hence, if $\|g\| > \tau$, it follows that

$$p_x < 1 - \sqrt{\tfrac{\tau}{2}},$$

Similarly, we can use the bound

$$|p\|_2^2 \geq p_x^2$$

to get that 

$$p_x > 1 - \sqrt{\tau}. \square$$

<br>
The magnitude of the gradient spike during training is thus largely governed by the tail of the model’s probability distribution: when the model assigns high probability to the correct token, the gradient necessarily remains small. Reasoning traces are easier to for the model to predict, effectively shifting the distribution of the correct token forward, moving the tail upwards and thereby reducing the frequency and severity of large gradients in practice.

Of course, if we used only the preceding lemma, one could only guarantee this in theory when the shift in probability exceeds roughly $\sqrt{\tau_{\text{direct}}}(1 - \sqrt{2}/2)$ in factor; the verification of this is left as an exercise to the reader. However, the inequalities employed above are somewhat generous, and in most realistic settings the stability improvement from CoT training probably appears far stronger than this conservative bound would suggest.

<br>
<h1> From Surprise to Stability </h1>
<br>

For the most part, changes in logit gradients translate directly to changes in parameter gradients, and from this gradient perspective, the benefits of Chain-of-Thought supervision extend beyond teaching the model to just reason: they reveal reasoning itself as a task that naturally aligns the model’s learning dynamics with the behaviors we seek to cultivate. Training on reasoning sequences elicit smaller, steadier gradient updates. These tempered updates allow the model’s latent capacities—abstraction, consistency, self-monitoring—to emerge without being drowned out by gradient noise, while also keeping learning trajectories closer to the model’s original pre-trained distribution. In this way, the reasoning objective acts as an elicitation prior: it guides optimization toward coherent, human-aligned behavior through the intrinsic structure of the task. This demonstrates that reasoning is not just a tool for alignment: it is also a natural training objective for it.

Reasoning sequences in this light act as a variational regularizer on the cross-entropy objective, implicitly minimizing curvature in the loss surface. But beyond stabilizing optimization, these gentler updates confer several deeper benefits. In fine-tuning, where a narrow slice of data adjusts a model with billions of parameters, stability matters as much as accuracy. Smoother gradients prevent overcorrection and reduce the risk of catastrophic forgetting, allowing the model to incorporate new reasoning patterns without erasing the representations that support them. Intuitively, we are teaching the model to reason more, not to memorize differently: the training signal refines the structure of thought rather than replacing its contents. This makes reasoning supervision doubly valuable: it extends the model’s abilities while preserving the foundation it was built upon.

This perspective helps illuminate why many modern alignment strategies, like the “reasoning-first” design of the o1 training process, prove so effective. Most textbook data, for example, announces the answer before revealing the logic, preserving a sharp spike in surprisal that gradients must struggle to descend. Reasoning-first data, by contrast, tilts that spike into a staircase, letting probability mass shift forward gradually as intermediate steps unfold. The resulting model learns to reason and to learn in smaller, steadier increments.

Geometrically, this translates to a refinement of the model’s internal landscape. Pre-training yields a vast but rugged terrain, full of steep cliffs where token probabilities shift abruptly from one step to the next. Post-training on reasoning traces acts less like the addition of new knowledge and more like a smoothing operation across this surface. The same information remains, but its contours soften; gradients that once fluctuated violently begin to flow coherently along intermediate steps. The model does not merely learn new facts—it learns to traverse its own knowledge more gracefully. In the end, the gradient tells a simple story: reasoning is a way of making prediction coherent. When a model learns to smooth its own landscape—distributing complexity over time and thought—it becomes not only a better reasoner, but a gentler learner.

<br>
<h1> References </h1>
<br>

<p id="ref1">[1] Jason Wei, Xuezhi Wang, Dale Schuurmans, Maarten Bosma, Brian Ichter, Fei Xia, Ed Chi, Quoc V. Le, and Denny Zhou. 2022. *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models.* In Advances in Neural Information Processing Systems 35 (NeurIPS 2022).</p>
