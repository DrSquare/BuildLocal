# Local AI Co-Building

**Small models. Useful apps. Built together.**

This repository accompanies a 40-minute kickoff on building useful AI applications that run locally. The presentation, `Local_AI_Original.pdf`, includes the walkthroughs and detailed reference material. Its model and tooling snapshot is dated September 23, 2026.

## Why local AI?

Running a model on-device can keep sensitive inputs on the device, support workflows after setup when offline, reduce network round trips, and give teams control over model versions and behavior. Local inference is not automatically the right choice: consider device limits, and distinguish where a model runs from whether its weights are available and what its license permits.

Choose deliberately between:

- **On-device:** bounded tasks that need to work offline.
- **Hosted:** tasks whose quality or scale exceed device capacity.
- **Hybrid:** explicit cloud fallback where the task and data policy allow it.

The workshop default is a local core, visible failures, and no silent cloud fallback. Budget for peak memory beyond model weights, including context, runtime workspaces, the application, and operating system.

## Choose a model and runtime

Start with the task, input types, target device, latency and memory limits, and expected failure behavior. Compare candidates on the same representative cases and device; measure output quality, latency, peak RAM/VRAM, reliability, and abstention. A shortlist is a starting point, not a fit guarantee. Verify the exact model revision, artifact, runtime compatibility, and license before deployment or redistribution.

| Need | Workshop candidate | Resource |
| --- | --- | --- |
| Text and image tasks | Qwen3.5 2B / 4B | [2B model card](https://huggingface.co/Qwen/Qwen3.5-2B) · [4B model card](https://huggingface.co/Qwen/Qwen3.5-4B) |
| Semantic retrieval | Qwen3 Embedding 0.6B | [Model card](https://huggingface.co/Qwen/Qwen3-Embedding-0.6B) |
| Multimodal alternative | Gemma 4 E2B | [Model card](https://huggingface.co/google/gemma-4-E2B-it) |
| Compact text assistant | LFM2.5 1.2B Instruct | [Model card](https://huggingface.co/LiquidAI/LFM2.5-1.2B-Instruct) |
| Speech transcription | Whisper tiny / base | [whisper.cpp](https://github.com/ggml-org/whisper.cpp) |

Runtime options depend on deployment: [Ollama](https://ollama.com/) for a laptop workshop, [llama.cpp](https://github.com/ggml-org/llama.cpp) for native desktop or edge, [MLX-LM](https://github.com/ml-explore/mlx-lm) on Apple silicon, [ONNX Runtime Web](https://onnxruntime.ai/docs/tutorials/web/) in a browser, and [LiteRT-LM](https://developers.google.com/edge/litert-lm) for mobile inference.

## Three example builds

1. **Ask my documents:** index a small folder, retrieve relevant passages, and answer with source IDs or abstain when evidence is missing. The walkthrough uses Qwen3 Embedding 0.6B and Qwen3.5 2B.
2. **Meeting notes:** transcribe a recorded clip locally, then extract editable actions with evidence timestamps. Keep unknown owners and due dates empty.
3. **Receipt to spreadsheet:** extract fields from a receipt image, validate the values, and let a person review and approve them before CSV export.

Each build is an application workflow—not just a model call. Bound and validate inputs, handle cancellation and errors, show evidence beside generated claims, and require human review before consequential writes.

## Laptop setup

Install [Ollama](https://ollama.com/) and Python, then download the models for the build you want:

```sh
ollama pull qwen3.5:2b
ollama pull qwen3-embedding:0.6b  # document Q&A
ollama pull qwen3.5:4b            # receipt extraction
python -m pip install ollama pydantic numpy
```

The presentation also shows how to inspect installed models with `ollama list` and `ollama show`. To test local-only behavior, set `OLLAMA_NO_CLOUD=1` for the Ollama service, restart it, and verify the workflow with networking disabled. For speech transcription, see the [whisper.cpp build and model instructions](https://github.com/ggml-org/whisper.cpp); FFmpeg is installed separately.

## Useful references

- [Ollama FAQ and local-only settings](https://docs.ollama.com/faq)
- [Ollama structured outputs](https://docs.ollama.com/capabilities/structured-outputs)
- [Ollama image and vision input](https://docs.ollama.com/capabilities/vision)
- [Measuring intelligence efficiency for local AI](https://arxiv.org/pdf/2511.07885)
- [Open-source model directory](https://github.com/BayAreaKGroup/OpenModel)

See `Local_AI_Original.pdf` for the full model-fit worksheet, example architectures, acceptance tests, setup commands, and additional reading links.
