# Chain-of-Thought Reasoning without Prompting: Unlocking the True Potential of Large Language Models

---

## Introduction

Reasoning capability - involves understanding, deduction, and inference. To achieve this, there are two approaches: **prompting techniques** or **fine-tuning models on specialized datasets**—both of which come with their own challenges.

Authors of this paper have introduced **Chain-of-Thought (CoT) Decoding without Prompting**, which enhances the reasoning ability of LLMs by altering the decoding strategy. This approach not only eliminates the need for human intervention but also reveals the true extent of a model’s inherent reasoning potential.

---

## Challenges in Achieving Reasoning Capability

### 1. Prompting Approaches
- **Few-shot Prompting:** Providing relevant examples within the prompt to guide the model.
- **Zero-shot Prompting:** Giving explicit instructions to the model without prior examples.

**Limitations:**
- **Human Effort:** Crafting effective prompts requires manual intervention and expertise.
- **Inconsistency:** Performance can vary significantly across different tasks.

### 2. Model Fine-Tuning
- **Supervised Training:** Models are fine-tuned on domain-specific reasoning tasks.

**Limitations:**
- **High Cost:** Training models on specialized datasets is computationally expensive.
- **Data Scarcity:** Obtaining high-quality, supervised reasoning data is difficult.

---

## Proposed Solution: Chain-of-Thought (CoT) Decoding without Prompting

### Why CoT-Decoding?
- CoT paths incorporate intermediate reasoning steps, leading to more accurate final answers.
- Unlike prompting methods, CoT-decoding **explores inherent reasoning capabilities** without external guidance.
- It highlights the model’s ability to break down problems into logical steps and derive the correct answer organically.

---

## Understanding the Standard Decoding Approach

### How Does Standard Decoding (Top-k) Work?
1. **Autoregressive Generation:**
    - LLMs generate tokens sequentially, where each new token depends on the previously generated tokens.
2. **Logits and Probabilities:**
    - At each decoding step:
        - The model produces logits (raw scores) over the vocabulary.
        - These logits are converted to probabilities using a softmax function.
        - The token with the highest probability is chosen (greedy decoding).

**Limitation:**
- **Limited Exploration:** Greedy decoding tends to follow the most probable path, often missing out on alternative reasoning paths that may lead to a more accurate solution.

---

## Key Findings from the Paper

### Major Observations:
- **Multiple Valid Paths:** There are often multiple plausible reasoning paths, but the path with the highest probability (k=0) is not always the best.
- **CoT Paths Tend to Have Lower Initial Probabilities:** Reasoning paths often explore less probable branches initially but yield higher-confidence final answers.
- **Higher Confidence in CoT Paths:** The final answer generated through CoT paths shows higher confidence, quantified by a greater probability difference (Δ) between the top token and the next-best token.

---

## What is CoT-Decoding?

### Objective:
To incorporate reasoning into LLMs without requiring human-crafted prompts.

### How Does It Work?
- CoT-decoding explores alternative paths and selects the one with the **highest answer confidence (Δ value)**.
- By avoiding human intervention, this approach provides a **more authentic measure** of the model’s true reasoning capability.

---

## Deep Dive into the Strategy

### Experimenting with PaLM-2
- **Comparing k=0 and k>0:**
    - **k=0 (Greedy Decoding):** The model directly attempts to solve the problem without exploring intermediate reasoning steps.
    - **k>0 (CoT-Decoding):** The model explores diverse paths by considering multiple alternatives before converging on an answer.

**Why Does Greedy Decoding Struggle?**
- **Lack of Task Awareness:** Models often do not recognize the complexity of a task.
- **Training Bias:** LLMs are predominantly trained on simpler problems with direct answers, which limits their ability to explore intermediate reasoning steps.

---

## Identifying CoT Paths: The Key Intuition

### Significant Probability Difference (Δ)
- CoT paths exhibit a **large probability gap between the top token and the next-best token**.
- **Example:**
    - For the answer **“60”**, consider:
        - Token 1: “6” → \( p(6) = 0.98 \)
        - Token 2 (alternative): “8” → \( p(8) = 0.1 \)
    - \( Δ = 0.98 - 0.1 = 0.88 \)

- This probability difference is computed for all answer tokens and averaged to identify the path with the **highest Δ value**.

---

## Performance Comparison: CoT-Decoding vs. Other Approaches

| Approach                              | GSM8K (top-100)     | Year Parity |
|----------------------------------------|--------------------|--------------------------|
| Greedy Decoding (k=0)                  | 44.0%              | 57.0%                    |
| Top-k Decoding (Max Log-Prob)          | 37.0%              | 55.0%                    |
| Top-k Decoding (Length-Normalized)     | 51.0%              | 57.0%                    |
| CoT-Decoding (Max Δ Confidence)        | **72.0%**          | **95.0%**                |

**Conclusion:** CoT-Decoding consistently outperforms traditional approaches by leveraging diverse reasoning paths.

---

## Exploring Sampling to Reveal Reasoning Paths

### Hypothesis:
Could introducing sampling uncover different reasoning paths?

### Why Sampling Falls Short:
- **First Token Bias:** LLMs have an inherent tendency to select the first token that leads directly to a simple answer.
- **Randomness Doesn’t Guarantee CoT:** While sampling introduces randomness, it does not explicitly encourage the exploration of CoT paths.

---

## Branching at Different Decoding Stages

### Early Branching (First Step)
- Encourages greater diversity in decoding paths.
- Ideal for tasks where multiple reasoning paths can lead to correct solutions.

### Mid-Path Branching
- Effective for tasks like **year parity problems**, where mid-level branching captures intermediate reasoning better.

### Late Branching
- Less effective due to path dependency—once initial tokens are selected, the model’s subsequent choices become constrained, limiting exploration.

---

## Aggregation of Decoding Paths for Stability

### Why Aggregate Paths?
- Relying on a **single path** can be unstable, as small changes in logits may lead to different outcomes.
- Aggregating multiple paths mitigates this instability and enhances accuracy.

### Weighted Confidence Aggregation:
- Calculate the total confidence for each unique answer.
- Select the answer that **maximizes weighted confidence.**

### Example:
- **Answer “60”** occurs across 4 paths with different confidence scores.
- **Answer “40”** occurs across 3 paths.
- The final answer is chosen based on the **average confidence score** of all paths leading to each answer.

---

## Experiments and Key Results

### CoT-Decoding Effectiveness
- CoT-decoding significantly increases reasoning capability compared to approaches such as:
    - Top-k sampling
    - Nucleus sampling
    - Beam search
- Consistent improvements were observed across all tested LLMs—**PaLM-2, Mistral, and Gemma**—and across different model scales.

### Comparable Performance to Instruction-Tuned Models
- Pre-trained models enhanced with CoT-decoding achieved performance comparable to instruction-tuned models without requiring additional fine-tuning.

### Impact of Increasing k
- Increasing **k** leads to higher accuracy, suggesting that CoT paths may reside in lower-ranked paths.
- However, CoT path ranks were consistently better in instruction-tuned models, highlighting the interplay between training methodology and reasoning performance.

### CoT Reasoning and Task Difficulty
- **Task Complexity Dependency:**
    - Accurate CoT paths are more likely when tasks require **1-2 step knowledge manipulation**.
    - Reasoning becomes more challenging with **3 or more steps**, where models struggle to track intermediate information, leading to failure in complex tasks.

### Arithmetic Task Flaws
- For arithmetic tasks, models exhibited a **left-to-right calculation pattern**, exposing a training bias.
- Combining CoT-decoding with **external prompts** significantly improved performance in complex tasks.

---

## Summary of Key Contributions

1. **Introduced CoT-Decoding without Prompting:**
    - Captures reasoning capability without requiring prompt engineering.

2. **Demonstrated Higher Answer Confidence in CoT Paths:**
    - Paths with higher Δ values align with correct answers 88% of the time.

3. **Improved Accuracy through Path Aggregation:**
    - Aggregating paths based on answer confidence stabilizes results.

4. **Revealed Limitations of Sampling and Late Branching:**
    - Sampling does not reliably explore CoT paths.
    - Late branching is influenced by initial token choices, limiting diversity.

5. **Validated Effectiveness Across Multiple LLMs:**
    - CoT-decoding proved effective across PaLM-2, Mistral, and Gemma, enhancing pretrained models to achieve instruction-tuned performance levels.

---
