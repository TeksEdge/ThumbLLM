# ThumbLLM Recipe: Qwen3.5-4B CPU Edition

## Model

* **Model:** unsloth/Qwen3.5-4B-MTP-GGUF
* **Model creator:** Not specified in the ThumbLLM source code
* **Model source:** https://huggingface.co/unsloth/Qwen3.5-4B-MTP-GGUF
* **Quantization:** Q4_K_M
* **Format:** GGUF
* **Model file:** Qwen3.5-4B-Q4_K_M.gguf
* **Model size:** Determined from the downloaded GGUF at runtime
* **Model license:** Not specified in the supplied ThumbLLM source
* **Upstream/base model:** Qwen3.5-4B

## ThumbLLM Release

* **ThumbLLM version:** 0.1.0
* **Edition:** Qwen3.5-4B CPU Edition
* **Release date:** 2026-08-29
* **Executable:** ThumbLLM.exe
* **SHA-256:** FC1446F802A98869746FC24EF7B5E88561336399A0E8EEF01C0A11508427739D

## Platform

* **Operating system:** Windows
* **Architecture:** x64
* **Backend:** CPU
* **Target hardware:** 64-bit Windows AMD Ryzen laptop CPU

This edition is explicitly configured for CPU-only inference.

A Radeon iGPU may be detected by Windows, but it is intentionally not used for inference.

## Runtime

* **Runtime:** llama.cpp
* **llama.cpp build:** 10603
* **llama.cpp commit:** c060ca974
* **Runtime package:** llama.cpp native `llama-server` — Windows x86_64
* **Runtime version:** 0.2.0-dev
* **Compiler:** Clang 20.1.8

Exact runtime identification:

```text
version: 0.2.0-dev (build 10603, commit c060ca974)
built with Clang 20.1.8 for Windows x86_64
```

ThumbLLM prefers the bundled native `llama-server.exe`.

The application source also permits a CPU `llama-cpp-python` fallback when the native server cannot be found, but the native llama-server configuration documented here is the intended runtime recipe for this release.

## Purpose

This ThumbLLM edition packages **Qwen3.5-4B Q4_K_M** as a preconfigured **Windows x64 CPU-only** local AI application.

It is designed for users who want to run Qwen3.5-4B locally on a 64-bit Windows AMD Ryzen laptop or similar x64 CPU without requiring a discrete GPU.

The Radeon iGPU is intentionally not used by this edition.

This release combines Qwen3.5-4B Q4_K_M with llama.cpp build 10603 and a CPU-focused inference configuration using automatic CPU thread selection, Flash Attention, quantized KV cache, large-context support, and explicit GPU-offload prevention.

## ThumbLLM Features

* Built-in local desktop chat
* Local OpenAI-compatible API
* Automatic GGUF model download
* Resumable model downloads
* HTTP Range resume support
* Automatic network retry and backoff
* Model verification
* GGUF header validation
* SHA-256 integrity verification
* Automatic replacement of corrupt model files
* Streaming generation
* Generation statistics
* Generation cancellation
* Context and prompt management
* Automatic conversation-history trimming
* Hardware information display
* Hardware-aware CPU thread selection
* Local inference after the initial model download
* Explicit CPU-only llama.cpp configuration
* Thinking / reasoning UI control
* Clean llama-server shutdown when ThumbLLM exits

## Model Download

* **Download source:** Hugging Face
* **Repository:** unsloth/Qwen3.5-4B-MTP-GGUF
* **Filename:** Qwen3.5-4B-Q4_K_M.gguf
* **Expected size:** Determined from the remote HTTP response
* **Verification method:** GGUF header validation + SHA-256
* **Expected model SHA-256:** 3874209241c9a397e2f62cd3f70f80fd2dfbf0dfccb6838416bdb48a714e8630

Direct download:

```text
https://huggingface.co/unsloth/Qwen3.5-4B-MTP-GGUF/resolve/main/Qwen3.5-4B-Q4_K_M.gguf
```

The model weights are **downloaded separately**.

The GGUF is not bundled with the ThumbLLM executable.

When running as a compiled executable, ThumbLLM stores the GGUF beside the executable.

The downloader uses a temporary file:

```text
Qwen3.5-4B-Q4_K_M.tmp
```

during download.

The downloader configuration includes:

```text
Chunk size:              1 MiB
Maximum network retries: 8
Request timeout:         90 seconds
Retry backoff base:      2 seconds
Maximum backoff:         30 seconds
Integrity clean retries: 1
```

The downloader supports:

* resumable partial downloads
* HTTP Range requests
* automatic reconnection after transient network failures
* exponential retry backoff
* HTTP `Retry-After` handling
* verification of resumed byte offsets
* safe restart if a server ignores a Range request
* repair of stale or oversized partial downloads
* premature EOF detection
* oversized-transfer detection
* free-disk-space checking
* GGUF header verification
* full SHA-256 verification
* one clean byte-zero retry after an integrity failure
* atomic installation of the verified final GGUF
* automatic replacement of a corrupt existing model

Transient network failures preserve resumable temporary data where possible.

An explicit user cancellation removes the partial temporary download.

## Inference Configuration

### Core Settings

* **Context size:** 65,536
* **Threads:** Auto — physical CPU core count
* **Threads batch:** Auto — logical processor count
* **Batch size:** 2,048
* **Micro-batch size:** 512
* **GPU layers:** 0
* **Flash Attention:** ON
* **Memory mapping (mmap):** ON
* **Memory locking (mlock):** OFF

Additional generation settings:

```text
Maximum generated tokens: 12288
Temperature:              0.7
Top-p:                    0.9
Context auto-trim:        ON
Context safety margin:    256 tokens
Parallel slots:           1
```

### CPU Settings

* **CPU backend:** CPU
* **Thread count:** Automatically detected physical CPU core count
* **Thread affinity:** DEFAULT
* **NUMA:** DEFAULT / not explicitly configured

Decode configuration:

```text
N_THREADS = 0
CPU_PRIORITY = 2
POLL = 50
CPU_STRICT = False
```

Prompt / batch configuration:

```text
N_THREADS_BATCH = 0
CPU_PRIORITY_BATCH = 1
POLL_BATCH = 1
CPU_STRICT_BATCH = False
```

`N_THREADS = 0` tells ThumbLLM to use the detected number of physical CPU cores for decode.

`N_THREADS_BATCH = 0` tells ThumbLLM to use the detected number of logical processors for prompt and batch processing.

No explicit CPU affinity mask or CPU range is configured.

No explicit NUMA mode is configured.

The hardware-information stage is informational only and does not reject a computer because of its CPU, RAM, or GPU configuration.

### GPU / Accelerator Settings

* **GPU backend:** N/A
* **Primary device:** none
* **GPU layers:** 0
* **Tensor split:** N/A
* **Main GPU:** N/A
* **Split mode:** N/A

GPU inference is explicitly disabled using:

```text
--device none
--gpu-layers 0
```

Additional GPU-related configuration:

```text
KV_OFFLOAD = False
OP_OFFLOAD = False
```

Therefore:

* model layers are not offloaded to a GPU
* KV work is not offloaded to a GPU
* tensor operations are not offloaded to a GPU

The Radeon 740M-class iGPU identified by the hardware profile is intentionally unused for inference.

### KV Cache

* **K cache type:** q4_0
* **V cache type:** q4_0
* **KV offload:** OFF
* **KV configuration notes:** Both K and V KV caches are quantized to q4_0. GPU KV offload is explicitly disabled.

Active values:

```text
KV_CACHE_TYPE_K = q4_0
KV_CACHE_TYPE_V = q4_0
KV_OFFLOAD = False
```

### Speculative Decoding

* **Enabled:** NO
* **Type:** NONE
* **Draft model:** N/A
* **Maximum draft tokens:** N/A
* **Minimum draft tokens:** N/A
* **Acceptance settings:** N/A

The authoritative active setting is:

```text
SPEC_TYPE = "none"
```

The source contains retained MTP, DFlash, and other speculative-decoding configuration for alternate builds, but those settings are inactive.

No speculative-decoding arguments are emitted to llama-server in this release.

### Additional llama.cpp Settings

```text
CPU_ONLY_BUILD = True

INFERENCE_ENGINE = auto
LLAMA_SERVER_EXECUTABLE = auto
ALLOW_PYTHON_FALLBACK = True

SERVER_HOST = 127.0.0.1
SERVER_PORT = 8080
SERVER_STARTUP_TIMEOUT = 180
SERVER_REQUEST_TIMEOUT = 3600

N_CTX = 65536
MAX_TOKENS = 12288

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
DEVICE = none

FLASH_ATTN = on

KV_CACHE_TYPE_K = q4_0
KV_CACHE_TYPE_V = q4_0

KV_OFFLOAD = False
OP_OFFLOAD = False

REPACK = True

USE_MMAP = True
USE_MLOCK = False

NO_HOST = False
SWA_FULL = False

FIT = off

MULTI_GPU_MODE = single
TENSOR_SPLIT = ""
SERVER_PARALLEL = 1

BACKEND_SAMPLING = False

SPEC_TYPE = none

CACHE_PROMPT = True
CACHE_REUSE = 0

USE_JINJA = True

REASONING = off
SERVER_REASONING_MODE = auto
REASONING_FORMAT = deepseek
REASONING_BUDGET = -1

TEMPERATURE = 0.7
TOP_P = 0.9

CONTEXT_AUTO_TRIM = True
CONTEXT_SAFETY_MARGIN = 256
```

## Equivalent llama.cpp Configuration

ThumbLLM constructs the following equivalent native llama-server configuration.

Because the CPU thread values are determined dynamically, the two thread placeholders represent the hardware detected on the running computer.

```text
llama-server.exe ^
  --model "Qwen3.5-4B-Q4_K_M.gguf" ^
  --host 127.0.0.1 ^
  --port 8080 ^
  --ctx-size 65536 ^
  --threads <PHYSICAL_CPU_CORE_COUNT> ^
  --threads-batch <LOGICAL_PROCESSOR_COUNT> ^
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
  --mmap ^
  --cache-prompt ^
  --jinja
```

No speculative-decoding arguments are emitted.

No tensor-split, main-GPU, GPU-fit, or GPU split-mode arguments are emitted for the CPU-only build.

## API Configuration

* **API type:** OpenAI-compatible
* **Default host:** 127.0.0.1
* **Default port:** 8080
* **Base URL:** http://127.0.0.1:8080/v1
* **Network exposure:** LOCALHOST ONLY
* **Authentication:** NONE

Additional API configuration:

```text
API_ENABLED = True
API_ALLOW_LAN = False
API_REQUIRE_KEY = False

API_MODEL_ID = Qwen3.5-4B
LLAMA_MODEL_ALIAS = local-qwen3.5-4b
```

Endpoints include:

```text
POST /v1/chat/completions
GET  /v1/models
GET  /health
```

The model profile contains:

```text
API_KEY = "local-qwen"
```

but API authentication is disabled because:

```text
API_REQUIRE_KEY = False
```

## Benchmark Configuration

* **Prompt / test:** Single long-form text generation test
* **Prompt tokens:** Not recorded
* **Generated tokens:** 1,211
* **Context occupancy:** Not recorded
* **Number of runs:** 1 measured run
* **Warm-up:** Not recorded
* **Other benchmark conditions:** CPU-only inference; Thinking disabled; GPU offload disabled; 16 decode threads; 65,536-token context; temperature 0.7; top-p 0.9; llama.cpp build 10603, commit c060ca974.

## Performance

| Metric                    | Result |
| ------------------------- | -----: |
| Model load time           | Not recorded |
| Prompt processing         | Not recorded |
| Token generation / decode | **18.0 tok/s** |
| Total generation time     | **67.4 s** |
| Generated tokens          | **1,211** |
| Peak RAM usage            | Not recorded |
| Peak VRAM usage           | N/A — CPU-only |

## Tested Hardware

* **System:** AMD Ryzen AI Max+ 395 system
* **CPU:** AMD Ryzen AI Max+ 395
* **CPU cores / threads:** 16 cores / 32 threads
* **Active decode threads:** 16
* **GPU / accelerator:** Not used for inference
* **VRAM:** N/A
* **System RAM:** [ENTER INSTALLED RAM]
* **Operating system:** Windows x64
* **Inference backend:** CPU
* **llama.cpp build:** 10603
* **llama.cpp commit:** c060ca974

ThumbLLM calculates live generation statistics during use, including:

```text
Generated tokens
Tokens per second
Generation time
Temperature
Top-p
Context size
Active CPU thread count
```

These runtime values are not stored as fixed release benchmark results.

## Why These Settings

This release is deliberately configured as a **CPU-only Qwen3.5-4B Q4_K_M application**.

### Q4_K_M Quantization

The active model profile selects:

```text
Qwen3.5-4B-Q4_K_M.gguf
```

The supplied source does not contain comparative benchmark data against alternate Qwen3.5-4B quantizations, so no unsupported performance claim is made here.

### Physical CPU Cores for Decode

The active value:

```text
N_THREADS = 0
```

activates automatic hardware-aware decode-thread selection.

ThumbLLM chooses the detected number of physical CPU cores.

The source specifically identifies physical-core targeting as the default strategy for quantized CPU decode.

### Logical Processors for Prompt Processing

The active value:

```text
N_THREADS_BATCH = 0
```

causes prompt and batch processing to use the detected logical processor count.

This allows prompt processing to use the complete logical CPU topology while decode targets physical cores.

### Explicit CPU-Only Operation

CPU-only inference is enforced using:

```text
--device none
--gpu-layers 0
```

This remains true even if the bundled llama.cpp binary contains GPU backend support.

### GPU Offload Disabled

The active configuration also sets:

```text
KV_OFFLOAD = False
OP_OFFLOAD = False
```

preventing KV and tensor operations from being moved to GPU compute.

### Radeon iGPU Intentionally Unused

A Radeon 740M-class integrated GPU may be detected by Windows.

The build intentionally does not use it for inference.

### Flash Attention

Flash Attention is enabled:

```text
--flash-attn on
```

### Quantized KV Cache

Both caches use:

```text
q4_0
```

instead of f16.

This reduces KV-cache memory requirements, particularly with the configured 65,536-token context window.

### Batch Configuration

The active settings are:

```text
N_BATCH = 2048
N_UBATCH = 512
```

### Repack

Repacking is enabled:

```text
REPACK = True
```

The source identifies this as useful for CPU-friendly packed kernels.

### mmap

Memory mapping is enabled:

```text
USE_MMAP = True
```

for model-loading and operating-system page-cache behavior.

### mlock

Memory locking is disabled:

```text
USE_MLOCK = False
```

to avoid requiring Windows lock-memory privileges.

### Single Parallel Slot

The server uses:

```text
SERVER_PARALLEL = 1
```

The source identifies a single decode slot as the preferred configuration for single-chat latency rather than concurrent generations.

### Speculative Decoding Disabled

The codebase retains infrastructure for MTP, DFlash, and other speculative methods, but the active setting remains:

```text
SPEC_TYPE = none
```

No speculative-decoding configuration is therefore part of this release.

### No Explicit CPU Affinity

No CPU masks, ranges, strict topology pinning, or NUMA mode are configured.

This prevents the application from making fixed CPU-topology assumptions across different Ryzen laptop systems.

## Tested Alternatives

The source retains alternate configuration values for future or different ThumbLLM builds, but no benchmark measurements comparing them are included.

| Configuration               |       Decode |   Difference | Result   |
| --------------------------- | -----------: | -----------: | -------- |
| CPU-only Qwen3.5-4B Q4_K_M  | Not recorded |     Baseline | SELECTED |
| ROCm / GPU profile          | Not recorded | Not recorded | INACTIVE |
| MTP speculative decoding    | Not recorded | Not recorded | DISABLED |
| DFlash speculative decoding | Not recorded | Not recorded | DISABLED |

No performance difference should be inferred from this table without separate measurements.

## Known Limitations

* This release intentionally performs CPU-only inference.
* GPU model-layer offload is disabled.
* GPU KV offload is disabled.
* GPU operation offload is disabled.
* A detected Radeon iGPU is intentionally unused.
* Speculative decoding is disabled.
* LAN API access is disabled.
* The API listens on localhost only.
* API authentication is disabled.
* Internet access is required for the initial GGUF download unless a verified model already exists beside the executable.
* The model GGUF is not bundled with the executable.
* Exact inference performance varies with CPU architecture, clock speed, memory bandwidth, available RAM, context length, and background system load.
* Native llama-server is the preferred runtime. The Python fallback may not expose every native llama.cpp option in this recipe.
* No fixed benchmark measurements are included in the supplied release source.

## Recommended Hardware

The supplied code deliberately does not enforce a minimum hardware requirement.

### Minimum

* **RAM:** No fixed minimum specified by the supplied source
* **VRAM:** N/A
* **CPU:** 64-bit CPU capable of running the bundled Windows x86_64 llama.cpp runtime
* **Storage:** Sufficient space for the Qwen3.5-4B Q4_K_M GGUF, temporary download, executable, and runtime

### Recommended

* **RAM:** Not explicitly specified by the supplied source
* **VRAM:** N/A
* **CPU:** AMD Ryzen-class x64 processor
* **Storage:** SSD with sufficient free capacity for the model and temporary download

The downloader checks available disk space when the total remote model size is known.

It includes an additional approximately 16 MiB safety margin when calculating required free space.

## Files Included With This Release

```text
ThumbLLM.exe
ThumbLLM-Qwen3.5-4B-Q4_K_M-CPU-Win-x64-0.1.0.sha256
```

Executable SHA-256:

```text
FC1446F802A98869746FC24EF7B5E88561336399A0E8EEF01C0A11508427739D
```

The checksum file identifies:

```text
FC1446F802A98869746FC24EF7B5E88561336399A0E8EEF01C0A11508427739D  ThumbLLM.exe
```

The model GGUF is **not included** with the ThumbLLM executable.

On first run, ThumbLLM automatically downloads:

```text
Qwen3.5-4B-Q4_K_M.gguf
```

if a valid verified copy does not already exist beside the executable.

## Release Notes

This release packages **Qwen3.5-4B Q4_K_M** as a Windows x64 CPU-only ThumbLLM application.

Runtime:

```text
llama.cpp 0.2.0-dev
build 10603
commit c060ca974
Clang 20.1.8
Windows x86_64
```

Key release characteristics:

* Qwen3.5-4B
* Q4_K_M GGUF
* Windows x64
* CPU-only inference
* llama.cpp build 10603
* llama.cpp commit c060ca974
* explicit `--device none`
* explicit `--gpu-layers 0`
* automatic physical-core decode threading
* automatic logical-processor prompt threading
* 65,536-token context
* maximum generation allocation of 12,288 tokens
* 2,048 batch size
* 512 micro-batch size
* q4_0 K cache
* q4_0 V cache
* Flash Attention enabled
* mmap enabled
* mlock disabled
* KV GPU offload disabled
* operation GPU offload disabled
* repacking enabled
* one parallel decode slot
* speculative decoding disabled
* localhost OpenAI-compatible API
* resumable GGUF downloads
* network retry and exponential backoff
* SHA-256 GGUF verification
* corrupted-model detection and automatic replacement
* automatic conversation context trimming
* informational hardware detection
* automatic CPU topology-based thread selection

Although the selected model repository and ThumbLLM source retain MTP-related configuration, speculative decoding is **not enabled in this release**.

## Reproducibility

This recipe documents the configuration used by the corresponding ThumbLLM executable.

Performance will vary based on processor, CPU topology, clock speed, memory bandwidth, available RAM, operating system, background activity, prompt length, context occupancy, and other system characteristics.

The purpose of this recipe is to preserve the known-good combination of:

**model + quantization + runtime + platform + inference settings**

used for this ThumbLLM release.

### Model

```text
Repository:
unsloth/Qwen3.5-4B-MTP-GGUF

Model:
Qwen3.5-4B-Q4_K_M.gguf

Format:
GGUF

Quantization:
Q4_K_M

Model SHA-256:
3874209241c9a397e2f62cd3f70f80fd2dfbf0dfccb6838416bdb48a714e8630
```

### Runtime

```text
Runtime:
llama.cpp / llama-server

Version:
0.2.0-dev

Build:
10603

Commit:
c060ca974

Compiler:
Clang 20.1.8

Target:
Windows x86_64
```

### ThumbLLM Artifact

```text
Executable:
ThumbLLM.exe

ThumbLLM Version:
0.1.0

Edition:
Qwen3.5-4B CPU Edition

Platform:
Windows x64

Backend:
CPU

Executable SHA-256:
FC1446F802A98869746FC24EF7B5E88561336399A0E8EEF01C0A11508427739D
```

### Core Inference Recipe

```text
Backend:
CPU

Device:
none

GPU layers:
0

Context:
65536

Maximum generation:
12288

Decode threads:
Detected physical CPU core count

Batch threads:
Detected logical processor count

Decode priority:
2

Batch priority:
1

Decode poll:
50

Batch poll:
1

Batch:
2048

Micro-batch:
512

Flash Attention:
on

K cache:
q4_0

V cache:
q4_0

KV GPU offload:
off

Operation GPU offload:
off

Repack:
on

mmap:
on

mlock:
off

Parallel slots:
1

Speculative decoding:
none

Temperature:
0.7

Top-p:
0.9

API host:
127.0.0.1

API port:
8080

LAN API:
disabled

API authentication:
disabled
```

## Third-Party Software

ThumbLLM uses third-party software subject to its respective licenses.

See:

`THIRD-PARTY-NOTICES.md`

for licensing and attribution information applicable to this release.

