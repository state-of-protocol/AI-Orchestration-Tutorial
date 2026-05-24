# 🏗️ ARCHITECTURE.md — Logik & Aliran Data Ejen

**Seni Bina Dalaman Platform Ejen Autonomi**  
_Versi 1.0 · Mei 2026_

Dokumen ini menghuraikan **seni bina perisian dan aliran data** bagi ketiga‑tiga platform yang diajar di dalam repositori ini: Google AI Studio (Managed Agents & Antigravity), Anthropic Claude (MCP & Multi‑Agent), dan DeepSeek (MoE & Engram Memory). Ia disasarkan kepada pembangun yang ingin memahami mekanisme dalaman sebelum mengimplementasikan atau mengubah suai kod boilerplate.

---

## 📖 Isi Kandungan

- [1. Pengenalan: Tiga Falsafah Seni Bina Ejen](#1-pengenalan-tiga-falsafah-seni-bina-ejen)
- [2. Google AI Studio – The Managed Sandbox Architect](#2-google-ai-studio--the-managed-sandbox-architect)
  - [2.1 Komponen Utama](#21-komponen-utama)
  - [2.2 Aliran Data](#22-aliran-data)
  - [2.3 Rajah Aliran](#23-rajah-aliran)
- [3. Anthropic Claude – The Precision Orchestrator](#3-anthropic-claude--the-precision-orchestrator)
  - [3.1 Model Context Protocol (MCP)](#31-model-context-protocol-mcp)
  - [3.2 Seni Bina Multi‑Ejen](#32-seni-bina-multi-ejen)
  - [3.3 Aliran Data](#33-aliran-data)
  - [3.4 Rajah Aliran](#34-rajah-aliran)
- [4. DeepSeek – The High‑Efficiency MoE Engine](#4-deepseek--the-high-efficiency-moe-engine)
  - [4.1 Mixture‑of‑Experts (MoE)](#41-mixture-of-experts-moe)
  - [4.2 Sistem Memori Engram](#42-sistem-memori-engram)
  - [4.3 Rantaian Pemikiran (R1)](#43-rantaian-pemikiran-r1)
  - [4.4 Aliran Data](#44-aliran-data)
  - [4.5 Rajah Aliran](#45-rajah-aliran)
- [5. Perbandingan Seni Bina](#5-perbandingan-seni-bina)
- [6. Pola Orkestrasi Rentas Platform (Konseptual)](#6-pola-orkestrasi-rentas-platform-konseptual)

---

## 1. Pengenalan: Tiga Falsafah Seni Bina Ejen

Walaupun ketiga‑tiga platform boleh digunakan untuk membina ejen autonomi, setiap satu mempunyai pendekatan seni bina yang berbeza secara asas. Perbezaan ini timbul daripada keperluan infrastruktur, model perniagaan, dan penyelidikan asas syarikat masing‑masing.

| Platform | Falsafah Seni Bina | Ciri Pembeza |
|----------|-------------------|--------------|
| **Google AI Studio** | **Infrastruktur Terurus** – Ejen berjalan dalam persekitaran Linux terpencil yang disediakan oleh Google. Pembangun hanya menentukan tingkah laku, bukan infrastruktur. | Pengurusan siklus hayat sandbox automatik, integrasi Google Workspace, penstriman multimodal asli. |
| **Anthropic Claude** | **Protokol Kawalan** – Ejen berinteraksi dengan dunia luar melalui standard terbuka (MCP). Logik agen adalah ketat, terurus, dan boleh diaudit. | Koordinasi multi‑ejen dengan pengkhususan tugas, sambungan piawai ke sumber tempatan. |
| **DeepSeek** | **Pengoptimuman Kos & Skala** – Seni bina dalaman model (MoE) dan lapisan memori (Engram) direka untuk memproses jumlah token yang sangat besar pada kos yang sangat rendah. | Pemprosesan berskala perusahaan pada harga pengguna, pendedahan penuh rantaian pemikiran. |

> Memahami falsafah ini adalah kunci untuk memilih platform yang tepat dan mereka bentuk aplikasi yang berkesan.

---

## 2. Google AI Studio – The Managed Sandbox Architect

### 2.1 Komponen Utama

- **Gemini API (Model Bahasa)** – Model multimodal (Gemini 3.5 Flash/Pro) yang memproses teks, imej, audio, dan video.
- **Antigravity Interactions API** – Lapisan orkestrasi yang menguruskan penciptaan, penyelenggaraan, dan penamatan persekitaran Linux terpencil.
- **Managed Sandbox** – Mesin maya Linux sementara (berasaskan gVisor & Firecracker) yang disediakan secara automatik. Mengandungi sistem fail, akses internet terhad, dan runtime Python/Node.js.
- **Function Calling** – Mekanisme standard untuk ejen memanggil alatan luaran (API Google Workspace, perkhidmatan pihak ketiga).
- **Context Caching** – Cache sisi pelayan untuk permintaan besar (>100k token) bagi mengurangkan kos dan latensi.

### 2.2 Aliran Data

1. **Pengguna menghantar input** (teks + fail media) melalui SDK `@google/genai`.
2. **Pra‑pemprosesan**: Fail media dimuat naik ke Cloud Storage; rujukan URI dihantar bersama prompt.
3. **Semakan Cache**: SDK memeriksa sama ada kandungan prompt (terutama fail besar) telah di-cache. Jika ya, cache digunakan untuk mengelakkan pemprosesan semula yang mahal.
4. **Model Gemini memproses** permintaan dan mungkin menghasilkan *function call* (contoh: `run_code`, `search_web`, `read_gmail`).
5. **Pengurus Interaksi Antigravity** menerima function call dan memutuskan pelaksanaan:
   - Jika kod Python/Shell: dilaksanakan dalam sandbox.
   - Jika panggilan API Google: diarahkan melalui lapisan OAuth yang telah disahkan.
6. **Hasil pelaksanaan** dikembalikan kepada model Gemini sebagai konteks tambahan.
7. **Model menjana respons akhir** (teks atau artifak seperti imej/CSV) yang distrim ke klien secara langsung.
8. **Keadaan sandbox dikekalkan** untuk panggilan seterusnya dalam sesi yang sama. Pengguna boleh merujuk fail yang dijana sebelum ini.

### 2.3 Rajah Aliran

```
Pengguna
  │
  ├─(teks + fail)─► @google/genai SDK
  │                   │
  │                   ├─► Context Cache Manager (periksa cache)
  │                   │
  │                   └─► Gemini API (model multimodal)
  │                          │
  │                          ├─► Function Calling (tool decision)
  │                          │     │
  │                          │     └─► Antigravity Interaction Manager
  │                          │            │
  │                          │            ├─► Linux Sandbox (gVisor/Firecracker)
  │                          │            │     │
  │                          │            │     └─► Executes Python/Node.js,
  │                          │            │         writes files, returns stdout
  │                          │            │
  │                          │            └─► Google Workspace Extensions
  │                          │                  (Gmail, Docs, Sheets)
  │                          │
  │                          └─► Streams response tokens
  │                                │
  │                                └─► UI menerima token & mengemas kini
  │                                     paparan secara masa nyata
  │
  └─► Respons akhir + pautan ke fail output (jika ada)
```

**Ciri Keselamatan:** Sandbox diasingkan sepenuhnya. Tiada akses rangkaian keluar kecuali ke perkhidmatan Google yang dibenarkan. Sesi tamat selepas tempoh tidak aktif, dan semua data dimusnahkan.

---

## 3. Anthropic Claude – The Precision Orchestrator

### 3.1 Model Context Protocol (MCP)

MCP ialah protokol piawai (JSON‑RPC 2.0) yang membolehkan model Claude berinteraksi dengan **pelayan MCP** – proses tempatan atau jauh yang menyediakan akses kepada alatan dan sumber.

**Komponen MCP:**
- **MCP Client:** Diintegrasikan dalam aplikasi hos (contoh: aplikasi Node.js, Claude Code). Menghantar permintaan dan menerima respons.
- **MCP Server:** Proses bebas yang mendedahkan keupayaan tertentu (contoh: fail sistem, pangkalan data, API web). Setiap pelayan mempunyai konfigurasi keizinan.
- **Transport:** Biasanya melalui `stdio` (tempatan) atau HTTP/SSE (jauh). Tutorial menggunakan `stdio` untuk keselamatan maksimum.

**Aliran MCP:**
1. Aplikasi hos memulakan pelayan MCP sebagai proses anak.
2. Claude diberi senarai alat yang tersedia melalui *system prompt* yang dijana daripada definisi pelayan MCP.
3. Apabila Claude memutuskan untuk menggunakan alat, ia mengeluarkan *tool use content block*.
4. Aplikasi hos memajukan permintaan alat ke pelayan MCP yang sesuai.
5. Hasil dikembalikan kepada Claude sebagai konteks tambahan.

### 3.2 Seni Bina Multi‑Ejen

Claude boleh diorkestrasi sebagai **pasukan pembangun perisian** mini, menggunakan model Opus sebagai penyelaras dan beberapa contoh Sonnet sebagai pekerja khusus.

**Peranan:**
- **Coordinator (Claude Opus 4.7):** Menganalisis tugasan pengguna, memecahkannya kepada sub‑tugasan, mengagihkan kepada sub‑ejen, menyemak kemajuan, dan menggabungkan hasil.
- **Coder Sub‑agent (Claude Sonnet 5):** Melaksanakan sub‑tugasan pengekodan (menulis, mengubah suai, atau membaik pulih kod) menggunakan akses fail melalui MCP.
- **Reviewer Sub‑agent (Claude Sonnet 5):** Menyemak output Coder, menjalankan ujian (jika dikonfigurasi), dan memberikan kelulusan atau maklum balas.

### 3.3 Aliran Data

1. **Pengguna mengemukakan tugasan** (contoh: “Refaktor modul pengesahan ke JWT”).
2. **Coordinator (Opus)** menerima tugasan dan mengaktifkan *Extended Thinking* (jika diaktifkan, dengan bajet token).
3. **Coordinator menghasilkan pelan kerja** dalam bentuk senarai sub‑tugasan (contoh: “1. Analisis kebergantungan, 2. Tulis semula middleware JWT, 3. Kemas kini ujian”).
4. Untuk setiap sub‑tugasan:
   - **Coordinator memanggil Coder** (melalui API berasingan atau dengan menukar peranan dalam konteks) dengan arahan spesifik.
   - **Coder menggunakan MCP** untuk membaca fail yang berkaitan, menulis perubahan, dan melaporkan kemajuan.
   - **Reviewer** (boleh berjalan selari atau selepas Coder) menyemak perubahan, menjalankan ujian, dan memberikan `approved` atau `changes_requested`.
   - Jika `changes_requested`, kitaran semakan diulang sehingga diluluskan.
5. **Coordinator menggabungkan** semua perubahan yang diluluskan dan menyediakan ringkasan akhir kepada pengguna.

### 3.4 Rajah Aliran

```
Pengguna ──► Coordinator (Claude Opus)
                 │
                 ├─(Extended Thinking)─► Pelan Kerja
                 │
                 ├─► Sub‑tugasan 1 ──► Coder (Claude Sonnet)
                 │       │                  │
                 │       │                  └─► MCP Server (filesystem)
                 │       │                         │
                 │       │                         └─► Baca/Tulis fail
                 │       │
                 │       └─► Reviewer (Claude Sonnet)
                 │              │
                 │              ├─► MCP Server (test runner)
                 │              │
                 │              └─► Keputusan: approve / changes_requested
                 │
                 ├─► Sub‑tugasan 2 (sama seperti di atas)
                 │
                 └─► Gabungan hasil → Respons akhir kepada Pengguna
```

**Nota Pelaksanaan:** Dalam kod boilerplate, komunikasi antara ejen dilakukan melalui panggilan API Claude yang berasingan, menggunakan mesej sistem yang berbeza untuk menetapkan peranan masing‑masing. Status dan output sub‑ejen disimpan dalam struktur data sementara di aplikasi hos.

---

## 4. DeepSeek – The High‑Efficiency MoE Engine

### 4.1 Mixture‑of‑Experts (MoE)

DeepSeek V4‑Pro menggunakan seni bina MoE dengan **384 pakar** (expert), di mana hanya **6 pakar aktif** bagi setiap token. Ini membolehkan model mempunyai jumlah parameter yang besar (1.6 trilion) sambil mengekalkan kos inferens yang rendah.

**Cara Ia Berfungsi:**
- **Router (Gating Network):** Bagi setiap token input, rangkaian penghala memilih 6 pakar yang paling sesuai daripada 384 berdasarkan kandungan token.
- **Pakar:** Setiap pakar ialah sub‑rantaian neural yang dikhususkan kepada jenis pengetahuan tertentu (contoh: kod Python, matematik, bahasa Melayu).
- **Penggabungan:** Output daripada 6 pakar digabungkan secara linear untuk menghasilkan representasi token yang diproses.
- **Kecekapan:** Hanya parameter pakar aktif (~36B) yang digunakan untuk setiap token, bukan keseluruhan 1.6T parameter. Ini menjimatkan VRAM dan mempercepatkan inferens.

### 4.2 Sistem Memori Engram

Untuk menyokong tetingkap konteks 1 juta token tanpa membebankan GPU, DeepSeek menggunakan **Engram Conditional Memory**.

**Mekanisme:**
- **Storan Hash:** Konteks lampau disimpan dalam jadual hash di luar GPU (dalam RAM atau Redis), diindeks oleh vektor kandungan.
- **Carian Semula:** Semasa penjanaan, model mengeluarkan pertanyaan untuk mendapatkan semula konteks relevan daripada jadual hash berdasarkan persamaan semantik.
- **Cache Luaran:** Tutorial menggunakan Redis sebagai lapisan Engram untuk menyimpan perbualan atau dokumen besar, yang boleh dirujuk oleh model tanpa perlu memproses semula semua token.

### 4.3 Rantaian Pemikiran (R1)

Model DeepSeek‑R1 dilatih khas untuk penakulan mendalam menggunakan pengukuhan pembelajaran (RL). Ia menghasilkan **rantaian pemikiran (CoT)** sebelum memberikan jawapan akhir.

**Ciri Penting:**
- **Output Beranotasi:** R1 membungkus pemikiran dalam tag `<think>...</think>`.
- **Pengekstrakan:** Aplikasi mesti menghuraikan tag ini untuk memisahkan pemikiran daripada jawapan akhir.
- **Paparan UI:** Tutorial mengesyorkan memaparkan pemikiran dalam komponen yang boleh diruntuhkan (lihat `DESIGN.md`).

### 4.4 Aliran Data

1. **Input pengguna** dihantar melalui klien serasi OpenAI ke endpoint DeepSeek.
2. **Pemeriksaan Cache:** Sistem memeriksa Redis untuk melihat jika terdapat konteks tersimpan (Engram Memory) yang relevan. Jika ya, ia diambil dan ditambah sebagai awalan konteks.
3. **Penghalaan Model:**
   - Jika tugas adalah perbualan biasa, gunakan V4‑Pro (MoE).
   - Jika tugas memerlukan penakulan kompleks (matematik, logik, kod tegar), gunakan R1 (atau V4‑Pro dengan flag CoT diaktifkan).
4. **Pemprosesan MoE:** Setiap token dihalakan kepada 6 pakar yang sesuai, menghasilkan output langkah demi langkah.
5. **Penghuraian CoT (jika R1):** Output distrim ke klien. Parser di sisi klien mengesan tag `<think>` dan mengasingkan teks.
6. **Kemas Kini Memori:** Selepas respons, konteks penting disimpan ke Redis menggunakan hash Engram untuk rujukan masa depan.
7. **Paparan Output:** Pemikiran (jika ada) dan jawapan akhir dipaparkan dalam UI dengan widget kos yang dikemas kini.

### 4.5 Rajah Aliran

```
Input Pengguna
     │
     ├─► Redis Cache (Engram Memory)
     │     │
     │     └─► Konteks lampau diambil (jika ada)
     │
     ├─► Penghalaan Tugas
     │     │
     │     ├─► DeepSeek V4‑Pro (MoE)
     │     │     │
     │     │     └─► Router pilih 6/384 pakar per token
     │     │
     │     └─► DeepSeek R1 (penalaran)
     │           │
     │           └─► Keluaran <think>...</think> + jawapan
     │
     ├─► Penghurai CoT (sisi klien)
     │     │
     │     ├─► Ekstrak teks antara <think> → papar sebagai "Jalan Fikiran"
     │     └─► Baki teks → papar sebagai "Jawapan"
     │
     ├─► Kemas Kini Kos (widget)
     │     │
     │     └─► Papar token terpakai, anggaran RM/USD
     │
     └─► Respons Akhir kepada Pengguna
```

**Nota Kecekapan:** Oleh kerana kos input V4‑Pro sangat rendah (terutama dengan cache hit), keseluruhan aliran boleh memproses jutaan token dengan kos kurang daripada RM 0.50. Ini membolehkan analisis pangkalan kod besar atau korpus dokumen korporat secara ekonomi.

---

## 5. Perbandingan Seni Bina

| Dimensi | Google AI Studio | Anthropic Claude | DeepSeek |
|---------|------------------|------------------|----------|
| **Model Orkestrasi** | Terpusat – API menguruskan segala‑galanya | Teragih – Penyelaras + Sub‑ejen melalui MCP | Model‑sisi – MoE + Cache Luaran |
| **Lokasi Pelaksanaan Alatan** | Dalam sandbox terurus (infra Google) | Dalam pelayan MCP tempatan (infra pengguna) | Tiada – model hanya menjana teks; alat luaran perlu diurus oleh aplikasi hos |
| **Pengurusan Keadaan** | Keadaan sandbox dikekalkan merentas panggilan | Keadaan diurus oleh aplikasi hos & MCP | Keadaan luaran melalui Redis/Engram |
| **Kawalan Pemikiran Ejen** | Terhad – Gemini tidak mendedahkan CoT dalaman | Penuh – Extended Thinking boleh dikembangkan dengan `budget_tokens` | Penuh – R1 mendedahkan `<think>` secara telus |
| **Keselamatan** | Terasing sepenuhnya (gVisor/Firecracker) | Bergantung pada konfigurasi keizinan MCP (`mcp-config.json`) | Bergantung pada pengurusan cache dan sanitasi output |
| **Skala Kos** | Sederhana (Flash sangat murah) | Tinggi (Opus mahal, Sonnet sederhana) | Sangat rendah (V4‑Pro 75% diskaun) |

---

## 6. Pola Orkestrasi Rentas Platform (Konseptual)

Walaupun tutorial ini memisahkan platform kepada tiga akademi bebas, secara konseptual adalah mungkin untuk mengorkestrasi **ejen silang platform**. Contohnya:

- Gunakan **DeepSeek V4‑Pro** untuk pra‑pemprosesan dokumen besar (kos rendah, konteks besar).
- Hantar ringkasan kepada **Claude Opus** untuk penakulan kompleks dan penjanaan kod.
- Gunakan **Google Gemini Flash** untuk respons pantas kepada pengguna akhir atau integrasi multimodal (contoh: analisis video).

Pola seperti ini memerlukan **orkestrator pusat** (boleh dibina dengan LangGraph, kustom Python, dll.) yang menghalakan sub‑tugas kepada platform paling sesuai.

> Tutorial ini tidak merangkumi orkestrasi rentas platform secara langsung, tetapi menyediakan asas yang kukuh untuk setiap platform secara individu supaya pembangun boleh menggabungkannya sendiri.

---

## 📖 Seterusnya

- Untuk spesifikasi API terperinci setiap platform, rujuk `API_SPEC.md` dalam direktori akademi masing‑masing.
- Untuk peraturan pengekodan dan kekangan platform, lihat `RULES.md`.
- Untuk panduan langkah demi langkah membina ejen, ikuti `USER_FLOW.md`.

---

_Dokumen ini diselenggara oleh komuniti State of Protocol.  
Sumbangan untuk menambah baik rajah atau penjelasan dialu‑alukan – sila lihat [CONTRIBUTING.md](../.github/CONTRIBUTING.md)._
```

---

`ARCHITECTURE.md` ini kini menyediakan pandangan mendalam tentang kerja dalaman setiap platform, melengkapi dokumen `SKILL.md` dan `DESIGN.md` yang telah kita bina sebelum ini. Ia boleh diletakkan di akar repositori sebagai rujukan seni bina utama, atau disalin ke dalam setiap subdirektori akademi dengan pelarasan kecil.
