# ThumbLLM Recipe: Qwen3.5 4B MTP CPU Edition

## Model

- **Model:** unsloth/Qwen3.5-4B-MTP-GGUF
- **Model creator:** Qwen
- **GGUF packager:** Unsloth AI
- **Model source:** https://huggingface.co/unsloth/Qwen3.5-4B-MTP-GGUF
- **Quantization:** Q4_K_M
- **Format:** GGUF
- **Model file:** Qwen3.5-4B-Q4_K_M.gguf
- **Model size:** 2.83 GB
- **Model license:** Apache-2.0
- **Upstream model:** Qwen/Qwen3.5-4B
- **Upstream base model:** Qwen/Qwen3.5-4B-Base

## ThumbLLM Release

- **ThumbLLM version:** 0.1.0
- **Edition:** Qwen3.5-4B CPU Edition
- **Release / revision date:** 2026-08-29
- **Executable:** ThumbLLM-Qwen3.5-4B-MTP-Q4_K_M-CPU-Win-x64-0.1.0.exe
- **SHA-256:** FC1446F802A98869746FC24EF7B5E88561336399A0E8EEF01C0A11508427739D

The date above is the `APP_REVISION` value embedded in this ThumbLLM build.

## Platform

- **Operating system:** Windows
- **Architecture:** x64
- **Backend:** CPU
- **Target hardware:** 64-bit Windows x64 CPU, with AMD Ryzen-class processors as the primary target

This edition is explicitly configured for CPU-only inference.

GPU inference is disabled even when a compatible GPU or integrated GPU is present.

## Runtime

- **Runtime:** llama.cpp
- **Runtime version:** 0.2.0-dev
- **llama.cpp build:** 10603
- **llama.cpp commit:** c060ca974
- **Runtime package:** Native `llama-server.exe` for Windows x86_64
- **Compiler:** Clang 20.1.8
- **Target:** Windows x86_64

Exact runtime identification:

```text
version: 0.2.0-dev (build 10603, commit c060ca974)
built with Clang 20.1.8 for Windows x86_64
```

ThumbLLM prefers the bundled native `llama-server.exe`.

The source also permits a CPU `llama-cpp-python` fallback if the native server cannot be found. The native llama-server configuration documented here is the intended runtime for this release.

## Purpose

This ThumbLLM edition packages **Qwen3.5-4B Q4_K_M** as a preconfigured **Windows x64 CPU-only** local AI application.

It is designed for users who want to run Qwen3.5-4B directly on a Windows CPU without requiring a discrete GPU or manually configuring llama.cpp.

This release combines Qwen3.5-4B Q4_K_M with llama.cpp build 10603 and a CPU-focused inference configuration using automatic CPU topology detection, Flash Attention, quantized KV cache, large-context support, and explicit prevention of GPU offload.

## ThumbLLM Features

- Built-in local desktop chat
- Local OpenAI-compatible API
- Automatic GGUF model download
- Resumable model downloads
- HTTP Range resume support
- Automatic network retry and backoff
- GGUF model validation
- SHA-256 model verification
- Automatic replacement of corrupt model files
- Streaming generation
- Generation statistics
- Generation cancellation
- Context and prompt management
- Automatic conversation-history trimming
- Hardware information display
- Hardware-aware CPU thread selection
- Local inference after the initial model download
- Explicit CPU-only llama.cpp configuration
- Thinking / reasoning UI control
- Clean llama-server shutdown when ThumbLLM exits

## Model Download

- **Download source:** Hugging Face
- **Repository:** unsloth/Qwen3.5-4B-MTP-GGUF
- **Filename:** Qwen3.5-4B-Q4_K_M.gguf
- **Expected size:** 2.83 GB
- **Verification method:** GGUF header validation + SHA-256
- **Expected model SHA-256:** 3874209241c9a397e2f62cd3f70f80fd2dfbf0dfccb6838416bdb48a714e8630

Direct download:

```text
https://huggingface.co/unsloth/Qwen3.5-4B-MTP-GGUF/resolve/main/Qwen3.5-4B-Q4_K_M.gguf
```

The model weights are **downloaded separately**.

The GGUF is not bundled with the ThumbLLM executable.

When running as a compiled executable, ThumbLLM stores the model beside `ThumbLLM-Qwen3.5-4B-MTP-Q4_K_M-CPU-Win-x64-0.1.0.exe`.

During download, ThumbLLM uses:

```text
Qwen3.5-4B-Q4_K_M.tmp
```

The downloader configuration includes:

```text
Chunk size:               1 MiB
Maximum network retries:  8
Request timeout:          90 seconds
Retry backoff base:       2 seconds
Maximum normal backoff:   30 seconds
Clean integrity retries:  1
Free-space safety margin: 16 MiB
```

The downloader supports:

- HTTP Range resume
- preservation of partial downloads after transient network failures
- automatic retry after connection interruptions
- exponential retry backoff
- HTTP `Retry-After` handling
- verification of resumed byte offsets
- safe restart if a server ignores a Range request
- detection and repair of stale or oversized partial files
- premature EOF detection
- oversized-transfer detection
- free-disk-space checking
- GGUF header validation
- full SHA-256 verification
- one clean byte-zero retry following an integrity failure
- atomic installation of the verified final model
- automatic replacement of corrupt existing model files

An explicit user cancellation removes the temporary partial download.

Transient network or recoverable storage failures may preserve the temporary file so a later run can resume it.

## Inference Configuration

### Core Settings

- **Context size:** 65,536 tokens
- **Maximum generated tokens:** 12,288
- **Threads:** Auto — physical CPU core count
- **Threads batch:** Auto — logical processor count
- **Batch size:** 2,048
- **Micro-batch size:** 512
- **GPU layers:** 0
- **Flash Attention:** ON
- **Memory mapping (mmap):** ON
- **Memory locking (mlock):** OFF
- **Temperature:** 0.7
- **Top-p:** 0.9
- **Parallel slots:** 1
- **Context auto-trim:** ON
- **Context safety margin:** 256 tokens

### CPU Settings

- **CPU backend:** CPU
- **Thread count:** Auto — detected physical CPU core count
- **Thread count on tested Ryzen AI Max+ 395:** 16
- **Batch thread count:** Auto — detected logical processor count
- **Batch thread count on tested Ryzen AI Max+ 395:** 32
- **Thread affinity:** Default / no explicit affinity mask
- **NUMA:** Default / no explicit NUMA mode
- **CPU strict:** OFF
- **CPU strict batch:** OFF
- **Decode priority:** 2
- **Batch priority:** 1
- **Decode polling:** 50
- **Batch polling:** 1

Active thread configuration:

```text
N_THREADS = 0
N_THREADS_BATCH = 0
```

A value of `0` activates ThumbLLM's hardware-aware automatic selection.

Decode uses the detected number of **physical CPU cores**.

Prompt and batch processing use the detected number of **logical processors**.

The hardware-information stage is informational only. It does not reject a system because of its CPU, RAM, or GPU configuration.

### GPU / Accelerator Settings

- **GPU backend:** N/A — CPU-only inference
- **Primary device:** none
- **GPU layers:** 0
- **Tensor split:** N/A
- **Main GPU:** N/A
- **Split mode:** N/A
- **KV GPU offload:** OFF
- **Operation GPU offload:** OFF

CPU-only operation is explicitly enforced using:

```text
--device none
--gpu-layers 0
```

Additional offload controls:

```text
KV_OFFLOAD = False
OP_OFFLOAD = False
```

Therefore:

- model layers are not offloaded to a GPU
- KV operations are not offloaded to a GPU
- tensor operations are not offloaded to a GPU

Any GPU detected in the system is intentionally unused for inference by this edition.

### KV Cache

- **K cache type:** q4_0
- **V cache type:** q4_0
- **KV offload:** OFF
- **KV configuration notes:** Both K and V caches use q4_0. GPU KV offload is explicitly disabled.

Active values:

```text
KV_CACHE_TYPE_K = q4_0
KV_CACHE_TYPE_V = q4_0
KV_OFFLOAD = False
```

### Speculative Decoding

- **Enabled:** NO
- **Type:** NONE
- **Draft model:** N/A
- **Maximum draft tokens:** N/A
- **Minimum draft tokens:** N/A
- **Acceptance settings:** N/A

The authoritative active setting is:

```text
SPEC_TYPE = "none"
```

The source retains configuration infrastructure for MTP, DFlash, and other speculative-decoding methods, but those settings are inactive.

No speculative-decoding arguments are emitted to llama-server for this release.

The `MTP` designation in the Hugging Face repository name therefore does **not** mean that MTP speculative decoding is enabled in this ThumbLLM edition.

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

On the tested **AMD Ryzen AI Max+ 395**, ThumbLLM resolves automatic threading to 16 physical decode threads and 32 logical prompt/batch threads.

The equivalent native command for that system is:

```text
llama-server.exe ^
  --model "Qwen3.5-4B-Q4_K_M.gguf" ^
  --host 127.0.0.1 ^
  --port 8080 ^
  --ctx-size 65536 ^
  --threads 16 ^
  --threads-batch 32 ^
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

On another CPU, ThumbLLM automatically substitutes that machine's detected physical-core count for `--threads` and logical-processor count for `--threads-batch`.

No speculative-decoding arguments are emitted.

No tensor-split, main-GPU, GPU-fit, or GPU split-mode arguments are emitted in the CPU-only profile.

## API Configuration

- **API type:** OpenAI-compatible
- **Default host:** 127.0.0.1
- **Default port:** 8080
- **Base URL:** http://127.0.0.1:8080/v1
- **Network exposure:** LOCALHOST ONLY
- **Authentication:** NONE
- **API enabled:** YES
- **LAN access:** OFF
- **API model ID:** Qwen3.5-4B
- **llama.cpp model alias:** local-qwen3.5-4b

Endpoints include:

```text
POST /v1/chat/completions
GET  /v1/models
GET  /health
```

The active model profile contains:

```text
API_KEY = "local-qwen"
```

but authentication is disabled because:

```text
API_REQUIRE_KEY = False
```

## Tested Hardware

- **System:** AMD Ryzen AI Max+ 395 system
- **CPU:** AMD Ryzen AI Max+ 395 (Strix Halo / Zen 5)
- **CPU cores / threads:** 16 cores / 32 threads
- **Active decode threads:** 16
- **Active prompt / batch threads:** 32
- **GPU / accelerator:** AMD Radeon 8060S integrated graphics — present but not used for inference
- **GPU compute:** Disabled
- **VRAM:** N/A for CPU-only benchmark
- **System RAM:** Not recorded for this benchmark
- **Memory type / speed:** Not recorded for this benchmark
- **Operating system:** Windows x64
- **Driver:** N/A for CPU-only inference
- **Inference backend:** CPU
- **llama.cpp version:** 0.2.0-dev
- **llama.cpp build:** 10603
- **llama.cpp commit:** c060ca974
- **Compiler:** Clang 20.1.8

AMD specifies the Ryzen AI Max+ 395 as a 16-core / 32-thread Zen 5 processor with Radeon 8060S integrated graphics.

The Radeon 8060S was not used for this ThumbLLM benchmark.

## Benchmark Configuration

- **Prompt / test:** Exact prompt was not recorded
- **Prompt tokens:** Not recorded
- **Generated tokens:** Not tokenizer-verified
- **Context occupancy:** Not recorded
- **Number of documented runs:** 1
- **Warm-up:** Not recorded
- **Thinking:** OFF
- **Inference backend:** CPU only
- **GPU offload:** Disabled
- **Decode threads:** 16
- **Prompt / batch threads:** 32
- **Context size:** 65,536
- **Temperature:** 0.7
- **Top-p:** 0.9
- **llama.cpp build:** 10603
- **llama.cpp commit:** c060ca974

The documented benchmark is based on a screenshot captured during an active long-form generation on the Ryzen AI Max+ 395.

At the time of capture, ThumbLLM displayed:

```text
18.0 tok/s
Tokens: 1211
Time: 67.4s
Temp: 0.7
Top-p: 0.9
Ctx: 64K
CPU: 16t
```

### Benchmark Measurement Note

The current ThumbLLM application increments its displayed `Tokens` counter once for each output-bearing streamed response chunk.

Therefore, the displayed `Tokens: 1211` value is an **application stream-count metric** and has not been independently verified by tokenizing the generated response.

The displayed `18.0 tok/s` value is likewise an **application-reported generation-throughput measurement** calculated from that stream count and elapsed wall-clock time.

It should not be represented as a native llama.cpp decode benchmark until llama.cpp's own prompt/decode timing data is recorded.

The screenshot was also captured while the **Stop** button was still active, so `67.4 s` and `1211` represent values **at the time of capture**, not necessarily the completed generation totals.

## Performance

| Metric | Result |
| --- | ---: |
| Model load time | Not recorded |
| Prompt processing | Not recorded |
| Native llama.cpp decode speed | Not recorded |
| ThumbLLM app-reported generation throughput at capture | **18.0 tok/s** |
| ThumbLLM stream-count metric at capture | **1,211** |
| Elapsed generation time at capture | **67.4 s** |
| Completed generation time | Not recorded |
| Peak RAM usage | Not recorded |
| Peak VRAM usage | N/A — CPU-only |

The relationship between the two displayed application metrics is internally consistent:

```text
1211 / 67.4 ≈ 18.0
```

This benchmark should therefore be treated as an **observed ThumbLLM application-level performance result**, not yet as a formal native llama.cpp decode benchmark.

## Additional Benchmark Results

| Test | Prompt Processing | Generation Throughput | Notes |
| --- | ---: | ---: | --- |
| Ryzen AI Max+ 395 observed run | Not recorded | **18.0 app-reported tok/s** | CPU-only; 16 decode threads; screenshot captured while generation was still active |

A future benchmark revision should record:

- exact benchmark prompt
- exact prompt token count
- native llama.cpp prompt-processing speed
- native llama.cpp decode speed
- completed output token count
- completed generation time
- multiple measured runs
- warm-up procedure
- peak RAM usage

## Why These Settings

### Q4_K_M Quantization

The active model is:

```text
Qwen3.5-4B-Q4_K_M.gguf
```

The GGUF is approximately 2.83 GB.

Q4_K_M is the selected quantization for this ThumbLLM edition. No comparative quantization benchmark is currently recorded for this release, so the recipe does not claim that it is the fastest or highest-quality available quantization.

### Physical CPU Cores for Decode

The active setting is:

```text
N_THREADS = 0
```

This causes ThumbLLM to select the detected number of physical CPU cores.

On the Ryzen AI Max+ 395 benchmark system:

```text
Decode threads = 16
```

### Logical Processors for Prompt Processing

The active setting is:

```text
N_THREADS_BATCH = 0
```

This causes ThumbLLM to use the detected logical processor count for prompt and batch processing.

On the Ryzen AI Max+ 395:

```text
Prompt / batch threads = 32
```

### Explicit CPU-Only Operation

CPU-only inference is enforced using:

```text
--device none
--gpu-layers 0
```

This prevents model-layer GPU offload even if the bundled llama.cpp binary contains GPU backend support.

### GPU Offload Disabled

The active configuration also sets:

```text
KV_OFFLOAD = False
OP_OFFLOAD = False
```

This prevents KV and tensor-operation GPU offload.

### Flash Attention

Flash Attention is enabled:

```text
--flash-attn on
```

No A/B Flash Attention benchmark is currently documented for this release.

### Quantized KV Cache

Both KV caches are configured as:

```text
q4_0
```

rather than f16.

This reduces KV-cache memory requirements relative to an f16 KV cache, which is useful with the configured 65,536-token context window.

No formal q4_0-vs-f16 KV benchmark is currently recorded for this release.

### Batch Configuration

The active batch configuration is:

```text
N_BATCH = 2048
N_UBATCH = 512
```

No batch-size A/B benchmark is currently recorded for this release.

### Repack

Repacking is enabled:

```text
REPACK = True
```

The source identifies repacking as useful for CPU-friendly packed kernels.

### mmap

Memory mapping is enabled:

```text
USE_MMAP = True
```

This allows normal operating-system memory mapping and page-cache behavior.

### mlock

Memory locking is disabled:

```text
USE_MLOCK = False
```

This avoids requiring Windows memory-lock privileges.

### Single Parallel Slot

The server uses:

```text
SERVER_PARALLEL = 1
```

This configuration prioritizes a single interactive generation slot rather than multiple simultaneous decode slots.

### Speculative Decoding Disabled

The codebase includes infrastructure for MTP, DFlash, and other speculative methods, but:

```text
SPEC_TYPE = none
```

means speculative decoding is not part of this release.

### No Explicit CPU Affinity

No CPU masks, CPU ranges, strict topology pinning, or NUMA configuration are emitted.

This allows ThumbLLM to operate across different x64 CPU topologies without hard-coding an affinity layout.

## Tested Alternatives

No controlled A/B performance measurements for alternative inference configurations are currently recorded for this release.

| Configuration | Performance | Difference | Result |
| --- | ---: | ---: | --- |
| CPU-only Qwen3.5-4B Q4_K_M | 18.0 app-reported tok/s observed | Baseline | SELECTED |
| ROCm / GPU profile | Not measured for this release | N/A | INACTIVE |
| MTP speculative decoding | Not measured for this release | N/A | DISABLED |
| DFlash speculative decoding | Not measured for this release | N/A | DISABLED |

No performance difference should be inferred for the unmeasured alternatives.

## Known Limitations

- This release intentionally performs CPU-only inference.
- GPU model-layer offload is disabled.
- GPU KV offload is disabled.
- GPU operation offload is disabled.
- A detected GPU or integrated GPU is intentionally unused for inference.
- Speculative decoding is disabled.
- Multimodal / image input is not enabled in this ThumbLLM edition.
- LAN API access is disabled.
- The API listens on localhost only.
- API authentication is disabled.
- Internet access is required for the initial GGUF download unless a verified model already exists beside the executable.
- The model GGUF is not bundled with the executable.
- Exact performance varies with CPU architecture, clock speed, memory bandwidth, system RAM, power configuration, context length, and background activity.
- Native llama-server is the intended runtime. The optional llama-cpp-python fallback may not expose every native llama.cpp control used by this recipe.
- The current ThumbLLM UI `Tokens` and `tok/s` metrics are based on output-bearing streamed chunks rather than a separately tokenizer-verified output count.
- The documented 18.0 tok/s result is one application-level observation, not a multi-run native llama.cpp benchmark.
- Native prompt-processing and decode timing measurements have not yet been recorded.

## Recommended Hardware

The application deliberately does not enforce a minimum hardware threshold.

### Minimum

- **RAM:** No formally established minimum for this release
- **VRAM:** N/A
- **CPU:** 64-bit x86 CPU capable of running the bundled Windows x86_64 llama.cpp runtime
- **Storage:** At least enough free space for the 2.83 GB model plus the ThumbLLM application and runtime

### Recommended

- **RAM:** No formally benchmarked recommendation yet
- **VRAM:** N/A
- **CPU:** Modern AMD Ryzen or comparable x64 CPU
- **Storage:** SSD with at least 4 GB of free space for the model, application, runtime, and download overhead

The downloader performs an automatic free-space check when the remote model size is known and includes an additional 16 MiB safety margin.

## Files Included With This Release

```text
ThumbLLM-Qwen3.5-4B-MTP-Q4_K_M-CPU-Win-x64-0.1.0.exe
ThumbLLM-Qwen3.5-4B-MTP-Q4_K_M-CPU-Win-x64-0.1.0.sha256
```

Executable SHA-256:

```text
FC1446F802A98869746FC24EF7B5E88561336399A0E8EEF01C0A11508427739D
```

The checksum file identifies:

```text
FC1446F802A98869746FC24EF7B5E88561336399A0E8EEF01C0A11508427739D  ThumbLLM-Qwen3.5-4B-MTP-Q4_K_M-CPU-Win-x64-0.1.0.exe
```

The model GGUF is **not included** with the ThumbLLM executable.

On first run, ThumbLLM automatically downloads:

```text
Qwen3.5-4B-Q4_K_M.gguf
```

if a valid verified copy is not already present beside the executable.

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

- Qwen3.5-4B
- Q4_K_M GGUF
- 2.83 GB model file
- Windows x64
- CPU-only inference
- llama.cpp build 10603
- llama.cpp commit c060ca974
- explicit `--device none`
- explicit `--gpu-layers 0`
- automatic physical-core decode threading
- automatic logical-processor prompt threading
- 65,536-token context
- maximum generation allocation of 12,288 tokens
- 2,048 batch size
- 512 micro-batch size
- q4_0 K cache
- q4_0 V cache
- Flash Attention enabled
- mmap enabled
- mlock disabled
- KV GPU offload disabled
- operation GPU offload disabled
- repacking enabled
- one parallel decode slot
- speculative decoding disabled
- localhost OpenAI-compatible API
- resumable GGUF downloads
- automatic retry and exponential backoff
- SHA-256 GGUF verification
- corrupted-model detection and automatic replacement
- automatic conversation context trimming
- informational hardware detection
- automatic CPU topology-based thread selection

A CPU-only observed run on an **AMD Ryzen AI Max+ 395** showed **18.0 app-reported tok/s** during an active long-form generation.

That value is retained as an application-level observation rather than represented as a native llama.cpp decode benchmark.

## Reproducibility

This recipe documents the configuration used by the corresponding ThumbLLM executable.

Performance will vary based on processor, CPU topology, clock speed, memory bandwidth, available RAM, operating system, power configuration, background activity, prompt length, context occupancy, and other system characteristics.

The purpose of this recipe is to preserve the known-good combination of:

**model + quantization + runtime + platform + inference settings**

used for this ThumbLLM release.

### Model

```text
Repository:
unsloth/Qwen3.5-4B-MTP-GGUF

Upstream model:
Qwen/Qwen3.5-4B

Model:
Qwen3.5-4B-Q4_K_M.gguf

Format:
GGUF

Quantization:
Q4_K_M

Size:
2.83 GB

License:
Apache-2.0

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
ThumbLLM-Qwen3.5-4B-MTP-Q4_K_M-CPU-Win-x64-0.1.0.exe

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

Prompt / batch threads:
Detected logical processor count

Tested decode threads:
16

Tested prompt / batch threads:
32

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

### Documented CPU Performance Observation

```text
Test CPU:
AMD Ryzen AI Max+ 395

CPU architecture:
Zen 5 / Strix Halo

CPU cores / threads:
16 / 32

Integrated GPU:
AMD Radeon 8060S

GPU used for inference:
No

ThumbLLM app-reported generation throughput at capture:
18.0 tok/s

ThumbLLM stream-count metric at capture:
1211

Elapsed time at capture:
67.4 seconds

Native llama.cpp decode benchmark:
Not yet recorded
```

## Third-Party Software

ThumbLLM uses third-party software subject to its respective licenses.

See:

`THIRD-PARTY-NOTICES.md`

for licensing and attribution information applicable to this release.

