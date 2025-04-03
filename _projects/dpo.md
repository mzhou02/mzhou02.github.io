---
layout: page
title: Reinforcement Learning with Human Feedback
description: This article is still in the works
img: 
importance: 1
category: reinforcement learning
mathjax: true
---
<script async src="https://www.googletagmanager.com/gtag/js?id=G-0823RLC0T3"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-0823RLC0T3');
</script>

<h2>The Alignment Challenge</h2>

Language models have become remarkably capable through self-supervised learning on vast corpora of human-written text. They've learned to predict the next word with impressive accuracy, absorbing patterns of knowledge, reasoning, and even some semblance of common sense. Yet, there's a fundamental gap between predicting the next word and generating text that humans genuinely find helpful, harmless, and honest.

This gap exists because the core objective of predicting the next token doesn't distinguish between the quality of different human writings. If 50% of explanations about a scientific concept contain a common misconception, we don't want our model to generate that misconception about half the time. Similarly, while GitHub contains enormous amounts of code, only a fraction represents best practices—yet we want our models to understand and generate high-quality code, not merely replicate the average.

This is the alignment challenge: how do we guide these powerful systems toward generating text that reflects human preferences and values, rather than simply reproducing the statistical patterns found in training data? How do we teach models to distinguish what should be written from what is written?

<h2>The Conceptual Foundation of RLHF</h2>

Reinforcement Learning from Human Feedback emerged as a powerful paradigm for aligning language models with human preferences. At its heart lies a simple idea: rather than trying to specify exactly what makes text "good" through rules or examples, we can learn what humans prefer directly from their comparative judgments. The key insight of RLHF is that humans find it much easier to compare two outputs and express a preference than to articulate exactly why one output is better than another. This relative preference signal turns out to be remarkably powerful for learning to generate better text.

Consider a simple example: if we ask a human whether they prefer response A or response B to a given query, they can usually make that judgment quite easily. But if we ask them to assign an absolute quality score to either response in isolation, or to articulate precisely what makes their preferred response better, they often struggle.

RLHF capitalizes on this insight by:

1. Collecting many such comparative judgments
2. Learning a reward model that can predict these preferences
3. Using this model to guide a language model toward generating preferred outputs

This approach has proven remarkably effective, enabling language models to produce outputs that better align with human values and preferences.

<h3>The Traditional RLHF Pipeline</h3>

The traditional RLHF approach unfolds as a three-stage process, with each stage building upon the previous one. Let's explore the conceptual flow of this pipeline, focusing on the key ideas rather than implementation details.

1. <strong>Supervised Fine-Tuning (SFT)</strong>: Create a foundation for further alignment by training the model on examples of desired behavior.
    
2. <strong>Reward Modeling</strong>: Learn to predict human preferences by training a model on pairs of outputs where humans indicated which they preferred.
    
3. <strong>Reinforcement Learning Optimization</strong>: Use the reward model to optimize the language model's outputs through reinforcement learning.

The beauty of this approach is in how it progressively refines the model's understanding of human preferences. First, we give the model a basic understanding of the types of responses we want through examples (SFT). This way, it makes the aligning stage much more stable. Then, we teach it to distinguish between better and worse responses by learning from human comparative judgments (reward modeling). Finally, we use this learned understanding to guide the model toward generating better responses (RL optimization).

<h3>Reward Modeling: Learning What Humans Prefer</h3>

The first part to understand within RLHF is the reward modeling stage. Imagine we have collected thousands of preference pairs: for each prompt $$x$$, we have two responses, $$y_1$$ and $$y_2$$, along with a human judgment about which response is preferred. Our goal is to learn a function $$r(x, y)$$ that assigns higher values to preferred responses.

The standard approach uses the Bradley-Terry preference model, which gives the probability that $$y_1$$ is preferred to $$y_2$$ for prompt $$x$$ as:

$$p(y_1 \succ y_2 | x) = \frac{\exp(r(x, y_1))}{\exp(r(x, y_1)) + \exp(r(x, y_2))} = \sigma(r(x, y_1) - r(x, y_2))$$

where $$\sigma$$ is the logistic function. This model has an intuitive interpretation: the probability of preferring one response over another depends on the difference in their reward values.

Training our reward model to predict human preferences will make our model learn a function that captures preferable human behaviors. Once trained, this reward model can evaluate any response to any prompt, providing a signal for what humans would likely prefer.

<h3>The Challenge of Reinforcement Learning</h3>

With a trained reward model in hand, the final stage of traditional RLHF uses reinforcement learning to optimize the language model's policy on the learned reward model. This involves a delicate balance: maximizing the reward while preventing the model from deviating too far from its original capabilities.

The RL objective typically takes the form:

$$\max_{\pi} \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi(y|x)}[r(x, y)] - \beta \cdot \text{KL}[\pi(y|x) \| \pi_{\text{ref}}(y|x)]$$

where:
- $$\pi$$ is the policy we're optimizing
- $$r(x, y)$$ is the reward function
- $$\beta$$ controls how strongly we penalize deviation from the reference policy $$\pi_{\text{ref}}$$
- KL refers to the Kullback-Leibler divergence, which measures how different two probability distributions are

While conceptually elegant, this approach introduces significant practical challenges:

- Reinforcement learning algorithms can be unstable and sensitive to hyperparameters
- The process is computationally expensive, requiring many model evaluations
- It creates a complex pipeline with multiple components that must work together
- As the policy changes during RL, the distribution of outputs shifts, potentially making the reward model less reliable

<h2>The Mathematical Bridge: From RLHF to DPO</h2>

This innate instability and computational infeasibility of training a reward model on human preferences while training a model on the reward model introduces the question: is there a way to train directly on human rankings and skiup the reward model step altogether?

<h3>The Optimal Policy Under a KL Constraint</h3>

We first examine that, for any reward function $$r(x,y)$$, the optimal policy $$\pi_r$$ that maximizes the expected reward while staying close to a reference policy $$\pi_{\text{ref}}$$ (as measured by KL divergence) has a specific mathematical form:

$$\pi_r(y|x) = \frac{1}{Z(x)} \pi_{\text{ref}}(y|x) \exp\left(\frac{1}{\beta}r(x,y)\right)$$

where $$Z(x)$$ is a normalizing constant (partition function) that ensures the distribution sums to 1:

$$Z(x) = \sum_y \pi_{\text{ref}}(y|x) \exp\left(\frac{1}{\beta}r(x,y)\right)$$

To demonstrate why, if we recall the RL objective

$$\max_{\pi} \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi(y|x)}[r(x, y) - \beta \cdot \text{KL}[\pi(y|x) \| \pi_{\text{ref}}(y|x)]]$$

Notice that this is equivalent to minimizing

$$\min_{\pi} \text{KL}[\pi(y|x) \| \pi_{\text{ref}}(y|x)] - \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi(y|x)}[ \frac{1}{\beta}r(x, y)]$$
$$=\min_{\pi} \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi(y|x)} \left[ \text{log}\left(\frac{\pi(y | x)}{\pi_{\text{ref}}(y|x)} \right)- \frac{1}{\beta}r(x, y) \right]$$
$$ = \min_{\pi} \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi(y|x)} \left[ \text{log}\left(\frac{\pi(y | x)}{\frac{1}{Z(x)} \pi_{\text{ref}}(y|x) \exp\left(\frac{1}{\beta}r(x,y)\right)} \right)- \text{log}(Z(x))\right]$$

Because $$\frac{1}{Z(x)} \pi_{\text{ref}}(y|x) \exp\left(\frac{1}{\beta}r(x,y)\right)$$ forms a valid probablity distribution, this is equivalent to

$$= \min_{\pi} \text{KL} \left[\pi(y|x) \bigg\| \frac{1}{Z(x)} \pi_{\text{ref}}(y|x) \exp\left(\frac{1}{\beta}r(x,y)\right) \right] -  \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi(y|x)} \text{log}(Z(x))$$

By Gibb's Inequality, this is minimized when $$\pi(y | x) = \frac{1}{Z(x)} \pi_{\text{ref}}(y|x) \exp\left(\frac{1}{\beta}r(x,y)\right)$$, finishing the proof. This tells us that the optimal policy applies a Boltzmann distribution over rewards, scaled by the reference policy. Higher rewards lead to higher probabilities in the optimal policy, with the KL constraint (controlled by $$\beta$$) determining how much the optimal policy can deviate from the reference policy.

<h3>The Key Insight: Rearranging the Equation</h3>

We can rearrange this insight on the optimal policy to express the reward function in terms of the policy:

$$r(x,y) = \beta \log \frac{\pi_r(y|x)}{\pi_{\text{ref}}(y|x)} + \beta \log Z(x)$$

Of course, we don't know the optimal policy—that's what we're trying to learn. But we do have human preference data that tells us which outputs are preferred to others. And since under the Bradley Terry model, the probability of a human preferring one output over another only deals with the differences in their rewards, we have some nice cancellation properties.

First, the Bradley Terry model says that

$$p(y_1 \succ y_2 | x) = \sigma(r(x, y_1) - r(x, y_2))$$

If we substitute our rearranged reward function, we get

$$p(y_1 \succ y_2 | x) = \sigma\left(\beta \log \frac{\pi_r(y_1|x)}{\pi_{\text{ref}}(y_1|x)} + \beta \log Z(x) - \beta \log \frac{\pi_r(y_2|x)}{\pi_{\text{ref}}(y_2|x)} - \beta \log Z(x)\right)$$
$$= \sigma\left(\beta \log \frac{\pi_r(y_1|x)}{\pi_{\text{ref}}(y_1|x)} - \beta \log \frac{\pi_r(y_2|x)}{\pi_{\text{ref}}(y_2|x)}\right)$$

Now, the partition function $$Z(x)$$ cancels out, and we can express the probability of one response being preferred over another directly in terms of the policy ratios, without needing to compute the intractable normalizing constant.

This cancellation is the key mathematical insight that makes DPO possible. It allows us to bypass both explicit reward modeling and reinforcement learning, and instead directly optimize a policy to predict human preferences.

<h2>Direct Preference Optimization: Elegant Simplicity</h2>

Direct Preference Optimization (DPO) leverages this mathematical insight we just explored to create a dramatically simpler approach to preference-based alignment. Instead of the three-stage pipeline of traditional RLHF, DPO offers a direct path from human preferences to an optimized language model.

<h3>The DPO Objective</h3>

DPO formulates a simple objective function that directly optimizes a policy $$\pi_\theta$$ to align with human preferences:

$$\mathcal{L}_{\text{DPO}}(\pi_\theta; \pi_{\text{ref}}) = -\mathbb{E}_{(x,y_w,y_l) \sim \mathcal{D}}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_w|x)}{\pi_{\text{ref}}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_{\text{ref}}(y_l|x)}\right)\right]$$

where:
- $$\pi_\theta$$ is the policy we're optimizing
- $$\pi_{\text{ref}}$$ is the reference policy
- $$(x, y_w, y_l)$$ represents a preference pair where $$y_w$$ is preferred over $$y_l$$ for prompt $$x$$
- $$\beta$$ controls the strength of the KL penalty
- $$\sigma$$ is the logistic function

This objective has an intuitive interpretation: it increases the probability of preferred responses relative to dispreferred responses, but does so in a way that accounts for the reference model's probabilities and includes an implicit KL penalty to prevent diverging too far from the reference model.

<h3>The Elegant Simplicity of DPO</h3>

The beauty of DPO lies in its simplicity. Instead of the complex pipeline of traditional RLHF, DPO requires just two steps:

<h4>The DPO Approach</h4>
1. Train a reference model (e.g., through supervised fine-tuning)
2. Directly optimize a policy to satisfy human preferences using the DPO objective

This approach offers several compelling advantages:

- <strong>Simplicity</strong>: A single optimization step replaces the multi-stage RLHF pipeline
- <strong>Stability</strong>: Using a simple classification loss is more stable than RL optimization
- <strong>Efficiency</strong>: Avoids the computational overhead of RL algorithms
- <strong>Theoretical elegance</strong>: Directly optimizes the same underlying objective as RLHF
- <strong>Interpretability</strong>: The implicit reward is directly tied to the policy

In essence, DPO accomplishes with one elegant step what traditional RLHF does with a complex pipeline of reward modeling and reinforcement learning.

<h3>Understanding the DPO Update</h3>

To build intuition for how DPO works, let's examine how it updates the policy. The gradient of the DPO loss with respect to the model parameters has the form:

$$\nabla_\theta \mathcal{L}_{\text{DPO}} \propto -\beta \cdot \sigma(\hat{r}_\theta(x, y_l) - \hat{r}_\theta(x, y_w)) \cdot (\nabla_\theta \log \pi_\theta(y_w|x) - \nabla_\theta \log \pi_\theta(y_l|x))$$

where $$\hat{r}_\theta(x, y) = \beta \log \frac{\pi_\theta(y|x)}{\pi_{\text{ref}}(y|x)}$$ is the implicit reward.

This update has a natural interpretation:

- It increases the probability of the preferred response ($$y_w$$)
- It decreases the probability of the dispreferred response ($$y_l$$)
- The magnitude of the update is larger when the current implicit reward incorrectly ranks the responses
- The KL penalty $$\beta$$ controls the overall magnitude of updates

In essence, DPO performs a form of preference learning that directly updates the policy to better align with human preferences, without the detour through explicit reward modeling and reinforcement learning.