# ThumbLLM Recipe: MiniCPM5-2B CPU Edition

## Model

* **Model:** openbmb/MiniCPM5-2B-GGUF
* **Model creator:** OpenBMB
* **Model source:** https://huggingface.co/openbmb/MiniCPM5-2B-GGUF
* **Upstream model:** openbmb/MiniCPM5-2B
* **Architecture:** LlamaForCausalLM
* **Parameters:** 2,516,756,480
* **Quantization:** Q4_K_M
* **Format:** GGUF
* **Model file:** `MiniCPM5-2B-Q4_K_M.gguf`
* **Model size:** approximately 1.56 GB
* **Model license:** Apache-2.0
* **Native model context:** 131,072 tokens

## ThumbLLM Release

* **ThumbLLM version:** 0.1.0
* **Edition:** MiniCPM5-2B CPU Edition
* **Release / revision date:** 2026-09-07
* **Executable:** `ThumbLLM-MiniCPM5-2B-Q4_K_M-CPU-Win-x64-v0.1.0.exe`
* **Executable SHA-256:** `41457B82C740B93963A7A33FA370E5B2C708BCD0AAB92E35AE28BA3B9B37497C`

## Platform

* **Operating system:** Windows
* **Architecture:** x64
* **Inference backend:** CPU
* **GPU required:** No
* **Primary tested CPU:** AMD Ryzen AI Max+ 395
* **GPU inference:** Disabled

This edition is explicitly configured for CPU-only inference.

Any compatible GPU or integrated GPU present in the system is intentionally not used for model inference.

## Runtime

* **Runtime:** llama.cpp
* **Runtime version:** 0.2.0-dev
* **llama.cpp build:** 10603
* **llama.cpp commit:** c060ca974
* **Runtime package:** Native `llama-server.exe` for Windows x86_64
* **Compiler:** Clang 20.1.8
* **Target:** Windows x86_64

ThumbLLM prefers the bundled native `llama-server.exe`.

## Purpose

This ThumbLLM edition packages MiniCPM5-2B Q4_K_M as a preconfigured Windows x64 CPU-only local AI application.

MiniCPM5-2B is a compact dense language model from OpenBMB designed for on-device and local deployment. The official model supports long-context use, coding, mathematics, tool use, reasoning, and agentic tasks.

The goal of this ThumbLLM edition is to make the model usable as a normal Windows application without requiring the user to install or manually configure llama.cpp.

## ThumbLLM Features

* Built-in local desktop chat
* Local OpenAI-compatible API
* Automatic GGUF model download
* Resumable model downloads
* HTTP Range resume support
* Automatic network retry and backoff
* GGUF model validation
* SHA-256 model verification
* Automatic replacement of corrupt model files
* Streaming generation
* Live generation statistics
* Generation cancellation
* Context and conversation-history management
* Hardware-aware CPU thread selection
* Thinking / reasoning UI control
* Automatic API port selection
* Multiple ThumbLLM instances can use different local API ports
* Persistent API host / port display
* Clean llama-server shutdown when ThumbLLM exits
* Local inference after the initial model download

## Model Download

* **Download source:** Hugging Face
* **Repository:** openbmb/MiniCPM5-2B-GGUF
* **Filename:** `MiniCPM5-2B-Q4_K_M.gguf`
* **Expected size:** approximately 1.56 GB
* **Verification:** GGUF validation + SHA-256
* **Expected model SHA-256:** `ec2d5801640099e97d8d7e8003ad4d81f336e757811f03a26173dddf386602fd`

Direct download:

```text
https://huggingface.co/openbmb/MiniCPM5-2B-GGUF/resolve/main/MiniCPM5-2B-Q4_K_M.gguf
```

The GGUF model is downloaded separately and is not embedded in the ThumbLLM executable.

After the initial verified download, the model can be used locally without requiring a cloud inference service.

## Inference Configuration

### Core Settings

* **Configured context size:** 65,536 tokens
* **Maximum generated tokens:** 12,288
* **Context auto-trim:** ON
* **Context safety margin:** 256 tokens
* **Temperature:** 1.0
* **Top-p:** 0.95
* **Parallel slots:** 1

The model itself supports a native context length of 131,072 tokens, but this ThumbLLM edition is configured for a 65,536-token runtime context.

### CPU Settings

* **Decode threads:** Auto — physical CPU core count
* **Prompt / batch threads:** Auto — logical processor count
* **Decode priority:** 2
* **Batch priority:** 1
* **Decode polling:** 100
* **Batch polling:** 1
* **CPU strict:** OFF
* **CPU strict batch:** OFF
* **Explicit CPU affinity:** None
* **NUMA override:** None

Active configuration:

```text
N_THREADS = 0
N_THREADS_BATCH = 0

CPU_PRIORITY = 2
CPU_PRIORITY_BATCH = 1

POLL = 100
POLL_BATCH = 1

CPU_STRICT = False
CPU_STRICT_BATCH = False
```

A value of `0` causes ThumbLLM to select CPU thread counts using detected hardware topology.

On the tested AMD Ryzen AI Max+ 395:

```text
Decode threads: 16
Prompt / batch threads: 32
```

### Batch Configuration

```text
N_BATCH = 2048
N_UBATCH = 512
```

* **Batch size:** 2,048
* **Micro-batch size:** 512

### CPU-Only Configuration

```text
CPU_ONLY_BUILD = True
N_GPU_LAYERS = 0
DEVICE = none

KV_OFFLOAD = False
OP_OFFLOAD = False
```

The native llama.cpp command explicitly uses:

```text
--device none
--gpu-layers 0
--no-kv-offload
--no-op-offload
```

This prevents model layers, KV operations, and tensor operations from being offloaded to a GPU.

### Flash Attention

Flash Attention is enabled:

```text
FLASH_ATTN = on
```

Equivalent llama.cpp option:

```text
--flash-attn on
```

### KV Cache

This MiniCPM5 edition uses F16 for both K and V cache types:

```text
KV_CACHE_TYPE_K = f16
KV_CACHE_TYPE_V = f16
KV_OFFLOAD = False
```

This differs from previous ThumbLLM CPU editions that used quantized `q4_0` KV caches.

### Model Loading

```text
REPACK = True
USE_MMAP = True
USE_MLOCK = False
```

* Repack: ON
* mmap: ON
* mlock: OFF

### Parallelism

```text
SERVER_PARALLEL = 1
```

The release uses one server decode slot to prioritize interactive single-user generation performance.

## Sampling Configuration

OpenBMB recommends the following sampling parameters for MiniCPM5-2B:

```text
Temperature = 1.0
Top-p = 0.95
```

This ThumbLLM edition uses those values.

## Thinking / Reasoning

Default runtime setting:

```text
REASONING = off
```

ThumbLLM includes a user-facing Thinking toggle.

When enabled, ThumbLLM sends the model:

```text
enable_thinking = true
```

The model supports thinking-mode generation.

## Speculative Decoding

Speculative decoding is disabled in this release:

```text
SPEC_TYPE = none
```

No draft model is loaded and no speculative-decoding arguments are emitted to llama-server.

OpenBMB publishes a MiniCPM5-2B DSpark draft model, but that acceleration path is not enabled in this ThumbLLM llama.cpp edition.

## Equivalent llama.cpp Configuration

On the tested AMD Ryzen AI Max+ 395, automatic CPU topology detection resolves to approximately:

```bat
llama-server.exe ^
  --model "MiniCPM5-2B-Q4_K_M.gguf" ^
  --host 127.0.0.1 ^
  --port 8080 ^
  --ctx-size 65536 ^
  --threads 16 ^
  --threads-batch 32 ^
  --cpu-strict 0 ^
  --cpu-strict-batch 0 ^
  --prio 2 ^
  --prio-batch 1 ^
  --poll 100 ^
  --poll-batch 1 ^
  --batch-size 2048 ^
  --ubatch-size 512 ^
  --gpu-layers 0 ^
  --flash-attn on ^
  --cache-type-k f16 ^
  --cache-type-v f16 ^
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

The actual API port may differ from 8080 if that port is already occupied.

## Automatic API Port Selection

ThumbLLM prefers:

```text
127.0.0.1:8080
```

If port 8080 is already in use, the application automatically searches for the next available port.

For example:

```text
ThumbLLM instance 1 → 127.0.0.1:8080
ThumbLLM instance 2 → 127.0.0.1:8081
ThumbLLM instance 3 → 127.0.0.1:8082
```

On Windows, ThumbLLM also uses a named mutex during startup to prevent two ThumbLLM processes launched simultaneously from selecting the same API port.

The active API host and port remain visible in the application's status bar.

## API Configuration

* **API type:** OpenAI-compatible
* **Preferred host:** 127.0.0.1
* **Preferred port:** 8080
* **Port behavior:** Automatically selects next available port
* **Network exposure:** Localhost only
* **LAN access:** OFF
* **API authentication:** Disabled
* **API enabled:** YES

Base URL example:

```text
http://127.0.0.1:8080/v1
```

Endpoints include:

```text
POST /v1/chat/completions
GET  /v1/models
GET  /health
```

If another local service is already using port 8080, check the ThumbLLM status bar or Settings dialog for the active port.

## Tested Hardware

* **System:** AMD Ryzen AI Max+ 395 system
* **CPU:** AMD Ryzen AI Max+ 395
* **CPU architecture:** Zen 5
* **CPU cores / threads:** 16 cores / 32 threads
* **Active decode threads:** 16
* **Active prompt / batch threads:** 32
* **Integrated GPU:** Radeon 8060S
* **GPU used for inference:** No
* **Inference backend:** CPU
* **Operating system:** Windows x64

## Observed Performance

Approximately:

```text
36 tok/s
```

was observed in ThumbLLM during CPU-only MiniCPM5-2B Q4_K_M generation on the AMD Ryzen AI Max+ 395.

### Benchmark Measurement Note

The current ThumbLLM UI throughput counter is an application-level generation metric based on output-bearing streamed response chunks and wall-clock generation time.

Therefore, the observed approximately 36 tok/s result should be described as:

> **ThumbLLM app-reported generation throughput**

rather than a formal native llama.cpp `llama-bench` decode result.

Exact performance will vary with CPU architecture, clock speed, memory bandwidth, power configuration, context occupancy, prompt length, and background system load.

## Performance

| Metric                                      | Result                 |
| ------------------------------------------- | ---------------------- |
| Model load time                             | Not formally recorded  |
| Prompt processing                           | Not formally recorded  |
| Native llama.cpp decode speed               | Not formally recorded  |
| ThumbLLM app-reported generation throughput | Approximately 36 tok/s |
| GPU usage                                   | None                   |
| Decode threads on tested system             | 16                     |
| Prompt / batch threads on tested system     | 32                     |

## Why These Settings

### Q4_K_M

Q4_K_M provides a compact 1.56 GB model footprint while retaining useful model capability.

It is the official OpenBMB Q4_K_M GGUF used by this ThumbLLM edition.

### Physical Cores for Decode

ThumbLLM uses:

```text
N_THREADS = 0
```

which resolves to the physical CPU core count.

On the Ryzen AI Max+ 395:

```text
16 decode threads
```

### Logical Processors for Prompt Processing

ThumbLLM uses:

```text
N_THREADS_BATCH = 0
```

which resolves to the logical processor count.

On the Ryzen AI Max+ 395:

```text
32 prompt / batch threads
```

### Poll 100

The MiniCPM5 CPU edition uses:

```text
POLL = 100
```

for aggressive CPU polling during generation.

### F16 KV Cache

Both K and V caches use F16:

```text
K = f16
V = f16
```

This configuration was selected for the MiniCPM5 CPU edition rather than the quantized KV configuration used by some previous ThumbLLM builds.

### Flash Attention

Flash Attention is enabled:

```text
FLASH_ATTN = on
```

### CPU-Only Execution

GPU offload is explicitly disabled.

This makes the release suitable for users who want to run a compact modern language model without requiring a discrete GPU.

## Known Limitations

* This release intentionally performs CPU-only inference.
* GPU model-layer offload is disabled.
* GPU KV offload is disabled.
* GPU tensor-operation offload is disabled.
* Speculative decoding is disabled.
* Multimodal / image input is not enabled in this ThumbLLM edition.
* LAN API access is disabled.
* API authentication is disabled because the API is restricted to localhost.
* Internet access is required for the initial model download unless a verified GGUF is already present.
* The GGUF model is not bundled inside the executable.
* The configured runtime context is 65,536 tokens rather than the model's full 131,072-token native context.
* Exact generation performance varies by hardware and workload.
* The approximately 36 tok/s observation is an application-level ThumbLLM metric rather than a native llama.cpp benchmark.

## Recommended Hardware

### Minimum

* Windows x64
* 64-bit CPU capable of running the bundled llama.cpp runtime
* Enough RAM for the model and runtime
* Approximately 2 GB or more free storage for the model plus application overhead

### Recommended

* Modern AMD Ryzen or comparable x64 CPU
* SSD storage
* Multiple physical CPU cores
* Sufficient RAM for the selected context length

No discrete GPU is required.

## Files Included With This Release

```text
ThumbLLM-MiniCPM5-2B-Q4_K_M-CPU-Win-x64-v0.1.0.exe
ThumbLLM-MiniCPM5-2B-Q4_K_M-CPU-Win-x64-v0.1.0.exe.sha256
```

Executable SHA-256:

```text
41457B82C740B93963A7A33FA370E5B2C708BCD0AAB92E35AE28BA3B9B37497C
```

Checksum file contents:

```text
41457B82C740B93963A7A33FA370E5B2C708BCD0AAB92E35AE28BA3B9B37497C  ThumbLLM-MiniCPM5-2B-Q4_K_M-CPU-Win-x64-v0.1.0.exe
```

## Verify the Executable

In Windows PowerShell:

```powershell
Get-FileHash ".\ThumbLLM-MiniCPM5-2B-Q4_K_M-CPU-Win-x64-v0.1.0.exe" -Algorithm SHA256
```

Expected result:

```text
41457B82C740B93963A7A33FA370E5B2C708BCD0AAB92E35AE28BA3B9B37497C
```

## Model Attribution

MiniCPM5-2B is developed by OpenBMB.

Official model:

https://huggingface.co/openbmb/MiniCPM5-2B

Official GGUF:

https://huggingface.co/openbmb/MiniCPM5-2B-GGUF

ThumbLLM is not affiliated with or endorsed by OpenBMB.

## License

MiniCPM5-2B and its official GGUF distribution are released under the Apache-2.0 license.

ThumbLLM also includes or bundles third-party software components subject to their respective licenses.

See the repository's `licenses/` directory and `THIRD_PARTY_NOTICES.md` for additional licensing information.

