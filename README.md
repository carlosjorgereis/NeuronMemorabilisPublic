# Neuron Memorabilis: The Incorruptible Architecture

<p align="center">
  <img src="neuron_memorabilis.png" alt="Neuron Memorabilis Banner" width="369">
</p>

***FIRST OF ALL, BE AWARE OF LICENCE***

Neuron Memorabilis is a bio‑adaptive neural unit designed to transcend the limitations of traditional, always‑plastic artificial neurons. It introduces individual temporal memory and self‑regulated plasticity, allowing each neuron to decide when to keep learning and when to “retire” as a consolidated expert.

🏛️ Core Concept
Instead of blindly following every gradient update, a Neuron Memorabilis acts as a self‑governed unit.
Each neuron/filter tracks its own historical merit across multiple time scales (m0, m2, m5, m8), evaluates its recent behavior against its long‑term record (the Humility Rule), and can become immortal: it keeps producing outputs, but stops receiving gradients.

This logic is fully encapsulated inside the neuron:

Works as a drop‑in replacement for nn.Linear and nn.Conv2d.
No custom training loop logic is required beyond registering the batch loss once.
Immortality and “de‑immortality” are handled internally by each unit.
🚀 Key Features

Full Encapsulation & Plug‑n‑Play

One class: NeuronMemorabilis.
Two modes via a flag:
linear=True → behaves like nn.Linear (MLPs, Transformers, FFNs).
linear=False → behaves like nn.Conv2d (CNNs).
Same API as standard PyTorch layers in your forward.
Individual Merit Memory (m0, m2, m5, m8)

Each output unit (neuron or filter) keeps four exponential memories:
m8: long‑term consolidated merit.
m5: medium‑term merit.
m2: short‑term merit.
m0: ultra short‑term merit.
Merit is computed from:
global batch performance (exp(−loss)), and
local activation statistics of each unit.
Humility Rule (Temporal Vigilance)

Recent merit (m0 + m2 + m5) must not collapse relative to long‑term merit (m8).
If it does, the neuron fails humility and cannot be immortal, even with high past performance.
This prevents “frozen” neurons from staying locked in when the data distribution shifts.
Immortality Pedestal (Self‑Freezing)

A neuron becomes immortal when:
it passes the Humility Rule, and
its long‑term merit m8 exceeds a configurable threshold (e.g., 0.95).
Immortal neurons:
still participate in the forward pass,
but their outputs are gradient‑detached internally, so they no longer update weights.
If conditions degrade (e.g., humility fails), the neuron can lose immortality and resume learning.
💰 Efficiency & Stability

Gradient Domestication:

Immortal units are automatically removed from the backpropagation chain.
Training focuses computation on units that have not yet converged.
Resource Savings:

As more units reach the immortality pedestal, effective FLOPs in backward pass can drop substantially.
Especially beneficial for large linear stacks (FFNs in Transformers, wide CNNs).
Noise Resilience & Continual Learning:

Long‑term merit + humility rule allow the neuron to:
resist stochastic noise and transient spikes, and
re‑enter plastic mode when sustained evidence indicates a paradigm shift.

---
📖 **Full Technical Documentation available in Neuron_Memorabilis_Specs_EN.pdf**

No additional control logic is needed: each NeuronMemorabilis instance updates its own memories, applies the humility rule, decides on immortality, and cuts gradients for immortal units internally.

📖 Deep Dive
The full mathematical rationale (merit definition, memory dynamics, humility inequality, and immortality thresholding) is available in the technical documentation (Neuron_Memorabilis_Specs.pdf).

---
*Created by The Architect. Stabilizing the future of intelligence through historical conviction.*
