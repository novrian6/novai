# NovAI

**Indonesian Language Model Development by KaryaVirtual**

NovAI is an AI and language-model development initiative by
[KaryaVirtual](https://karyavirtual.com/), focused on Indonesian-language
NLP, language-model pretraining, fine-tuning, evaluation, and practical
AI engineering.

## NovAI Base 102M

The first publicly released NovAI base model is **NovAI Base 102M**,
an Indonesian-language causal language model with approximately
102.6 million parameters.

### Model

**Hugging Face:**

https://huggingface.co/karyavirtual/novai-base-102m

### Model specifications

| Specification | Value |
|---|---:|
| Parameters | 102,558,240 |
| Architecture | Llama-style decoder-only Transformer |
| Layers | 12 |
| Hidden size | 720 |
| Intermediate size | 1,920 |
| Attention heads | 12 |
| KV heads | 4 |
| Vocabulary size | 50,257 |
| Context length | 1,024 tokens |
| Tokenizer | GPT-2 tokenizer |
| Training tokens | ~2 billion |
| Validation tokens | ~10 million |

## Pretraining

NovAI Base 102M was pretrained from newly initialized model weights
on approximately 2 billion Indonesian-language tokens.

The training corpus was prepared from
[FineWeb-2](https://huggingface.co/datasets/HuggingFaceFW/fineweb-2),
using the Indonesian Latin (`ind_Latn`) subset.

The model architecture uses the Llama implementation available through
Hugging Face Transformers. The model weights were initialized and
pretrained separately for NovAI.

The model does not use pretrained Llama, GPT, Claude, Gemini, or other
commercial foundation-model weights as its base checkpoint.

## Training

The documented training run completed:

- Training steps: 15,259 / 15,259
- Training tokens: approximately 2,000,027,648
- Validation tokens: approximately 10 million
- Training loss: 1.7229
- Validation loss: 1.7012
- Training hardware: NVIDIA A100 40GB

These figures refer to the publicly documented NovAI Base 102M
pretraining run.

## Repository Purpose

This GitHub repository contains project documentation, technical
materials, evaluation code, examples, and supporting resources for
NovAI.

The model weights are hosted separately on Hugging Face.

## Project Structure

```text
novai/
├── README.md
├── LICENSE
├── CITATION.cff
└── docs/
    └── novai-base-102m.md
