# ThumbLLM Recipe: Qwen3.5-9B Q4_K_M CPU Edition

## Model

* **Model:** Qwen/Qwen3.5-9B
* **GGUF repository:** unsloth/Qwen3.5-9B-GGUF
* **Model creator:** Qwen Team
* **GGUF creator:** Unsloth
* **Model source:** https://huggingface.co/unsloth/Qwen3.5-9B-GGUF
* **Upstream model:** https://huggingface.co/Qwen/Qwen3.5-9B
* **Quantization:** Q4_K_M
* **Format:** GGUF
* **Model file:** Qwen3.5-9B-Q4_K_M.gguf
* **Model size:** 5.68 GB
* **Model license:** Apache License 2.0
* **Parameters:** 9B
* **Native context:** 262,144 tokens
* **ThumbLLM configured context:** 65,536 tokens

Qwen3.5-9B is the upstream Qwen 9B model rather than a third-party post-trained derivative.

The upstream architecture is a causal language model with a vision encoder and a hybrid language-model layout using Gated DeltaNet linear attention and gated attention. This ThumbLLM edition currently exposes **text chat and an OpenAI-compatible text API only**; no multimodal projector is configured in this build.

## ThumbLLM Release

* **ThumbLLM version:** 0.1.0
* **Revision:** 2026-09-10
* **Edition:** Qwen3.5-9B Q4_K_M CPU Edition
* **Release date:** 2026-09-12
* **Executable:** ThumbLLM-Qwen3.5-9B-Q4_K_M-CPU-Win-x64-v0.1.0.exe
* **SHA-256:** [ADD FINAL EXECUTABLE SHA-256]

## Platform

* **Operating system:** Windows
* **Architecture:** x64
* **Backend:** CPU
* **Target hardware:** Modern 64-bit AMD or Intel Windows CPU; build profile targets an AMD Ryzen laptop CPU
* **GPU required:** No

## Runtime

* **Runtime:** llama.cpp
* **llama.cpp build:** b10603
* **llama.cpp commit:** c060ca974
* **Runtime package:** Bundled native Windows x64 `llama-server`
* **Compiler:** Clang 20.1.8, Windows x86_64
* **Engine selection:** Native `llama-server` preferred; `llama-cpp-python` fallback allowed

## Purpose

This ThumbLLM edition packages **Qwen3.5-9B Q4_K_M** as a preconfigured **Windows x64 CPU-only** local AI application.

It is designed for users who want to run the upstream Qwen3.5 9B model locally without requiring a discrete GPU or manually configuring llama.cpp.

This release uses a tested combination of model, quantization, runtime, and inference settings selected for stable CPU-only local inference.

GPU inference is explicitly disabled with:

```text
--device none
--gpu-layers 0
```

## ThumbLLM Features

* Built-in local desktop chat
* Local OpenAI-compatible API
* Automatic GGUF model download
* SHA-256 model verification
* Resumable model download
* Automatic retry after transient download failures
* Streaming generation
* Generation statistics
* Context and prompt management
* Automatic context trimming
* Thinking-mode toggle
* Local inference after the initial model download
* Hardware information at startup
* Automatic API port selection
* Native llama-server process lifecycle management

## Model Download

* **Download source:** Hugging Face
* **Repository:** unsloth/Qwen3.5-9B-GGUF
* **Repository URL:** https://huggingface.co/unsloth/Qwen3.5-9B-GGUF
* **Filename:** Qwen3.5-9B-Q4_K_M.gguf
* **Expected size:** 5.68 GB
* **Verification method:** SHA-256
* **Expected model SHA-256:** `03b74727a860a56338e042c4420bb3f04b2fec5734175f4cb9fa853daf52b7e8`
* **Direct model URL:** https://huggingface.co/unsloth/Qwen3.5-9B-GGUF/resolve/main/Qwen3.5-9B-Q4_K_M.gguf

The model weights are **downloaded separately** and are not bundled with the ThumbLLM executable.

ThumbLLM validates an existing GGUF before use. If the model is missing or invalid, the downloader can download a clean copy, resume partial downloads where supported, retry transient failures, and verify the final SHA-256 before installation.

## Inference Configuration

### Core Settings

* **Context size:** 65,536
* **Maximum output tokens:** 12,288
* **Context auto-trim:** ON
* **Context safety margin:** 256 tokens
* **Threads:** 8
* **Threads batch:** 16
* **Batch size:** 512
* **Micro-batch size:** 256
* **GPU layers:** 0
* **Flash Attention:** ON
* **Memory mapping (mmap):** ON
* **Memory locking (mlock):** OFF
* **Temperature:** 0.7
* **Top-p:** 0.9
* **Parallel slots:** 1
* **Prompt cache:** ON
* **Cache reuse:** 0
* **Jinja:** ON

### CPU Settings

* **CPU backend:** CPU
* **Decode threads:** 8
* **Prompt/batch threads:** 16
* **CPU priority:** 1
* **CPU batch priority:** 1
* **Poll:** 1
* **Poll batch:** 1
* **CPU strict:** OFF
* **CPU strict batch:** OFF
* **Thread affinity:** Default / no explicit mask
* **NUMA:** Default / not explicitly configured

### GPU / Accelerator Settings

* **GPU backend:** N/A
* **Primary device:** `none`
* **GPU layers:** 0
* **GPU offload:** OFF
* **KV offload:** OFF
* **Operation offload:** OFF
* **Tensor split:** N/A
* **Main GPU:** N/A
* **Split mode:** Single / no multi-GPU split
* **GPU fit:** OFF

The active profile intentionally ignores the detected Radeon 740M-class iGPU and forces llama.cpp to use CPU inference.

### KV Cache

* **K cache type:** q4_0
* **V cache type:** q4_0
* **KV offload:** OFF
* **KV configuration notes:** Quantized q4_0 K/V cache remains on the CPU path to reduce context-memory usage.

### Speculative Decoding

* **Enabled:** NO
* **Type:** NONE
* **Configured spec type:** `none`
* **Draft model:** N/A
* **Maximum draft tokens:** N/A while speculative decoding is disabled
* **Minimum draft tokens:** N/A while speculative decoding is disabled
* **Acceptance settings:** N/A

The source retains legacy Qwen3.5-4B MTP/DFlash draft settings for possible future recipes, but they are inactive in this Qwen3.5-9B build because:

```text
SPEC_TYPE = "none"
```

### Reasoning / Thinking

* **Thinking default:** OFF
* **User toggle:** Available in desktop UI
* **Server reasoning mode:** auto
* **Reasoning format:** deepseek
* **Reasoning budget:** -1

### Additional llama.cpp Settings

```text
INFERENCE_ENGINE=auto
LLAMA_SERVER_EXECUTABLE=auto
ALLOW_PYTHON_FALLBACK=True

SERVER_HOST=127.0.0.1
SERVER_PORT=8080
SERVER_PORT_SEARCH_LIMIT=100
SERVER_STARTUP_TIMEOUT=180
SERVER_REQUEST_TIMEOUT=3600

N_CTX=65536
MAX_TOKENS=12288
CONTEXT_AUTO_TRIM=True
CONTEXT_SAFETY_MARGIN=256

TEMPERATURE=0.7
TOP_P=0.9

N_THREADS=8
N_THREADS_BATCH=16
CPU_PRIORITY=1
CPU_PRIORITY_BATCH=1
POLL=1
POLL_BATCH=1
CPU_STRICT=False
CPU_STRICT_BATCH=False

N_BATCH=512
N_UBATCH=256

N_GPU_LAYERS=0
FLASH_ATTN=on
KV_OFFLOAD=False
OP_OFFLOAD=False
REPACK=True
NO_HOST=False
SWA_FULL=False

KV_CACHE_TYPE_K=q4_0
KV_CACHE_TYPE_V=q4_0

DEVICE=none
FIT=off
USE_MMAP=True
USE_MLOCK=False

MULTI_GPU_MODE=single
TENSOR_SPLIT=
SERVER_PARALLEL=1

BACKEND_SAMPLING=False
SPEC_TYPE=none

CACHE_PROMPT=True
CACHE_REUSE=0
USE_JINJA=True

REASONING=off
SERVER_REASONING_MODE=auto
REASONING_FORMAT=deepseek
REASONING_BUDGET=-1
```

## Equivalent llama.cpp Configuration

```text
llama-server.exe ^
  --model "Qwen3.5-9B-Q4_K_M.gguf" ^
  --host 127.0.0.1 ^
  --port 8080 ^
  --ctx-size 65536 ^
  --threads 8 ^
  --threads-batch 16 ^
  --cpu-strict 0 ^
  --cpu-strict-batch 0 ^
  --prio 1 ^
  --prio-batch 1 ^
  --poll 1 ^
  --poll-batch 1 ^
  --batch-size 512 ^
  --ubatch-size 256 ^
  --gpu-layers 0 ^
  --flash-attn on ^
  --cache-type-k q4_0 ^
  --cache-type-v q4_0 ^
  --parallel 1 ^
  --cache-reuse 0 ^
  --reasoning auto ^
  --reasoning-format deepseek ^
  --reasoning-budget -1 ^
  --device none ^
  --no-kv-offload ^
  --no-op-offload ^
  --repack ^
  --mmap ^
  --cache-prompt ^
  --jinja
```

The preferred API port is 8080. If port 8080 is already occupied, ThumbLLM searches sequentially for another available port, up to 100 ports beginning with 8080.

## API Configuration

* **API type:** OpenAI-compatible
* **Model ID:** Qwen3.5-9B
* **Default host:** 127.0.0.1
* **Preferred port:** 8080
* **Base URL:** `http://127.0.0.1:8080/v1` when port 8080 is available
* **Chat endpoint:** `/v1/chat/completions`
* **Models endpoint:** `/v1/models`
* **Health endpoint:** `/health`
* **Network exposure:** LOCALHOST ONLY
* **LAN access:** OFF
* **Authentication:** NONE
* **API key required:** NO

## Tested Hardware

* **System:** AMD Ryzen Windows laptop
* **CPU:** [ADD EXACT TEST CPU]
* **CPU cores / threads:** [ADD EXACT CORE / THREAD COUNT]
* **GPU / accelerator:** Radeon 740M-class iGPU detected but intentionally unused
* **VRAM:** N/A for inference
* **System RAM:** [ADD TEST SYSTEM RAM]
* **Memory type / speed:** [ADD IF RECORDED]
* **Operating system:** Windows x64
* **Driver:** N/A for CPU-only inference

## Benchmark Configuration

* **Prompt / test:** [ADD BENCHMARK DESCRIPTION]
* **Prompt tokens:** [ADD VALUE]
* **Generated tokens:** [ADD VALUE]
* **Context occupancy:** [ADD VALUE / N/A]
* **Number of runs:** [ADD VALUE]
* **Warm-up:** [ADD YES / NO]
* **Other benchmark conditions:** CPU-only; `--device none`; `--gpu-layers 0`; record whether Thinking was ON or OFF

## Performance

| Metric                    | Result |
| ------------------------- | -----: |
| Model load time           | [ADD VALUE] |
| Prompt processing         | [ADD VALUE] tok/s |
| Token generation / decode | [ADD VALUE] tok/s |
| Total generation time     | [ADD VALUE] |
| Peak RAM usage            | [ADD VALUE] |
| Peak VRAM usage           | N/A |

## Additional Benchmark Results

| Test | Prompt Processing | Decode | Notes |
| ---- | ----------------: | -----: | ----- |
| Test 1 | [ADD VALUE] | [ADD VALUE] | CPU-only |
| Test 2 | [ADD VALUE] | [ADD VALUE] | CPU-only |
| Test 3 | [ADD VALUE] | [ADD VALUE] | CPU-only |

## Why These Settings

This Qwen3.5-9B release uses the stable CPU configuration active in `main.py`.

The selected profile uses:

* 8 decode threads
* 16 prompt/batch threads
* CPU priority 1
* Batch priority 1
* Poll 1
* Poll batch 1
* Batch size 512
* Micro-batch size 256
* Flash Attention enabled
* q4_0 K/V KV cache
* GPU layers set to 0
* Device forced to `none`
* KV offload disabled
* Operation offload disabled
* mmap enabled
* mlock disabled
* Repack enabled
* One parallel decode slot
* Speculative decoding disabled

The source labels the active 8/16 thread, priority 1, poll 1 configuration as the **Stable CPU Settings**.

A more aggressive CPU profile remains in the source but is commented out:

```text
N_THREADS=0
N_THREADS_BATCH=0
CPU_PRIORITY=2
CPU_PRIORITY_BATCH=1
POLL=50
POLL_BATCH=1
```

That alternative is not active in this release.

## Tested Alternatives

| Configuration | Decode | Difference | Result |
| ------------- | -----: | ---------: | ------ |
| Auto threads / priority 2 / poll 50 | [ADD VALUE IF RECORDED] | [ADD VALUE] | REJECTED / inactive |
| 8 decode / 16 batch / priority 1 / poll 1 | [ADD VALUE] | Baseline | SELECTED |
| GPU/ROCm profile retained in comments | N/A | N/A | NOT ACTIVE |

## Known Limitations

* This edition intentionally uses CPU-only inference and does not use the detected GPU.
* The Q4_K_M quantization reduces memory and storage requirements but may differ in quality from the original higher-precision model.
* The upstream Qwen3.5-9B model supports a larger native context than this build; ThumbLLM intentionally configures 65,536 tokens.
* The upstream architecture supports vision, but this ThumbLLM edition does not configure a multimodal projector and should be treated as a text-chat/API release.
* Speculative decoding is disabled in the active configuration.
* LAN API exposure is disabled by default.
* Port 8080 is preferred, but another port may be selected automatically if it is already occupied.
* Performance varies with CPU architecture, memory bandwidth, thermals, context length, and background activity.

## Recommended Hardware

### Minimum

* **RAM:** 16 GB
* **VRAM:** N/A
* **CPU:** Modern 64-bit AMD or Intel Windows CPU
* **Storage:** At least 8 GB free for the model, executable, runtime, logs, and download overhead

### Recommended

* **RAM:** 32 GB or more, particularly for longer contexts
* **VRAM:** N/A
* **CPU:** Modern high-performance multi-core AMD or Intel x64 processor
* **Storage:** SSD recommended

## Files Included With This Release

```text
ThumbLLM-Qwen3.5-9B-Q4_K_M-CPU-Win-x64-v0.1.0.exe
ThumbLLM-Qwen3.5-9B-Q4_K_M-CPU-Win-x64-v0.1.0.sha256
```

The model GGUF is **not included** with the ThumbLLM executable.

## Release Notes

This is the initial ThumbLLM CPU edition for **Qwen3.5-9B Q4_K_M**.

Unlike ThumbLLM editions built around post-trained Qwen derivatives, this edition runs the upstream Qwen3.5-9B model itself using Unsloth's Q4_K_M GGUF.

The build is designed for CPU-only Windows x64 inference:

* 65,536-token configured context
* Up to 12,288 output tokens
* 8 decode threads
* 16 prompt/batch threads
* 512 / 256 batch and micro-batch
* Flash Attention enabled
* q4_0 K/V cache
* GPU layers 0
* Device forced to `none`
* GPU KV and operation offload disabled
* Speculative decoding disabled
* Thinking mode available through the desktop toggle
* OpenAI-compatible localhost API
* Automatic model download and SHA-256 verification

## Reproducibility

This recipe documents the configuration used by the corresponding ThumbLLM executable.

Performance will vary based on processor, memory bandwidth, operating system, background activity, thermal limits, context length, and other system characteristics.

The purpose of this recipe is to preserve the known-good combination of:

**Qwen3.5-9B + Q4_K_M + llama.cpp + Windows x64 + CPU-only inference settings**

used for this ThumbLLM release.

## Third-Party Software

ThumbLLM uses third-party software subject to its respective licenses.

See:

`THIRD-PARTY-NOTICES.md`

for licensing and attribution information applicable to this release.

