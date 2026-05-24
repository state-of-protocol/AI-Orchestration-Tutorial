# 📁 STRUCTURE.md — Hierarki Fail & Organisasi Projek

**Panduan Struktur Repositori Orchestra AI-Agent Tutorial**  
_Versi 1.0 · Mei 2026_

Dokumen ini menjelaskan susun atur fail dan folder dalam repositori. Struktur ini direka untuk menyokong tiga "Akademi" bebas (Google AI Studio, Anthropic Claude, DeepSeek) sambil mengekalkan konsistensi melalui rangka kerja 7-fail yang seragam.

---

## 📖 Isi Kandungan

- [1. Gambaran Keseluruhan](#1-gambaran-keseluruhan)
- [2. Struktur Direktori Utama](#2-struktur-direktori-utama)
- [3. Penjelasan Setiap Direktori & Fail](#3-penjelasan-setiap-direktori--fail)
  - [3.1 `google-ai-studio/` – Akademi Google](#31-google-ai-studio--akademi-google)
  - [3.2 `anthropic-claude/` – Akademi Claude](#32-anthropic-claude--akademi-claude)
  - [3.3 `deepseek/` – Akademi DeepSeek](#33-deepseek--akademi-deepseek)
  - [3.4 `shared/` – Sumber Bersama](#34-shared--sumber-bersama)
  - [3.5 `.github/` – Konfigurasi Komuniti](#35-github--konfigurasi-komuniti)
  - [3.6 Fail Akar](#36-fail-akar)
- [4. Konvensyen Penamaan](#4-konvensyen-penamaan)
- [5. Amalan Terbaik](#5-amalan-terbaik)

---

## 1. Gambaran Keseluruhan

Repositori menggunakan struktur modular di mana setiap platform AI mempunyai subdirektori sendiri yang mengandungi **7 dokumen spesifikasi** (SKILL, DESIGN, STRUCTURE, ARCHITECTURE, API_SPEC, RULES, USER_FLOW) dan folder `boilerplate/` dengan kod permulaan. Sumber yang dikongsi (kalkulator kos, perbandingan) ditempatkan dalam direktori `shared/`, dan konfigurasi GitHub ditempatkan dalam `.github/`.

**Falsafah Reka Bentuk:**  
Pemisahan fizikal ini membolehkan pelajar mempelajari satu platform tanpa perlu memahami platform lain. Ia juga memudahkan penyelenggaraan kerana setiap akademi boleh dikemas kini secara bebas.

---

## 2. Struktur Direktori Utama

```
AI-Orchestration-Tutorial/
│
├── README.md                          # Halaman utama projek
├── LICENSE                            # Lesen MIT
├── SKILL.md                           # Spesifikasi teknikal induk (semua platform)
├── DESIGN.md                          # Prinsip UI/UX induk
├── STRUCTURE.md                       # Fail ini – penerangan hierarki
│
├── google-ai-studio/                  # 🏛️ Akademi 1: Google AI Studio
│   ├── SKILL.md
│   ├── DESIGN.md
│   ├── STRUCTURE.md
│   ├── ARCHITECTURE.md
│   ├── API_SPEC.md
│   ├── RULES.md
│   ├── USER_FLOW.md
│   └── boilerplate/
│       ├── agent.py                   # Ejen terurus (Managed Agent)
│       ├── webhook/                   # Contoh webhook untuk Cloud Functions
│       │   └── main.py
│       ├── skills/                    # Kemahiran ejen (AGENTS.md)
│       │   └── workspace-analyst.md
│       └── .antigravity/             # Konfigurasi persekitaran Antigravity
│           └── sandbox.yaml
│
├── anthropic-claude/                  # 🦉 Akademi 2: Anthropic Claude
│   ├── SKILL.md
│   ├── DESIGN.md
│   ├── STRUCTURE.md
│   ├── ARCHITECTURE.md
│   ├── API_SPEC.md
│   ├── RULES.md
│   ├── USER_FLOW.md
│   └── boilerplate/
│       ├── multi-agent/               # Sistem pasukan ejen
│       │   ├── coordinator.ts
│       │   ├── coder.ts
│       │   ├── reviewer.ts
│       │   └── index.ts               # Titik masuk
│       ├── mcp/                       # Pelayan MCP contoh
│       │   ├── filesystem-server/
│       │   │   ├── package.json
│       │   │   ├── tsconfig.json
│       │   │   └── src/
│       │   │       └── index.ts
│       │   └── database-server/
│       │       ├── package.json
│       │       └── src/
│       │           └── index.ts
│       ├── mcp-config.example.json   # Templat konfigurasi MCP
│       └── claude-code/              # Skrip Claude Code
│           └── .clauderc
│
├── deepseek/                          # 🐉 Akademi 3: DeepSeek
│   ├── SKILL.md
│   ├── DESIGN.md
│   ├── STRUCTURE.md
│   ├── ARCHITECTURE.md
│   ├── API_SPEC.md
│   ├── RULES.md
│   ├── USER_FLOW.md
│   └── boilerplate/
│       ├── cot-agent.py              # Ejen Rantaian Pemikiran (R1)
│       ├── engram-memory.py          # Lapisan memori Engram
│       ├── docker-compose.yml        # Redis + vLLM (opsional)
│       └── config/
│           └── deepseek.env.example  # Pembolehubah persekitaran
│
├── shared/                            # 🔧 Sumber dikongsi
│   ├── cost-calculator/              # Kalkulator kos token interaktif
│   │   ├── index.html
│   │   ├── style.css
│   │   └── calculator.js
│   ├── comparison/                    # Panduan perbandingan & matriks
│   │   ├── decision-guide.md
│   │   └── pricing-2026.csv
│   └── images/                        # Tangkapan skrin & grafik
│       ├── google-demo.png
│       ├── claude-artifacts.png
│       └── deepseek-cost.png
│
└── .github/                           # Konfigurasi GitHub
    ├── workflows/
    │   └── ci.yml                     # Ujian automatik boilerplate
    ├── CONTRIBUTING.md
    ├── PULL_REQUEST_TEMPLATE.md
    └── ISSUE_TEMPLATE/
        ├── bug_report.md
        └── feature_request.md
```

---

## 3. Penjelasan Setiap Direktori & Fail

### 3.1 `google-ai-studio/` – Akademi Google

**Tujuan:** Tutorial untuk platform Google AI Studio, memfokuskan kepada Managed Agents, Antigravity sandbox, dan integrasi Google Workspace.

| Fail/Direktori | Penerangan |
|----------------|------------|
| **7 fail spesifikasi** (`SKILL.md`, dll.) | Mengandungi panduan khusus platform mengikut rangka kerja standard. |
| `boilerplate/agent.py` | Contoh ejen terurus lengkap: satu panggilan API untuk menyediakan ejen dalam sandbox Linux. |
| `boilerplate/webhook/main.py` | Ejen webhook untuk Cloud Functions, menunjukkan pola *stateless agent*. |
| `boilerplate/skills/` | Fail `AGENTS.md` yang mentakrifkan kemahiran tersuai (contoh: penganalisis Google Workspace). |
| `boilerplate/.antigravity/sandbox.yaml` | Konfigurasi deklaratif persekitaran sandbox (pakej, pembolehubah, had sumber). |

**Ciri khas:** Direktori `.antigravity/` adalah wajib untuk projek yang dieksport dari AI Studio. Ia mengandungi metadata sandbox yang membolehkan pembinaan semula persekitaran tepat di tempatan atau CI/CD.

---

### 3.2 `anthropic-claude/` – Akademi Claude

**Tujuan:** Tutorial untuk platform Anthropic Claude, menekankan ketepatan, kawalan melalui MCP, dan orkestrasi multi-ejen.

| Fail/Direktori | Penerangan |
|----------------|------------|
| `boilerplate/multi-agent/` | Implementasi corak Koordinator → Coder → Reviewer menggunakan SDK Anthropic. |
| `boilerplate/multi-agent/coordinator.ts` | Ejen utama (Claude Opus) yang mengurai tugas dan mengagihkan kepada sub-ejen. |
| `boilerplate/multi-agent/coder.ts` | Sub-ejen pelaksana (Claude Sonnet). |
| `boilerplate/multi-agent/reviewer.ts` | Sub-ejen semakan kualiti. |
| `boilerplate/mcp/` | Pelayan MCP contoh: `filesystem-server` (baca/tulis fail tempatan) dan `database-server` (akses pangkalan data). |
| `boilerplate/mcp-config.example.json` | Templat konfigurasi MCP yang perlu disalin dan diedit dengan laluan/kredensial sebenar. |
| `boilerplate/claude-code/.clauderc` | Konfigurasi untuk mod ejen autonomi terminal (Claude Code). |

**Ciri khas:** Fail `mcp-config.json` (tidak disertakan, hanya `.example`) adalah kunci kepada keselamatan — ia menyenarai putih direktori yang boleh diakses ejen. Tutorial menekankan agar tidak mendedahkan kredensial sebenar dalam repositori.

---

### 3.3 `deepseek/` – Akademi DeepSeek

**Tujuan:** Tutorial untuk platform DeepSeek, memfokuskan kepada kecekapan kos, penakulan mendalam (R1), dan pemprosesan berskala besar.

| Fail/Direktori | Penerangan |
|----------------|------------|
| `boilerplate/cot-agent.py` | Ejen yang menggunakan model R1 dengan penghuraian tag `<think>` untuk mengekstrak jalan pemikiran. |
| `boilerplate/engram-memory.py` | Implementasi lapisan memori Engram menggunakan Redis sebagai cache, membolehkan konteks 1M token diurus dengan cekap. |
| `boilerplate/docker-compose.yml` | Konfigurasi Docker untuk menjalankan Redis dan (secara pilihan) vLLM/Ollama bagi model tempatan. |
| `boilerplate/config/deepseek.env.example` | Templat pembolehubah persekitaran (API key, URL endpoint, dll.). |

**Ciri khas:** Docker Compose membolehkan pelajar menjalankan keseluruhan tindanan secara setempat, mengasingkan persekitaran eksperimen tanpa menjejaskan sistem hos.

---

### 3.4 `shared/` – Sumber Bersama

| Fail/Direktori | Penerangan |
|----------------|------------|
| `cost-calculator/` | Kalkulator token interaktif (HTML/JS) untuk menganggar kos API merentas platform. |
| `comparison/decision-guide.md` | Panduan pemilihan platform berdasarkan keperluan projek (Jadual interaktif). |
| `comparison/pricing-2026.csv` | Data harga API terkini (Mei 2026) untuk rujukan programatik. |
| `images/` | Tangkapan skrin UI dan rajah seni bina yang digunakan dalam README dan dokumen spesifikasi. |

> Kalkulator kos boleh dibuka terus dalam pelayar tanpa pelayan, memudahkan percubaan sebelum pelajar memilih platform.

---

### 3.5 `.github/` – Konfigurasi Komuniti

| Fail/Direktori | Penerangan |
|----------------|------------|
| `workflows/ci.yml` | GitHub Actions untuk menguji kod boilerplate (linting, semakan pergantungan) pada setiap PR. |
| `CONTRIBUTING.md` | Panduan untuk penyumbang (cara mencadangkan perubahan, piawaian kod). |
| `PULL_REQUEST_TEMPLATE.md` | Templat untuk huraian PR. |
| `ISSUE_TEMPLATE/` | Templat isu untuk laporan pepijat dan permintaan ciri. |

---

### 3.6 Fail Akar

| Fail | Penerangan |
|------|------------|
| `README.md` | Halaman utama projek – pengenalan, matriks perbandingan, panduan mula pantas. |
| `LICENSE` | Lesen MIT. |
| `SKILL.md` | **Induk** spesifikasi teknikal yang menggabungkan keperluan semua platform. |
| `DESIGN.md` | **Induk** prinsip UI/UX untuk semua platform. |
| `STRUCTURE.md` | Fail ini – penerangan hierarki projek. |

> **Nota:** Dokumen `SKILL.md`, `DESIGN.md`, dan `STRUCTURE.md` di peringkat akar adalah versi gabungan untuk gambaran menyeluruh. Setiap akademi mempunyai salinan sendiri yang dioptimumkan untuk platform tersebut.

---

## 4. Konvensyen Penamaan

- **Direktori:** `kebab-case` (contoh: `google-ai-studio`, `cost-calculator`).
- **Fail Markdown:** `UPPERCASE.md` untuk dokumen spesifikasi utama (SKILL, DESIGN, dll.), `lowercase.md` untuk fail sampingan.
- **Fail Python/TypeScript:** `snake_case.py` atau `kebab-case.ts` mengikut kebiasaan bahasa.
- **Fail konfigurasi:** `.example` suffix untuk templat yang perlu disalin (contoh: `mcp-config.example.json`).

---

## 5. Amalan Terbaik

1. **Jangan commit rahsia:** API key, token, dan kredensial **tidak boleh** dimasukkan ke dalam repositori. Gunakan `.env.example` dan `.gitignore` yang sesuai.
2. **Gunakan `.gitignore` global:** Setiap subdirektori mungkin mempunyai fail `.gitignore` sendiri untuk mengelakkan folder `node_modules/`, `__pycache__/`, atau fail persekitaran maya Python.
3. **Dokumentasi dalam direktori:** Setiap direktori `boilerplate/` mempunyai fail `README.md` ringkas yang menerangkan cara menjalankan kod.
4. **Konsisten merentas akademi:** Walaupun setiap akademi bebas, struktur 7-fail dan penamaan direktori `boilerplate/` diseragamkan untuk memudahkan penyelenggaraan.

---

## 📖 Seterusnya

- Kembali ke [README.md](../README.md) untuk panduan mula pantas.
- Lihat `ARCHITECTURE.md` dalam setiap akademi untuk memahami aliran data di sebalik kod.

---

_Dokumen ini diselenggara oleh komuniti State of Protocol.  
Sumbangan untuk menambah baik struktur dialu-alukan — sila lihat [CONTRIBUTING.md](../.github/CONTRIBUTING.md)._
```

---

`STRUCTURE.md` ini menyediakan peta lengkap repositori, memastikan setiap penyumbang dan pelajar dapat menavigasi projek dengan mudah sambil mengekalkan konsistensi antara platform.
