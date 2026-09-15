# ThumbLLM Recipe: Spark-X2.5-4B Q4_K_M CPU Edition

## Model

* **Model:** XHToken/Spark-X2.5-4B-GGUF
* **Model creator:** XHToken
* **Model source:** https://huggingface.co/XHToken/Spark-X2.5-4B-GGUF
* **Quantization:** Q4_K_M
* **Format:** GGUF
* **Model file:** Spark-X2.5-4B-Q4_K_M.gguf
* **Model size:** [VERIFY FINAL GGUF FILE SIZE]
* **Model license:** [VERIFY MODEL LICENSE]
* **Upstream/base model:** Spark-X2.5-4B
* **Native context:** Up to 1,000,000 tokens, as documented in the active model profile
* **ThumbLLM configured context:** 65,536 tokens
* **Languages:** More than 200, as documented in the active model profile

Spark-X2.5-4B is described by the active ThumbLLM model profile as a compact
general-purpose language model for conversation, writing, translation, reasoning,
coding, tool use, and agentic workflows.

The model profile describes a hybrid architecture using full attention and
sliding-window attention.

## ThumbLLM Release

* **ThumbLLM version:** 0.1.0
* **App revision:** 2026-09-14
* **Edition:** Spark-X2.5-4B Q4_K_M CPU Edition
* **Release date:** 2026-09-15
* **Executable:** ThumbLLM-Spark-X2.5-4B-Q4_K_M-CPU-Win-x64-v0.1.0.exe
* **SHA-256:** [ADD FINAL EXECUTABLE SHA-256]

## Platform

* **Operating system:** Windows
* **Architecture:** x64
* **Backend:** CPU
* **Target hardware:** 64-bit Windows AMD Ryzen system
* **GPU usage:** Disabled for inference
* **Detected iGPU:** Radeon-class iGPU may be present but is intentionally unused

## Runtime

* **Runtime:** llama.cpp
* **llama.cpp version:** 0.4.0-dev
* **llama.cpp build:** 10909
* **llama.cpp commit:** a2878d30d
* **Runtime package:** Bundled native `llama-server.exe`
* **Compiler:** Clang 20.1.8
* **Runtime platform:** Windows x86_64

## Purpose

This ThumbLLM edition packages **Spark-X2.5-4B Q4_K_M** as a preconfigured
**Windows x64 CPU-only** local AI application.

It is designed to run Spark-X2.5-4B locally on AMD Ryzen-class Windows systems
without requiring GPU inference.

This release uses a tested combination of model, quantization, runtime, and
inference settings selected to provide strong local inference performance
without requiring users to manually configure llama.cpp.

The Radeon iGPU is intentionally not used. ThumbLLM explicitly requests CPU-only
execution through llama.cpp.

## ThumbLLM Features

* Built-in local desktop chat
* Local OpenAI-compatible API
* Automatic GGUF model download
* SHA-256 model verification
* Resumable model downloads
* Automatic retry support for interrupted downloads
* Streaming generation
* Generation statistics
* Tokens-per-second display
* Context management and automatic history trimming
* Thinking-mode toggle
* Automatic API port selection
* Hardware information at startup
* Local inference after the initial model download
* Hardware-specific llama.cpp configuration

## Model Download

* **Download source:** https://huggingface.co/XHToken/Spark-X2.5-4B-GGUF
* **Repository:** XHToken/Spark-X2.5-4B-GGUF
* **Filename:** Spark-X2.5-4B-Q4_K_M.gguf
* **Expected size:** [VERIFY FINAL GGUF FILE SIZE]
* **Verification method:** SHA-256
* **Expected model SHA-256:** `adfcfa19a4ed6a5985da8bf565fe15f8e1a7e131d79bae2d19d48d1c40109428`

The model weights are **downloaded separately** and are not bundled inside the
ThumbLLM executable.

After the model has been downloaded and verified, inference runs locally.

## Inference Configuration

### Core Settings

* **Context size:** 65,536 tokens
* **Maximum output:** 12,288 tokens
* **Temperature:** 0.7
* **Top-p:** 0.9
* **Threads:** Auto — physical CPU cores detected at runtime
* **Threads batch:** Auto — logical processors detected at runtime
* **Batch size:** 2,048
* **Micro-batch size:** 512
* **GPU layers:** 0
* **Flash Attention:** On
* **Load mode:** mmap
* **Memory mapping:** On through `--load-mode mmap`
* **Memory locking:** Off
* **Parallel slots:** 1
* **Prompt cache:** On
* **Cache reuse:** 0
* **Jinja chat templates:** On
* **Context auto-trim:** On
* **Context safety margin:** 256 tokens

### CPU Settings

* **CPU backend:** CPU
* **Decode thread count:** Auto-selected physical core count
* **Prompt/batch thread count:** Auto-selected logical processor count
* **Decode priority:** 2
* **Prompt/batch priority:** 1
* **Decode poll:** 50
* **Prompt/batch poll:** 1
* **CPU strict:** Off
* **CPU strict batch:** Off
* **Thread affinity:** Default / not explicitly pinned
* **NUMA:** Default / not explicitly configured

The application performs a Windows hardware preflight before model startup.
When `N_THREADS = 0`, ThumbLLM selects the detected physical core count for
decode. When `N_THREADS_BATCH = 0`, it selects the detected logical processor
count for prompt and batch processing.

### GPU / Accelerator Settings

* **GPU backend:** N/A — CPU-only build
* **Primary device:** none
* **GPU layers:** 0
* **KV offload:** Off
* **Operation offload:** Off
* **Tensor split:** N/A
* **Main GPU:** N/A
* **Split mode:** Single / no multi-GPU split
* **GPU fit:** Off
* **Backend sampling:** Off

CPU-only execution is explicitly enforced with:

```text
--device none
--gpu-layers 0
--no-kv-offload
--no-op-offload
```

### KV Cache

* **K cache type:** q4_0
* **V cache type:** q4_0
* **KV offload:** Off
* **KV configuration notes:** Quantized K/V cache is used to reduce context-memory requirements compared with F16 KV.

### Speculative Decoding

* **Enabled:** No
* **Type:** None
* **Draft model:** N/A
* **Maximum draft tokens:** N/A
* **Minimum draft tokens:** N/A
* **Acceptance settings:** N/A

The source retains optional speculative-decoding controls for other ThumbLLM
editions, but they are inactive because:

```text
SPEC_TYPE = "none"
```

### Reasoning / Thinking

* **Desktop Thinking toggle:** Available
* **Default desktop state:** Off
* **Server reasoning mode:** auto
* **Reasoning format:** deepseek
* **Reasoning budget:** -1

The desktop application controls per-request thinking behavior while the native
llama.cpp server starts in automatic reasoning mode.

### Additional llama.cpp Settings

```text
CPU_ONLY_BUILD = True

N_THREADS = 0
N_THREADS_BATCH = 0

CPU_PRIORITY = 2
CPU_PRIORITY_BATCH = 1

POLL = 50
POLL_BATCH = 1

CPU_STRICT = False
CPU_STRICT_BATCH = False

N_BATCH = 2048
N_UBATCH = 512

N_GPU_LAYERS = 0
FLASH_ATTN = on

KV_CACHE_TYPE_K = q4_0
KV_CACHE_TYPE_V = q4_0

KV_OFFLOAD = False
OP_OFFLOAD = False
REPACK = True

DEVICE = none
FIT = off

SERVER_PARALLEL = 1
BACKEND_SAMPLING = False

SPEC_TYPE = none

CACHE_PROMPT = True
CACHE_REUSE = 0
USE_JINJA = True

LOAD_MODE = mmap
LAZY_MODE = disabled

REASONING = off
SERVER_REASONING_MODE = auto
REASONING_FORMAT = deepseek
REASONING_BUDGET = -1
```

## Equivalent llama.cpp Configuration

ThumbLLM resolves the actual decode and prompt thread counts after hardware
detection. The equivalent native server command is therefore:

```text
llama-server.exe ^
  --model "Spark-X2.5-4B-Q4_K_M.gguf" ^
  --host 127.0.0.1 ^
  --port 8080 ^
  --ctx-size 65536 ^
  --threads <PHYSICAL_CPU_CORES_DETECTED_AT_RUNTIME> ^
  --threads-batch <LOGICAL_PROCESSORS_DETECTED_AT_RUNTIME> ^
  --cpu-strict 0 ^
  --cpu-strict-batch 0 ^
  --prio 2 ^
  --prio-batch 1 ^
  --poll 50 ^
  --poll-batch 1 ^
  --batch-size 2048 ^
  --ubatch-size 512 ^
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
  --load-mode mmap ^
  --cache-prompt ^
  --jinja
```

Port `8080` is preferred. If it is unavailable, ThumbLLM automatically searches
for the next free port.

## API Configuration

* **API type:** OpenAI-compatible
* **Default host:** 127.0.0.1
* **Preferred port:** 8080
* **Base URL:** `http://127.0.0.1:8080/v1`
* **Chat completions:** `http://127.0.0.1:8080/v1/chat/completions`
* **Models endpoint:** `http://127.0.0.1:8080/v1/models`
* **Health endpoint:** `http://127.0.0.1:8080/health`
* **Network exposure:** Localhost only
* **LAN access:** Disabled
* **Authentication:** None required by default
* **Port selection:** Automatic if 8080 is already occupied

ThumbLLM reserves the selected API port during startup to reduce collisions
between multiple ThumbLLM instances launched at nearly the same time.

## Tested Hardware

* **System:** [ADD TEST SYSTEM]
* **CPU:** [ADD EXACT CPU]
* **CPU cores / threads:** Auto-detected by ThumbLLM at runtime
* **GPU / accelerator:** Radeon iGPU present on the intended Ryzen platform but intentionally unused
* **VRAM:** N/A for inference
* **System RAM:** [ADD SYSTEM RAM]
* **Memory type / speed:** [ADD IF KNOWN]
* **Operating system:** Windows x64 [ADD EXACT VERSION]
* **Driver:** N/A for CPU inference

## Benchmark Configuration

* **Prompt / test:** [ADD BENCHMARK PROMPT OR TEST]
* **Prompt tokens:** [ADD VALUE]
* **Generated tokens:** [ADD VALUE]
* **Context occupancy:** [ADD VALUE / N/A]
* **Number of runs:** [ADD VALUE]
* **Warm-up:** [YES / NO]
* **Other benchmark conditions:** GPU inference disabled; CPU-only native llama.cpp runtime

## Performance

| Metric                    |        Result |
| ------------------------- | ------------: |
| Model load time           |       [VALUE] |
| Prompt processing         | [VALUE] tok/s |
| Token generation / decode | [VALUE] tok/s |
| Total generation time     |       [VALUE] |
| Peak RAM usage            |       [VALUE] |
| Peak VRAM usage           |           N/A |

## Additional Benchmark Results

| Test     | Prompt Processing |        Decode | Notes   |
| -------- | ----------------: | ------------: | ------- |
| [TEST 1] |     [VALUE] tok/s | [VALUE] tok/s | [NOTES] |
| [TEST 2] |     [VALUE] tok/s | [VALUE] tok/s | [NOTES] |
| [TEST 3] |     [VALUE] tok/s | [VALUE] tok/s | [NOTES] |

## Why These Settings

This build is deliberately configured as a CPU-only ThumbLLM edition.

The active profile uses automatic CPU topology detection rather than a fixed
thread count. Decode threads are mapped to the detected physical core count,
while prompt/batch threads use the detected logical processor count.

The selected CPU scheduling profile uses:

```text
Priority:       2
Batch priority: 1
Poll:           50
Batch poll:     1
```

This is the active performance-oriented CPU profile in the source. A more
conservative 8/16-thread, priority-1, poll-1 profile is retained in the source
as a commented alternative but is not active in this release.

Batching is configured at `2048 / 512`, while GPU layers remain hard-disabled
at zero.

Flash Attention remains enabled on the CPU backend.

The K and V caches use `q4_0` to reduce the memory cost of the 65,536-token
context window.

GPU offload, KV offload, and operation offload are all explicitly disabled so
that performance represents CPU inference rather than hidden iGPU acceleration.

The new llama.cpp runtime uses:

```text
--load-mode mmap
```

instead of relying on the older explicit mmap switch path.

Speculative decoding remains disabled in this edition.

## Tested Alternatives

| Configuration | Decode | Difference | Result |
| --- | ---: | ---: | --- |
| Auto physical/logical threads, priority 2, poll 50 | [VALUE] tok/s | Baseline | **SELECTED** |
| 8 decode / 16 batch threads, priority 1, poll 1 | [VALUE] tok/s | [VALUE] | Retained as stable alternative |
| [OTHER TESTED CONFIGURATION] | [VALUE] tok/s | [VALUE] | [SELECTED / REJECTED] |

Only measured results should be added to this table.

## Known Limitations

* This edition is CPU-only. GPU inference is intentionally disabled.
* The configured 65,536-token context is much smaller than the up-to-1M context described in the active model profile.
* Speculative decoding is disabled.
* Performance depends heavily on CPU architecture, memory bandwidth, thermals, system RAM, context length, and background activity.
* The Q4_K_M GGUF is a compressed quantization and may differ in output quality from higher-precision model weights.
* Model license and final GGUF file size should be verified before publishing this recipe.
* The native llama.cpp build and command-line options are version-specific to the runtime documented here.

## Recommended Hardware

### Minimum

* **RAM:** [ADD VERIFIED MINIMUM]
* **VRAM:** N/A
* **CPU:** Modern 64-bit x86 Windows CPU with supported llama.cpp CPU instructions
* **Storage:** Enough free SSD space for the GGUF plus application/runtime files

### Recommended

* **RAM:** [ADD VERIFIED RECOMMENDATION]
* **VRAM:** N/A
* **CPU:** Modern AMD Ryzen-class multi-core CPU
* **Storage:** SSD recommended

## Files Included With This Release

```text
ThumbLLM-Spark-X2.5-4B-Q4_K_M-CPU-Win-x64-v0.1.0.exe
ThumbLLM-Spark-X2.5-4B-Q4_K_M-CPU-Win-x64-v0.1.0.sha256
```

The model GGUF is **not included** with the ThumbLLM executable.

ThumbLLM downloads:

```text
Spark-X2.5-4B-Q4_K_M.gguf
```

separately and verifies it against:

```text
adfcfa19a4ed6a5985da8bf565fe15f8e1a7e131d79bae2d19d48d1c40109428
```

## Release Notes

This edition adds **Spark-X2.5-4B Q4_K_M** to ThumbLLM as a CPU-only Windows x64
application.

It also moves this ThumbLLM build to a newer native llama.cpp runtime:

```text
version: 0.4.0-dev
build: 10909
commit: a2878d30d
compiler: Clang 20.1.8
platform: Windows x86_64
```

The active inference profile uses automatic CPU topology detection, a
performance-oriented CPU scheduling profile, 2048/512 batching, q4_0 K/V cache,
Flash Attention, mmap model loading, and explicit CPU-only execution.

After the initial model download, the model can run locally without requiring a
cloud inference service.

## Reproducibility

This recipe documents the configuration used by the corresponding ThumbLLM
executable.

Performance will vary based on processor, memory bandwidth, operating system,
background activity, thermals, context length, and other system characteristics.

The purpose of this recipe is to preserve the known-good combination of:

**model + quantization + runtime + platform + inference settings**

used for this ThumbLLM release.

## Third-Party Software

ThumbLLM uses third-party software subject to its respective licenses.

See:

`THIRD-PARTY-NOTICES.md`

for licensing and attribution information applicable to this release.

