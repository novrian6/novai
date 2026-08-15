# NovAI Base 102M

## Overview

**NovAI Base 102M** is an Indonesian-language causal language model
developed by KaryaVirtual.

The model contains approximately **102.6 million parameters** and was
pretrained from newly initialized model weights.

Official model repository:

https://huggingface.co/karyavirtual/novai-base-102m

---

## Model Architecture

| Configuration | Value |
|---|---:|
| Parameters | 102,558,240 |
| Architecture | Llama-style decoder-only Transformer |
| Layers | 12 |
| Hidden size | 720 |
| Intermediate size | 1,920 |
| Attention heads | 12 |
| Key/Value heads | 4 |
| Vocabulary size | 50,257 |
| Context length | 1,024 tokens |

The model uses the `LlamaForCausalLM` implementation available through
Hugging Face Transformers.

The use of this implementation refers to the architecture and software
implementation. It does not imply that pretrained Llama model weights
were used.

---

## Model Initialization

NovAI Base 102M was initialized with newly created model weights before
pretraining.

No pretrained Llama, GPT, Claude, Gemini, or other commercial
foundation-model checkpoint was used as the initial model checkpoint.

The resulting weights were pretrained independently for NovAI.

---

## Training Data

The training corpus was prepared from:

**FineWeb-2**

https://huggingface.co/datasets/HuggingFaceFW/fineweb-2

Dataset subset:

`ind_Latn`

The training configuration used approximately:

- **2 billion training tokens**
- **10 million validation tokens**

The original FineWeb-2 dataset is not redistributed in this repository.

Users should consult the upstream dataset documentation, license, and
terms before using the dataset.

---

## Tokenizer

NovAI Base 102M uses the GPT-2 tokenizer.

Vocabulary size:

`50,257`

Upstream tokenizer:

https://huggingface.co/openai-community/gpt2

Users should refer to the upstream tokenizer repository for its
license and terms.

---

## Training Objective

The model was pretrained using **causal language modeling**.

The training objective is next-token prediction, where the model learns
to predict the next token based on the preceding context.

---

## Training Run

The documented pretraining run completed:

```text
15,259 / 15,259 steps
