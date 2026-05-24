<!-- ────────────────────────────────────────────────────────────── -->
<!--  Orchestra AI‑Agent Tutorial                                   -->
<!--  https://github.com/state-of-protocol/AI-Orchestration-Tutorial -->
<!-- ────────────────────────────────────────────────────────────── -->

<h1 align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://img.shields.io/badge/ORCHESTRA-AI--AGENT-white?style=for-the-badge&logo=github">
    <img alt="Orchestra AI-Agent" src="https://img.shields.io/badge/ORCHESTRA-AI--AGENT-black?style=for-the-badge&logo=github">
  </picture>
</h1>

<p align="center">
  <b>Build, Orchestrate & Deploy Autonomous AI Agents</b><br>
  <em>Hands‑on tutorials for Google Gemini · Anthropic Claude · DeepSeek</em>
</p>

<p align="center">
  <!-- PAUTAN BAHARU: Laman Web Interaktif -->
  <a href="https://state-of-protocol.github.io/orchestra-website/">
    <img src="https://img.shields.io/badge/🌐-Laman_Web_Interaktif-4285F4?style=for-the-badge" alt="Laman Web Interaktif">
  </a>
  <a href="https://github.com/state-of-protocol/AI-Orchestration-Tutorial/stargazers">
    <img src="https://img.shields.io/github/stars/state-of-protocol/AI-Orchestration-Tutorial?style=social" alt="GitHub Stars">
  </a>
  <a href="https://github.com/state-of-protocol/AI-Orchestration-Tutorial/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License: MIT">
  </a>
  <a href="https://github.com/state-of-protocol/AI-Orchestration-Tutorial/actions">
    <img src="https://img.shields.io/github/actions/workflow/status/state-of-protocol/AI-Orchestration-Tutorial/ci.yml?style=flat-square" alt="CI Status">
  </a>
  <a href="https://discord.gg/example">
    <img src="https://img.shields.io/badge/Discord-Join%20Community-5865F2?style=flat-square&logo=discord&logoColor=white" alt="Discord">
  </a>
</p>

---

## 📖 Table of Contents

- [What Is This?](#-what-is-this)
- [🌐 Interactive Website & Tools](#-interactive-website--tools)  <!-- BAHARU -->
- [Why Orchestra AI‑Agent?](#-why-orchestra-ai-agent)
- [Platform Comparison Matrix](#-platform-comparison-matrix)
- [Repository Structure](#-repository-structure)
- [Quick Start ― Pick Your Platform](#-quick-start--pick-your-platform)
  - [🐉 DeepSeek (Efficiency & Scale)](#-deepseek-efficiency--scale)
  - [🦉 Anthropic Claude (Precision & MCP Control)](#-anthropic-claude-precision--mcp-control)
  - [🏛️ Google AI Studio (Managed Infrastructure)](#-google-ai-studio-managed-infrastructure)
- [Learning Paths](#-learning-paths)
- [The 7‑File Documentation Framework](#-the-7-file-documentation-framework)
- [Prerequisites](#-prerequisites)
- [📄 Whitepaper](#-whitepaper)  <!-- BAHARU -->
- [Contributing](#-contributing)
- [Community](#-community)
- [License](#-license)

---

## 🎯 What Is This?

**Orchestra AI‑Agent** is a hands‑on, production‑grade tutorial repository that teaches you how to build, orchestrate, and deploy autonomous AI agents across the **three dominant agent platforms of 2026**:

| Platform | Core Model | Unique Strength |
| :--- | :--- | :--- |
| **Google AI Studio** | Gemini 3.5 Flash / Pro | Managed Agents + Antigravity sandbox — spin up a full Linux agent with **one API call** |
| **Anthropic Claude** | Claude Opus 4.7 / Sonnet 5 | Multi‑agent Dev Teams + MCP (Model Context Protocol) — coordinate agents like a senior engineering squad |
| **DeepSeek** | DeepSeek V4‑Pro / R1 | 1.6T‑param MoE with 1M context — **10× cheaper** than Western competitors at equal capability |

> **The problem:** Every platform has a fundamentally different architecture for agent orchestration. Google runs agents inside managed Linux sandboxes; Anthropic connects agents through the Model Context Protocol (MCP); DeepSeek relies on Mixture‑of‑Experts (MoE) routing and Engram conditional memory. Learning one platform does **not** teach you the others.
>
> **Our solution:** Three independent "mini‑academies" inside one repository — each with its own tech stack, design philosophy, architecture, API spec, coding rules, and phased user flow. Plus a companion **interactive website** with live demos, cost calculator, and Vibe Code Playground.

---

## 🌐 Interactive Website & Tools  <!-- BAHARU -->

**[🌐 Buka Laman Web Interaktif](https://state-of-protocol.github.io/orchestra-website/)**

Projek ini disertakan dengan **laman web interaktif penuh** yang dibina khas untuk memudahkan pembelajaran dan eksperimen:

| Komponen Laman Web | Tujuan | Pautan Terus |
|-------------------|--------|-------------|
| **🏠 Laman Utama** | Hab utama dengan perbandingan platform, demo penstriman ejen, dan penjejak kemajuan | [Laman Utama](https://state-of-protocol.github.io/orchestra-website/) |
| **🧮 Kalkulator Kos Token** | Anggarkan kos API serta-merta (USD & MYR) — bandingkan semua 6 model | [Kalkulator](https://state-of-protocol.github.io/orchestra-website/cost-calculator.html) |
| **🧪 Vibe Code Playground** | Tulis dan jalankan kod ejen terus dalam pelayar (Cloud & Local mode) | [Playground](https://state-of-protocol.github.io/orchestra-website/#vibe-code) |
| **🖥️ Demo Penstriman** | Simulasi langsung ejen "berfikir" dan memanggil alatan | [Demo](https://state-of-protocol.github.io/orchestra-website/#demo) |

> 💡 **Tip:** Guna kalkulator kos untuk menganggar bajet sebelum memilih platform. Kemudian, terus ke Vibe Code Playground untuk menguji kod boilerplate tanpa perlu memasang apa-apa!

---

## 🚀 Why Orchestra AI‑Agent?

- **Production‑grade from Day 1:** Every tutorial follows the same **7‑file specification framework** used by Senior Systems Engineers and Master Architects to prevent scope creep (`SKILL.md`, `DESIGN.md`, `STRUCTURE.md`, `ARCHITECTURE.md`, `API_SPEC.md`, `RULES.md`, `USER_FLOW.md`).
- **Copy‑paste ready:** Boilerplate code, MCP server configs, Docker Compose files, and `AGENTS.md` skill definitions included for every platform.
- **Cost‑aware:** Built‑in token calculators and pricing references help you budget before you build. Try the **[🧮 interactive cost calculator](https://state-of-protocol.github.io/orchestra-website/cost-calculator.html)** on the website.
- **Community‑driven:** Discord server + forum for cross‑platform orchestration discussions.

---

## 📊 Platform Comparison Matrix

| Feature | 🏛️ Google AI Studio | 🦉 Anthropic Claude | 🐉 DeepSeek |
| :--- | :--- | :--- | :--- |
| **Flagship Model (2026)** | Gemini 3.5 Flash / Pro | Claude Opus 4.7 / Sonnet 5 | DeepSeek V4‑Pro / R1 |
| **Context Window** | 1M tokens | 1M tokens (128K output) | 1M tokens |
| **Native Agent System** | Antigravity 2.0 + Managed Agents API | Dev Teams (Coordinator → Sub‑agents) + Claude Code | Engram Conditional Memory + Chain‑of‑Thought |
| **Orchestration Philosophy** | Managed sandbox — one API call provisions a Linux VM with code execution, web search, and file I/O | MCP (Model Context Protocol) — agents connect to local tools, databases, and repos through JSON‑RPC 2.0 | MoE routing (384 experts, 6 active per layer) — efficient at scale; `<think>` tags expose reasoning traces |
| **Key Protocol / SDK** | `@google/genai` V2, Antigravity CLI (`agy`) | `@anthropic-ai/sdk`, MCP SDK (TypeScript/Python/C#) | OpenAI‑compatible endpoint, Ollama / vLLM / SGLang |
| **Best For** | Multimodal apps, Google Workspace integration, rapid prototyping | Precision coding, multi‑agent software engineering, financial analysis | Massive‑scale automation, cost‑sensitive commercial agents, math/reasoning |
| **Pricing (per 1M tokens)** | ~$0.15 (input) / ~$0.60 (output) — Flash tier | ~$15 (Opus input) / ~$3 (Sonnet input) | **$0.0036 (input, cache hit) / $0.87 (output)** — V4‑Pro with permanent 75% discount |
| **Thinking/Reasoning** | Built into agent loop | Adaptive Thinking (model decides depth; 4 effort levels) + Interleaved Thinking between tool calls | Chain‑of‑Thought (R1) with exposed `<think>…</think>` reasoning traces |

> **Pricing note:** DeepSeek announced a permanent 75% price cut on V4‑Pro API effective after May 31, 2026 — making it **~10–50× cheaper** than Western alternatives for equivalent workloads.

---

## 📁 Repository Structure

```
AI-Orchestration-Tutorial/
├── README.md                          # ← You are here
│
├── google-ai-studio/                  # 🏛️ Academy 1: Managed Infrastructure
│   ├── SKILL.md
│   ├── DESIGN.md
│   ├── STRUCTURE.md
│   ├── ARCHITECTURE.md
│   ├── API_SPEC.md
│   ├── RULES.md
│   ├── USER_FLOW.md
│   └── boilerplate/
│       ├── agent.py
│       ├── .antigravity/
│       └── skills/
│
├── anthropic-claude/                  # 🦉 Academy 2: Precision & MCP
│   ├── SKILL.md
│   ├── DESIGN.md
│   ├── STRUCTURE.md
│   ├── ARCHITECTURE.md
│   ├── API_SPEC.md
│   ├── RULES.md
│   ├── USER_FLOW.md
│   └── boilerplate/
│       ├── mcp-config.example.json
│       ├── coordinator.ts
│       ├── coder.ts
│       └── reviewer.ts
│
├── deepseek/                          # 🐉 Academy 3: Efficiency & Scale
│   ├── SKILL.md
│   ├── DESIGN.md
│   ├── STRUCTURE.md
│   ├── ARCHITECTURE.md
│   ├── API_SPEC.md
│   ├── RULES.md
│   ├── USER_FLOW.md
│   └── boilerplate/
│       ├── docker-compose.yml
│       ├── engram_hash.py
│       └── deepseek_r1.py
│
├── WHITEPAPER.md                      # 📄 Full project architecture & philosophy
│
├── shared/
│   ├── cost-calculator/               # Standalone token cost calculator
│   └── comparison/                    # Cross‑platform benchmarks
│
└── .github/
    ├── CONTRIBUTING.md
    └── PULL_REQUEST_TEMPLATE.md
```

---

## ⚡ Quick Start ― Pick Your Platform

### 🐉 DeepSeek (Efficiency & Scale)

```bash
# 1. Clone the repo
git clone https://github.com/state-of-protocol/AI-Orchestration-Tutorial.git
cd AI-Orchestration-Tutorial/deepseek

# 2. Set your API key (OpenAI‑compatible endpoint)
export DEEPSEEK_API_KEY="sk-your-key-here"

# 3. Install dependencies
pip install openai redis llama-index

# 4. Run your first CoT agent
python boilerplate/deepseek_r1.py \
  --prompt "Analyze this 50,000-line monorepo for circular dependencies"

# Expected output:
# <think>First, I'll scan package.json files... The dependency graph shows...</think>
# ✅ Analysis complete. Found 3 circular dependencies. Report saved to deps-report.json.
```

> 💡 **Cost:** This analysis processes ~200K tokens. With DeepSeek V4‑Pro at $0.0036/M input tokens (cache hit), the total cost is **less than $0.01**.  
> 🧮 **Try our [interactive cost calculator](https://state-of-protocol.github.io/orchestra-website/cost-calculator.html)** to estimate your own costs.

---

### 🦉 Anthropic Claude (Precision & MCP Control)

```bash
# 1. Navigate to the Claude academy
cd AI-Orchestration-Tutorial/anthropic-claude

# 2. Install Anthropic SDK + MCP SDK
npm install @anthropic-ai/sdk @modelcontextprotocol/sdk
# or: pip install anthropic mcp

# 3. Configure your MCP server
cp boilerplate/mcp-config.example.json mcp-config.json
# Edit mcp-config.json with your database credentials and allowed paths

# 4. Start the multi‑agent Dev Team
npm run dev-team -- --task "Refactor the authentication module to use JWT instead of sessions"
```

> 🔐 **Safety:** The `mcp-config.json` enforces a whitelist of directories the agent can modify.

---

### 🏛️ Google AI Studio (Managed Infrastructure)

```bash
# 1. Navigate to the Google academy
cd AI-Orchestration-Tutorial/google-ai-studio

# 2. Get your free API key at https://aistudio.google.com/apikey
export GEMINI_API_KEY="your-key-here"

# 3. Install the Google GenAI SDK
pip install google-genai

# 4. Run a Managed Agent — one API call, full Linux sandbox
python boilerplate/agent.py
```

```python
# The entire agent in 4 lines
from google import genai

client = genai.Client()
interaction = client.interactions.create(
    agent="antigravity-preview-05-2026",
    input="Analyze this CSV, find the top 5 trends, and generate a visualization as chart.png.",
    environment="remote",
)
print(interaction.output_text)
```

---

## 🗺️ Learning Paths

Every platform academy follows an identical **three‑phase progression**:

| Phase | What You Build | Google AI Studio | Anthropic Claude | DeepSeek |
| :---: | :--- | :--- | :--- | :--- |
| **1** | Basic API integration | Gemini Flash chat | Single Claude call | OpenAI‑compatible endpoint |
| **2** | Tool calling | Google Workspace extensions | MCP Server + database | Engram memory layer |
| **3** | Full agent orchestration | Managed Agent + sandbox | Multi‑agent Dev Team | Batch processing at scale |

> 🧭 **Not sure where to start?** Use our **[interactive comparison matrix](https://state-of-protocol.github.io/orchestra-website/#comparison)** on the website to find the best platform for your project.

---

## 📐 The 7‑File Documentation Framework

| # | File | Purpose |
| :---: | :--- | :--- |
| 1 | **`SKILL.md`** | Tech stack, SDKs, runtime requirements |
| 2 | **`DESIGN.md`** | UI/UX principles, streaming patterns |
| 3 | **`STRUCTURE.md`** | File hierarchy, folder conventions |
| 4 | **`ARCHITECTURE.md`** | Data flow diagrams, agent loops |
| 5 | **`API_SPEC.md`** | Endpoints, request/response schemas |
| 6 | **`RULES.md`** | Coding constraints, safety thresholds |
| 7 | **`USER_FLOW.md`** | Phased development roadmap |

---

## 📋 Prerequisites

| Requirement | Google AI Studio | Anthropic Claude | DeepSeek |
| :--- | :---: | :---: | :---: |
| **API Key** | [Google AI Studio](https://aistudio.google.com/apikey) | [Anthropic Console](https://console.anthropic.com/) | [DeepSeek Platform](https://platform.deepseek.com/) |
| **Runtime** | Python 3.11+ or Node.js 22+ | TypeScript / Node.js 18+ | Python 3.10+ |
| **Key SDK** | `google-genai` | `@anthropic-ai/sdk` + MCP SDK | `openai` |

---

## 📄 Whitepaper

Untuk pemahaman yang lebih mendalam tentang seni bina projek, falsafah reka bentuk, dan peta rujukan silang antara laman web dan dokumentasi, baca **[WHITEPAPER.md](./WHITEPAPER.md)**.

---

## 🤝 Contributing

See [`CONTRIBUTING.md`](.github/CONTRIBUTING.md) for detailed guidelines.

---

## 💬 Community

- **[Discord](https://discord.gg/example)** — Live discussions
- **[GitHub Discussions](https://github.com/state-of-protocol/AI-Orchestration-Tutorial/discussions)** — Q&A

---

## 📄 License

MIT License — see [`LICENSE`](LICENSE).

---

<p align="center">
  <sub>Built with ❤️ by the <a href="https://github.com/state-of-protocol">State of Protocol</a> community.<br>
  <em>Empowering the next generation of AI engineers in Malaysia, Southeast Asia, and beyond.</em></sub>
</p>
```

---
