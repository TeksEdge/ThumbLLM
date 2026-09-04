# ThumbLLM API

ThumbLLM exposes a local **OpenAI-compatible HTTP API** that allows scripts, applications, benchmark tools, and other clients to communicate with the language model running inside ThumbLLM.

The API runs locally on the user's computer and is available whenever ThumbLLM is running and the model has successfully loaded.

---

## Default API Address

```text
http://127.0.0.1:8080
```

OpenAI-compatible base URL:

```text
http://127.0.0.1:8080/v1
```

Default configuration:

| Setting                    | Default           |
| -------------------------- | ----------------- |
| Host                       | `127.0.0.1`       |
| Port                       | `8080`            |
| LAN access                 | Disabled          |
| API key required           | No                |
| API format                 | OpenAI-compatible |
| Default inference location | Local machine     |

Because ThumbLLM binds to `127.0.0.1` by default, the API is accessible only from the same computer running ThumbLLM.

---

# Supported ThumbLLM API

ThumbLLM officially relies on the following API endpoints:

| Method | Endpoint               | Purpose                                     |
| ------ | ---------------------- | ------------------------------------------- |
| `GET`  | `/health`              | Check whether the inference server is ready |
| `GET`  | `/v1/models`           | Discover the currently loaded model         |
| `POST` | `/v1/chat/completions` | Generate chat completions                   |

The bundled llama.cpp server may expose additional endpoints, but applications should not depend on those endpoints being part of the stable ThumbLLM API.

See [Additional llama.cpp Endpoints](#additional-llamacpp-endpoints).

---

# Quick API Test

Start ThumbLLM and wait until the model reports that it is loaded and ready.

Then open this address in a web browser:

```text
http://127.0.0.1:8080/v1/models
```

A successful response will contain information about the currently loaded model.

For example:

```json
{
  "object": "list",
  "data": [
    {
      "id": "Qwen3.5-4B-Q4_K_M.gguf",
      "object": "model",
      "owned_by": "llamacpp"
    }
  ]
}
```

The exact model ID may differ between ThumbLLM editions.

Some native llama.cpp builds may return the full path to the GGUF file as the model ID.

For this reason, API clients should **discover the model ID dynamically using `/v1/models` rather than hard-coding it**.

---

# Health Check

## Request

```http
GET /health
```

Example:

```text
http://127.0.0.1:8080/health
```

A healthy server normally returns an HTTP `200` response.

Depending on the bundled llama.cpp version, the exact JSON response may vary.

---

# List Models

## Request

```http
GET /v1/models
```

Example:

```text
http://127.0.0.1:8080/v1/models
```

Example response:

```json
{
  "object": "list",
  "data": [
    {
      "id": "Qwen3.5-4B-Q4_K_M.gguf",
      "object": "model",
      "owned_by": "llamacpp"
    }
  ]
}
```

Use the value returned in:

```text
data[0].id
```

as the model identifier for subsequent requests.

---

# Chat Completions

The primary ThumbLLM inference endpoint is:

```http
POST /v1/chat/completions
```

Full URL:

```text
http://127.0.0.1:8080/v1/chat/completions
```

## Basic Request

```json
{
  "model": "MODEL_ID",
  "messages": [
    {
      "role": "user",
      "content": "What is the capital of France?"
    }
  ],
  "temperature": 0.7,
  "max_tokens": 256,
  "stream": false
}
```

Example response:

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "The capital of France is Paris."
      }
    }
  ],
  "object": "chat.completion",
  "model": "MODEL_ID",
  "usage": {
    "prompt_tokens": 17,
    "completion_tokens": 12,
    "total_tokens": 29
  }
}
```

---

# Messages

ThumbLLM uses the standard chat-message structure:

```json
{
  "role": "user",
  "content": "Hello!"
}
```

Common roles are:

```text
system
user
assistant
```

Example conversation:

```json
{
  "model": "MODEL_ID",
  "messages": [
    {
      "role": "system",
      "content": "You are a helpful assistant."
    },
    {
      "role": "user",
      "content": "Who discovered penicillin?"
    },
    {
      "role": "assistant",
      "content": "Alexander Fleming."
    },
    {
      "role": "user",
      "content": "What year did he discover it?"
    }
  ],
  "max_tokens": 256,
  "stream": false
}
```

The client is responsible for sending previous conversation messages when conversation history is required.

---

# Common Request Parameters

ThumbLLM's native server uses llama.cpp's OpenAI-compatible API.

Common parameters include:

| Parameter     | Type    | Purpose                            |
| ------------- | ------- | ---------------------------------- |
| `model`       | string  | Model ID returned by `/v1/models`  |
| `messages`    | array   | Conversation messages              |
| `temperature` | number  | Sampling temperature               |
| `top_p`       | number  | Nucleus sampling probability       |
| `max_tokens`  | integer | Maximum number of generated tokens |
| `stream`      | boolean | Enable streaming responses         |
| `seed`        | integer | Sampling seed when supported       |

Example:

```json
{
  "model": "MODEL_ID",
  "messages": [
    {
      "role": "user",
      "content": "Explain quantum entanglement in one paragraph."
    }
  ],
  "temperature": 0.7,
  "top_p": 0.9,
  "max_tokens": 512,
  "stream": false
}
```

Additional llama.cpp request parameters may be supported depending on the runtime version bundled with a particular ThumbLLM edition.

---

# Thinking and Reasoning Models

Some models supported by ThumbLLM can produce a separate reasoning stream.

A response may therefore contain both:

```json
{
  "content": "The capital of France is Paris.",
  "reasoning_content": "..."
}
```

`content` is the model's final answer.

`reasoning_content` contains reasoning generated by models and templates that support separate thinking output.

Applications that only need the final answer should read:

```text
choices[0].message.content
```

and should not treat `reasoning_content` as the final response.

## Disable Thinking

For compatible llama.cpp models and chat templates, a client can request non-thinking behavior with:

```json
{
  "reasoning_effort": "none",
  "chat_template_kwargs": {
    "enable_thinking": false
  }
}
```

Example:

```json
{
  "model": "MODEL_ID",
  "messages": [
    {
      "role": "user",
      "content": "What is the capital of France?"
    }
  ],
  "temperature": 0,
  "max_tokens": 128,
  "stream": false,
  "reasoning_effort": "none",
  "chat_template_kwargs": {
    "enable_thinking": false
  }
}
```

Thinking controls depend on the model, chat template, and bundled llama.cpp version.

Clients should therefore not assume that every ThumbLLM edition supports identical reasoning behavior.

---

# Important: `max_tokens` and Reasoning

For reasoning-capable models, reasoning tokens may count toward the requested output-token limit.

For example, a request using:

```json
{
  "max_tokens": 100
}
```

could consume all 100 tokens in reasoning before producing a final `content` response.

The API may then return:

```json
{
  "finish_reason": "length",
  "message": {
    "content": "",
    "reasoning_content": "..."
  }
}
```

If this occurs:

* increase `max_tokens`, or
* explicitly disable thinking when reasoning is not required.

---

# Streaming Responses

Set:

```json
{
  "stream": true
}
```

to receive tokens as they are generated.

Example request:

```json
{
  "model": "MODEL_ID",
  "messages": [
    {
      "role": "user",
      "content": "Write a short paragraph about local AI."
    }
  ],
  "max_tokens": 256,
  "stream": true
}
```

Streaming responses use Server-Sent Events.

Individual events are returned in the form:

```text
data: {"choices":[{"delta":{"content":"Local"}}]}
```

followed by additional chunks:

```text
data: {"choices":[{"delta":{"content":" AI"}}]}
```

The stream ends with:

```text
data: [DONE]
```

---

# PowerShell Examples

PowerShell's native `Invoke-RestMethod` is recommended because it avoids command-line JSON quoting problems.

## Discover the Model

```powershell
$models = Invoke-RestMethod `
    -Method GET `
    -Uri "http://127.0.0.1:8080/v1/models"

$model = $models.data[0].id

$model
```

## Send a Chat Request

```powershell
$models = Invoke-RestMethod `
    -Method GET `
    -Uri "http://127.0.0.1:8080/v1/models"

$model = $models.data[0].id

$body = @{
    model = $model
    messages = @(
        @{
            role = "user"
            content = "What is the capital of France?"
        }
    )
    temperature = 0
    max_tokens = 512
    stream = $false
} | ConvertTo-Json -Depth 10 -Compress

$response = Invoke-RestMethod `
    -Method POST `
    -Uri "http://127.0.0.1:8080/v1/chat/completions" `
    -ContentType "application/json" `
    -Body $body

$response.choices[0].message.content
```

Expected result:

```text
The capital of France is Paris.
```

## Show the Complete Response

```powershell
$response | ConvertTo-Json -Depth 10
```

---

# Windows Command Prompt Example

The quoting rules for Windows Command Prompt differ from PowerShell.

First obtain the model ID from:

```text
http://127.0.0.1:8080/v1/models
```

Then use:

```bat
curl.exe -X POST http://127.0.0.1:8080/v1/chat/completions -H "Content-Type: application/json" -d "{\"model\":\"MODEL_ID\",\"messages\":[{\"role\":\"user\",\"content\":\"What is the capital of France?\"}],\"temperature\":0,\"max_tokens\":512,\"stream\":false}"
```

Replace:

```text
MODEL_ID
```

with the exact ID returned by `/v1/models`.

### PowerShell Warning

Do not copy the Windows Command Prompt `curl.exe` example unchanged into PowerShell.

PowerShell processes quoting differently and can alter the JSON before it reaches curl.

For PowerShell, use the `Invoke-RestMethod` examples above.

---

# Python Example

The following example uses only Python's standard library and does not require additional packages.

```python
import json
import urllib.request

API_BASE = "http://127.0.0.1:8080"

# Discover model
with urllib.request.urlopen(
    API_BASE + "/v1/models"
) as response:
    models = json.loads(response.read().decode("utf-8"))

model_id = models["data"][0]["id"]

# Create request
payload = {
    "model": model_id,
    "messages": [
        {
            "role": "user",
            "content": "What is the capital of France?",
        }
    ],
    "temperature": 0.0,
    "max_tokens": 512,
    "stream": False,
}

data = json.dumps(payload).encode("utf-8")

request = urllib.request.Request(
    API_BASE + "/v1/chat/completions",
    data=data,
    headers={
        "Content-Type": "application/json",
        "Accept": "application/json",
    },
    method="POST",
)

with urllib.request.urlopen(
    request,
    timeout=300,
) as response:
    result = json.loads(
        response.read().decode("utf-8")
    )

answer = result["choices"][0]["message"]["content"]

print(answer)
```

---

# Python Example with Thinking Disabled

```python
import json
import urllib.request

API_BASE = "http://127.0.0.1:8080"

with urllib.request.urlopen(
    API_BASE + "/v1/models"
) as response:
    models = json.loads(response.read().decode("utf-8"))

model_id = models["data"][0]["id"]

payload = {
    "model": model_id,
    "messages": [
        {
            "role": "user",
            "content": "What is the capital of France?",
        }
    ],
    "temperature": 0.0,
    "max_tokens": 512,
    "stream": False,
    "seed": 42,
    "reasoning_effort": "none",
    "chat_template_kwargs": {
        "enable_thinking": False,
    },
}

data = json.dumps(payload).encode("utf-8")

request = urllib.request.Request(
    API_BASE + "/v1/chat/completions",
    data=data,
    headers={
        "Content-Type": "application/json",
        "Accept": "application/json",
    },
    method="POST",
)

with urllib.request.urlopen(
    request,
    timeout=300,
) as response:
    result = json.loads(
        response.read().decode("utf-8")
    )

message = result["choices"][0]["message"]

print(message.get("content", ""))
```

---

# JavaScript Example

```javascript
async function askThumbLLM(question) {
    const baseUrl = "http://127.0.0.1:8080";

    const modelsResponse = await fetch(
        `${baseUrl}/v1/models`
    );

    const models = await modelsResponse.json();
    const model = models.data[0].id;

    const response = await fetch(
        `${baseUrl}/v1/chat/completions`,
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                model,
                messages: [
                    {
                        role: "user",
                        content: question
                    }
                ],
                temperature: 0.7,
                max_tokens: 512,
                stream: false
            })
        }
    );

    const result = await response.json();

    return result.choices[0].message.content;
}

askThumbLLM("What is the capital of France?")
    .then(console.log);
```

Browser security rules may restrict requests from some local HTML pages. A local application, development server, Node.js program, Python script, or other HTTP client can avoid those browser-specific restrictions.

---

# Using ThumbLLM with OpenAI-Compatible Clients

Applications that allow a custom OpenAI-compatible server can generally use:

```text
Base URL:
http://127.0.0.1:8080/v1
```

The model should be obtained from:

```text
GET /v1/models
```

The current default ThumbLLM configuration does not require an API key.

Some third-party clients require a value in the API-key field even when the server does not validate it. In that situation, a placeholder may be accepted by the client.

Whether a particular third-party client works depends on the degree of OpenAI API compatibility it expects.

---

# Authentication

The current default ThumbLLM configuration does not require authentication because the API is bound to localhost.

Default:

```text
API key required: No
LAN access:       Disabled
Host:             127.0.0.1
```

Future ThumbLLM editions may enable API-key authentication.

When API-key authentication is enabled, requests use:

```http
Authorization: Bearer YOUR_API_KEY
```

Example:

```text
Authorization: Bearer YOUR_API_KEY
```

Never expose a ThumbLLM API to an untrusted network without appropriate authentication and network controls.

---

# Local-Only Security Model

By default, ThumbLLM listens on:

```text
127.0.0.1
```

This means another computer on the network cannot directly connect to the API.

This is intentional.

ThumbLLM is designed primarily as a local inference application.

The model, prompts, and generated responses can remain on the user's computer when using the local API.

Internet access may still be required for initial model download or any external services deliberately used by the client application.

---

# API and GUI Usage

The ThumbLLM desktop interface and the API use the same underlying loaded model.

The current CPU-focused configuration is optimized primarily for a single active generation workload.

Running multiple large inference requests simultaneously may:

* reduce performance,
* cause requests to wait,
* increase memory usage, or
* increase response latency.

For benchmarking, send requests sequentially unless the specific ThumbLLM edition has been configured and tested for parallel inference.

---

# Response Usage Information

Responses may include token usage statistics:

```json
{
  "usage": {
    "prompt_tokens": 17,
    "completion_tokens": 182,
    "total_tokens": 199
  }
}
```

Native llama.cpp responses may also include timing information.

For example:

```json
{
  "timings": {
    "prompt_n": 17,
    "predicted_n": 182,
    "prompt_per_second": 49.34,
    "predicted_per_second": 12.36
  }
}
```

The exact timing fields may vary with the bundled llama.cpp version.

Applications should not require these non-core timing fields to exist.

---

# Finish Reasons

A chat completion may contain:

```json
{
  "finish_reason": "stop"
}
```

Common values include:

```text
stop
length
```

`stop` normally means the model completed naturally.

`length` normally means the request reached the configured output-token limit before the model finished.

For reasoning-capable models, a `length` result with empty `content` may indicate that the output-token budget was consumed by reasoning.

---

# Error Handling

Clients should be prepared for standard HTTP errors such as:

| Status | Meaning                                          |
| ------ | ------------------------------------------------ |
| `400`  | Invalid request                                  |
| `401`  | Authentication failure when API keys are enabled |
| `404`  | Endpoint not found                               |
| `500`  | Server or inference error                        |

Example error response:

```json
{
  "error": {
    "message": "Invalid request",
    "type": "server_error"
  }
}
```

The exact error structure may vary between native llama.cpp versions and ThumbLLM's Python fallback mode.

---

# Additional llama.cpp Endpoints

When ThumbLLM is operating through its native bundled `llama-server`, llama.cpp may expose additional endpoints such as:

```text
/completion
/tokenize
/apply-template
```

For example, llama.cpp's raw completion endpoint may accept:

```json
{
  "prompt": "What is the capital of France?",
  "n_predict": 64,
  "temperature": 0
}
```

These endpoints can be useful for development and diagnostics.

However:

> **Additional llama.cpp endpoints are runtime implementation details and are not guaranteed to remain part of ThumbLLM's stable public API.**

Applications intended to work across ThumbLLM editions should prefer:

```text
/health
/v1/models
/v1/chat/completions
```

---

# Runtime Compatibility

ThumbLLM packages a particular llama.cpp runtime with each edition.

As llama.cpp evolves, optional request parameters, response fields, reasoning controls, timing information, or additional endpoints may change.

The following are considered the primary ThumbLLM compatibility surface:

```text
GET  /health
GET  /v1/models
POST /v1/chat/completions
```

Code using additional llama.cpp-specific features should account for runtime-version differences.

---

# Troubleshooting

## `/v1/models` Does Not Load

Check that:

1. ThumbLLM is running.
2. The model finished loading.
3. Port `8080` is not being used by another application.
4. You are connecting to:

```text
http://127.0.0.1:8080/v1/models
```

---

## Another llama.cpp Application Is Running

Applications such as LM Studio may also run `llama-server.exe`.

That is normally fine as long as each server uses a different TCP port.

To determine what process owns port `8080` on Windows:

```bat
netstat -ano | findstr ":8080"
```

Then inspect the PID:

```bat
tasklist /FI "PID eq YOUR_PID"
```

---

## Empty `content`

If the response contains:

```json
{
  "content": "",
  "reasoning_content": "..."
}
```

and:

```json
{
  "finish_reason": "length"
}
```

increase `max_tokens` or disable thinking.

---

## PowerShell JSON Errors

If curl produces errors such as:

```text
parse error while parsing object key
```

or:

```text
Could not resolve host
```

the problem may be shell quoting rather than ThumbLLM.

Use PowerShell's native:

```text
Invoke-RestMethod
```

with:

```text
ConvertTo-Json
```

as shown earlier in this document.

---

## Test Whether Inference Works

In PowerShell:

```powershell
$body = @{
    prompt = "What is the capital of France? Answer only with the city."
    n_predict = 64
    temperature = 0
} | ConvertTo-Json -Compress

Invoke-RestMethod `
    -Method POST `
    -Uri "http://127.0.0.1:8080/completion" `
    -ContentType "application/json" `
    -Body $body
```

This raw llama.cpp endpoint can be useful for diagnostics when the native server is active.

For application development, prefer `/v1/chat/completions`.

---

# Benchmarking

ThumbLLM's API can be used by external benchmark programs.

A benchmark can:

1. start ThumbLLM,
2. discover the loaded model through `/v1/models`,
3. submit questions through `/v1/chat/completions`,
4. capture responses,
5. record latency and token usage,
6. grade the saved responses separately.

For deterministic factuality-style testing, a request may use:

```json
{
  "temperature": 0,
  "seed": 42,
  "stream": false,
  "reasoning_effort": "none",
  "chat_template_kwargs": {
    "enable_thinking": false
  }
}
```

Benchmark methodology should always document the exact model, quantization, ThumbLLM edition, llama.cpp runtime, sampling settings, context size, and judging methodology.

---

# Minimal Client Flow

A robust ThumbLLM client should follow this sequence:

```text
Start ThumbLLM
      │
      ▼
GET /health
      │
      ▼
GET /v1/models
      │
      ▼
Read data[0].id
      │
      ▼
POST /v1/chat/completions
      │
      ▼
Read choices[0].message.content
```

Do not assume the model ID from the ThumbLLM executable filename.

Discover it through the API.

---

# Stable API Summary

## Server

```text
http://127.0.0.1:8080
```

## OpenAI-Compatible Base URL

```text
http://127.0.0.1:8080/v1
```

## Health

```text
GET /health
```

## Models

```text
GET /v1/models
```

## Chat

```text
POST /v1/chat/completions
```

## Final Answer

Read:

```text
choices[0].message.content
```

## Optional Reasoning

May appear in:

```text
choices[0].message.reasoning_content
```

---

# Project

**ThumbLLM** is a TeksEdge project focused on making optimized local language-model inference simple to run on personal computers.

Each ThumbLLM edition packages a selected model profile and tested llama.cpp inference configuration into a standalone application.

The API allows developers to use that same locally running model from their own scripts and applications without requiring a separate inference server.

