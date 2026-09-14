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

The organization publishes at [huggingface.co/experimentalmachines](https://huggingface.co/experimentalmachines).

**Compiled for phones.** ExecuTorch 1.4 programs for the XNNPACK CPU backend with a 32,768-token context window. Weights are int4 in groups of 32 with int8 dynamic activations, the layout Arm's KleidiAI kernels accelerate on Arm CPUs with the i8mm and dotprod extensions, which recent flagship and mid-range phones have. Each of the three cards reports the memory and speed measured on a Dimensity 9400. The Qwen3 file needs about 7 GB for its KV cache at the full window, so it ran only on a 16 GB phone; the LFM2.5 files need about 1 GB.

| Repository | Base model | File |
|---|---|---|
| [LFM2.5-1.2B-Instruct-ExecuTorch-XNNPACK-32k](https://huggingface.co/experimentalmachines/LFM2.5-1.2B-Instruct-ExecuTorch-XNNPACK-32k) | LiquidAI/LFM2.5-1.2B-Instruct | 827 MB |
| [LFM2.5-2.6B-ExecuTorch-XNNPACK-32k](https://huggingface.co/experimentalmachines/LFM2.5-2.6B-ExecuTorch-XNNPACK-32k) | LiquidAI/LFM2.5-2.6B | 1.81 GB |
| [Qwen3-1.7B-ExecuTorch-XNNPACK-32k](https://huggingface.co/experimentalmachines/Qwen3-1.7B-ExecuTorch-XNNPACK-32k) | Qwen/Qwen3-1.7B | 1.35 GB |

**Abliterated.** Refusal-direction ablation of the LFM2.5 models with [heretic](https://github.com/p-e-w/heretic), with a 200-trial search that trades refusal rate against KL divergence from the original and a chosen point on that Pareto front. Each repository holds the merged safetensors weights and an ExecuTorch export of them made with the same 32k recipe as the compiled models above.

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

[executorch-model-exporter](https://github.com/ExperimentalMachines/executorch-model-exporter) is the pipeline being built to export small open-weight models from Hugging Face to ExecuTorch on GitHub-hosted runners, smoke-test each program with the runner the app uses, and publish to this organization. Its XNNPACK path covers Qwen3, Qwen2.5, Llama 3.2 and SmolLM2; the Qualcomm and MediaTek NPU backends and the Hub watcher are planned. The models above were exported by hand, with the recipes recorded in their cards.

## Where the studies live

The benchmark repositories linked from experimentalmachines.org, such as MI300X-vs-H200, torchneuronx and snapdragon-vs-m5, and the training code and weights linked from experimentalintelligence.org, are still published under [alpharomercoma](https://github.com/alpharomercoma) on GitHub and Hugging Face. The sites are the index to them.

OpenGrad carries its own index. The training code and configs, the run ledger and per-checkpoint evaluation records, the derived result index and the study reports — SFT and DPO execution, the general-capability diagnosis, the quantization evaluation, the errata and the campaign audit — are committed in [github.com/arjhinety/OpenGrad](https://github.com/arjhinety/OpenGrad), frozen at tag [`study-001`](https://github.com/arjhinety/OpenGrad/tree/study-001). Its weights, training corpora and evaluation records are published under [arjhinety](https://huggingface.co/arjhinety) on Hugging Face, and the study write-ups at [opengrad.arjhinety.com](https://opengrad.arjhinety.com).

## Contact

alpha@experimentalmachines.org
