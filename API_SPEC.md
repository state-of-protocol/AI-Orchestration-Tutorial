# 📡 API_SPEC.md — Spesifikasi API & Skema Data

**Kontrak API untuk Platform Ejen Autonomi**  
_Versi 1.0 · Mei 2026_

Dokumen ini mentakrifkan **spesifikasi API, struktur permintaan/respons, dan skema data** untuk setiap platform yang diajar dalam repositori ini. Ia merangkumi butiran teknikal yang diperlukan untuk mengintegrasikan, menguji, dan menyahpepijat ejen.

---

## 📖 Isi Kandungan

- [1. Gambaran Keseluruhan](#1-gambaran-keseluruhan)
- [2. Google AI Studio API](#2-google-ai-studio-api)
  - [2.1 Endpoint Utama](#21-endpoint-utama)
  - [2.2 Interactions API (Managed Agents)](#22-interactions-api-managed-agents)
  - [2.3 Function Calling & Extensions](#23-function-calling--extensions)
  - [2.4 Context Caching](#24-context-caching)
- [3. Anthropic Claude API](#3-anthropic-claude-api)
  - [3.1 Messages API](#31-messages-api)
  - [3.2 Extended & Adaptive Thinking](#32-extended--adaptive-thinking)
  - [3.3 Model Context Protocol (MCP)](#33-model-context-protocol-mcp)
- [4. DeepSeek API](#4-deepseek-api)
  - [4.1 Chat Completions (OpenAI‑Compatible)](#41-chat-completions-openai-compatible)
  - [4.2 Parameter Khusus R1 (Chain‑of‑Thought)](#42-parameter-khusus-r1-chain-of-thought)
  - [4.3 Cost Metrics Endpoint](#43-cost-metrics-endpoint)
- [5. Pengesahan & Keselamatan](#5-pengesahan--keselamatan)
- [6. Pengendalian Ralat](#6-pengendalian-ralat)

---

## 1. Gambaran Keseluruhan

| Platform | Base URL | Kaedah Pengesahan | Format |
|----------|----------|-------------------|--------|
| Google AI Studio | `https://generativelanguage.googleapis.com/v1beta` | API Key (Header `x-goog-api-key`) / OAuth 2.0 | REST + gRPC |
| Anthropic Claude | `https://api.anthropic.com/v1` | API Key (Header `x-api-key`) + `anthropic-version` | REST + SSE |
| DeepSeek | `https://api.deepseek.com/v1` | API Key (Header `Authorization: Bearer`) | REST (serasi OpenAI) |

---

## 2. Google AI Studio API

### 2.1 Endpoint Utama

| Endpoint | Kaedah | Tujuan |
|----------|--------|--------|
| `/models/gemini-3.5-flash:generateContent` | POST | Penjanaan teks/multimodal standard |
| `/models/gemini-3.5-pro:generateContent` | POST | Tugas kompleks, konteks panjang |
| `/interactions` | POST | **Managed Agent** – satu panggilan untuk menyediakan sandbox |
| `/interactions/{id}` | GET | Dapatkan status interaksi |
| `/cachedContents` | POST | Cipta cache konteks |

### 2.2 Interactions API (Managed Agents)

Ini adalah API utama untuk tutorial Google AI Studio. Ia membolehkan satu panggilan API menyediakan ejen dalam persekitaran Linux terpencil dan melaksanakan tugas autonomi.

#### Permintaan: `POST /interactions`

**Header:**
```
Content-Type: application/json
x-goog-api-key: {API_KEY}
```

**Badan Permintaan:**
```json
{
  "agent": "antigravity-preview-05-2026",
  "input": "Analyze this CSV file for sales trends and generate a chart.",
  "environment": "remote",
  "tools": [
    {
      "name": "code_execution",
      "type": "code_execution"
    },
    {
      "name": "google_search",
      "type": "google_search"
    }
  ],
  "files": [
    {
      "uri": "gs://bucket/sales-data.csv",
      "mime_type": "text/csv"
    }
  ],
  "previous_interaction_id": "interact-abc123",
  "environment_id": "env-xyz789",
  "system_instruction": "You are a data analyst. Always provide chart images.",
  "safety_settings": [
    {
      "category": "HARM_CATEGORY_DANGEROUS_CONTENT",
      "threshold": "BLOCK_ONLY_HIGH"
    }
  ],
  "generation_config": {
    "temperature": 0.4,
    "max_output_tokens": 8192
  }
}
```

**Parameter Penting:**
| Parameter | Jenis | Penerangan |
|-----------|-------|-------------|
| `agent` | string | ID ejen terurus. Gunakan `"antigravity-preview-05-2026"` untuk akses preview. |
| `input` | string | Arahan tugasan (prompt). |
| `environment` | string | `"remote"` untuk sandbox cloud, `"local"` untuk Antigravity CLI. |
| `tools` | array | Senarai alat yang tersedia untuk ejen (`code_execution`, `google_search`, `gmail`, `sheets`, `docs`). |
| `files` | array | Fail untuk dimuatkan ke dalam konteks (URI Google Cloud Storage). |
| `previous_interaction_id` | string | ID interaksi sebelumnya untuk meneruskan sesi. |
| `environment_id` | string | ID persekitaran sandbox yang ingin disambung semula. |
| `system_instruction` | string | Arahan sistem untuk ejen. |
| `safety_settings` | array | Penetapan tapisan kandungan. |
| `generation_config` | objek | Parameter penjanaan (suhu, token maksimum). |

#### Respons Berjaya (200 OK)

Respons distrim sebagai **Server‑Sent Events (SSE)**. Setiap cebisan (`chunk`) mengandungi token atau status:

**Contoh aliran SSE:**
```
data: {"type":"status","status":"provisioning_sandbox"}

data: {"type":"status","status":"sandbox_ready","environment_id":"env-xyz789"}

data: {"type":"content","text":"Sure, let me analyze the CSV file."}

data: {"type":"tool_call","tool":"code_execution","arguments":{"code":"import pandas as pd\n..."}}

data: {"type":"tool_result","tool":"code_execution","output":"Data loaded: 5000 rows"}

data: {"type":"content","text":"The sales trend shows a 15% increase in Q2."}

data: {"type":"file","name":"sales-trend-chart.png","uri":"gs://bucket/output/sales-trend-chart.png","mime_type":"image/png"}

data: {"type":"completion","reason":"STOP","usage":{"prompt_tokens":1250,"completion_tokens":430,"total_tokens":1680}}
```

**Skema Cebisan:**

```json
{
  "type": "content | tool_call | tool_result | file | status | completion",
  "text": "string (untuk content)",
  "tool": "string (untuk tool_call/tool_result)",
  "arguments": { "code": "string" },
  "output": "string",
  "file": {
    "name": "string",
    "uri": "string",
    "mime_type": "string"
  },
  "status": "string",
  "environment_id": "string",
  "usage": {
    "prompt_tokens": 0,
    "completion_tokens": 0,
    "total_tokens": 0
  },
  "reason": "STOP | SAFETY | MAX_TOKENS"
}
```

#### Respons Ralat

```json
{
  "error": {
    "code": 429,
    "message": "Resource has been exhausted. Quota exceeded.",
    "status": "RESOURCE_EXHAUSTED"
  }
}
```

### 2.3 Function Calling & Extensions

Apabila tidak menggunakan Managed Agent, Function Calling digunakan untuk menyambung ke Google Workspace.

**Definisi Alat (dalam permintaan):**
```json
{
  "tools": [
    {
      "functionDeclarations": [
        {
          "name": "read_gmail",
          "description": "Read recent emails from Gmail inbox",
          "parameters": {
            "type": "object",
            "properties": {
              "max_results": {
                "type": "integer",
                "description": "Number of emails to fetch"
              }
            },
            "required": ["max_results"]
          }
        }
      ]
    }
  ]
}
```

**Respons Function Call (daripada model):**
```json
{
  "candidates": [
    {
      "content": {
        "parts": [
          {
            "functionCall": {
              "name": "read_gmail",
              "args": { "max_results": 10 }
            }
          }
        ]
      }
    }
  ]
}
```

### 2.4 Context Caching

**Cipta Cache:**
```json
POST /cachedContents
{
  "model": "models/gemini-3.5-flash",
  "contents": [
    {
      "role": "user",
      "parts": [{ "text": "Long document content here..." }]
    }
  ],
  "ttl": "3600s"
}
```

**Respons:**
```json
{
  "name": "cachedContents/abc123",
  "model": "models/gemini-3.5-flash",
  "usageMetadata": {
    "totalTokenCount": 150000
  }
}
```

**Menggunakan Cache:**
```json
POST /models/gemini-3.5-flash:generateContent
{
  "cachedContent": "cachedContents/abc123",
  "contents": [...]
}
```

> 💡 **Penjimatan:** Cache mengurangkan kos token input sehingga 80%.

---

## 3. Anthropic Claude API

### 3.1 Messages API

**Endpoint:** `POST /v1/messages`

**Header:**
```
Content-Type: application/json
x-api-key: {API_KEY}
anthropic-version: 2026-05-01
```

**Badan Permintaan Asas:**
```json
{
  "model": "claude-sonnet-5-20260501",
  "max_tokens": 4096,
  "system": "You are a senior software engineer. Write clean, tested code.",
  "messages": [
    {
      "role": "user",
      "content": "Write a Python function to validate JWT tokens."
    }
  ],
  "tools": [
    {
      "name": "read_file",
      "description": "Read a file from the local filesystem",
      "input_schema": {
        "type": "object",
        "properties": {
          "path": { "type": "string", "description": "Absolute path to the file" }
        },
        "required": ["path"]
      }
    }
  ]
}
```

**Respons:**
```json
{
  "id": "msg_abc123",
  "type": "message",
  "role": "assistant",
  "model": "claude-sonnet-5-20260501",
  "content": [
    {
      "type": "text",
      "text": "Here's a JWT validation function:\n\n```python\nimport jwt\n..."
    },
    {
      "type": "tool_use",
      "id": "toolu_001",
      "name": "read_file",
      "input": { "path": "/project/auth.py" }
    }
  ],
  "stop_reason": "end_turn",
  "stop_sequence": null,
  "usage": {
    "input_tokens": 320,
    "output_tokens": 450,
    "cache_creation_input_tokens": 0,
    "cache_read_input_tokens": 0
  }
}
```

**Jenis Kandungan (content blocks):**
- `text` – teks biasa
- `tool_use` – permintaan untuk menggunakan alat
- `tool_result` – hasil daripada alat (dihantar oleh klien dalam permintaan seterusnya)
- `thinking` – pemikiran dalaman (jika thinking diaktifkan)

### 3.2 Extended & Adaptive Thinking

**Parameter Thinking:**
```json
{
  "thinking": {
    "type": "enabled",
    "budget_tokens": 4000
  }
}
```

Atau untuk Adaptive Thinking (model memutuskan sendiri kedalaman):
```json
{
  "thinking": {
    "type": "adaptive",
    "effort": "high"  // "auto", "low", "medium", "high"
  }
}
```

**Respons dengan Thinking:**
```json
{
  "content": [
    {
      "type": "thinking",
      "thinking": "First, I need to understand the JWT structure... The token has three parts..."
    },
    {
      "type": "text",
      "text": "Here's the function..."
    }
  ],
  "usage": {
    "input_tokens": 320,
    "output_tokens": 1200,  // termasuk thinking tokens
    "thinking_tokens": 750
  }
}
```

> ⚠️ **Nota:** Token pemikiran dikenakan caj seperti token output biasa.

### 3.3 Model Context Protocol (MCP)

MCP bukan sebahagian daripada API HTTP Anthropic secara langsung. Ia adalah protokol tempatan antara aplikasi hos dan pelayan MCP. Walau bagaimanapun, alat MCP diterjemahkan ke definisi `tools` dalam API Messages.

**Konfigurasi `mcp-config.json`:**
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-filesystem", "/allowed/path"],
      "permissions": {
        "read": ["/allowed/path/**"],
        "write": ["/allowed/path/output/**"]
      }
    },
    "database": {
      "command": "python",
      "args": ["-m", "mcp_server_postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost:5432/mydb"
      }
    }
  }
}
```

**Aliran MCP (diringkaskan):**
1. Aplikasi hos membaca `mcp-config.json`.
2. Hos melancarkan setiap pelayan MCP sebagai proses berasingan.
3. Hos menanyakan setiap pelayan untuk senarai alat yang disediakan.
4. Hos menggabungkan definisi alat ini ke dalam permintaan API Claude.
5. Apabila Claude memanggil `tool_use`, hos menghantar permintaan ke pelayan MCP yang sesuai.
6. Hasil dikembalikan kepada Claude sebagai `tool_result`.

---

## 4. DeepSeek API

### 4.1 Chat Completions (OpenAI‑Compatible)

**Endpoint:** `POST /v1/chat/completions`

**Header:**
```
Content-Type: application/json
Authorization: Bearer {API_KEY}
```

**Badan Permintaan:**
```json
{
  "model": "deepseek-v4-pro",
  "messages": [
    {
      "role": "system",
      "content": "You are a cost-efficient code analyzer."
    },
    {
      "role": "user",
      "content": "Find circular dependencies in this monorepo."
    }
  ],
  "temperature": 0.3,
  "max_tokens": 8192,
  "stream": true,
  "frequency_penalty": 0.0,
  "presence_penalty": 0.0
}
```

**Respons (bukan streaming):**
```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion",
  "created": 1717000000,
  "model": "deepseek-v4-pro",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": "Analysis complete. Found 3 circular dependencies..."
      },
      "finish_reason": "stop"
    }
  ],
  "usage": {
    "prompt_tokens": 1250,
    "completion_tokens": 430,
    "total_tokens": 1680
  }
}
```

### 4.2 Parameter Khusus R1 (Chain‑of‑Thought)

Untuk mengaktifkan mod penalaran R1, gunakan model `deepseek-r1` atau tambah parameter khas.

**Model R1:**
```json
{
  "model": "deepseek-r1",
  "messages": [...]
}
```

**Output R1 mengandungi tag `<think>`:**
```
<think>
Okay, let's analyze this step by step.
First, I need to find all package.json files...
The dependency graph shows that package A depends on B, B depends on C, and C depends on A. This is a cycle.
</think>

Analysis complete. I found one circular dependency cycle: A → B → C → A.

Here's the detailed breakdown...
```

**Parameter Tambahan (jika disokong):**
```json
{
  "model": "deepseek-v4-pro",
  "enable_chain_of_thought": true,
  "cot_mode": "hidden"  // "hidden" (default) atau "exposed" untuk mendedahkan <think>
}
```

> 📝 **Nota Tutorial:** Semua respons R1 mesti dihuraikan oleh parser pihak klien untuk memisahkan blok `<think>` daripada jawapan. Lihat `boilerplate/cot-agent.py` untuk contoh.

### 4.3 Cost Metrics Endpoint

**Endpoint:** `GET /v1/usage`

**Header:**
```
Authorization: Bearer {API_KEY}
```

**Parameter Query:**
- `start_date` – tarikh mula (YYYY-MM-DD)
- `end_date` – tarikh tamat (YYYY-MM-DD)

**Respons:**
```json
{
  "total_tokens": 12500000,
  "total_cost_usd": 0.045,
  "breakdown": [
    {
      "date": "2026-05-24",
      "model": "deepseek-v4-pro",
      "tokens": 2500000,
      "cost_usd": 0.009
    }
  ],
  "current_pricing": {
    "deepseek-v4-pro": {
      "input_per_1m_tokens": 0.015,
      "output_per_1m_tokens": 0.87,
      "cache_hit_per_1m_tokens": 0.0036,
      "discount": 0.75,
      "discount_description": "Permanent 75% discount effective after May 31, 2026"
    }
  }
}
```

---

## 5. Pengesahan & Keselamatan

| Platform | Mekanisme | Cara Simpan |
|----------|-----------|-------------|
| Google AI Studio | API Key dalam header `x-goog-api-key`, atau OAuth 2.0 untuk Google Workspace | Pembolehubah persekitaran: `GEMINI_API_KEY` |
| Anthropic Claude | API Key dalam header `x-api-key`, versi API dalam `anthropic-version` | Pembolehubah persekitaran: `ANTHROPIC_API_KEY` |
| DeepSeek | API Key dalam header `Authorization: Bearer {key}` | Pembolehubah persekitaran: `DEEPSEEK_API_KEY` |

**Amalan Terbaik Keselamatan:**
- **Jangan sesekali** commit API key ke repositori. Gunakan `.env` (yang disenaraikan dalam `.gitignore`).
- Gunakan **pembolehubah persekitaran** dalam pengeluaran.
- Untuk aplikasi web, lakukan panggilan API melalui **pelayan perantaraan (backend)** untuk melindungi kunci.
- Pantau penggunaan API secara berkala melalui papan pemuka pembekal.

---

## 6. Pengendalian Ralat

### Kod Status HTTP

| Kod | Makna | Tindakan |
|-----|-------|----------|
| `200` | Berjaya | – |
| `400` | Permintaan tidak sah | Semak parameter, format JSON |
| `401` | Tidak disahkan | Semak API key |
| `429` | Had kadar melebihi | Tunggu dan cuba semula (lihat header `Retry-After`) |
| `500` | Ralat pelayan | Cuba semula dengan `exponential backoff` |
| `503` | Perkhidmatan tidak tersedia | Tunggu, mungkin isu sementara |

### Contoh Respons Ralat (umum)

```json
{
  "error": {
    "message": "You exceeded your current quota. Please check your plan and billing details.",
    "type": "insufficient_quota",
    "code": 429
  }
}
```

**Strategi Cubaan Semula:**
```python
import time
import random

def call_with_retry(func, max_retries=5):
    for attempt in range(max_retries):
        try:
            return func()
        except RateLimitError:
            wait = (2 ** attempt) + random.uniform(0, 1)
            time.sleep(wait)
    raise Exception("Max retries exceeded")
```

---

## 📖 Seterusnya

- Lihat `RULES.md` untuk kekangan pengekodan spesifik platform.
- Untuk panduan praktikal langkah demi langkah, ikuti `USER_FLOW.md` dalam setiap akademi.
- Rujuk `ARCHITECTURE.md` untuk memahami aliran data di sebalik API ini.

---

_Dokumen ini diselenggara oleh komuniti State of Protocol.  
Sumbangan untuk menambah baik spesifikasi API dialu‑alukan – sila lihat [CONTRIBUTING.md](../.github/CONTRIBUTING.md)._
```

---

`API_SPEC.md` ini menyediakan spesifikasi endpoint dan skema data yang terperinci serta praktikal untuk ketiga‑tiga platform. Ia merangkumi contoh permintaan dan respons, parameter penting, serta panduan keselamatan dan pengendalian ralat yang diperlukan oleh pembangun untuk mengintegrasikan ejen ke dalam aplikasi mereka. Fail ini melengkapkan set dokumen spesifikasi yang telah kita bina bersama.
