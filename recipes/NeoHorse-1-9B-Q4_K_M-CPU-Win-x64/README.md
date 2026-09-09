# ThumbLLM Recipe: NeoHorse-1-9B Q4_K_M CPU Edition

## Model

* **Model:** TokenRhythm/NeoHorse-1-9B-GGUF
* **Model creator:** TokenRhythm / NeoHorse Team
* **Model source:** https://huggingface.co/TokenRhythm/NeoHorse-1-9B-GGUF
* **Quantization:** Q4_K_M
* **Format:** GGUF
* **Model file:** NeoHorse-1-9B-Q4_K_M.gguf
* **Model size:** 5.63 GB
* **Model license:** Apache License 2.0
* **Upstream/base model:** Qwen/Qwen3.5-9B

NeoHorse-1-9B is an approximately 9B-parameter text model post-trained from Qwen3.5-9B for agentic tool use, coding, instruction following, reasoning, and conversational use.

The upstream model card reports a native context length of 262,144 tokens, extensible up to 1,010,000 tokens. This ThumbLLM edition intentionally configures a 65,536-token context window.

The Q4_K_M GGUF is a standard llama.cpp quantization generated from the BF16 GGUF without an importance matrix. The repository states that the GGUF has an embedded chat template and does not contain an MTP draft head.

## ThumbLLM Release

* **ThumbLLM version:** 0.1.0
* **Edition:** NeoHorse-1-9B Q4_K_M CPU Edition
* **Release date:** 2026-09-09
* **Executable:** ThumbLLM-NeoHorse-1-9B-Q4_K_M-CPU-Win-x64-v0.1.0.exe
* **SHA-256:** [GENERATE FROM FINAL EXECUTABLE BEFORE RELEASE]

## Platform

* **Operating system:** Windows
* **Architecture:** x64
* **Backend:** CPU
* **Target hardware:** AMD Ryzen laptop CPU / modern x64 CPU, CPU-only inference

## Runtime

* **Runtime:** llama.cpp
* **llama.cpp version:** 0.2.0-dev
* **llama.cpp build:** 10603
* **llama.cpp commit:** c060ca974
* **Runtime package:** Bundled native Windows x64 `llama-server`
* **Compiler:** Clang 20.1.8, Windows x86_64
* **Fallback:** `llama-cpp-python` allowed if the native server is unavailable

## Purpose

This ThumbLLM edition packages **NeoHorse-1-9B Q4_K_M** as a preconfigured **Windows x64 CPU-only** local AI application.

It is designed for users who want to run a capable approximately 9B-parameter local model without requiring a discrete GPU or manually configuring llama.cpp.

NeoHorse-1-9B is targeted at agentic workflows, tool use, coding, reasoning, and instruction following. This ThumbLLM build uses the official TokenRhythm Q4_K_M GGUF and a stable CPU inference profile.

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
* Download resume, retry, cancellation, and integrity checking
* Streaming generation
* Generation statistics
* Context and prompt management
* Automatic context trimming
* Local inference after the initial model download
* Hardware information at startup
* Hardware-specific llama.cpp configuration
* Automatic API port selection if the preferred port is occupied
* Thinking-mode toggle in the desktop UI
* Native llama-server preferred with optional llama-cpp-python fallback

## Model Download

* **Download source:** Hugging Face
* **Repository:** TokenRhythm/NeoHorse-1-9B-GGUF
* **Repository URL:** https://huggingface.co/TokenRhythm/NeoHorse-1-9B-GGUF
* **Filename:** NeoHorse-1-9B-Q4_K_M.gguf
* **Expected size:** 5.63 GB
* **Verification method:** SHA-256
* **Expected model SHA-256:** `a412add5457474d898ef0bf0cdbc057f7ffc3ea5cedba997c5123d36f116a957`
* **Direct model URL:** https://huggingface.co/TokenRhythm/NeoHorse-1-9B-GGUF/resolve/main/NeoHorse-1-9B-Q4_K_M.gguf

The model weights are **downloaded separately** and are not bundled inside the ThumbLLM executable.

ThumbLLM uses the configured model downloader to validate an existing GGUF or download the model when necessary. The downloader handles partial downloads, resume behavior, retries, cancellation, and final SHA-256 integrity verification.

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
* **Jinja chat template support:** ON
* **Reasoning default:** OFF
* **Server reasoning mode:** auto
* **Reasoning format:** deepseek
* **Reasoning budget:** -1

### CPU Settings

* **CPU backend:** CPU
* **Thread count:** 8
* **Thread batch count:** 16
* **CPU priority:** 1
* **CPU batch priority:** 1
* **Poll:** 1
* **Poll batch:** 1
* **CPU strict:** OFF
* **CPU strict batch:** OFF
* **Thread affinity:** Default; no explicit CPU mask or range
* **NUMA:** Default / not explicitly configured
* **Compute path:** llama.cpp CPU kernels

### GPU / Accelerator Settings

* **GPU backend:** N/A
* **Primary device:** none
* **GPU layers:** 0
* **GPU offload:** OFF
* **KV offload:** OFF
* **Operation offload:** OFF
* **Tensor split:** N/A
* **Main GPU:** N/A
* **Split mode:** Single / no multi-GPU split
* **GPU fit:** OFF

This build is intentionally CPU-only even if the bundled llama.cpp binary contains GPU-capable backends.

### KV Cache

* **K cache type:** q4_0
* **V cache type:** q4_0
* **KV offload:** OFF
* **KV configuration notes:** Quantized q4_0 K/V cache is used to reduce context-memory requirements. KV work remains on the CPU path.

### Speculative Decoding

* **Enabled:** NO
* **Type:** NONE
* **Configured setting:** `SPEC_TYPE = "none"`
* **Draft model:** N/A
* **Maximum draft tokens:** N/A
* **Minimum draft tokens:** N/A
* **Acceptance settings:** N/A

NeoHorse's official GGUF repository explicitly states that these files contain **no MTP draft head**. ThumbLLM therefore leaves MTP/speculative decoding disabled for this edition.

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
  --model "NeoHorse-1-9B-Q4_K_M.gguf" ^
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

The preferred API port is 8080. If port 8080 is unavailable, ThumbLLM searches sequentially for the next available port, up to 100 consecutive ports.

## API Configuration

* **API type:** OpenAI-compatible
* **Default host:** 127.0.0.1
* **Preferred port:** 8080
* **Port fallback:** Automatic search for the next available port
* **Base URL:** `http://127.0.0.1:8080/v1` when port 8080 is available
* **Chat endpoint:** `/v1/chat/completions`
* **Models endpoint:** `/v1/models`
* **Health endpoint:** `/health`
* **Network exposure:** LOCALHOST ONLY
* **LAN access:** OFF
* **Authentication:** NONE
* **API key required:** NO

## Tested Hardware

* **System:** [TO BE RECORDED FOR THIS RELEASE]
* **CPU:** [TO BE RECORDED FOR THIS RELEASE]
* **CPU cores / threads:** [TO BE RECORDED FOR THIS RELEASE]
* **GPU / accelerator:** Not used for inference
* **VRAM:** N/A
* **System RAM:** [TO BE RECORDED FOR THIS RELEASE]
* **Memory type / speed:** [TO BE RECORDED IF RELEVANT]
* **Operating system:** Windows x64
* **Driver:** N/A for CPU-only inference

## Benchmark Configuration

* **Prompt / test:** [TO BE RECORDED]
* **Prompt tokens:** [TO BE RECORDED]
* **Generated tokens:** [TO BE RECORDED]
* **Context occupancy:** [TO BE RECORDED]
* **Number of runs:** [TO BE RECORDED]
* **Warm-up:** [TO BE RECORDED]
* **Other benchmark conditions:** CPU-only inference; GPU explicitly disabled; thinking mode should be recorded as ON or OFF for each benchmark

## Performance

| Metric                    | Result |
| ------------------------- | -----: |
| Model load time           | [TO BE RECORDED] |
| Prompt processing         | [TO BE RECORDED] |
| Token generation / decode | [TO BE RECORDED] |
| Total generation time     | [TO BE RECORDED] |
| Peak RAM usage            | [TO BE RECORDED] |
| Peak VRAM usage           | N/A |

## Additional Benchmark Results

| Test | Prompt Processing | Decode | Notes |
| ---- | ----------------: | -----: | ----- |
| Test 1 | [TO BE RECORDED] | [TO BE RECORDED] | CPU-only |
| Test 2 | [TO BE RECORDED] | [TO BE RECORDED] | CPU-only |
| Test 3 | [TO BE RECORDED] | [TO BE RECORDED] | CPU-only |

## Why These Settings

This NeoHorse-1-9B Q4_K_M release uses the stable CPU settings active in `main.py`.

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

The source labels these values as the **Stable CPU Settings**.

A more aggressive CPU profile remains present but commented out:

```text
N_THREADS=0
N_THREADS_BATCH=0
CPU_PRIORITY=2
CPU_PRIORITY_BATCH=1
POLL=50
POLL_BATCH=1
```

That profile is not active in this release.

The official NeoHorse GGUF has no MTP draft head, so speculative MTP decoding is not enabled.

## Tested Alternatives

| Configuration | Decode | Difference | Result |
| ------------- | -----: | ---------: | ------ |
| Auto threads / priority 2 / poll 50 | [TO BE RECORDED] | [TO BE RECORDED] | REJECTED / inactive |
| 8 decode / 16 batch / priority 1 / poll 1 | [TO BE RECORDED] | Baseline | SELECTED |
| ROCm / GPU-offload profile retained in comments | N/A | N/A | NOT ACTIVE |

## Known Limitations

* This edition intentionally uses CPU-only inference and does not use GPU acceleration.
* Q4_K_M reduces memory and storage requirements compared with higher-precision variants but may reduce model quality relative to BF16 or higher-bit quantizations.
* The upstream NeoHorse model supports a larger native context than this build; ThumbLLM is intentionally configured for 65,536 tokens.
* Performance depends on processor architecture, memory bandwidth, thermals, system load, and context length.
* Speculative MTP decoding is unavailable because the official NeoHorse GGUF does not include an MTP draft head.
* LAN API exposure is disabled by default.
* Port 8080 is preferred, but another port may be selected automatically if it is occupied.
* The native llama-server path is preferred. The llama-cpp-python fallback may expose fewer low-level llama.cpp controls.

## Recommended Hardware

### Minimum

* **RAM:** 16 GB recommended minimum for practical use with this 5.63 GB model and runtime/context overhead
* **VRAM:** N/A
* **CPU:** 64-bit Windows-compatible AMD or Intel x64 processor supported by the bundled llama.cpp runtime
* **Storage:** At least 8 GB free for the model, executable, runtime, logs, and download overhead

### Recommended

* **RAM:** 32 GB or more, especially for longer contexts
* **VRAM:** N/A
* **CPU:** Modern high-performance multi-core AMD or Intel x64 processor
* **Storage:** SSD recommended for faster model loading and download handling

## Files Included With This Release

```text
ThumbLLM-NeoHorse-1-9B-Q4_K_M-CPU-Win-x64-v0.1.0.exe
ThumbLLM-NeoHorse-1-9B-Q4_K_M-CPU-Win-x64-v0.1.0.sha256
```

The model GGUF is **not included** with the ThumbLLM executable.

## Release Notes

This is the initial ThumbLLM CPU edition for **TokenRhythm NeoHorse-1-9B Q4_K_M**.

NeoHorse-1-9B is an approximately 9B-parameter model post-trained from Qwen3.5-9B for agentic tool use, coding, reasoning, instruction following, and conversational workloads.

This ThumbLLM edition uses the official 5.63 GB Q4_K_M GGUF and runs entirely on CPU.

Active release configuration:

* 65,536-token context
* Up to 12,288 output tokens
* 8 decode threads
* 16 prompt/batch threads
* 512 batch size
* 256 micro-batch size
* Flash Attention enabled
* q4_0 K/V KV cache
* GPU layers 0
* Device `none`
* GPU KV and operation offload disabled
* Speculative decoding disabled
* OpenAI-compatible localhost API
* Preferred API port 8080 with automatic free-port fallback
* Automatic model download and SHA-256 verification

## Reproducibility

This recipe documents the configuration used by the corresponding ThumbLLM executable.

Performance will vary based on processor, memory bandwidth, operating system, background activity, thermal limits, context length, and other system characteristics.

The purpose of this recipe is to preserve the known-good combination of:

**NeoHorse-1-9B + Q4_K_M + llama.cpp + Windows x64 + CPU-only inference settings**

used for this ThumbLLM release.

## Third-Party Software

ThumbLLM uses third-party software subject to its respective licenses.

See:

`THIRD-PARTY-NOTICES.md`

for licensing and attribution information applicable to this release.
