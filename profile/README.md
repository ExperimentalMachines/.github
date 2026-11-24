# Experimental Machines

Independent measurements of AI hardware, small models trained in the open, and tools to run open weights on a phone. We publish the measurements, methods, and raw results behind our claims.

## Two sites

| Site | Focus |
|---|---|
| [experimentalmachines.org](https://experimentalmachines.org) | AI accelerator benchmarks from datacenter to phone scale. Source: [experimentalmachines.org](https://github.com/ExperimentalMachines/experimentalmachines.org). |
| [experimentalintelligence.org](https://experimentalintelligence.org) | Small models trained from scratch, with weights, code, and training logs. Source: [experimentalintelligence.org](https://github.com/ExperimentalMachines/experimentalintelligence.org). |

## OpenWeights

[OpenWeights](https://github.com/ExperimentalMachines/openweights) is an Android app for open-weight language models. It searches Hugging Face in the app, estimates whether a model fits before download, and runs locally without an account, cloud service, or telemetry. It supports GGUF through llama.cpp and compiled `.pte` programs through ExecuTorch. It is available on [Google Play](https://play.google.com/store/apps/details?id=io.github.alpharomercoma.openweights) under Apache 2.0.

The phone measurements behind the app include [latency on five chips](https://experimentalmachines.github.io/openweights/latency.html), [the exported-window study](https://experimentalmachines.github.io/openweights/window.html), and [reruns](https://experimentalmachines.github.io/openweights/reruns.html).

## Models on Hugging Face

The [Experimental Machines Hugging Face organization](https://huggingface.co/experimentalmachines) has 55 public model repositories and 6 public datasets. The catalog is organized through collections; use them as the model navigation surface.

| Collection | Contents |
|---|---|
| [LFM2.5 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/lfm25-for-executorch-6aad16d614c3540ca70fc977) | LFM2.5 1.2B and 2.6B ExecuTorch exports. |
| [LFM2.5 Abliterated for ExecuTorch](https://huggingface.co/collections/experimentalmachines/lfm25-abliterated-for-executorch-6aad2156b906a298950899cb) | The 1.2B and 2.6B heretic exports, with 2k through 32k windows. |
| [Qwen3 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/qwen3-for-executorch-6aadea320b7db407b2124bad) | Qwen3 deployment exports. |
| [Qwen2.5 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/qwen25-for-executorch-6aadea30b6dc6303fe2d606b) | Qwen2.5 deployment exports. |
| [Llama 3.2 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/llama-32-for-executorch-6aad16db65a6e6b677b18e49) | Llama 3.2 deployment exports. |
| [SmolLM2 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/smollm2-for-executorch-6aad16dcc1832da6f696a90d) | SmolLM2 deployment exports. |
| [QwenGrad for ExecuTorch](https://huggingface.co/collections/experimentalmachines/qwengrad-for-executorch-6aa5008c9414ee393594e3a8) | The evaluated ExecuTorch deployment build for the OpenGrad study. |
| [LFM3 Mobile Agent](https://huggingface.co/collections/experimentalmachines/lfm3-mobile-agent-6b03a79f8e23b8dde16e8326) | Screenshot-only Android agents: LFM3-VL-2B v1–v4.4 and four comparison fine-tunes, their phone GGUFs, v4 as a LiteRT-LM file, and the training datasets. |

Each export card identifies the source revision, quantization recipe, context window, file checksum, and smoke-test result. The compiled LFM2.5 exports use the fixed prompt-state recipe and GPTQ-solved int4 weights; benchmark reports distinguish model behavior from export validation.

`QwenGrad-DPO` is intentionally outside the collections because it is the standard Transformers-format research checkpoint from the [OpenGrad](https://github.com/arjhinety/OpenGrad) study, rather than an app deployment artifact. Its paired ExecuTorch program is in the QwenGrad collection.

## LFM3 Mobile Agent

[LFM3 Mobile Agent](https://github.com/ExperimentalMachines/lfm3-mobile-agent) post-trains LiquidAI's LFM3-VL-2B into a small, screenshot-only Android agent that runs on the phone (4-bit GGUF, llama.cpp). On three held-out AndroidWorld seeds the main model, v4, solved 54 of 348 tasks against GUI-Owl-1.5-2B's 47: non-inferior under a preregistered margin, superiority not shown, and GUI-Owl is clearly better on MobileWorld. The repository holds the training code, data builders, benchmark harnesses, raw results, preregistrations and audits; the weights, GGUFs and training data are in the [collection](https://huggingface.co/collections/experimentalmachines/lfm3-mobile-agent-6b03a79f8e23b8dde16e8326).

[Mobile Agent](https://github.com/ExperimentalMachines/mobile-agent) runs v4 on the phone's GPU with LiteRT-LM: one int8 file ([LFM3-VL-2B-MobileAgent-v4-LiteRT](https://huggingface.co/experimentalmachines/LFM3-VL-2B-MobileAgent-v4-LiteRT)) that picks the same action as the original model on 90.6% of 180 test steps, about 3.5 s per model call on a Snapdragon reference phone, and a minimal Android app. In first tests on the phone it completed 3 of 11 everyday tasks.

## OpenGrad

[OpenGrad](https://github.com/arjhinety/OpenGrad) is a controlled post-training study of small open-weight models. It publishes pre-registered evaluation gates, per-example records, negative findings, weights, code, and training logs. The frozen evidence is tagged [`study-001`](https://github.com/arjhinety/OpenGrad/tree/study-001), with reports at [opengrad.arjhinety.com](https://opengrad.arjhinety.com/studies/001).

## Tooling

[execupack](https://github.com/ExperimentalMachines/execupack) exports supported open-weight models on GitHub-hosted runners, smoke-tests each `.pte` with the same runner used by the app, and publishes the artifacts and reports to Hugging Face. The exporter supports XNNPACK, Qualcomm QNN, MediaTek NeuroPilot, and Vulkan where a family has a validated recipe.

## Contact

alpha@experimentalmachines.org · arjhine@experimentalmachines.org
