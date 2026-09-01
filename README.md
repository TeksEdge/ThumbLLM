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
  <img src="assets/screenshots/thumbllm-chat-window.png" alt="ThumbLLM desktop chat interface" width="900">
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

## Current Release

The first ThumbLLM release is built specifically for CPU inference on Windows x64.

```text
Model:        Ornith-1.5-9B-OBLITERATED
Quantization: Q4_K_M
Runtime:      llama.cpp
Backend:      CPU
Platform:     Windows x64
```

This is currently the **only ThumbLLM executable**.

It is intended as the first proof of the ThumbLLM idea: take a specific local model configuration, tune it, package it, and make it easy for someone else to run.

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
* automatic model download
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

Running a local LLM is easier than it used to be.

Running one **well** can still involve figuring out:

```text
Which quantization?
Which llama.cpp build?
How many CPU threads?
What context size?
What batch size?
Which runtime flags?
What actually performs best on this hardware?
```

ThumbLLM takes an opinionated approach:

> **Test one configuration, find what works, and package the recipe with the application.**

The user shouldn't have to rediscover the settings.

ThumbLLM is therefore **not intended to be another general-purpose model manager**.

A model manager gives you models and configuration options.

ThumbLLM gives you a configuration that has already been tested.

---

# Known-Good Recipes

Configuration matters.

During testing of the current **Ornith-1.5-9B-OBLITERATED Q4_K_M** CPU build, this configuration:

```text
--threads 9
--threads-batch 12
```

produced roughly:

```text
9 tok/s
```

Allowing llama.cpp to determine the CPU topology automatically:

```text
--threads 0
--threads-batch 0
```

produced roughly:

```text
13 tok/s
```

Same model.

Same quantization.

Same computer.

Approximately **44% faster decode**.

That is the kind of optimization ThumbLLM is designed to capture.

The working configuration becomes part of the application instead of something every user has to discover independently.

---

# Local by Design

### 🔒 Private

Prompts, source code, documents, notes, and other inputs do not need to be sent to a remote inference provider.

### 📴 Offline

Once the model is downloaded, inference can run without a cloud AI connection.

### 💰 No Token Meter

Local inference does not create a per-token cloud API charge.

### 🎛️ Controlled

The model, quantization, runtime, and inference configuration are known and preserved.

---

# Portable AI

The **Thumb** in ThumbLLM is intentional.

The application and model can be stored together on removable storage:

```text
USB Drive
│
├── ThumbLLM.exe
└── model.gguf
```

Plug the drive into a compatible Windows x64 computer, launch ThumbLLM, and run the model locally.

The idea is to make the **working AI environment portable**, not merely the model weights.

---

# What Can You Do With It?

The current ThumbLLM release can be used for:

* private local chat
* writing and brainstorming
* document summarization
* information extraction
* coding assistance
* structured data generation
* local agents and scripts
* experimentation with local AI
* applications that support OpenAI-compatible APIs

Because the model exposes an API, it can function as more than a chatbot.

It can become a local software component.

---

# The Idea Behind ThumbLLM

Open-weight models are increasingly useful as software components, but running them still often feels like assembling an inference stack.

ThumbLLM asks a simpler question:

> **What if a well-tuned local model were distributed like a normal application?**

That means preserving:

```text
The model
The quantization
The runtime
The runtime version
The configuration
The interface
```

as one known working package.

For now, ThumbLLM begins with **one executable and one tested CPU configuration**.

That's intentional.

Start with something that works.

Measure it.

Package it.

Make it easy to run.

---

# In One Sentence

> **ThumbLLM turns an open-weight language model into a portable, preconfigured local AI application with built-in chat and an OpenAI-compatible API.**

### **Local AI, packaged like software.**
