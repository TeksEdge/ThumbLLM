<p align="center">
  <img src="assets/logo/thumbllm-logo-transparent.png" alt="ThumbLLM logo" width="220">
</p>

<p align="center">
  <strong>Local AI packaged like an app. Download, double-click, chat.</strong>
</p>

<p align="center">
  One model. One quant. One runtime. One optimized recipe. Simple.
</p>

<p align="center">
  <img src="assets/screenshots/thumbllm-chat-window.png" alt="ThumbLLM desktop chat interface" width="600">
</p>

<p align="center">
  <a href="https://github.com/TeksEdge/ThumbLLM/releases">
    <strong>⬇️ View ThumbLLM Releases for Windows</strong>
  </a>
</p>

# ThumbLLM

**ThumbLLM turns an open-weight language model into a portable, preconfigured local AI application.**

Instead of installing and configuring llama.cpp yourself, ThumbLLM packages a **known-good model, quantization, runtime, and tested inference recipe** into an application you can simply launch.

```text
Model
+ Quantization
+ llama.cpp
+ Tested settings
+ Desktop chat
+ Local API
= ThumbLLM
```

> **Download it. Double-click it. Run the model locally.**

If the required GGUF model is not already present, ThumbLLM can download and verify it automatically. Once downloaded, inference runs locally on your computer without requiring a cloud AI service.

---

## Releases

| Edition | Model | Quantization | Backend | Runtime | Platform | Download | Recipe |
|---|---|---|---|---|---|---|---|
| ThumbLLM 0.1.0 — Qwen3.5 4B CPU Edition | unsloth/Qwen3.5-4B-MTP-GGUF | Q4_K_M | CPU | llama.cpp b10603 | Windows x64 | [Download](https://github.com/TeksEdge/ThumbLLM/releases/tag/Qwen3.5-4B-CPU-v0.1.0) | [View Recipe](recipes/Qwen3.5-4-MTP-Q4_K_M-CPU-Win-x64/README.md) |
| ThumbLLM 0.1.0 — Ornith 1.5 9B CPU Edition | OBLITERATUS/Ornith-1.5-9B-OBLITERATED | Q4_K_M | CPU | llama.cpp b10603 | Windows x64 | [Download](https://github.com/TeksEdge/ThumbLLM/releases/tag/Release-1-Ornith-1.5-9B-CPU) | [View Recipe](recipes/Ornith-1.5-9B-OBLITERATED-Q4_K_M-CPU-Win-x64/README.md) |

>   ⚠️  **Windows SmartScreen:** ThumbLLM releases are currently unsigned, so Windows may display **"Windows protected your PC"** and **"Unknown publisher."** Download ThumbLLM only from this official GitHub repository and verify the supplied SHA-256 checksum before running the application.

---

# One Local Model. Two Interfaces.

## 💬 Desktop Chat

Double-click ThumbLLM and interact with the model through its built-in desktop interface.

Features include:

* streaming responses
* generation statistics
* tokens-per-second measurements
* reasoning/thinking display when supported
* context management
* runtime information
* automatic model detection
* automatic and resumable model download
* model verification

For someone who simply wants to use the model, this may be all they need.

---

## 🔌 OpenAI-Compatible Local API

The same model is also available through a local OpenAI-compatible API.

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
         LLM
```

That means local applications, agents, coding tools, scripts, RAG systems, and other software can use ThumbLLM as an inference backend.

**The chat makes the model useful to a person.**

**The API makes the model useful to software.**

---

# Why ThumbLLM?

ThumbLLM takes a simple approach:

**Test one configuration, find what works, and package the recipe with the application.**

The user shouldn't have to rediscover the settings.

ThumbLLM is therefore **not intended to be another general-purpose model manager**.

A model manager gives you models and configuration options.

**ThumbLLM gives you a configuration that has already been tested.**

Each ThumbLLM edition is built around:

* one model
* one quantization
* one runtime
* one target platform
* one tested inference configuration

The goal is to make running a local model feel more like launching an application and less like assembling an inference stack.

---

# Local by Design

### 🔒 Private ➜ Prompts, source code, documents, notes, and other inputs do not need to be sent to a remote inference provider.

### 📴 Offline ➜ Once the model is downloaded, inference can run without a cloud AI connection.

### 💰 No Token Meter ➜ Local inference does not create a per-token cloud API charge.

### 🎛️ Controlled ➜ The model, quantization, runtime, and inference configuration are known and preserved.

---

# What Can You Do With It?

ThumbLLM editions can be used for:

* private local chat
* building web pages and documents
* writing and brainstorming
* document summarization
* information extraction
* coding assistance
* structured data generation
* local agents and scripts
* experimentation with local AI
* applications that support OpenAI-compatible APIs

Because the model exposes an API, ThumbLLM can function as more than a chatbot.

It can become a local software component.

---

# Recipes

Every ThumbLLM edition has a corresponding recipe documenting the model and inference configuration used to build it.

Recipes are stored in the [`recipes`](recipes/) directory.

The recipe records details such as:

* model and GGUF file
* quantization
* llama.cpp build
* platform and architecture
* inference backend
* context size
* thread configuration
* batch configuration
* KV cache configuration
* tested hardware
* observed performance

This makes each ThumbLLM edition reproducible rather than mysterious.

---

# In One Sentence

> **ThumbLLM turns an open-weight language model into a portable, preconfigured local AI application with built-in chat and an OpenAI-compatible API.**

### **Local AI, packaged like software.**
