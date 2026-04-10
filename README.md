# LLM Model Parsing & LoRA Fine-Tuning Pipeline

**Khushi Mundhada**

---

## Overview

This project explores system-level interaction with Large Language Models (LLMs), focusing on understanding model architecture, performing parameter-efficient fine-tuning using LoRA, and experimenting with basic model composition.

The implementation includes:

* Model architecture parsing
* LoRA-based fine-tuning on a subset of the IMDB dataset
* Output comparison before and after fine-tuning
* System-level analysis of model behavior and limitations

---

## Design Decisions

The model parser was structured using a hierarchical traversal approach via `named_children()` to reflect the modular structure of transformer architectures. A shallow parser was implemented for readability, while a recursive version was introduced to allow deeper inspection of nested layers.

This design was chosen because:

* It mirrors how transformer models are internally organized (embeddings → blocks → heads)
* It keeps the output interpretable and structured like a tree

Trade-offs:

* A shallow parser improves readability but misses deeper layer details
* A fully recursive parser captures more information but becomes verbose and harder to interpret
* The current implementation balances clarity and depth by limiting recursion

---

## Working Conditions

* Platform: Google Colab
* GPU: NVIDIA T4
* RAM: ~12–16 GB

Approximate timings:

* Model loading: ~5–10 seconds
* Dataset preprocessing: ~2–3 seconds
* Fine-tuning (2 epochs on 5% IMDB): ~60–75 seconds

Bottlenecks:

* Tokenization overhead during dataset preparation
* Memory constraints when scaling to larger models

Mitigation:

* Used a small dataset subset for efficiency
* Applied LoRA to reduce the number of trainable parameters (~0.2%) instead of full fine-tuning

---

## Extensibility & Scalability

To extend this system to handle multiple models (10–50):

* Introduce a modular pipeline where each stage (load → parse → fine-tune → evaluate → merge) is reusable
* Store configurations (model name, tokenizer, LoRA parameters) in structured formats like JSON/YAML
* Run models sequentially or in parallel depending on available compute

What would break first:

* GPU memory and compute limitations when handling larger or multiple models
* Training time scaling significantly with model size

Handling different architectures (e.g., LLaMA vs Qwen):

* Abstract the parser to operate on generic module patterns instead of hardcoded layer names
* Use architecture-specific adapters for parsing and fine-tuning configurations
* Normalize outputs into a common structure for comparison across models

---

## Creativity & Future Vision

To evolve this into an automated model improvement system:

1. **Feedback-driven fine-tuning loop**
   Parse → Generate outputs → Evaluate → Identify weaknesses → Fine-tune → Repeat

2. **Dynamic adapter routing**
   Maintain multiple LoRA adapters for different tasks and dynamically select or combine them during inference

3. **Architecture-aware optimization**
   Use parsed model structure to decide where fine-tuning should be applied (e.g., attention vs MLP layers)

Unique improvement:

* Instead of blindly fine-tuning, use insights from the parser to guide *targeted modifications*, making the system more efficient and interpretable

---

## Honest Reflection

Straightforward:

* Loading models and datasets
* Applying LoRA using existing libraries

Challenging:

* Understanding internal transformer architecture
* Designing a reusable parser
* Distinguishing actual fine-tuning improvements from decoding effects

External Help:

* Documentation and LLM tools were used for understanding APIs and debugging
* Final implementation, structuring, and analysis were independently reasoned and refined

---

## Conclusion

This project demonstrates practical system-level understanding of LLMs, including architecture parsing, efficient fine-tuning, and model behavior analysis. It highlights both the potential and limitations of lightweight fine-tuning approaches and emphasizes the importance of thoughtful design, evaluation, and scalability considerations.
