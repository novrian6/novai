# NovAI Base 102M — Training and Evaluation Results

## Overview

This document records the training configuration and reported results for **NovAI Base 102M**, an Indonesian-language causal language model developed by KaryaVirtual.

The model was pretrained from newly initialized weights using a decoder-only Transformer architecture implemented through Hugging Face Transformers.

**Official model repository:**  
https://huggingface.co/karyavirtual/novai-base-102m

**Source repository:**  
https://github.com/novrian6/novai

---

## 1. Model

### Model Configuration

| Property | Value |
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
| Weight tying | Enabled |

The model configuration was explicitly constructed using `LlamaConfig` and instantiated with `LlamaForCausalLM`.

The parameter count was verified programmatically during training:

**102,558,240 parameters**

The training script also contains an explicit assertion requiring the parameter count to equal 102,558,240.

---

## 2. Initialization

NovAI Base 102M was initialized as a new model configuration rather than loaded from an existing pretrained foundation-model checkpoint.

The training code creates the model using:

`base_model = LlamaForCausalLM(config)`

The model weights were therefore initialized for the NovAI training run.

The project uses the Llama architecture implementation available through Hugging Face Transformers. This architectural implementation should not be interpreted as the use of pretrained Llama model weights.

No pretrained Llama, GPT, Claude, Gemini, or other commercial foundation-model checkpoint was used as the initial model checkpoint.

---

## 3. Training Dataset

The training corpus was prepared from:

**FineWeb-2**  
https://huggingface.co/datasets/HuggingFaceFW/fineweb-2

**Language subset:** `ind_Latn`

The local training corpus contained approximately **2 billion training tokens**.

A separate validation region of **10,000,000 tokens** was reserved from the end of the tokenized dataset.

The dataset was stored as a memory-mapped `uint16` token array and loaded using NumPy `memmap`.

The training script verifies that sampled token IDs do not exceed the model vocabulary size.

---

## 4. Tokenizer

NovAI Base 102M uses the GPT-2 tokenizer:

`gpt2`

The tokenizer vocabulary size is:

**50,257**

The tokenizer is loaded using:

`AutoTokenizer.from_pretrained("gpt2")`

The GPT-2 EOS token is used as the padding token when necessary.

**Upstream tokenizer:**  
https://huggingface.co/openai-community/gpt2

---

## 5. Training Configuration

The documented training run used the following configuration.

| Parameter | Value |
|---|---:|
| Batch size | 16 |
| Sequence length | 1,024 |
| Gradient accumulation | 8 |
| Tokens per optimization step | 131,072 |
| Target training tokens | 2,000,000,000 |
| Actual processed tokens | 2,000,027,648 |
| Validation split | 10,000,000 tokens |
| Learning rate | 6e-4 |
| Warmup steps | 300 |
| Optimizer | AdamW |
| Betas | (0.9, 0.95) |
| Weight decay | 0.1 |
| Maximum gradient norm | 1.0 |
| LR scheduler | Cosine with warmup |
| Precision | BF16 on supported A100 hardware |
| Gradient scaling | Disabled for BF16 |
| `torch.compile` | Disabled |
| Training objective | Causal language modeling |

---

## 6. Token Calculation

The effective number of tokens processed per optimization step was:

**Batch size × Sequence length × Gradient accumulation**

**16 × 1,024 × 8 = 131,072 tokens/step**

The number of optimization steps required to process approximately 2 billion training tokens was:

**ceil(2,000,000,000 / 131,072) = 15,259 steps**

The resulting number of tokens actually processed was:

**15,259 × 131,072 = 2,000,027,648 tokens**

Therefore, the completed training run processed approximately:

**2.000 billion tokens**

---

## 7. Training Hardware

The training configuration was designed to run on:

**NVIDIA A100 40GB**

The training script explicitly checks for CUDA availability and reports the active GPU.

It also issues a warning if the active GPU is not identified as an A100.

The A100 environment was used with BF16 automatic mixed precision when supported.

---

## 8. Training Objective

NovAI Base 102M was pretrained using causal language modeling.

For each sequence, the model receives token IDs as both the input and language-model labels.

The relevant training operation is equivalent to:

`outputs = model(input_ids=input_ids, labels=input_ids, use_cache=False)`

`LlamaForCausalLM` performs the appropriate next-token shift internally.

The objective is therefore next-token prediction over the training corpus.

---

## 9. Training Progress

The documented training run completed:

**15,259 / 15,259 steps**

The final reported training result was:

**Training loss: 1.7229**

The training run therefore reached the requested approximately 2-billion-token training target.

---

## 10. Validation

The training corpus reserved:

**10,000,000 tokens**

for validation.

Validation batches were sampled exclusively from this held-out region.

The validation function in the training script is:

`estimate_validation_loss(num_batches=10)`

Each validation batch contains:

**16 sequences × 1,024 tokens = 16,384 tokens**

Therefore, one validation estimate evaluates:

**10 × 16,384 = 163,840 tokens**

The reported validation loss for the completed run was:

**Validation loss: 1.7012**

### Important Methodological Note

The value **1.7012** is the reported validation-loss estimate produced by the training run.

The **10,000,000 token** figure describes the size of the held-out validation region. It does not mean that 10 million tokens were evaluated in every validation-loss calculation.

The validation function samples 10 batches, corresponding to approximately **163,840 tokens per validation estimate**.

---

## 11. Reported Results

| Metric | Result |
|---|---:|
| Training steps | 15,259 |
| Training tokens | 2,000,027,648 |
| Reserved validation tokens | 10,000,000 |
| Training loss | 1.7229 |
| Validation loss | 1.7012 |
| Parameters | 102,558,240 |

---

## 12. Loss Interpretation

The reported validation loss of **1.7012** is lower than the reported training loss of **1.7229**.

This difference should not automatically be interpreted as evidence of superior generalization.

Possible factors include:

- stochastic batch sampling;
- differences between training and validation sampling;
- timing of the reported measurements;
- the relatively small validation sample used by the validation estimation function;
- the fact that training loss is accumulated during optimization while validation loss is measured separately.

Further evaluation using a fixed and sufficiently large validation sample is recommended for more rigorous comparison.

---

## 13. Perplexity

Perplexity was not directly reported as an independently measured metric in the supplied training output.

For that reason, this repository does not claim a final benchmark perplexity value.

For a causal language model, perplexity can be derived from the corresponding average negative log-likelihood under the same evaluation methodology.

A future evaluation run should calculate perplexity explicitly over a fixed validation corpus and document:

- number of evaluated tokens;
- tokenizer;
- sequence length;
- stride;
- loss aggregation method;
- resulting validation loss;
- resulting perplexity.

---

## 14. Evaluation Status

The results documented here are primarily pretraining and validation results.

They should not be interpreted as comprehensive downstream benchmark results.

In particular, the current results do not establish performance on:

- instruction following;
- conversational dialogue;
- general knowledge QA;
- summarization;
- translation;
- classification;
- coding;
- safety evaluation;
- human preference evaluation.

NovAI Base 102M is a base language model and has not been presented as an instruction-tuned conversational model.

---

## 15. Reproducibility

The training configuration is documented in the associated training workflow.

Key reproducibility parameters include:

| Parameter | Value |
|---|---|
| Model | KaryaVirtual/novai-base-102m |
| Parameters | 102,558,240 |
| Training tokens | 2,000,027,648 |
| Validation region | 10,000,000 tokens |
| Batch size | 16 |
| Sequence length | 1,024 |
| Gradient accumulation | 8 |
| Learning rate | 6e-4 |
| Warmup | 300 steps |
| Optimizer | AdamW |
| Scheduler | Cosine with warmup |
| Hardware | NVIDIA A100 40GB |

The released model checkpoint and tokenizer are available from:

https://huggingface.co/karyavirtual/novai-base-102m

---

## 16. Limitations

NovAI Base 102M is a relatively small language model.

The reported loss values alone do not establish that the model is superior to other Indonesian language models.

Performance can vary substantially depending on:

- evaluation dataset;
- tokenizer;
- sequence length;
- prompt format;
- sampling strategy;
- benchmark methodology;
- model size;
- training data;
- downstream fine-tuning.

The model may generate incorrect, biased, repetitive, or factually unreliable outputs.

It should therefore be evaluated appropriately before deployment in production or high-impact applications.

---

## 17. Next Evaluation Steps

Future evaluation work may include:

1. Full fixed-corpus validation loss.
2. Explicit perplexity calculation.
3. Indonesian-language benchmark evaluation.
4. Comparison with similarly sized language models.
5. Zero-shot evaluation.
6. Downstream fine-tuning evaluation.
7. Instruction-tuned NovAI evaluation.
8. Generation-quality evaluation.

All future benchmark results should be reported separately from the pretraining loss results documented above.

---

## 18. Official Resources

### Hugging Face Model

https://huggingface.co/karyavirtual/novai-base-102m

### Hugging Face Organization

https://huggingface.co/karyavirtual

### GitHub

https://github.com/novrian6/novai

### KaryaVirtual

https://karyavirtual.com/

---

## 19. Citation

If you use NovAI Base 102M in research or derivative work, please cite:

@software{novriansyah_novai_2026,
  author       = {Novriansyah, Nova},
  title        = {NovAI Base 102M},
  year         = {2026},
  organization = {KaryaVirtual},
  url          = {https://huggingface.co/karyavirtual/novai-base-102m},
  repository   = {https://github.com/novrian6/novai}
}

---

## Summary

NovAI Base 102M is a **102,558,240-parameter Indonesian-language causal language model** pretrained from newly initialized weights.

The documented training run processed:

**2,000,027,648 tokens**

over:

**15,259 optimization steps**

using:

**NVIDIA A100 40GB**

with reported final metrics of:

| Metric | Result |
|---|---:|
| Training loss | 1.7229 |
| Validation loss | 1.7012 |

The released model is available at:

https://huggingface.co/karyavirtual/novai-base-102m
