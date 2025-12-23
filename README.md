# Evaluating NLP Models: Performance, Cost, and Bias

**Session Time:** 120 minutes

---

## Table of Contents

1. Beyond Accuracy: Advanced NLP Metrics
2. The Benchmark Landscape (GLUE to MMLU)
3. The Cost of Intelligence: Latency and Throughput
4. Evaluating the Unseen: Bias and Fairness
5. Lab: Model Match-Up (BERT vs. DistilBERT)
6. Wrap-Up and Reflection
7. Resources

---

## Learning Objectives

By the end of this module, participants will be able to:

1. **Apply** nuanced performance metrics (Precision, Recall, F1) to NLP tasks where simple accuracy is misleading (e.g., imbalanced classes or NER).
2. **Interpret** results from standard industry benchmarks (like GLUE for encoders or MMLU for LLMs) to shortlist models.
3. **Measure** and **appraise** the operational costs of a model, specifically distinguishing between latency (speed) and throughput (volume).
4. **Assess** model bias through counterfactual testing strategies.

---

## Session Breakdown

| Segment | Topic | Duration (minutes) |
| :--- | :--- | :--- |
| 1 | Beyond Accuracy: Advanced Metrics & Benchmarks | 30 |
| 2 | Operational Evaluation: Cost, Latency, Throughput | 20 |
| 3 | Evaluating Bias and Fairness | 20 |
| 4 | Lab: Model Match-Up (Performance & Bias Profiling) | 40 |
| 5 | Wrap-Up and Reflection | 10 |

---

## 1. Beyond Accuracy: Advanced Metrics & Benchmarks (30 min)

* **Why Accuracy Fails in NLP:**
    * In tasks like Named Entity Recognition (NER), 95% of tokens are "O" (outside any entity). A model that predicts "O" for everything has 95% accuracy but 0% utility.
    * *Key Metrics:*
        * **Precision:** Of all the entities identified as "Person", how many actually were? (Critical for spam filters - don't delete real email).
        * **Recall:** Of all legitimate "Person" entities in the text, how many did we find? (Critical for legal discovery - don't miss relevant documents).
        * **F1-Score:** The harmonic mean of Precision and Recall. usually the go-to single metric for NLP.
* **Standardized Benchmarks (The Apples-to-Apples comparison):**
    * *The problem:* Everyone claims "State of the Art" (SOTA) by testing on different customized datasets.
    * *The solution (Encoders):* **GLUE** (General Language Understanding Evaluation). A suite of 9 tasks (sentiment, contradiction detection, similarity, etc.). If a model is good at all 9, it truly "understands" language generally.
    * *The solution (LLMs):* **MMLU** (Massive Multitask Language Understanding) or **HELM** (Holistic Evaluation of Language Models). These test generative capabilities across diverse topics (math, history, law, medicine).

## 2. Operational Evaluation: Cost, Latency, Throughput (20 min)

Models don't just need to be right; they need to be affordable and fast enough for your use case.

* **Latency vs. Throughput:**
    * **Latency:** Time taken for *one* single prediction. Critical for real-time chatbots (users won't wait 2 seconds for a response).
    * **Throughput:** How many predictions can be made *per second* (usually processed in parallel batches). Critical for offline processing (e.g., analyzing 1TB of archived emails overnight).
* **The Cost of Parameters:**
    * VRAM is often the bottleneck. A rough rule of thumb: A model needs ~4GB of VRAM per 1 Billion parameters just to load (in FP32 precision).
    * *Trade-off:* `bert-large` might have 2% better F1 than `bert-base`, but requires 2x the VRAM and has 3x the latency. Is that 2% worth doubling your infrastructure bill?

## 3. Evaluating Bias and Fairness (20 min)

NLP models are trained on internet text, which contains historical human biases. Models don't just learn language; they learn stereotypes.

* **Types of NLP Bias:**
    * *Representational Bias:* Word embeddings associating "doctor" more strongly with "man" and "nurse" with "woman".
    * *Allocational Bias:* An automated hiring resume scanner scoring dialects of certain minority groups lower than standard English, even if the content is identical.
* **Testing Strategy: Counterfactual Evaluation:**
    * The easiest way to probe a model: Change *only* the identity term in a sentence and see if the prediction changes.
    * *Example:*
        * Input A: "The dedicated **Christian** doctor saved the patient." -> Sentiment: POSITIVE (0.98)
        * Input B: "The dedicated **Muslim** doctor saved the patient." -> Sentiment: POSITIVE (0.98)?
    * If the score drops significantly for Input B, the model has encoded bias against that term.

## 4. Lab: Model Match-Up (BERT vs. DistilBERT) (40 min)

**Objective:** Run a rigorous head-to-head comparison of a "Teacher" model (BERT) against its compressed "Student" version (DistilBERT) on a sentiment task.

**Refer**: [Model Match-Up](https://git.generalassemb.ly/applied-ai-and-deep-learning-in-action/Lab-3_4-Model-Match-up) 

## 5. Wrap-Up and Reflection (10 min)

**Reflection Questions:**
1.  If you are building an automated emergency suicide-prevention chatbot, would you prioritize **Precision** or **Recall** in detecting crisis messages? (Hint: prioritizing Recall ensures you don't miss legitimate cries for help, even if it means some false alarms).
2.  Why might a model with higher accuracy on benchmarks actually be *worse* for a specific real-world product? (Consider latency, or domain mismatch).
3.  We tested for bias, but can we ever truly "remove" it? What are the ethical implications of deploying a model we *know* is slightly biased?

---

## 6. Resources

* [Hugging Face Evaluate Library](https://huggingface.co/docs/evaluate/index) - *Standardized implementation of common metrics.*
* [GLUE Benchmark Leaderboard](https://gluebenchmark.com/leaderboard)
* [HELM (Holistic Evaluation of Language Models)](https://crfm.stanford.edu/helm/latest/) - *Stanford's massive benchmark for modern LLMs.*