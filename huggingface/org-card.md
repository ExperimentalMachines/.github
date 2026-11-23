<div align="center">
  <h1>Experimental Machines</h1>
  <p>Open-weight models compiled for phones, with published recipes, checksums, smoke tests, and benchmark evidence.</p>
  <p>
    <a href="https://experimentalmachines.org">experimentalmachines.org</a> &middot;
    <a href="https://experimentalintelligence.org">experimentalintelligence.org</a> &middot;
    <a href="https://github.com/ExperimentalMachines">GitHub</a> &middot;
    <a href="https://play.google.com/store/apps/details?id=io.github.alpharomercoma.openweights">OpenWeights on Google Play</a>
  </p>
</div>

## Model catalog

This organization has 52 public model repositories and 5 public datasets. Collections are the catalog and recommendation surface; they group deployment exports by family and keep the organization page focused on artifacts that run in [OpenWeights](https://github.com/ExperimentalMachines/openweights).

| Collection | Contents |
|---|---|
| [LFM2.5 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/lfm25-for-executorch-6aad16d614c3540ca70fc977) | LFM2.5 1.2B and 2.6B exports. |
| [LFM2.5 Abliterated for ExecuTorch](https://huggingface.co/collections/experimentalmachines/lfm25-abliterated-for-executorch-6aad2156b906a298950899cb) | The 1.2B and 2.6B heretic exports. |
| [Qwen3 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/qwen3-for-executorch-6aadea320b7db407b2124bad) | Qwen3 exports. |
| [Qwen2.5 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/qwen25-for-executorch-6aadea30b6dc6303fe2d606b) | Qwen2.5 exports. |
| [Llama 3.2 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/llama-32-for-executorch-6aad16db65a6e6b677b18e49) | Llama 3.2 exports. |
| [SmolLM2 for ExecuTorch](https://huggingface.co/collections/experimentalmachines/smollm2-for-executorch-6aad16dcc1832da6f696a90d) | SmolLM2 exports. |
| [QwenGrad for ExecuTorch](https://huggingface.co/collections/experimentalmachines/qwengrad-for-executorch-6aa5008c9414ee393594e3a8) | The evaluated deployment build from the OpenGrad study. |

Every compiled export card records its source revision, quantization recipe, fixed context window, file checksum, and host-runner smoke-test result. Benchmark results are reported separately, so a runnable export is never represented as a behavioral recommendation without device evidence.

The LFM2.5 entries use the corrected prompt-state recipe and GPTQ-solved int4 weights. The heretic variants are published only as their validated ExecuTorch deployment exports; there are no separate bare heretic source-checkpoint repositories in this organization.

## Research artifacts

[LFM3 Mobile Agent](https://huggingface.co/collections/experimentalmachines/lfm3-mobile-agent-6b03a79f8e23b8dde16e8326) is a screenshot-only Android agent post-trained from LiquidAI's LFM3-VL-2B: every version (v1–v4.3), four comparison fine-tunes, their 4-bit GGUFs for llama.cpp, and the training datasets. The main model, v4, solved 54 of 348 held-out AndroidWorld tasks against GUI-Owl-1.5-2B's 47 (non-inferior under a preregistered margin, superiority not shown). Code, harnesses, raw results and audits: [lfm3-mobile-agent](https://github.com/ExperimentalMachines/lfm3-mobile-agent). [LFM3-VL-2B-MobileAgent-v4-LiteRT](https://huggingface.co/experimentalmachines/LFM3-VL-2B-MobileAgent-v4-LiteRT) runs v4 on a phone's GPU with LiteRT-LM (about 3.5 s per model call on a Snapdragon reference phone) in the [Mobile Agent](https://github.com/ExperimentalMachines/mobile-agent) app.

[QwenGrad-DPO](https://huggingface.co/experimentalmachines/QwenGrad-DPO) is intentionally outside the collections. It is the standard Transformers-format checkpoint from the [OpenGrad](https://github.com/arjhinety/OpenGrad) research study; its paired phone deployment export is in the QwenGrad collection.

## How exports are made

[execupack](https://github.com/ExperimentalMachines/execupack) builds supported models on GitHub-hosted runners, smoke-tests them with the same runner used by the app, and publishes the `.pte` programs, reports, and app-readable configuration here. The supported delegates are XNNPACK, Qualcomm QNN, MediaTek NeuroPilot, and Vulkan where the model family has a validated recipe.

Phone measurements and their raw results are published at [latency](https://experimentalmachines.github.io/openweights/latency.html), [context window](https://experimentalmachines.github.io/openweights/window.html), and [reruns](https://experimentalmachines.github.io/openweights/reruns.html).

Contact: alpha@experimentalmachines.org · arjhine@experimentalmachines.org
