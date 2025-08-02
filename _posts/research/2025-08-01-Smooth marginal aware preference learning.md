## Learning from Preferences with Stability: A Deep Dive into Marginal-Aware Fine-Tuning

In the era of large language models (LLMs), *fine-tuning with human preferences* has become central to aligning models with user intent. While conventional Reinforcement Learning from Human Feedback (RLHF) approaches have been instrumental, new methods are emerging that blend mathematical precision with data efficiency.

In this blog, we explore recent advances and **smooth extension** based on the paper [“Marginal-Aware Preference Learning” (arXiv:2502.10993)](https://arxiv.org/pdf/2502.10993), focusing on **Rejective Sampling**, **MaxNeg-MinPos optimization**, **smooth extensions with log-exp-sum**, and a novel **lower bound control** for training stability. We aim to distill both the intuition and the equations that power this robust framework.

---

### Data Generation with Rejective Sampling

Traditional preference datasets often rely on uniform sampling or bandit feedback. However, **Rejective Sampling** introduces a more *discriminative selection*, prioritizing preference pairs where the difference in model scores is marginal. This concentrates learning on "hard" examples.

**Key idea**: Sample a pair (positive, negative) not just at random, but conditioned on the marginal gap between them being *informative but not too noisy*.

This rejects highly confident or noisy samples, creating better learning signals and reducing variance during preference optimization.

---

### Marginal-Aware Preference Learning with MaxNeg-MinPos Pair

Unlike conventional Direct Preference Optimization (DPO), the **Marginal-Aware** approach focuses on the *most informative pairs*:

- Pick **hardest positive** (minimum among positives)
- Pick **hardest negative** (maximum among negatives)

Let’s define the setup:

- Let $\pi$ be the fine-tuned policy, and $\pi_0$ be the base policy.
- $A^+$ and $A^-$ are the positive and negative responses to a prompt.

Then we can combine this strategy with the following algorithms for preference learning: 

#### DPO (Direct Preference Optimization)

Objective

$$
\max \mathbb{E}_{(x, y^+, y^-)} \left[ \log \frac{\pi(y^+ \mid x)/\pi_0(y^+ \mid x)}{\pi(y^- \mid x)/\pi_0(y^- \mid x) + \pi(y^+ \mid x)/\pi_0(y^+ \mid x)} \right]
$$

This favors higher reward for $y^+$ over $y^-$, but doesn't focus on extremal pairs.

#### CPO (Contrastive Preference Optimization)

Aims to maximize the difference in scores across a batch using:

$$
\max_{\theta} \left( \min_{y^+ \in A^+} \log \pi(y^+ \mid x) - \max_{y^- \in A^-} \log \pi(y^- \mid x) \right)
$$

This creates a *MaxNeg-MinPos* structure, focusing on the most confusing contrast within a batch.

#### ORPO (One-sided Regularized Preference Optimization)

Adds regularization on only one side (typically the negative):

$$
\min_{\theta} \max_{y^- \in A^-} \left[ \log \pi(y^- \mid x) - \lambda \log \pi_0(y^- \mid x) \right]
$$

This keeps the negative logits from deviating too far from the base policy, stabilizing learning.

---

### Smoothing the Max-Min Objective with Log-Exp-Sum

To make optimization smoother and differentiable (especially across batches), the authors approximate max and min using the **log-sum-exp** trick:

**Smooth Maximum:**
$$
\max_i a_i \approx \frac{1}{\tau} \log \sum_i \exp(\tau a_i)
$$

Smooth Minimum:
$$
\min_i a_i \approx -\frac{1}{\tau} \log \sum_i \exp(-\tau a_i)
$$

Where $\tau$ is the temperature controlling approximation sharpness.

**Applied to preference learning:**

- Smooth-Max over negative logits: encourages exploration over many bad examples.
- Smooth-Min over positive logits: reduces brittleness to outliers.

From the smoothing derivation:

$$
\text{Smooth MaxNeg-MinPos:} \quad \frac{1}{\tau} \left( \log \sum_{j} \exp(\tau \cdot \log \pi(y^-_j)) - \log \sum_{i} \exp(\tau \cdot \log \pi(y^+_i)) \right)
$$

This approximation allows gradients to flow across all samples, not just the hardest one.

---

### Upper Bound Control in Smoothing for Training Stability

#### Rewrite of the Smoothed Maximum

Let $\{A_1, A_2, \dots, A_n\}$ be a batch of scalar scores (e.g., logits or log-probs).

Define:

- $A_{(1)} = \max_i A_i$
- Let $\delta_i = A_i - A_{(1)} \le 0$

Then we can rewrite smoothed maximum:

$$
\begin{split}
\text{SmoothMax}(A) &= \frac{1}{\tau} \log \left( \sum_{i=1}^{n} \exp(\tau A_i) \right)\\
&= \frac{1}{\tau} \log \left( \exp(\tau A_{(1)}) \cdot \sum_{i=1}^{n} \exp(\tau (A_i - A_{(1)})) \right) \\
&= A_{(1)} + \frac{1}{\tau} \log \left( \sum_{i=1}^{n} \exp(\tau (A_i - A_{(1)})) \right)
\end{split}
$$

This can be written as:

$$
\text{SmoothMax}(A) = A_{(1)} + \frac{1}{\tau} \log \left( 1 + \sum_{i \ne (1)} \exp(-\tau \delta_i) \right)
$$



#### Error Analysis

Define the **smoothing error**:

$$
\epsilon_{\text{max}} = \text{SmoothMax}(A) - A_{(1)} = \frac{1}{\tau} \log \left( 1 + \sum_{i \ne (1)} \exp(-\tau \delta_i) \right)
$$

Now consider the largest contributing term in the sum: let $\delta = A_{(1)} - A_{(2)}$.

Then:

$$
\epsilon_{\text{max}} \le \frac{1}{\tau} \log \left( 1 + (n - 1) \exp(-\tau \delta) \right)
$$

---

#### Upper Bound Approximation and Control

We define the upper bound on this error as:

$$
\Delta := \frac{1}{\tau} \log \left( 1 + (n - 1) \exp(-\tau \delta) \right)
$$

This value quantifies **how far the smoothed max may fall below the true max**.

To **prevent vanishing gradients** and maintain consistent learning signals, we require:

$$
\Delta \ge \epsilon
$$

This gives us a **temperature condition**:

$$
\frac{1}{\tau} \log \left( 1 + (n - 1) \exp(-\tau \delta) \right) \ge \epsilon
$$

---

#### Temperature Scheduling Strategy

We can **solve for $\tau$** numerically to maintain $\Delta \ge \epsilon$ for a target minimum signal.

Assuming large $\tau$ (sharp max), the bound tightens:

$$
\Delta \approx \frac{(n - 1)}{\tau} \exp(-\tau \delta)
$$

Solving:

$$
\exp(-\tau \delta) \ge \frac{\tau \epsilon}{n - 1}
\quad \Rightarrow \quad
\tau \le \frac{1}{\delta} \log \left( \frac{n - 1}{\tau \epsilon} \right)
$$

This implicit equation shows that:

- **Smaller $\delta$** (small gap between max and 2nd max) → **larger $\Delta$** → safer learning
- As $\delta$ grows, $\tau$ must also increase to preserve the same signal strength

These error bounds help guarantee:

- **Gradient flow is non-zero**
- **No collapse to overconfident logits**
- **Training stability over long epochs**
- **Temperature $\tau$ becomes a tuning knob to control regularization**

Similar analysis can be applied to minimum smoothing method. 

---

### Take-away

The Marginal-Aware Preference Learning framework bridges *rigorous optimization* and *data-efficient fine-tuning*, creating a more stable, interpretable alternative to RLHF.

Keypoints: 

- Sampling harder pairs with rejective sampling  
- Focusing on max-min logits  
- Smoothing the objective with log-exp-sum  
- Controlling difference bounds for stability 

### Experimental Results

To be released... 