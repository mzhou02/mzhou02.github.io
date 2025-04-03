---
layout: page
title: Trust Region and Proximal Methods
description: 
img: 
importance: 1
category: reinforcement learning
mathjax: true
---
<h2>Trust Region Methods in Reinforcement Learning</h2>

<h4>Beyond Value-Based Methods: The Policy Gradient Approach</h4>

While value-based methods like Q-learning provide powerful tools for reinforcement learning, they operate indirectly by first learning a value function and then deriving a policy. An alternative approach is to parameterize the policy directly and optimize it to maximize expected returns. This policy gradient approach offers several advantages, particularly for problems with continuous action spaces or where stochastic policies are beneficial.

In the policy gradient framework, we represent the policy as a parameterized function $$\pi_\theta(a|s)$$, where $$\theta$$ are the parameters (e.g., weights of a neural network). The objective is to find parameters $$\theta$$ that maximize the expected return:

$$J(\theta) = \mathbb{E}_{\tau \sim \pi_\theta} \left[ \sum_{t=0}^{T} \gamma^t r_t \right]$$

where $$\tau$$ represents a trajectory (sequence of states, actions, and rewards) sampled by following policy $$\pi_\theta$$.

The policy gradient theorem provides a way to compute the gradient of this objective:

$$\nabla_\theta J(\theta) = \mathbb{E}_{\tau \sim \pi_\theta} \left[ \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(a_t|s_t) \cdot G_t \right]$$

where $$G_t = \sum_{k=t}^{T} \gamma^{k-t} r_k$$ is the return from time step $$t$$.

This formula leads to a straightforward algorithm: sample trajectories using the current policy, compute the gradients according to the above formula, and update the parameters using gradient ascent:

$$\theta \leftarrow \theta + \alpha \nabla_\theta J(\theta)$$

While this approach is theoretically sound, it often suffers from high variance in gradient estimates, poor sample efficiency, and sensitivity to step size. These limitations have motivated the development of more sophisticated policy optimization methods, including trust region approaches.

<h4>The Stability Challenge in Policy Optimization</h4>

Standard policy gradient methods face a fundamental challenge: determining the appropriate step size for parameter updates. Too small a step and learning progresses slowly; too large a step and the policy may change drastically, potentially degrading performance and leading to unstable learning.

This challenge becomes particularly acute when using function approximation, such as neural networks, to represent the policy. In such cases, a seemingly small change in parameters might result in a dramatic change in behavior. Moreover, the reinforcement learning objective is non-stationary and typically non-convex, making optimization inherently difficult.

The issue can be understood through the lens of catastrophic forgetting. Consider fine-tuning a language model on a specific task: aggressive optimization toward task performance might cause the model to rapidly diverge from its pre-trained state, losing the general language understanding that made it valuable in the first place. The model excels at the narrow task but forgets its broader capabilities.

Trust region methods address this challenge by imposing constraints on how much the policy can change in a single update, ensuring that learning remains stable while still making meaningful progress.

<h2>Trust Region Policy Optimization (TRPO)</h2>

<h4>The Theoretical Foundation: Conservative Policy Iteration</h4>

Trust Region Policy Optimization (TRPO), introduced by Schulman et al. in 2015, builds upon theoretical work in conservative policy iteration. The key insight is that policy updates should be constrained to prevent large, potentially harmful changes.

Consider the relationship between policies and their performance. If we have a current policy $$\pi_{\text{old}}$$ and are considering a new policy $$\pi$$, we can express the expected return of $$\pi$$ in terms of the advantage function of $$\pi_{\text{old}}$$:

$$J(\pi) = J(\pi_{\text{old}}) + \mathbb{E}_{s \sim \rho_\pi, a \sim \pi} \left[ A^{\pi_{\text{old}}}(s, a) \right]$$

where $$\rho_\pi$$ is the state visitation frequency under policy $$\pi$$, and $$A^{\pi_{\text{old}}}(s, a)$$ is the advantage function, which measures how much better it is to take action $$a$$ in state $$s$$ compared to the average action according to $$\pi_{\text{old}}$$.

This equation is exact, but it's problematic for optimization because the state distribution $$\rho_\pi$$ depends on the new policy $$\pi$$, which we're trying to find. To address this, TRPO relies on a local approximation:

$$J(\pi) \approx J(\pi_{\text{old}}) + \mathbb{E}_{s \sim \rho_{\pi_{\text{old}}}, a \sim \pi} \left[ A^{\pi_{\text{old}}}(s, a) \right]$$

This approximation assumes that the state visitation frequencies under the old and new policies are similar, which is reasonable if the policies themselves are similar. But how do we ensure that the policies remain similar enough for this approximation to hold?

<h4>Constraining Policy Updates with KL Divergence</h4>

TRPO formalizes the notion of "similar policies" using the Kullback-Leibler (KL) divergence, a measure of how one probability distribution differs from another. Specifically, TRPO constrains the maximum KL divergence between the old and new policies at any state:

$$\max_s D_{KL}(\pi_{\text{old}}(\cdot|s) \parallel \pi(\cdot|s)) \leq \delta$$

where $$\delta$$ is a hyperparameter controlling the size of the trust region.

The TRPO optimization problem thus becomes:

$$\max_{\theta} \mathbb{E}_{s \sim \rho_{\pi_{\text{old}}}, a \sim \pi_\theta} \left[ A^{\pi_{\text{old}}}(s, a) \right]$$
$$\text{subject to } \max_s D_{KL}(\pi_{\text{old}}(\cdot|s) \parallel \pi_\theta(\cdot|s)) \leq \delta$$

This constrained optimization ensures that the new policy doesn't deviate too far from the old one, maintaining the validity of the local approximation and preventing catastrophic updates.

<h4>The TRPO Algorithm</h4>

Solving the constrained optimization problem directly is challenging. TRPO uses a series of approximations:

1. Replace the maximum KL divergence with the average KL divergence:
   $$\mathbb{E}_{s \sim \rho_{\pi_{\text{old}}}} \left[ D_{KL}(\pi_{\text{old}}(\cdot|s) \parallel \pi_\theta(\cdot|s)) \right] \leq \delta$$

2. Use a linear approximation for the objective and a quadratic approximation for the constraint:
   $$\max_{\theta} g^T (\theta - \theta_{\text{old}})$$
   $$\text{subject to } \frac{1}{2} (\theta - \theta_{\text{old}})^T H (\theta - \theta_{\text{old}}) \leq \delta$$
   
   where $$g$$ is the gradient of the objective and $$H$$ is the Hessian of the KL divergence with respect to $$\theta$$.

3. Solve this approximated problem using the natural gradient:
   $$\theta_{\text{new}} = \theta_{\text{old}} + \sqrt{\frac{2\delta}{g^T H^{-1} g}} H^{-1} g$$

In practice, computing the Hessian $$H$$ and its inverse is computationally expensive for large neural networks. TRPO approximates the Hessian-vector products using the conjugate gradient algorithm and uses backtracking line search to ensure that the KL constraint is satisfied while maximizing the objective.

The complete TRPO algorithm involves:
1. Collecting trajectories using the current policy
2. Estimating advantages using, for example, Generalized Advantage Estimation (GAE)
3. Computing the search direction using conjugate gradient
4. Performing a line search to find the optimal step size
5. Updating the policy parameters

<h4>Strengths and Limitations of TRPO</h4>

TRPO has several notable strengths:
- It provides monotonic policy improvement guarantees under certain conditions
- It's relatively robust to hyperparameter choices, particularly the step size
- It has demonstrated strong performance across a variety of continuous control tasks

However, TRPO also has significant limitations:
- It's computationally expensive due to the second-order optimization
- The constrained optimization problem is complex to implement
- It doesn't naturally extend to architectures with shared parameters between policy and value functions
- The exact constraint enforcement can sometimes be overly conservative

These limitations motivated the development of simpler alternatives that retain the key insights of trust region methods while being more practical to implement.

<h2>Proximal Policy Optimization (PPO)</h2>

<h4>Simplifying Trust Region Methods</h4>

Proximal Policy Optimization (PPO), introduced by Schulman et al. in 2017, aims to capture the benefits of TRPO while addressing its practical limitations. PPO maintains the core idea of restricting policy updates to a trust region but employs a simpler approach that avoids second-order optimization.

The key insight of PPO is that we can achieve similar performance to TRPO using a clipped surrogate objective function that can be optimized with first-order methods like stochastic gradient descent.

<h4>The Clipped Surrogate Objective</h4>

PPO works with the ratio of probabilities between the new and old policies:

$$r_t(\theta) = \frac{\pi_\theta(a_t|s_t)}{\pi_{\theta_{\text{old}}}(a_t|s_t)}$$

This ratio indicates how much more (or less) likely the action $$a_t$$ is under the new policy compared to the old policy. If $$r_t(\theta) > 1$$, the new policy assigns higher probability to the action; if $$r_t(\theta) < 1$$, it assigns lower probability.

The standard surrogate objective would be:

$$L^{SURR}(\theta) = \mathbb{E}_t \left[ r_t(\theta) \cdot A_t \right]$$

where $$A_t$$ is the estimated advantage at time $$t$$.

Maximizing this objective would increase the probability of actions with positive advantages and decrease the probability of actions with negative advantages. However, without constraints, this could lead to excessively large policy updates.

PPO addresses this by clipping the probability ratio:

$$L^{CLIP}(\theta) = \mathbb{E}_t \left[ \min(r_t(\theta) \cdot A_t, \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon) \cdot A_t) \right]$$

where $$\epsilon$$ is a hyperparameter (typically around 0.2) that controls the clipping range.

This clipped objective has an intuitive interpretation:
- For actions with positive advantages ($$A_t > 0$$), the objective encourages increasing their probability, but only up to a point ($$r_t(\theta) \leq 1+\epsilon$$).
- For actions with negative advantages ($$A_t < 0$$), the objective encourages decreasing their probability, but again, only up to a point ($$r_t(\theta) \geq 1-\epsilon$$).

The clipping mechanism effectively creates a trust region that prevents the new policy from deviating too far from the old one.

<h4>The Complete PPO Algorithm</h4>

The complete PPO algorithm typically includes additional components:

1. <strong>Value function estimation</strong>: PPO often optimizes a value function $$V_\phi(s)$$ alongside the policy, using a loss function like:
   $$L^{VF}(\phi) = \mathbb{E}_t \left[ (V_\phi(s_t) - V_t^{target})^2 \right]$$

2. <strong>Entropy bonus</strong>: To encourage exploration, PPO may include an entropy term in the objective:
   $$L^{ENT}(\theta) = \mathbb{E}_t \left[ H(\pi_\theta(\cdot|s_t)) \right]$$

3. <strong>Combined objective</strong>: The final objective combines these components:
   $$L^{TOTAL}(\theta, \phi) = \mathbb{E}_t \left[ L^{CLIP}(\theta) - c_1 L^{VF}(\phi) + c_2 L^{ENT}(\theta) \right]$$
   where $$c_1$$ and $$c_2$$ are hyperparameters controlling the relative importance of each term.

The PPO algorithm proceeds as follows:

<pre>
Algorithm: Proximal Policy Optimization (PPO)
1. Initialize policy parameters θ and value function parameters φ
2. Repeat:
   a. Collect set of trajectories by running policy π_θ in the environment
   b. Compute advantages using generalized advantage estimation or another method
   c. Compute rewards-to-go for value function targets
   d. For each epoch:
      i. For each minibatch:
         1. Update θ by maximizing the PPO-Clip objective
         2. Update φ by minimizing the value function loss
3. Until convergence
</pre>

<h4>PPO Variants and Implementation Details</h4>

There are several common variants and implementation details that can significantly affect PPO's performance:

1. <strong>PPO-Clip vs. PPO-Penalty</strong>: Besides the clipping approach described above, an alternative is to use a KL penalty term, similar to TRPO but without explicit constraint enforcement:
   $$L^{PENALTY}(\theta) = \mathbb{E}_t \left[ r_t(\theta) \cdot A_t - \beta \cdot D_{KL}(\pi_{\theta_{\text{old}}}(\cdot|s_t) \parallel \pi_\theta(\cdot|s_t)) \right]$$
   where $$\beta$$ is adapted based on whether the actual KL divergence is above or below a target value.

2. <strong>Normalized advantages</strong>: To reduce variance, advantages are often normalized to have zero mean and unit variance within each batch.

3. <strong>Value function clipping</strong>: Some implementations also clip the value function updates to prevent large changes:
   $$L^{VF-CLIP}(\phi) = \mathbb{E}_t \left[ \max((V_\phi(s_t) - V_t^{target})^2, (V_{\phi_{\text{old}}}(s_t) + \text{clip}(V_\phi(s_t) - V_{\phi_{\text{old}}}(s_t), -\epsilon, \epsilon) - V_t^{target})^2) \right]$$

4. <strong>Minibatch updates</strong>: Rather than updating on all collected data at once, PPO typically uses minibatch updates.

5. <strong>Orthogonal initialization and layer normalization</strong>: These techniques can improve training stability and performance.

In practice, PPO has become more widely adopted due to its simplicity and strong empirical performance, while TRPO remains valuable in contexts where theoretical guarantees are paramount or where the additional computational cost is justified.

<h2>Extensions and Applications of Trust Region Methods</h2>

<h4>Generalized Advantage Estimation (GAE)</h4>

A critical component of both TRPO and PPO is advantage estimation. Generalized Advantage Estimation (GAE), also introduced by Schulman et al., provides a way to trade off bias and variance in advantage estimates:

$$A^{GAE(\gamma, \lambda)}_t = \sum_{l=0}^{\infty} (\gamma \lambda)^l \delta_{t+l}$$

where $$\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)$$ is the TD error at time $$t$$, and $$\lambda \in [0, 1]$$ is a hyperparameter controlling the bias-variance trade-off.

GAE with $$\lambda = 0$$ corresponds to using TD errors as advantages (low variance but high bias), while $$\lambda = 1$$ corresponds to using Monte Carlo returns minus the baseline (high variance but low bias). Intermediate values of $$\lambda$$ interpolate between these extremes.

<h4>Trust Region Methods for Fine-Tuning Large Models</h4>

Trust region methods have found important applications in fine-tuning large pre-trained models, such as language models, for specific tasks. The challenge in such contexts is to adapt the model to perform well on the target task without forgetting the general knowledge encoded in its parameters.

For example, when fine-tuning a language model on a specific text generation task, a KL divergence penalty between the original and fine-tuned model can prevent catastrophic forgetting:

$$L(\theta) = L_{task}(\theta) - \beta \cdot D_{KL}(\pi_{\theta_0}(\cdot|s) \parallel \pi_\theta(\cdot|s))$$

where $$L_{task}(\theta)$$ is the task-specific loss, $$\pi_{\theta_0}$$ is the original model, and $$\beta$$ controls the strength of the regularization.

This approach ensures that the fine-tuned model doesn't deviate too far from the original, preserving its general capabilities while adapting to the specific task.