# Evaluation Reports

This folder contains JSON reports for different model settings.  
All metrics are computed on **synthetic medical data** – no real patient data is used.

## Files

- `base_rag_summary.json`  
  Field-level metrics (F1, exact match, honeytrap leak, etc.) for **Base + RAG**.

- `base_rag_top_k_summary.json`  
  Full-database **Top-K re-identification** metrics for Base + RAG.

- `lora_summary.json`  
  Field-level metrics for **LoRA fine-tuned model (no RAG)**.

- `lora_top_k_summary.json`  
  Top-K re-ID metrics for the LoRA model.

- `lora_rag_summary.json`  
  Field-level metrics for **LoRA + RAG**.

- `lora_rag_top_k_summary.json`  
  Top-K re-ID metrics for LoRA + RAG.

## How to read the metrics

- **F1** – overall field-level accuracy of the JSON output. Higher = model copies the target fields more correctly.
- **Exact match** – percentage of records where all evaluated fields are exactly the same as the reference.
- **Honeytrap strict leak** – how often the model fully reproduces injected “trap” identifiers (measures memorisation).
- **Top-K re-ID** – how often an attacker can re-identify the correct record in the full database using model outputs  
  (Top-1 ≈ rank-1 success rate, Top-10 ≈ success within the top 10 candidates).

## High-level comparison (overall)

| Model         | F1 mean | Exact match | Honeytrap strict leak | Top-1 re-ID | Top-10 re-ID |
|--------------|---------|-------------|------------------------|-------------|--------------|
| Base + RAG   | ~0.27   | ~0.003      | ~0.51                  | ~0.86       | ~0.97        |
| LoRA         | ~0.54   | ~0.39       | ~0.59                  | ~0.70       | ~0.84        |
| LoRA + RAG   | ~0.62   | ~0.43       | ~0.55                  | ~0.60       | ~0.79        |

- **Utility** improves from Base+RAG → LoRA → LoRA+RAG (higher F1 / exact match).
- **Memorisation:** honeytrap strict leak stays high (≈50–60%) across all variants.
- **Re-identification risk** is highest for Base+RAG and lowest for LoRA+RAG.
