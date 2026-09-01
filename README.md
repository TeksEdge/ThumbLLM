# ThumbLLM

**ThumbLLM packages open-weight language models as portable, preconfigured local AI applications.**

Instead of asking the user to install a model manager, configure llama.cpp, choose a quantization, tune runtime flags, wire up an API, and figure out what works on their hardware, each ThumbLLM release is built around a specific known-good combination:

```text
Model
+ Quantization
+ Runtime
+ Hardware target
+ Tested inference recipe
+ Desktop chat
+ Local API
= ThumbLLM
```

The goal is simple:

> **Turn a local language model into something that behaves like an application.**

A ThumbLLM build can be copied to a machine, launched locally, and used through both a built-in chat interface and an OpenAI-compatible API.

For many builds, the only external asset is the model itself. If it is not already present, ThumbLLM can download the expected GGUF, verify it, and then run it locally.

After that, inference stays on your hardware.

---

# Why ThumbLLM Exists

Running a local model is technically easier than it used to be.

Running one **well** is still surprisingly complicated.

A typical local AI setup may require the user to understand:

* GGUF quantizations
* model architectures
* llama.cpp releases
* CPU thread counts
* GPU offload
* CUDA
* ROCm
* Vulkan
* MLX
* context size
* KV cache types
* Flash Attention
* speculative decoding
* batch sizes
* memory requirements
* model-specific quirks
* runtime compatibility
* API configuration

And the settings that work well for one model or one machine may be poor choices for another.

ThumbLLM takes a different approach.

Instead of distributing a generic model launcher and expecting the user to configure it, ThumbLLM releases are intended to be **opinionated builds**.

A release says, effectively:

> We tested this model, with this quantization, on this runtime, for this class of hardware, using these settings.

That recipe becomes part of the application.

---

# What a ThumbLLM Release Contains

A ThumbLLM release is designed around a specific configuration.

For example:

```text
ThumbLLM
Model: Ornith-1.5-9B-OBLITERATED
Quantization: Q4_K_M
Runtime: llama.cpp
Backend: CPU
OS: Windows
Architecture: x64
```

Another release might target:

```text
ThumbLLM
Model: Qwen3.8-27B
Quantization: Q4_K_XL
Runtime: llama.cpp
Backend: ROCm
OS: Windows
Architecture: x64
```

The intent is that the user does not need to reconstruct the inference recipe.

The application already knows what it expects.

---

# Two Interfaces, One Local Model

ThumbLLM is not only a chat application.

Each build can expose the same local model in two ways.

## 1. Built-In Desktop Chat

Users can interact directly with the model through the ThumbLLM desktop interface.

Depending on the build, the application may provide:

* streaming responses
* generation statistics
* token-per-second measurements
* context management
* reasoning/thinking display
* model and runtime information
* hardware information
* startup diagnostics
* automatic model detection
* automatic model download
* checksum verification

For someone who simply wants to talk to the model, the desktop chat may be all they ever need.

---

## 2. OpenAI-Compatible Local API

ThumbLLM can also expose the running model through a local OpenAI-compatible API.

Conceptually:

```text
Application / Agent / Script
          |
          v
http://127.0.0.1:PORT/v1
          |
          v
      ThumbLLM
          |
          v
      llama.cpp
          |
          v
        Model
```

This is important because it turns ThumbLLM from a standalone chat program into a reusable piece of local AI infrastructure.

Any software capable of talking to an OpenAI-compatible endpoint can potentially use a ThumbLLM instance as its model backend.

Examples include:

* agents
* coding tools
* document processors
* automation scripts
* desktop applications
* research tools
* custom frontends
* RAG systems
* workflow engines
* local AI experiments

---

# Multiple ThumbLLMs Can Become a Local Model Fleet

One interesting consequence of the API architecture is that ThumbLLM does not have to represent only one model on one computer.

Multiple ThumbLLM instances can potentially run simultaneously, assuming the system has enough RAM or VRAM.

For example:

```text
ThumbLLM Instance 1
Qwen3.8-4B
Port 8081

ThumbLLM Instance 2
Ornith-1.5-9B-OBLITERATED
Port 8082
```

An agent or application can then choose between them.

```text
                   ┌─────────────────────┐
                   │    Hermes Agent     │
                   └──────────┬──────────┘
                              │
                         Model Router
                         /          \
                        /            \
                       v              v
              localhost:8081   localhost:8082
                    |                |
                    v                v
               Qwen3.8-4B       Ornith-1.5-9B
```

The smaller model might handle:

* classification
* routing
* extraction
* simple tool calls
* short structured tasks
* high-frequency requests

The larger or differently trained model might handle:

* difficult reasoning
* longer responses
* creative generation
* unconstrained tasks
* complex agent decisions

That creates a local equivalent of a multi-model cloud architecture.

Except the models are running on your own machine.

---

# Local Model Routing

A particularly interesting use case is **local model specialization**.

Instead of asking one large model to perform every task, an agent could route requests to several small or midsize models.

For example:

```text
Incoming Request
       |
       v
Small Router Model
       |
       +------ Code ----------> Coding Model
       |
       +------ Writing -------> Writing Model
       |
       +------ Reasoning ------> Reasoning Model
       |
       +------ Extraction -----> Small Fast Model
```

Each endpoint could simply be another ThumbLLM process.

This opens up architectures such as:

* fast model + smart model
* censored model + uncensored model
* coding model + general model
* vision model + text model
* reasoning model + extraction model
* router model + specialist models
* CPU model + GPU model

The only hard limit is the hardware available to run them.

---

# Why Local APIs Matter

The built-in chat makes ThumbLLM useful to a person.

The API makes ThumbLLM useful to **software**.

That difference is significant.

A local API means a ThumbLLM model can quietly become a component in a larger system.

For example:

```text
Email
  |
  v
Local Agent
  |
  v
ThumbLLM API
  |
  v
Summarize / classify / extract
```

Or:

```text
Folder of Documents
        |
        v
Local Processing Script
        |
        v
ThumbLLM
        |
        v
Structured JSON
```

Or:

```text
Hermes Agent
     |
     +--> ThumbLLM Model A
     |
     +--> ThumbLLM Model B
     |
     +--> Local Search
     |
     +--> Local Files
```

The user does not necessarily need to know that several models are involved.

They simply become local services available to other software.

---

# Privacy

One of the strongest reasons to run models locally is straightforward:

> **Your prompts do not need to leave your computer.**

For workloads that can be handled locally, ThumbLLM can eliminate the need to send the input to a remote inference provider.

Potentially sensitive material can therefore remain on the machine running the model.

Examples include:

* private notes
* source code
* business documents
* internal reports
* financial spreadsheets
* customer information
* unpublished writing
* confidential research
* personal correspondence
* legal drafts
* proprietary data

Local inference does not automatically solve every security problem, but it removes one major dependency:

**remote model inference.**

---

# Security and Controlled Environments

Local models can also be useful in environments where internet access is intentionally limited.

Examples include:

* development networks
* isolated labs
* secure workstations
* offline research environments
* factory systems
* field laptops
* air-gapped systems
* temporary remote sites
* restricted enterprise networks

Once the required model is present, a ThumbLLM build can operate without relying on a cloud inference endpoint.

That makes local AI possible in places where cloud AI may be unavailable, undesirable, expensive, slow, or prohibited.

---

# Offline AI

Cloud AI assumes connectivity.

Local AI does not have to.

ThumbLLM can be useful in situations where connectivity is unreliable or nonexistent.

Examples include:

* airplanes
* ships
* remote field sites
* rural locations
* disaster-response environments
* hotels with poor internet
* temporary offices
* workshops
* laboratories
* job sites

A laptop containing ThumbLLM can continue generating, summarizing, coding, extracting, and reasoning without an external AI service.

---

# Thumb-Drive AI

The name **ThumbLLM** is intentional.

One of the design goals is portability.

A ThumbLLM application, its runtime, and its model can potentially live together on removable storage.

For example:

```text
USB Drive
│
├── ThumbLLM-Ornith-1.5-9B-OBLITERATED-Q4_K_M-CPU-Win-x64.exe
│
├── Ornith-1.5-9B-OBLITERATED-Q4_K_M.gguf
│
└── llama_runtime/
```

Plug the drive into a compatible machine.

Launch the application.

Run the model.

That makes the model itself portable.

Not merely the weights.

The **working inference environment** travels with it.

---

# A Portable AI Appliance

A useful way to think about ThumbLLM is as a tiny software appliance.

Traditional AI deployment often looks like:

```text
Install runtime
Install dependencies
Download model
Find correct quant
Configure backend
Configure GPU
Tune arguments
Start server
Configure API
Connect frontend
Debug
```

ThumbLLM aims to reduce that to something closer to:

```text
Launch ThumbLLM
```

This is deliberately closer to the experience of launching a normal application than configuring an inference framework.

---

# Known-Good Recipes

Performance tuning is a major part of the project.

The highest-performing configuration is often not obvious.

For example, while tuning a CPU build of:

```text
Ornith-1.5-9B-OBLITERATED Q4_K_M
```

a seemingly reasonable configuration used:

```text
--threads 9
--threads-batch 12
```

Decode performance was approximately:

```text
9 tok/s
```

Allowing llama.cpp to determine the CPU topology instead:

```text
--threads 0
--threads-batch 0
```

produced approximately:

```text
13 tok/s
```

Same model.

Same quantization.

Same runtime.

Same machine.

Roughly:

```text
44% faster decode
```

That is exactly the kind of knowledge ThumbLLM is intended to capture.

The user should not need to rediscover those settings.

---

# Hardware-Specific Builds

There will not necessarily be one universal ThumbLLM executable.

Different hardware benefits from different runtimes and optimization strategies.

ThumbLLM releases may target:

## CPU

```text
CPU
```

Designed for systems where the model runs entirely on the processor.

Possible targets include:

* AMD Ryzen
* AMD Threadripper
* AMD EPYC
* Intel Core
* Intel Core Ultra
* Intel Xeon

---

## NVIDIA CUDA

```text
CUDA
```

For NVIDIA GPUs using CUDA acceleration.

Examples might include:

* RTX 3060
* RTX 3090
* RTX 4090
* RTX 5090
* RTX PRO
* data-center NVIDIA GPUs

---

## AMD ROCm

```text
ROCm
```

For supported AMD GPUs and APUs.

Examples may include:

* Radeon GPUs
* Radeon PRO
* Ryzen AI Max / Strix Halo
* supported Instinct GPUs

---

## Vulkan

```text
Vulkan
```

Vulkan can provide a broadly available GPU backend across different hardware vendors and operating systems.

It can also be useful where CUDA or ROCm is unavailable or undesirable.

---

## Apple MLX

```text
MLX
```

Apple Silicon builds can target MLX and unified memory on:

* MacBook Air
* MacBook Pro
* Mac mini
* Mac Studio

---

# One Model May Have Several ThumbLLM Editions

A single model might eventually have multiple releases.

For example:

```text
ThumbLLM-Ornith-1.5-9B-OBLITERATED-Q4_K_M-CPU-Win-x64
ThumbLLM-Ornith-1.5-9B-OBLITERATED-Q4_K_M-Vulkan-Win-x64
ThumbLLM-Ornith-1.5-9B-OBLITERATED-Q4_K_M-ROCm-Win-x64
ThumbLLM-Ornith-1.5-9B-OBLITERATED-Q4_K_M-CUDA-Win-x64
ThumbLLM-Ornith-1.5-9B-OBLITERATED-Q4_K_M-MLX-Mac-arm64
```

They may contain the same model family but use completely different inference recipes.

That is intentional.

The executable tells you what it was built for.

---

# Use Case: Private Executive Assistant

A local model can process business material without sending it to a cloud LLM.

For example:

```text
Meeting Notes
Travel Plans
Customer Names
Action Items
Expenses
Deadlines
        |
        v
     ThumbLLM
        |
        v
Executive Brief
```

This can be useful when traveling, working from a hotel, or operating somewhere with limited network access.

---

# Use Case: Local Coding Assistant

Point a development tool at the local ThumbLLM API:

```text
IDE / Agent
    |
    v
localhost:8080/v1
    |
    v
ThumbLLM
```

Now code generation or code analysis can happen against a locally running model.

Potential benefits:

* source code stays local
* no per-token API charges
* predictable availability
* experimentation with open models
* custom model selection

---

# Use Case: Document Processing

ThumbLLM can serve as a local inference engine for repetitive document tasks.

Examples:

* summarization
* classification
* extraction
* normalization
* tagging
* rewriting
* structured JSON generation
* report generation

A folder watcher could send every incoming document to the local API without involving a cloud model.

---

# Use Case: Local RAG

A local retrieval system can use ThumbLLM as its generation endpoint.

```text
Local Documents
      |
      v
Vector Search
      |
      v
Retrieved Context
      |
      v
ThumbLLM API
      |
      v
Answer
```

The document store, retrieval pipeline, prompts, and model can all remain on the local machine.

---

# Use Case: Local Agents

Agents become especially interesting when the model endpoint itself is local.

```text
Local Agent
   |
   +--> filesystem
   |
   +--> scripts
   |
   +--> browser/tools
   |
   +--> ThumbLLM API
```

The model can plan, classify, decide, extract, generate, or route tasks while remaining part of a local software stack.

---

# Use Case: Multi-Model Agents

A more advanced agent can use several ThumbLLM instances simultaneously.

Example:

```text
Hermes Agent
     |
     v
Model Router
  /       \
 /         \
v           v
Qwen3.8-4B  Ornith-1.5-9B-OBLITERATED
Fast        More capable / different behavior
```

The agent might use Qwen3.8-4B for inexpensive high-frequency tasks while escalating selected requests to Ornith-1.5-9B-OBLITERATED.

This is similar to the routing architectures increasingly used by cloud AI systems, but implemented locally.

---

# Use Case: CPU-Only AI

A GPU should not be a prerequisite for useful local AI.

Smaller and midsize quantized models can run entirely on modern CPUs.

That means ThumbLLM can potentially turn ordinary systems into AI machines:

```text
Laptop
+
Ryzen CPU
+
16–32 GB RAM
+
ThumbLLM
=
Local LLM
```

CPU inference will generally be slower than a high-end GPU, but it greatly expands where local AI can run.

A model producing 10–15 tokens per second can already be completely usable for interactive tasks.

---

# Use Case: Repurposing Existing Computers

Local AI does not always require buying new hardware.

ThumbLLM builds may make it practical to reuse:

* older gaming PCs
* office desktops
* workstation laptops
* mini PCs
* home servers
* previous-generation GPUs
* spare Macs

A machine that would not make sense as a cloud AI server may still be extremely useful as a local model appliance.

---

# Use Case: Dedicated AI Stations

A ThumbLLM machine can also operate as a dedicated model server.

For example:

```text
Home Network

Laptop ────────┐
Desktop ───────┼────> ThumbLLM Server
Tablet ────────┘            |
                             v
                         Local Model
```

LAN API exposure should be enabled deliberately and protected appropriately, but the underlying architecture makes this possible.

One sufficiently powerful computer could provide AI inference to several local applications.

---

# Use Case: Model Experimentation

Researchers and enthusiasts often want to compare models without repeatedly reconfiguring runtimes.

ThumbLLM releases can make those comparisons easier.

For example:

```text
ThumbLLM Model A
ThumbLLM Model B
ThumbLLM Model C
```

Each can have:

* known quantization
* known runtime
* known flags
* known API behavior
* known hardware target

That reduces configuration drift between tests.

---

# Use Case: Reproducible Local AI

A major problem with local model benchmarks is that tiny configuration differences can produce dramatically different results.

Examples include:

* thread count
* GPU layers
* context size
* KV quantization
* Flash Attention
* speculative decoding
* runtime version
* backend
* tensor split
* CPU affinity

ThumbLLM creates an opportunity to distribute the **recipe itself**.

Instead of saying:

> Download this model and try llama.cpp.

A release can say:

> This is the exact configuration I tested.

That makes performance results easier to reproduce.

---

# Use Case: Frozen AI Environments

Sometimes software stability matters more than constantly updating.

A ThumbLLM release can intentionally freeze:

```text
Model
Quant
Runtime
Runtime version
Configuration
Application
```

That means a working configuration can continue to exist even as llama.cpp and model ecosystems move forward.

A future runtime improvement can become a new ThumbLLM release rather than silently changing an existing one.

This is useful for:

* demonstrations
* reproducible research
* production workflows
* offline deployments
* long-term archives
* benchmark comparisons

---

# Local AI Without Runtime Drift

Inference frameworks evolve quickly.

A model that works perfectly with one llama.cpp release may behave differently after several months of changes.

ThumbLLM treats the runtime as part of the recipe.

The model is not the only thing being preserved.

The **environment that makes the model useful** is preserved too.

---

# ThumbLLM Is Not a Model Manager

There are already excellent applications for browsing, downloading, loading, and experimenting with arbitrary models.

ThumbLLM is solving a different problem.

A model manager says:

> Here are thousands of models and hundreds of settings. Choose what you want.

ThumbLLM says:

> Here is one model that has already been configured for this machine.

Both approaches are useful.

They serve different purposes.

---

# The Appliance Model

A good analogy is the difference between:

```text
Building a Linux server from packages
```

and:

```text
Downloading a preconfigured virtual appliance
```

ThumbLLM applies something similar to local AI.

The model, runtime, configuration, application, and interface become one logical unit.

---

# Portability

The long-term idea is not merely:

> Run a model locally.

It is:

> Carry your AI environment with you.

A portable drive could contain several ThumbLLM builds:

```text
AI-DRIVE/
│
├── Coding/
│   └── ThumbLLM-Coder...
│
├── General/
│   └── ThumbLLM-Ornith...
│
├── Fast/
│   └── ThumbLLM-Qwen3.8-4B...
│
└── Documents/
    └── ThumbLLM-Extractor...
```

Plug it into an appropriate machine and the models are ready to become usable applications.

---

# No Cloud Token Meter

Local inference changes the economics of model usage.

Once the hardware and model are available, generating another thousand tokens does not create another API invoice.

The cost becomes primarily:

* hardware
* electricity
* storage
* time

That makes some workloads particularly attractive locally:

* repeated document processing
* experimentation
* long chats
* development
* synthetic data generation
* batch extraction
* agent loops
* private workflows

---

# Open Models as Software Components

Perhaps the most important idea behind ThumbLLM is this:

Open-weight models are increasingly becoming **software components**.

Not just chatbots.

A local model can become:

```text
a parser
a classifier
a router
a reasoning engine
a code generator
a summarizer
a document processor
an agent brain
a structured-data generator
a natural-language interface
```

The API makes those capabilities available to other software.

ThumbLLM packages them into something reproducible and portable.

---

# Current and Planned Backends

ThumbLLM builds may target:

```text
CPU
CUDA
ROCm
Vulkan
MLX
```

Across:

```text
Windows
Linux
macOS
```

And architectures including:

```text
x64
arm64
```

Not every model will necessarily receive every build.

Releases will depend on:

* hardware availability
* runtime support
* measured performance
* model compatibility
* community interest

---

# Release Naming

ThumbLLM uses descriptive filenames so that a release identifies its intended environment.

Standard format:

```text
ThumbLLM-[Model]-[Quant]-[Runtime]-[OS]-[Arch]-v[Version]
```

Example:

```text
ThumbLLM-Ornith-1.5-9B-OBLITERATED-Q4_K_M-CPU-Win-x64-v1.6.1.exe
```

A user should be able to look at the filename and understand what it is meant to run.

---

# Example ThumbLLM Recipe

A CPU build might use a recipe such as:

```text
Model:
Ornith-1.5-9B-OBLITERATED

Quant:
Q4_K_M

Runtime:
llama.cpp

Backend:
CPU

GPU Layers:
0

Device:
none

Threads:
auto

Batch Threads:
auto

Flash Attention:
on

KV Cache:
Q4_0 / Q4_0

Batch:
2048

uBatch:
512

Parallel:
1

Reasoning:
off
```

The exact recipe varies by build.

The important part is that the user does not have to rediscover it.

---

# Philosophy

ThumbLLM is built around a few simple ideas.

## Local first

If the hardware can perform the inference locally, local execution should be a first-class option.

## Configuration matters

The model file alone does not determine performance.

Runtime and settings matter enormously.

## Reproducibility matters

A working recipe should be captured, documented, and preserved.

## Portability matters

Local AI should be able to move with the user.

## APIs matter

A model becomes much more useful when software can talk to it.

## Hardware diversity matters

Local AI should not belong exclusively to one GPU vendor or one operating system.

---

# What ThumbLLM Is Trying to Become

The long-term vision is a library of **small, self-contained local AI appliances**.

Each release answers six questions immediately:

```text
What model is this?

What quantization is it?

What runtime does it use?

What hardware does it target?

What operating system does it run on?

What tested inference recipe does it contain?
```

Instead of beginning with configuration, the user begins with a working model.

---

# The Bigger Idea

The future of local AI may not look like one enormous model permanently running on every computer.

It may look more like this:

```text
                     LOCAL AI SYSTEM

              ┌──────────────────────┐
              │     Agent / App      │
              └──────────┬───────────┘
                         │
                    Local Router
          ┌──────────────┼──────────────┐
          │              │              │
          v              v              v
      Fast Model     Smart Model    Specialist
          │              │              │
          v              v              v
      ThumbLLM       ThumbLLM        ThumbLLM
          │              │              │
          └──────────────┼──────────────┘
                         │
                    Your Hardware
```

Different models.

Different sizes.

Different strengths.

Different runtimes.

One local machine.

No requirement that every request travel to the cloud.

ThumbLLM is an attempt to make those models easy to package, preserve, distribute, and use.

---

# In One Sentence

> **ThumbLLM turns an open-weight language model into a portable, preconfigured local AI application with both a built-in chat interface and an OpenAI-compatible API.**

---

# In Even Fewer Words

**Local AI, packaged like software.**
# ThumbLLM
Portable, preconfigured local AI with built-in chat and an OpenAI-compatible API.
