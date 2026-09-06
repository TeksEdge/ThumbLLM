# ThumbLLM API

ThumbLLM exposes a local **OpenAI-compatible chat-completions API** that allows scripts, applications, benchmark tools, development tools, and other clients to communicate with the language model running inside ThumbLLM.

The API is available after ThumbLLM has successfully loaded its model and started its inference engine.

ThumbLLM is designed primarily for local inference. By default:

* the API listens only on the local computer,
* no API key is required,
* the preferred starting port is `8080`,
* and all model inference occurs locally.

> **Important:** ThumbLLM implements the OpenAI-compatible API surface needed for local chat inference. It does not claim compatibility with every endpoint in the OpenAI API.

---

## Quick Start

1. Start ThumbLLM.
2. Wait until the application reports that the model is loaded and ready.
3. Look at the API information displayed inside ThumbLLM.
4. Note the active port.
5. Call `/v1/models` to discover the loaded model.
6. Send prompts through `/v1/chat/completions`.

ThumbLLM displays an API line similar to:

```text
API: http://127.0.0.1:8080/v1  │  Port: 8080
```

Use the **actual port displayed by your running ThumbLLM instance**.

---

# API Address and Port Selection

## Preferred Address

ThumbLLM's preferred local API address is:

```text
http://127.0.0.1:8080
```

The preferred OpenAI-compatible base URL is:

```text
http://127.0.0.1:8080/v1
```

However, **port 8080 is not guaranteed**.

ThumbLLM checks whether its preferred port is available when it starts.

If port `8080` is already in use, ThumbLLM automatically searches the following ports until it finds an available one.

For example:

```text
8080
8081
8082
8083
...
```

The current configuration can search up to 100 consecutive ports beginning with the preferred port.

Therefore, a ThumbLLM instance might actually use:

```text
http://127.0.0.1:8081/v1
```

or:

```text
http://127.0.0.1:8082/v1
```

instead of port `8080`.

Always use the API address displayed by the running ThumbLLM application.

---

## Default Configuration

| Setting                            | Default                            |
| ---------------------------------- | ---------------------------------- |
| Host                               | `127.0.0.1`                        |
| Preferred starting port            | `8080`                             |
| Automatic alternate-port selection | Yes                                |
| LAN access                         | Disabled                           |
| API key required                   | No                                 |
| API style                          | OpenAI-compatible chat completions |
| Inference location                 | Local machine                      |

Because ThumbLLM binds to `127.0.0.1` by default, another computer on the network cannot directly access the API.

---

# Stable ThumbLLM API

Applications intended to work across ThumbLLM editions should rely on these endpoints:

| Method | Endpoint               | Purpose                                     |
| ------ | ---------------------- | ------------------------------------------- |
| `GET`  | `/health`              | Check whether the inference server is ready |
| `GET`  | `/v1/models`           | Discover the currently loaded model         |
| `POST` | `/v1/chat/completions` | Generate chat completions                   |

These three endpoints are the primary ThumbLLM API compatibility surface.

The native llama.cpp server may expose additional endpoints, but those should be treated as runtime-specific implementation details.

---

# ThumbLLM Runtime Modes

ThumbLLM can expose its API through two inference paths.

## Native llama-server

This is the preferred path when the bundled `llama-server` executable is available.

The architecture is:

```text
Your Application
      │
      ▼
ThumbLLM API Port
      │
      ▼
llama-server
      │
      ▼
Loaded GGUF Model
```

In this mode, external API requests are sent directly to the bundled llama.cpp server.

This means llama.cpp controls such as:

```text
chat_template_kwargs
reasoning_effort
stream
temperature
top_p
max_tokens
```

may be sent directly to the native runtime when supported.

---

## llama-cpp-python Fallback

If the native server cannot be used and the ThumbLLM edition allows fallback, ThumbLLM can expose an embedded HTTP API backed by `llama-cpp-python`.

The stable endpoints remain:

```text
GET  /health
GET  /v1/models
POST /v1/chat/completions
```

Some optional behavior differs from the native server.

In particular, reasoning controls may be normalized according to ThumbLLM's configured reasoning state.

Applications should therefore rely on the stable API surface rather than assuming every optional llama.cpp parameter behaves identically in every ThumbLLM runtime mode.

---

# Determine the Active API Address

Do not assume that port `8080` is always active.

After ThumbLLM loads, look for:

```text
API: http://127.0.0.1:PORT/v1
```

For example:

```text
API: http://127.0.0.1:8080/v1
```

or:

```text
API: http://127.0.0.1:8081/v1
```

Use that port in all subsequent requests.

In the examples below, port `8080` is used for readability.

Replace it if ThumbLLM displays a different active port.

---

# Quick API Test

Start ThumbLLM and wait until it reports:

```text
Model loaded and ready.
```

Then open:

```text
http://127.0.0.1:8080/v1/models
```

using the actual active port.

A successful response will contain information about the model currently loaded by that ThumbLLM edition.

For example:

```json
{
  "object": "list",
  "data": [
    {
      "id": "Qwen3.5-4B-Q4_K_M.gguf",
      "object": "model"
    }
  ]
}
```

Additional fields may also be present.

---

# Model IDs

The exact model ID returned by `/v1/models` depends on the ThumbLLM runtime and bundled llama.cpp version.

A model ID might look like:

```text
Qwen3.5-4B-Q4_K_M.gguf
```

or a native llama.cpp server may return a full path such as:

```text
C:\Models\Qwen3.5-4B-Q4_K_M.gguf
```

Therefore:

> **Do not hard-code the model ID.**

Instead:

1. call `/v1/models`,
2. read `data[0].id`,
3. use that exact value for subsequent requests.

ThumbLLM editions currently load one primary model at a time, but clients should still discover the model through the API rather than deriving it from the executable filename.

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

A healthy server should return an HTTP success response.

The exact JSON body may differ between:

* native llama-server,
* llama-cpp-python fallback,
* and different bundled llama.cpp versions.

Applications should primarily use the HTTP status to determine whether the server is ready.

The Python fallback also supports:

```text
/v1/health
```

but applications intended to work across ThumbLLM runtime modes should prefer:

```text
/health
```

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

Representative response:

```json
{
  "object": "list",
  "data": [
    {
      "id": "Qwen3.5-4B-Q4_K_M.gguf",
      "object": "model"
    }
  ]
}
```

Read:

```text
data[0].id
```

and use it as the model identifier for subsequent requests.

Do not depend on optional fields such as:

```text
owned_by
created
```

because those can vary between runtime modes.

---

# Chat Completions

The primary ThumbLLM inference endpoint is:

```http
POST /v1/chat/completions
```

Full example URL:

```text
http://127.0.0.1:8080/v1/chat/completions
```

---

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

Replace:

```text
MODEL_ID
```

with the exact value returned by:

```text
GET /v1/models
```

---

## Representative Response

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

The exact response may contain additional llama.cpp-specific fields.

Applications should read the final answer from:

```text
choices[0].message.content
```

---

# API Requests Are Independent of the Desktop Conversation

The ThumbLLM desktop chat interface and external API clients use the same underlying loaded model, but they do **not** automatically share conversation history.

An external API request does not inherit previous messages typed into the ThumbLLM desktop chat.

Likewise, API messages are not automatically inserted into the desktop chat history.

If an API client needs conversation history, it should send that history in the `messages` array.

For example:

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
  ]
}
```

The API client is responsible for maintaining this conversation state.

---

# System Prompts

API clients may provide their own system message:

```json
{
  "role": "system",
  "content": "You are a concise technical assistant."
}
```

Do not assume that an external API request automatically inherits the system prompt used by ThumbLLM's desktop chat interface.

If your application requires a particular system instruction, include it explicitly in the request.

---

# Common Request Parameters

Common OpenAI-compatible parameters include:

| Parameter           | Type         | Purpose                           |
| ------------------- | ------------ | --------------------------------- |
| `model`             | string       | Model ID returned by `/v1/models` |
| `messages`          | array        | Conversation messages             |
| `temperature`       | number       | Sampling temperature              |
| `top_p`             | number       | Nucleus sampling probability      |
| `max_tokens`        | integer      | Maximum generated-token budget    |
| `stream`            | boolean      | Enable streaming                  |
| `seed`              | integer      | Sampling seed when supported      |
| `stop`              | string/array | Optional stop sequence            |
| `presence_penalty`  | number       | Optional sampling control         |
| `frequency_penalty` | number       | Optional sampling control         |

Additional parameters may be accepted depending on:

* the selected model,
* chat template,
* ThumbLLM edition,
* llama.cpp version,
* and active inference runtime.

Do not assume that every optional OpenAI or llama.cpp parameter is available in every ThumbLLM edition.

---

# Output Token Limits and Context

`max_tokens` controls the maximum output budget requested by the client.

For example:

```json
{
  "max_tokens": 512
}
```

does not guarantee that 512 tokens will be generated.

Generation can stop earlier because of:

* EOS,
* stop sequences,
* context limits,
* model behavior,
* or other runtime constraints.

Each ThumbLLM edition can also use a different model context size.

Do not assume that every ThumbLLM edition has the same context window.

---

# Thinking and Reasoning Models

Some models can generate internal or separately returned reasoning.

Depending on the model and llama.cpp chat template, a response may contain fields such as:

```json
{
  "content": "The capital of France is Paris.",
  "reasoning_content": "..."
}
```

`content` is the final assistant answer.

`reasoning_content`, when present, represents separate reasoning output.

Applications interested only in the answer should normally read:

```text
choices[0].message.content
```

---

# Important: Native API Reasoning Behavior

When ThumbLLM is using its native bundled `llama-server`, an external API client talks directly to that server.

Therefore, the ThumbLLM desktop GUI's reasoning state should **not** be assumed to automatically apply to external API requests.

If your application requires non-thinking behavior, explicitly request it.

For compatible Qwen/llama.cpp models and chat templates:

```json
{
  "reasoning_effort": "none",
  "chat_template_kwargs": {
    "enable_thinking": false
  }
}
```

is the recommended request pattern.

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
  "max_tokens": 512,
  "stream": false,
  "reasoning_effort": "none",
  "chat_template_kwargs": {
    "enable_thinking": false
  }
}
```

This is particularly useful for:

* deterministic benchmarks,
* factual questions,
* structured output,
* applications where hidden reasoning would waste latency,
* and workloads where a thinking model might consume a large output budget before producing its final answer.

---

# Python Fallback Reasoning Behavior

When ThumbLLM is operating through its `llama-cpp-python` fallback API, ThumbLLM normalizes reasoning settings according to the application's configured reasoning state.

That means client-provided reasoning controls may not behave identically to native llama-server mode.

Applications that depend on precise reasoning controls should verify behavior with the specific ThumbLLM edition being used.

---

# `max_tokens` and Reasoning

Reasoning-capable models can consume a substantial portion of the requested output budget before producing a final answer.

For example:

```json
{
  "max_tokens": 100
}
```

may allow a thinking model to consume much or all of those tokens before emitting normal `content`.

The response could then end with:

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

For deterministic or performance-sensitive workloads, explicitly disabling thinking is usually preferable to simply increasing the output budget.

---

# Streaming Responses

Set:

```json
{
  "stream": true
}
```

to request streaming output.

Streaming uses Server-Sent Events.

A content chunk may look like:

```text
data: {"choices":[{"delta":{"content":"Local"}}]}
```

followed by:

```text
data: {"choices":[{"delta":{"content":" AI"}}]}
```

Thinking-capable models may also return fields such as:

```json
{
  "delta": {
    "reasoning_content": "..."
  }
}
```

The stream normally ends with:

```text
data: [DONE]
```

Clients should handle both ordinary content chunks and optional reasoning chunks.

---

# PowerShell

PowerShell's `Invoke-RestMethod` is recommended on Windows because it avoids many command-line JSON quoting problems.

## Set the Active Port

Use the port displayed by ThumbLLM:

```powershell
$port = 8080
$baseUrl = "http://127.0.0.1:$port"
```

Change `8080` if ThumbLLM selected another port.

---

## Check Health

```powershell
$port = 8080
$baseUrl = "http://127.0.0.1:$port"

Invoke-RestMethod `
    -Method GET `
    -Uri "$baseUrl/health"
```

---

## Discover the Model

```powershell
$port = 8080
$baseUrl = "http://127.0.0.1:$port"

$models = Invoke-RestMethod `
    -Method GET `
    -Uri "$baseUrl/v1/models"

$model = $models.data[0].id

$model
```

---

## Send a Chat Request

```powershell
$port = 8080
$baseUrl = "http://127.0.0.1:$port"

$models = Invoke-RestMethod `
    -Method GET `
    -Uri "$baseUrl/v1/models"

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
    -Uri "$baseUrl/v1/chat/completions" `
    -ContentType "application/json" `
    -Body $body

$response.choices[0].message.content
```

Expected result:

```text
The capital of France is Paris.
```

---

## Send a Chat Request with Thinking Disabled

```powershell
$port = 8080
$baseUrl = "http://127.0.0.1:$port"

$models = Invoke-RestMethod `
    -Method GET `
    -Uri "$baseUrl/v1/models"

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
    reasoning_effort = "none"
    chat_template_kwargs = @{
        enable_thinking = $false
    }
} | ConvertTo-Json -Depth 10 -Compress

$response = Invoke-RestMethod `
    -Method POST `
    -Uri "$baseUrl/v1/chat/completions" `
    -ContentType "application/json" `
    -Body $body

$response.choices[0].message.content
```

---

## Show the Complete Response

```powershell
$response | ConvertTo-Json -Depth 10
```

---

# Windows Command Prompt

PowerShell is recommended for Windows API testing.

If you use Command Prompt, JSON quoting must be escaped manually.

Example:

```bat
curl.exe -X POST http://127.0.0.1:8080/v1/chat/completions -H "Content-Type: application/json" -d "{\"model\":\"MODEL_ID\",\"messages\":[{\"role\":\"user\",\"content\":\"What is the capital of France?\"}],\"temperature\":0,\"max_tokens\":512,\"stream\":false}"
```

Replace:

```text
MODEL_ID
```

with the model ID returned by `/v1/models`.

### Important Windows Path Warning

A native llama.cpp server may return a Windows path as its model ID.

For example:

```text
C:\Models\Qwen3.5-4B-Q4_K_M.gguf
```

Backslashes inside JSON strings must be escaped correctly.

Because of this, PowerShell's `ConvertTo-Json`, Python's `json` module, or an OpenAI-compatible SDK is much safer than manually constructing JSON in Command Prompt.

---

# Python Standard Library Example

This example requires no third-party packages.

```python
import json
import urllib.request

PORT = 8080
API_BASE = f"http://127.0.0.1:{PORT}"

# Discover model.
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
    result = json.loads(response.read().decode("utf-8"))

answer = result["choices"][0]["message"]["content"]

print(answer)
```

Change:

```python
PORT = 8080
```

if ThumbLLM displays another active port.

---

# Python Example with Thinking Disabled

```python
import json
import urllib.request

PORT = 8080
API_BASE = f"http://127.0.0.1:{PORT}"

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
    result = json.loads(response.read().decode("utf-8"))

message = result["choices"][0]["message"]

print(message.get("content", ""))
```

---

# Python OpenAI SDK Example

ThumbLLM can also be used with the OpenAI Python client by supplying ThumbLLM as the custom base URL.

Install the package:

```bash
pip install openai
```

Then:

```python
from openai import OpenAI

PORT = 8080

client = OpenAI(
    base_url=f"http://127.0.0.1:{PORT}/v1",
    api_key="local",
)

models = client.models.list()
model_id = models.data[0].id

response = client.chat.completions.create(
    model=model_id,
    messages=[
        {
            "role": "user",
            "content": "What is the capital of France?",
        }
    ],
    temperature=0.0,
    max_tokens=512,
)

print(response.choices[0].message.content)
```

The default ThumbLLM configuration does **not** require an API key.

The value:

```python
api_key="local"
```

is simply a placeholder for client libraries that require a non-empty API-key field.

---

## OpenAI SDK with Thinking Disabled

For native llama-server mode:

```python
from openai import OpenAI

PORT = 8080

client = OpenAI(
    base_url=f"http://127.0.0.1:{PORT}/v1",
    api_key="local",
)

models = client.models.list()
model_id = models.data[0].id

response = client.chat.completions.create(
    model=model_id,
    messages=[
        {
            "role": "user",
            "content": "What is the capital of France?",
        }
    ],
    temperature=0.0,
    max_tokens=512,
    extra_body={
        "reasoning_effort": "none",
        "chat_template_kwargs": {
            "enable_thinking": False,
        },
    },
)

print(response.choices[0].message.content)
```

---

# JavaScript Example

```javascript
async function askThumbLLM(question) {
    const port = 8080;
    const baseUrl = `http://127.0.0.1:${port}`;

    const modelsResponse = await fetch(
        `${baseUrl}/v1/models`
    );

    if (!modelsResponse.ok) {
        throw new Error(
            `Model discovery failed: HTTP ${modelsResponse.status}`
        );
    }

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

    if (!response.ok) {
        throw new Error(
            `ThumbLLM request failed: HTTP ${response.status}`
        );
    }

    const result = await response.json();

    return result.choices[0].message.content;
}

askThumbLLM("What is the capital of France?")
    .then(console.log)
    .catch(console.error);
```

Browser CORS behavior can differ depending on the active ThumbLLM runtime and bundled llama.cpp version.

For development, Node.js, Python, PowerShell, or another native HTTP client is generally more predictable than a `file://` browser page.

---

# Using ThumbLLM with OpenAI-Compatible Applications

Applications that allow a custom OpenAI-compatible server can generally use ThumbLLM.

Configure:

```text
Base URL:
http://127.0.0.1:ACTIVE_PORT/v1
```

For example:

```text
http://127.0.0.1:8080/v1
```

Then discover the model through:

```text
GET /v1/models
```

Some applications perform this automatically.

If an application requires an API-key field even though ThumbLLM authentication is disabled, try a placeholder such as:

```text
local
```

or:

```text
thumbllm
```

The placeholder has no security function when ThumbLLM authentication is disabled.

Compatibility depends on how much of the OpenAI API the client expects.

Clients requiring only:

```text
/v1/models
/v1/chat/completions
```

are the most likely to work.

Applications that require unrelated OpenAI endpoints may not be compatible.

---

# What ThumbLLM Does Not Guarantee

ThumbLLM's stable public API is intentionally small.

Do not assume that the following OpenAI services exist merely because the API is OpenAI-compatible:

```text
/v1/responses
/v1/embeddings
/v1/audio
/v1/images
/v1/files
/v1/fine_tuning
```

An underlying runtime may expose additional functionality, but ThumbLLM does not treat those endpoints as part of its stable chat API unless explicitly documented by a particular edition.

---

# Authentication

The current default configuration does not require authentication.

Default:

```text
API key required: No
LAN access:       Disabled
Host:             127.0.0.1
```

When API-key authentication is enabled in a ThumbLLM build, clients authenticate using:

```http
Authorization: Bearer YOUR_API_KEY
```

Example:

```text
Authorization: Bearer YOUR_API_KEY
```

The native llama-server and Python fallback are both configured to honor ThumbLLM's API-key setting when authentication is enabled.

---

# Local-Only Security Model

By default ThumbLLM binds to:

```text
127.0.0.1
```

This means the server accepts connections only from the same computer.

Another computer on your LAN cannot directly connect to it.

This is intentional.

ThumbLLM is designed primarily as a personal local inference application.

Prompts and generated responses can remain on the user's machine when using only the local API.

Internet access may still be required for:

* the initial model download,
* software updates,
* or external services deliberately contacted by the client application.

---

# LAN Access

The default ThumbLLM configuration has LAN access disabled.

A build configured for LAN access can bind the API to all network interfaces.

If LAN access is enabled, the API should be treated as a network service rather than a purely local process.

Do not expose an unauthenticated ThumbLLM API to an untrusted network.

When enabling network access, use appropriate:

* authentication,
* Windows Firewall rules,
* router/network controls,
* and trusted-network boundaries.

---

# API and GUI Concurrency

The ThumbLLM desktop interface and API use the same underlying loaded model.

The current CPU-focused configuration is optimized primarily for a **single active inference workload**.

The native server configuration uses a single parallel generation slot.

Therefore, simultaneous requests can:

* wait behind another generation,
* reduce performance,
* increase response latency,
* increase memory pressure,
* or make a long generation appear to be stalled.

For benchmarking, send requests sequentially unless the ThumbLLM edition has explicitly been configured and tested for concurrent inference.

Avoid running a long desktop-chat generation at the same time as an external benchmark unless simultaneous execution is intentional.

---

# Long-Running Requests

Local CPU inference can take substantially longer than cloud inference, especially when:

* large outputs are requested,
* long prompts are used,
* reasoning is enabled,
* or several requests compete for the same inference engine.

External API clients should therefore configure an appropriate HTTP timeout.

For example:

```python
timeout=300
```

may be reasonable for ordinary testing, while longer workloads may require a larger timeout.

A client-side timeout does not necessarily mean the model or ThumbLLM crashed.

It can simply mean the client stopped waiting before local inference finished.

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

Native llama.cpp responses may also contain timing or runtime-specific information.

For example, some runtime versions may expose fields describing:

* prompt token count,
* generated token count,
* prompt processing speed,
* decode speed,
* or internal timing.

These are useful for diagnostics and benchmarking but are **not part of ThumbLLM's guaranteed stable API contract**.

Applications should not fail merely because optional timing fields are absent.

---

# Finish Reasons

A chat completion commonly contains:

```json
{
  "finish_reason": "stop"
}
```

or:

```json
{
  "finish_reason": "length"
}
```

`stop` normally indicates that generation completed naturally.

`length` normally indicates that the requested output-token budget was reached.

For reasoning-capable models, a `length` response with little or no final `content` may mean that the output budget was consumed by reasoning.

---

# Error Handling

Clients should handle HTTP errors rather than assuming every request succeeds.

Common examples include:

| Status | Meaning                                          |
| ------ | ------------------------------------------------ |
| `400`  | Invalid request                                  |
| `401`  | Authentication failure when API keys are enabled |
| `404`  | Endpoint not found                               |
| `500`  | Server or inference error                        |

The native llama.cpp runtime may return additional HTTP status codes.

Error response bodies can also vary slightly between runtime modes.

A representative error is:

```json
{
  "error": {
    "message": "Invalid request",
    "type": "server_error"
  }
}
```

Clients should primarily use:

* HTTP status,
* the presence of an `error` object,
* and the error message

rather than depending on one exact error schema.

---

# Recommended Client Error Checks

Before reading:

```text
choices[0].message.content
```

a robust client should verify that:

1. the HTTP request succeeded,
2. a response body was returned,
3. `choices` exists,
4. at least one choice exists,
5. a message exists,
6. and `content` is usable.

Reasoning models can legitimately return empty final content when their output budget is exhausted.

---

# Additional llama.cpp Endpoints

When ThumbLLM is running through the native bundled `llama-server`, the runtime may expose additional endpoints.

Examples can include:

```text
/completion
/tokenize
/apply-template
/v1/chat/completions/input_tokens
```

Availability depends on the llama.cpp build bundled with that ThumbLLM edition.

These endpoints can be useful for:

* diagnostics,
* token counting,
* template testing,
* raw completion testing,
* and development.

However:

> **These are llama.cpp implementation endpoints and are not guaranteed to remain part of ThumbLLM's stable public API.**

Applications intended to work across ThumbLLM editions should prefer:

```text
GET  /health
GET  /v1/models
POST /v1/chat/completions
```

The Python fallback does not necessarily expose the same auxiliary endpoints as the native server.

---

# Raw `/completion` Endpoint

When supported by the active native llama-server, `/completion` can be useful for diagnostics.

Example:

```json
{
  "prompt": "What is the capital of France?",
  "n_predict": 64,
  "temperature": 0
}
```

This bypasses OpenAI-style chat messages and uses llama.cpp's raw completion interface.

Do not build a portable ThumbLLM application around `/completion`.

For application development, prefer:

```text
/v1/chat/completions
```

---

# Runtime Compatibility

Each ThumbLLM edition packages a particular:

* language model,
* quantization,
* model profile,
* llama.cpp runtime,
* inference configuration,
* and hardware target.

As llama.cpp evolves, optional request parameters and response fields can change.

The portable ThumbLLM API surface is therefore intentionally limited to:

```text
GET  /health
GET  /v1/models
POST /v1/chat/completions
```

Code using additional llama.cpp-specific controls should account for runtime-version differences.

---

# ThumbLLM Lifecycle

The API exists only while the ThumbLLM application and its inference engine are running.

Typical lifecycle:

```text
Start ThumbLLM
      │
      ▼
Validate / download model
      │
      ▼
Select available API port
      │
      ▼
Start inference engine
      │
      ▼
Wait for server health check
      │
      ▼
Start ThumbLLM interface
      │
      ▼
API available
```

Closing ThumbLLM also shuts down the inference engine owned by that ThumbLLM instance.

Clients should therefore be prepared for connection failures if ThumbLLM is closed or restarted.

A restart can also result in a different active port if the previously preferred port is no longer available.

---

# Troubleshooting

## `/v1/models` Does Not Load

First check that ThumbLLM is fully loaded.

Then verify the active API line displayed by ThumbLLM.

For example:

```text
API: http://127.0.0.1:8081/v1  │  Port: 8081
```

If ThumbLLM selected `8081`, this will not work:

```text
http://127.0.0.1:8080/v1/models
```

Use:

```text
http://127.0.0.1:8081/v1/models
```

instead.

---

## Port 8080 Is Already in Use

This does not necessarily indicate a problem.

ThumbLLM automatically tries the next available port.

For example:

```text
Preferred API port 8080 is in use;
using port 8081 for this ThumbLLM instance.
```

Use the new port displayed by ThumbLLM.

---

## Multiple ThumbLLM Instances

Multiple ThumbLLM instances can run simultaneously if system resources permit.

Each instance selects its own available API port.

For example:

```text
ThumbLLM Instance 1
127.0.0.1:8080

ThumbLLM Instance 2
127.0.0.1:8081

ThumbLLM Instance 3
127.0.0.1:8082
```

Always connect to the intended instance's displayed port.

---

## Another llama.cpp Application Is Running

Applications such as LM Studio or another llama.cpp server may already occupy a port.

ThumbLLM normally works around this by selecting another available port.

To determine what owns a particular port on Windows:

```bat
netstat -ano | findstr ":8080"
```

Then inspect the PID:

```bat
tasklist /FI "PID eq YOUR_PID"
```

---

## Empty `content`

If a response contains:

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

the model may have consumed its output budget while reasoning.

Either:

* increase `max_tokens`, or
* disable thinking.

For native llama-server mode:

```json
{
  "reasoning_effort": "none",
  "chat_template_kwargs": {
    "enable_thinking": false
  }
}
```

is recommended when reasoning is not required.

---

## A Request Appears to Hang

First determine whether the model is actually stalled or simply generating for a long time.

Possible causes include:

* reasoning accidentally enabled,
* a very large `max_tokens` value,
* long prompt processing,
* another request already using the single inference slot,
* CPU saturation,
* or an unusually long model response.

For reasoning-capable models, explicitly disabling thinking can substantially reduce response time when reasoning is unnecessary.

Also check the active native llama-server log when diagnosing runtime behavior.

---

## PowerShell JSON Errors

Errors such as:

```text
parse error while parsing object key
```

or:

```text
Could not resolve host
```

can be caused by shell quoting rather than ThumbLLM itself.

Use:

```text
Invoke-RestMethod
```

with:

```text
ConvertTo-Json
```

instead of manually constructing complex JSON strings.

---

# Benchmarking ThumbLLM

ThumbLLM's local API can be used by external benchmark programs such as TeksBench.

A benchmark should:

1. start ThumbLLM,
2. wait for the model to finish loading,
3. determine the active API port,
4. call `/v1/models`,
5. read `data[0].id`,
6. submit prompts through `/v1/chat/completions`,
7. capture responses and token usage,
8. record latency,
9. and grade responses separately.

For deterministic tests against a reasoning-capable native llama.cpp model, a request can use:

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

This prevents a thinking-capable model from silently spending a large amount of time and output budget on reasoning when reasoning is not part of the benchmark methodology.

Benchmark methodology should document:

* ThumbLLM edition,
* exact model,
* quantization,
* active API port,
* llama.cpp runtime/build,
* context size,
* sampling settings,
* reasoning settings,
* output-token budget,
* hardware,
* number of runs,
* warm-up behavior,
* and judging methodology.

---

# Minimal Robust Client Flow

A robust client should follow this sequence:

```text
Start ThumbLLM
      │
      ▼
Read active API port
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
Build messages
      │
      ▼
Optionally set reasoning controls
      │
      ▼
POST /v1/chat/completions
      │
      ▼
Check HTTP status / error
      │
      ▼
Read choices[0].message.content
```

Do not assume:

```text
port = 8080
```

if ThumbLLM displays another port.

Do not assume the model ID from the ThumbLLM executable filename.

Discover both runtime values whenever possible.

---

# Stable API Summary

## Host

Default:

```text
127.0.0.1
```

---

## Preferred Starting Port

```text
8080
```

The actual port may differ.

Use the port displayed by ThumbLLM.

---

## OpenAI-Compatible Base URL

```text
http://127.0.0.1:ACTIVE_PORT/v1
```

Example:

```text
http://127.0.0.1:8080/v1
```

---

## Health

```text
GET /health
```

---

## Models

```text
GET /v1/models
```

---

## Chat

```text
POST /v1/chat/completions
```

---

## Model ID

Discover:

```text
data[0].id
```

from:

```text
GET /v1/models
```

---

## Final Answer

Read:

```text
choices[0].message.content
```

---

## Optional Reasoning

May appear in:

```text
choices[0].message.reasoning_content
```

or in streaming deltas.

---

## Disable Thinking in Native llama-server Mode

When supported:

```json
{
  "reasoning_effort": "none",
  "chat_template_kwargs": {
    "enable_thinking": false
  }
}
```

---

## Authentication

Default:

```text
None
```

If a client library requires an API-key field, a local placeholder such as:

```text
local
```

can be used while ThumbLLM authentication is disabled.

---

## LAN Access

Default:

```text
Disabled
```

---

# Project

**ThumbLLM** is a TeksEdge project focused on making optimized local language-model inference simple to run on personal computers.

Each ThumbLLM edition packages a selected model profile, quantization, llama.cpp runtime, and tested inference configuration into a standalone application.

The ThumbLLM API allows developers to use that same locally running model from their own software without manually starting or configuring a separate inference server.
