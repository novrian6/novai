# NovAI Base 102M — Evaluation

This directory contains evaluation materials for
**NovAI Base 102M**, an Indonesian-language causal language model
developed by KaryaVirtual.

## Model

**Model:** KaryaVirtual/novai-base-102m

**Hugging Face:**
https://huggingface.co/karyavirtual/novai-base-102m

## Evaluation Scope

The evaluation is based on the publicly released NovAI Base 102M
checkpoint.

The purpose of this evaluation is to measure the model's language-model
performance and document reproducible results from the released
checkpoint.

## Model Configuration

| Property | Value |
|---|---:|
| Parameters | 102,558,240 |
| Architecture | Llama-style decoder-only Transformer |
| Layers | 12 |
| Hidden size | 720 |
| Attention heads | 12 |
| KV heads | 4 |
| Vocabulary size | 50,257 |
| Context length | 1,024 tokens |

## Training Background

NovAI Base 102M was pretrained from newly initialized model weights
on approximately 2 billion Indonesian-language tokens.

Training corpus:

**FineWeb-2 — Indonesian (`ind_Latn`)**

https://huggingface.co/datasets/HuggingFaceFW/fineweb-2

Training hardware:

**NVIDIA A100 40GB**

## Evaluation Method

Evaluation should be performed directly against the released
`KaryaVirtual/novai-base-102m` checkpoint.

All reported metrics in `results.md` should correspond to an actual
evaluation run and should not be estimated or inferred from training
loss alone.

## Reproducibility

The evaluation environment, model checkpoint, tokenizer, dataset,
evaluation configuration, and reported metrics should be documented
whenever applicable.

Model checkpoint:

https://huggingface.co/karyavirtual/novai-base-102m

Source repository:

https://github.com/novrian6/novai

## Important Note

NovAI Base 102M is a **base pretrained language model**, not an
instruction-tuned conversational model.

Therefore, evaluation results should be interpreted in the context of
causal language-model pretraining rather than as a direct measure of
chatbot or instruction-following capability.
