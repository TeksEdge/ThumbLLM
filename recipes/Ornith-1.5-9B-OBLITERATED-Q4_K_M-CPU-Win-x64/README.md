# ThumbLLM Recipe: Ornith 1.5 9B CPU Edition

## Model

* Model: OBLITERATUS/Ornith-1.5-9B-OBLITERATED
* Model file: `Ornith-1.5-9B-OBLITERATED-Q4_K_M.gguf`
* Quantization: Q4_K_M
* Format: GGUF
* Base architecture: Qwen3.5 hybrid (Gated DeltaNet + full attention)
* Backend: CPU
* Platform: Windows
* Architecture: x64
* GPU inference: Disabled
* Model SHA-256: `88ce99bb696fbc921edc4f995c4beb12a66bf94319d00bfd5f44265d02fa6dfb`

## Runtime

* Runtime: llama.cpp
* Build: b10603
* Commit: `c060ca974`
* Runtime version: `0.2.0-dev`
* Compiler: Clang 20.1.8
* Target: Windows x86_64

## Purpose

This ThumbLLM edition packages Ornith 1.5 9B as a preconfigured Windows x64 CPU application.

The build is intended for users who want to run a capable 9-billion-parameter local language model entirely on an AMD Ryzen CPU without requiring a discrete GPU or manually configuring llama.cpp.

The system may contain an integrated or discrete GPU, but this edition deliberately disables GPU inference. The llama.cpp runtime is explicitly launched with `--device none` and `--gpu-layers 0`.

## ThumbLLM Features

* Built-in local desktop chat
* Local OpenAI-compatible API
* Automatic GGUF model download
* SHA-256 model verification
* Resumable model download with retry support
* Streaming generation
* Live generation statistics
* Tokens-per-second reporting
* Context history management
* Automatic context trimming
* Prompt caching
* Local model/API operation after the initial model download
* Hardware preflight on startup
* Automatic CPU topology detection
* Native llama-server execution
* Optional llama-cpp-python fallback
* Reasoning/thinking display support
* CPU-only inference enforcement

The active model and checksum are hardcoded into the application, and the application automatically downloads the expected GGUF when it is not already present.

## Inference Configuration

* Context size: 65,536 tokens
* Maximum generation: 12,288 tokens
* Decode threads: Automatic
* Prompt/batch threads: Automatic
* Thread configuration: `N_THREADS = 0`, `N_THREADS_BATCH = 0`
* Tested resolved decode threads: 8 physical CPU cores
* Tested resolved batch threads: 16 logical CPU threads
* CPU priority: 2
* Batch CPU priority: 1
* CPU strict affinity: Off
* Batch CPU strict affinity: Off
* Poll: 50
* Batch poll: 1
* Batch size: 2,048
* Micro-batch size: 512
* Flash Attention: On
* KV cache K: Q4_0
* KV cache V: Q4_0
* GPU layers: 0
* Device: `none`
* KV offload: Off
* Operation offload: Off
* Repack: On
* mmap: On
* mlock: Off
* Parallel slots: 1
* Prompt cache: On
* Cache reuse: 0
* Jinja chat templates: On
* Backend sampling: Off
* Speculative decoding: Off
* Reasoning default: Off

The application sets both thread values to `0`, then detects the installed CPU topology and uses the physical-core count for decode and logical-processor count for prompt/batch processing.

The CPU-only profile explicitly disables GPU layers, KV offload, and operation offload while keeping Flash Attention and tensor repacking enabled. It uses Q4_0 for both K and V KV-cache types.

### Effective llama-server Recipe

```text
--ctx-size 65536
--threads 8
--threads-batch 16
--cpu-strict 0
--cpu-strict-batch 0
--prio 2
--prio-batch 1
--poll 50
--poll-batch 1
--batch-size 2048
--ubatch-size 512
--gpu-layers 0
--device none
--flash-attn on
--cache-type-k q4_0
--cache-type-v q4_0
--parallel 1
--cache-reuse 0
--no-kv-offload
--no-op-offload
--repack
--mmap
--cache-prompt
--jinja
```

These values are emitted directly by the native llama-server command builder.
Speculative decoding is intentionally disabled:

```text
SPEC_TYPE = "none"
```

The source still contains legacy Qwen3.5-4B MTP/DFlash settings for rollback and future experimentation, but those settings are inactive in this Ornith build.

## Tested Hardware

* CPU: AMD Ryzen 7 7840HS
* CPU cores: 8 physical cores / 16 logical processors
* Integrated GPU: AMD Radeon 780M
* GPU usage for inference: None
* RAM: 16 GB
* Operating System: Windows 11 x64

The Radeon 780M is present in the test system but is not used for model inference. This edition runs the model entirely on the Ryzen 7 7840HS CPU.

## Performance

* Prompt processing: approximately 1,302 tok/s
* Decode: approximately 13.0 tok/s
* Measured decode: approximately 13.03 tok/s
* Model: Ornith-1.5-9B-OBLITERATED Q4_K_M
* Compute: CPU only
* GPU offload: 0 layers

A key optimization discovered during testing was allowing llama.cpp to use the detected CPU topology automatically.

Previous configuration:

```text
--threads 9
--threads-batch 12
```

Approximate decode performance:

```text
9 tok/s
```

Optimized configuration:

```text
N_THREADS = 0
N_THREADS_BATCH = 0
```

On the tested Ryzen 7 7840HS, ThumbLLM resolves those automatic settings to:

```text
--threads 8
--threads-batch 16
```

Approximate decode performance:

```text
13 tok/s
```

This represents roughly a 44% increase in decode throughput without changing the model, quantization, hardware, or llama.cpp build.

The optimization demonstrates an important part of the ThumbLLM approach: a seemingly reasonable fixed thread configuration can perform substantially worse than a hardware-aware configuration.

## Model Source

Repository:

```text
OBLITERATUS/Ornith-1.5-9B-OBLITERATED
```

GGUF:

```text
Ornith-1.5-9B-OBLITERATED-Q4_K_M.gguf
```

Download source:

```text
https://huggingface.co/OBLITERATUS/Ornith-1.5-9B-OBLITERATED/resolve/main/Ornith-1.5-9B-OBLITERATED-Q4_K_M.gguf
```

SHA-256:

```text
88ce99bb696fbc921edc4f995c4beb12a66bf94319d00bfd5f44265d02fa6dfb
```

## API

The ThumbLLM application exposes the model through a local OpenAI-compatible API.

Default configuration:

```text
Host: 127.0.0.1
Port: 8080
API base: http://127.0.0.1:8080/v1
Model ID: Ornith-1.5-9B-OBLITERATED
LAN access: Disabled
API key requirement: Disabled
```

The API is bound to localhost by default, so it is not exposed to other systems on the network unless LAN access is deliberately enabled.

## Generation Defaults

```text
Temperature: 0.7
Top-p: 0.9
Maximum output tokens: 12,288
Context window: 65,536
Context auto-trim: Enabled
Context safety margin: 256 tokens
Reasoning: Off
```

## CPU-Only Enforcement

This edition is deliberately configured as a true CPU-only build.

The active configuration is:

```text
N_GPU_LAYERS = 0
DEVICE = none
KV_OFFLOAD = false
OP_OFFLOAD = false
```

Even if the bundled llama.cpp runtime is capable of detecting a GPU, ThumbLLM explicitly instructs llama.cpp not to use it.

This makes the performance numbers in this recipe representative of CPU inference rather than hybrid CPU/GPU inference.

## Notes

This configuration represents a tested ThumbLLM recipe rather than a generic llama.cpp configuration.

The goal is to provide a known-good combination of:

* model
* quantization
* runtime
* runtime version
* operating system
* architecture
* hardware
* memory configuration
* inference settings

The configuration is intentionally opinionated.

ThumbLLM is designed so that users do not need to determine the correct llama.cpp flags themselves. The tested inference recipe is part of the application.

For this release, the most consequential CPU optimization was changing fixed thread counts to automatic hardware-aware thread selection.

On the tested AMD Ryzen 7 7840HS system, that changed the effective configuration from:

```text
9 decode threads / 12 batch threads
```

to:

```text
8 decode threads / 16 batch threads
```

and increased measured decode performance from approximately:

```text
9 tok/s
```

to approximately:

```text
13 tok/s
```

with no GPU acceleration.

