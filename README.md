# LLM Systems Engineering Assignment

## Overview

This project demonstrates a systems-level approach to working with Large Language Models (LLMs), including model architecture parsing, parameter-efficient fine-tuning, and model composition.

The focus is on modular design, efficiency, and scalability rather than just model performance.

---

## Task 1: Model Architecture Parsing

### Design Decisions

I implemented a recursive parser to inspect the internal structure of transformer-based models. The parser traverses each module and builds a hierarchical representation similar to a DOM tree.

Key design choices:
- Recursive traversal using `named_children()` for flexibility
- Capturing layer type, parameter count, and structure
- Separation of parsing and visualization logic for reusability

### Trade-offs

- Prioritized readability and modularity over extremely optimized traversal
- Did not rely on model-specific assumptions to ensure generalization

---

## Task 2: LoRA Fine-Tuning

### Design Decisions

- Used GPT2 due to its lightweight architecture and suitability for experimentation
- Applied LoRA (PEFT) to reduce training cost and memory usage
- Limited dataset size (IMDB subset) for faster execution
- Used short sequence length (128 tokens) for efficiency

### Observations

- The model shows slight adaptation to sentiment-related text
- LoRA significantly reduces the number of trainable parameters
- Training remains efficient even on CPU-based systems

---

## Task 3: Model Composition

### Approach

I merged the LoRA fine-tuned adapter into the base GPT2 model using `merge_and_unload()`.

This converts the model into a standalone version that contains fine-tuned knowledge without requiring external adapters.

### Observations

- The merged model produces outputs similar to the base model for short prompts
- This is expected due to limited fine-tuning (small dataset and fewer epochs)
- The merging process successfully integrates learned parameters into the base model

### Key Insight

Model composition is primarily about **structural integration and deployment efficiency**, not always visible output differences.

---

## Working Conditions

- Hardware: CPU (no GPU)
- RAM: ~8–16 GB
- Training Time: ~20–40 minutes
- Bottlenecks:
  - Training speed on CPU
  - Model loading time

### Solutions

- Reduced dataset size
- Limited training epochs
- Used LoRA to reduce computation

---

## Extensibility & Scalability

### Scaling to Multiple Models

To handle 10–50 models:
- Use parallel processing for parsing and evaluation
- Cache parsed model structures
- Maintain modular pipelines for each stage (parse → evaluate → fine-tune)

### Potential Bottlenecks

- Memory usage when loading multiple models
- Increased training time
- Evaluation complexity

### Handling Different Architectures

- Use generic parsing via `named_children()` instead of hardcoding
- Abstract architecture-specific components
- Build adapter layers for different model types (Llama, Qwen, etc.)

---

## Creativity & Future Vision

### 1. Self-Improving LLM Pipeline

A system that:
- Parses models
- Evaluates performance
- Detects weak areas
- Automatically fine-tunes or merges models

---

### 2. Multi-Model Voting System

- Multiple models generate outputs
- A scoring mechanism selects the best output
- Improves reliability and reduces hallucination

---

### 3. Selective Layer Merging

- Instead of merging entire models, selectively merge:
  - Attention layers
  - MLP layers
- Enables more controlled model composition

---

## Honest Reflection

### Straightforward Parts

- Loading models and tokenization
- Applying LoRA using PEFT

### Challenging Parts

- Understanding internal model structure
- Ensuring correct training setup (labels alignment)
- Interpreting subtle output differences

### Use of External Help

- Used documentation and references for:
  - Hugging Face Transformers
  - PEFT/LoRA implementation

- Ensured final implementation was understood and structured independently

---

## Conclusion

This project demonstrates how LLM systems can be built in a modular and scalable way. The focus on parsing, efficient fine-tuning, and model composition provides a strong foundation for developing advanced LLM pipelines.

Future work can extend this into fully automated systems capable of continuous learning and improvement.