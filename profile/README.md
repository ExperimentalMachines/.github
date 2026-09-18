# Experimental Machines

Independent measurements of AI hardware, small models trained in the open, and the tools to run open weights on a phone. The sites publish the logs behind their numbers.

## Two sites

| | |
|---|---|
| [experimentalmachines.org](https://experimentalmachines.org) | Benchmarks of AI accelerators at datacenter, laptop and phone scale: AMD MI300X against NVIDIA H200, Trainium and Inferentia against GPUs, Apple M5 against Snapdragon X2 Elite, Dimensity against Snapdragon. Source in [experimentalmachines.org](https://github.com/ExperimentalMachines/experimentalmachines.org). |
| [experimentalintelligence.org](https://experimentalintelligence.org) | Small models trained from scratch, with published weights, code and training logs. Source in [experimentalintelligence.org](https://github.com/ExperimentalMachines/experimentalintelligence.org). |

## OpenWeights

[OpenWeights](https://github.com/ExperimentalMachines/openweights) is an Android app that runs open-weight language models on the phone. It searches Hugging Face from inside the app, says whether a model fits the device before the download, and chats on the device with no account, no cloud and no telemetry. Two runtimes ship in one build: llama.cpp for GGUF files and ExecuTorch for compiled `.pte` files. It is on [Google Play](https://play.google.com/store/apps/details?id=io.github.alpharomercoma.openweights) under Apache 2.0.

The phone measurements behind the app are published as pages with their methods and raw results: [latency on five chips](https://experimentalmachines.github.io/openweights/latency.html), [the exported-window study](https://experimentalmachines.github.io/openweights/window.html) and [its reruns](https://experimentalmachines.github.io/openweights/reruns.html).

## OpenGrad

[OpenGrad](https://github.com/arjhinety/OpenGrad) is the frontier research lab repository: controlled post-training of small open-weight models in the open, with pre-registered evaluation gates, per-example records that recompute without a GPU, and negative results kept on the record rather than edited away. Study 001 on calibrated tool use in Qwen3.5-2B raised the tool-calling F1 from 0.6264 to 0.7470 with SFT, then measured what the promotion gate could not see: the promoted checkpoint refuses all 1,319 bare GSM8K questions and trails the base model by 12 to 23 points on IFEval, 8-shot maths and MMLU-Pro. Weight, code, training logs and the 93-claim audit are published — the frozen evidence at tag [`study-001`](https://github.com/arjhinety/OpenGrad/tree/study-001) and the write-up at [opengrad.arjhinety.com/studies/001](https://opengrad.arjhinety.com/studies/001).

## Models on Hugging Face

The organization publishes at [huggingface.co/experimentalmachines](https://huggingface.co/experimentalmachines): 31 model repositories.

**Exported by the pipeline.** 24 repositories across Qwen3, Qwen2.5, Llama 3.2 and SmolLM2, holding 113 ExecuTorch programs between them: every backend the exporter can build at every context window from 2k to 32k, 97 for XNNPACK on any arm64 CPU, 11 for Qualcomm's QNN, 4 for MediaTek and 1 for Vulkan. Each program is smoke-tested with the runner the app uses, and each folder's `config.json` estimates whether the window fits a 5 GB phone budget.

**Compiled by hand, and measured.** The families the exporter does not cover, at a 32,768-token window. Weights are int4 in groups of 32 with int8 dynamic activations, the layout Arm's KleidiAI kernels accelerate on Arm CPUs with the i8mm and dotprod extensions, which recent flagship and mid-range phones have. Each card reports the memory and speed measured on a Dimensity 9400. The Qwen3 file needs about 7 GB for its KV cache at the full window, so it ran only on a 16 GB phone; the LFM2.5 file needs about 1 GB.

| Repository | Base model | File |
|---|---|---|
| [LFM2.5-1.2B-Instruct-ExecuTorch-XNNPACK-32k](https://huggingface.co/experimentalmachines/LFM2.5-1.2B-Instruct-ExecuTorch-XNNPACK-32k) | LiquidAI/LFM2.5-1.2B-Instruct | 827 MB |
| [Qwen3-1.7B-ExecuTorch-XNNPACK-32k](https://huggingface.co/experimentalmachines/Qwen3-1.7B-ExecuTorch-XNNPACK-32k) | Qwen/Qwen3-1.7B | 1.35 GB |

**A defect worth knowing about.** Every LFM2.5 export published here before 2026-09-19 lost most of its tool calling, for two reasons found in September 2026: ExecuTorch's LFM2 definition never cleared the short convolution's state between prompts, so each prompt ran on the last one's, and the int4 weights were rounded rather than solved. On 141 held-out questions the old 1.2B export searched when needed on 10 percent of the questions that needed it and spoke of search results it had never fetched in a quarter of its replies; the re-export, with the state cleared in the graph and the int4 codes solved by GPTQ on the delegate's own grid, reads 49 and 1 percent at the same size and speed. The 1.2B repository carries the fixed file. The 2.6B export and the files beside the abliterated weights were withdrawn rather than shipped broken, and return when each has its own solve. The method, the numbers and what is still unknown are in [a compiled LFM2.5 that calls tools](https://github.com/alpharomercoma/openweights/blob/main/docs/research/executorch-state-and-recipes.md).

**Abliterated.** Refusal-direction ablation of the LFM2.5 models with [heretic](https://github.com/p-e-w/heretic), with a 200-trial search that trades refusal rate against KL divergence from the original and a chosen point on that Pareto front. Each repository holds the merged safetensors weights. The ExecuTorch export that used to sit beside them was withdrawn on 2026-09-19; see the defect below.

| Repository | Base model |
|---|---|
| [LFM2.5-1.2B-Instruct-heretic](https://huggingface.co/experimentalmachines/LFM2.5-1.2B-Instruct-heretic) | LiquidAI/LFM2.5-1.2B-Instruct |
| [LFM2.5-2.6B-heretic](https://huggingface.co/experimentalmachines/LFM2.5-2.6B-heretic) | LiquidAI/LFM2.5-2.6B |

**Tool-calling research.** The [OpenGrad](https://github.com/arjhinety/OpenGrad) study publishes the whole tool-use ladder — the promoted checkpoint and its parent, the corpus behind them, and the deployment formats. Scores are on the frozen 1,277-example confirmatory partition, the pre-registered internal evidence for tool routing.

| Repository | What it is | Confirmatory call F1 / recall |
|---|---|---|
| [OpenGrad-Qwen3.5-2B-M1-DPO-CanonicalV2-Final-v2](https://huggingface.co/arjhinety/OpenGrad-Qwen3.5-2B-M1-DPO-CanonicalV2-Final-v2) | `dpo-checkpoint-30`, selected on the frozen DEV partition and promoted under `tool_use_promotion_v4`; checkpoints 30, 60, 90 and 120 all published | **0.7548 / 0.7748** |
| [OpenGrad-Qwen3.5-2B-M0-SFT-CanonicalV2-Final](https://huggingface.co/arjhinety/OpenGrad-Qwen3.5-2B-M0-SFT-CanonicalV2-Final) | the parent M0 SFT checkpoint 1800, the calibrated frontier the DPO stage builds on | 0.7470 / 0.7594 |
| [OpenGrad-Qwen3.5-2B-M1-DPO-CanonicalV2-Final-v2-GGUF](https://huggingface.co/arjhinety/OpenGrad-Qwen3.5-2B-M1-DPO-CanonicalV2-Final-v2-GGUF) | BF16 plus nine llama.cpp quantization formats from one importance matrix; Q6_K at 1.45 GiB is the recommended one | Q6_K 0.7572 / 0.7881 |
| [QwenGrad-DPO](https://huggingface.co/experimentalmachines/QwenGrad-DPO) | the promoted checkpoint mirrored in this organization in standard `transformers` layout | same weights as the first row |
| [QwenGrad-Qwen3.5-2B-M1-DPO-v2-ExecuTorch-CPU](https://huggingface.co/experimentalmachines/QwenGrad-Qwen3.5-2B-M1-DPO-v2-ExecuTorch-CPU) | XNNPACK CPU deployment build at 8da4w, 2.89 GiB and 3.09× smaller than fp32, shipping the pinned renderer, the frozen prompt ids and its own scorer | export, 5,760-token window |

**Corpora and evaluation records.**

| Dataset | Contents |
|---|---|
| [OpenGrad-ToolPolicy-Canonical-v2](https://huggingface.co/datasets/arjhinety/OpenGrad-ToolPolicy-Canonical-v2) | 173,237 records from four sources, 161,966 trainable — the corpus behind M0 |
| [OpenGrad-ToolPolicy-Canonical-v2-M0-snapshot](https://huggingface.co/datasets/arjhinety/OpenGrad-ToolPolicy-Canonical-v2-M0-snapshot) | the M0 training snapshot, 103k records |
| [OpenGrad-ToolPolicy-Canonical-v2-minus-xlam](https://huggingface.co/datasets/arjhinety/OpenGrad-ToolPolicy-Canonical-v2-minus-xlam) | the xlam-held-out ablation corpus, 116k records |
| [OpenGrad-ToolPolicy-Canonical-v1](https://huggingface.co/datasets/arjhinety/OpenGrad-ToolPolicy-Canonical-v1) | the earlier normalization-v1 corpus, 214k records, kept for provenance |
| [OpenGrad-Qwen3.5-2B-M0-SFT-CorpusV1-evaluation](https://huggingface.co/datasets/arjhinety/OpenGrad-Qwen3.5-2B-M0-SFT-CorpusV1-evaluation) | evaluation records for the corpus-v1 M0 run, whose checkpoints are no longer published |

## Tooling

[executorch-model-exporter](https://github.com/ExperimentalMachines/executorch-model-exporter) exports small open-weight models from Hugging Face to ExecuTorch on GitHub-hosted runners, smoke-tests each program with the runner the app uses, and publishes to this organization. It has shipped the 24 repositories above. Its XNNPACK path covers Qwen3, Qwen2.5, Llama 3.2 and SmolLM2, and it also builds for Qualcomm's QNN, for MediaTek and for Vulkan. The LFM2.5 files were exported by hand, with the recipes recorded in their cards.

## Where the studies live

The benchmark repositories linked from experimentalmachines.org, such as MI300X-vs-H200, torchneuronx and snapdragon-vs-m5, and the training code and weights linked from experimentalintelligence.org, are still published under [alpharomercoma](https://github.com/alpharomercoma) on GitHub and Hugging Face. The sites are the index to them.

OpenGrad carries its own index. The training code and configs, the run ledger and per-checkpoint evaluation records, the derived result index and the study reports — SFT and DPO execution, the general-capability diagnosis, the quantization evaluation, the errata and the campaign audit — are committed in [github.com/arjhinety/OpenGrad](https://github.com/arjhinety/OpenGrad), frozen at tag [`study-001`](https://github.com/arjhinety/OpenGrad/tree/study-001). Its weights, training corpora and evaluation records are published under [arjhinety](https://huggingface.co/arjhinety) on Hugging Face, and the study write-ups at [opengrad.arjhinety.com](https://opengrad.arjhinety.com).

## Contact

alpha@experimentalmachines.org
