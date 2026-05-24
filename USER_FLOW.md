# 🧭 USER_FLOW.md — Aliran Pembelajaran & Pembangunan Ejen

**Panduan Berperingkat untuk Menguasai Setiap Platform Ejen**  
_Versi 1.0 · Mei 2026_

Dokumen ini menyediakan laluan pembelajaran yang jelas dan progresif untuk ketiga‑tiga platform dalam Orchestra AI‑Agent Tutorial. Setiap trek dibahagikan kepada tiga fasa: **Asas → Integrasi Alatan → Autonomi Penuh**. Ikuti langkah demi langkah untuk membina ejen yang berfungsi, selamat, dan cekap kos.

---

## 📖 Isi Kandungan

- [1. Sebelum Bermula](#1-sebelum-bermula)
- [2. Pilih Trek Anda](#2-pilih-trek-anda)
- [3. Trek Google AI Studio](#3-trek-google-ai-studio)
  - [Fasa 1G: Integrasi Asas & System Instructions](#fasa-1g-integrasi-asas--system-instructions)
  - [Fasa 2G: Function Calling & Google Workspace](#fasa-2g-function-calling--google-workspace)
  - [Fasa 3G: Managed Agents & Antigravity Automasi Penuh](#fasa-3g-managed-agents--antigravity-automasi-penuh)
- [4. Trek Anthropic Claude](#4-trek-anthropic-claude)
  - [Fasa 1C: Mesej Asas & Adaptive Thinking](#fasa-1c-mesej-asas--adaptive-thinking)
  - [Fasa 2C: MCP Server & Alatan Tempatan](#fasa-2c-mcp-server--alatan-tempatan)
  - [Fasa 3C: Pasukan Ejen (Dev Team)](#fasa-3c-pasukan-ejen-dev-team)
- [5. Trek DeepSeek](#5-trek-deepseek)
  - [Fasa 1D: API Serasi OpenAI & Parser CoT](#fasa-1d-api-serasi-openai--parser-cot)
  - [Fasa 2D: Memori Engram & Redis](#fasa-2d-memori-engram--redis)
  - [Fasa 3D: Automasi Skala Besar dengan Kos Ultra Rendah](#fasa-3d-automasi-skala-besar-dengan-kos-ultra-rendah)
- [6. Selepas Tamat – Ke Mana Seterusnya?](#6-selepas-tamat--ke-mana-seterusnya)

---

## 1. Sebelum Bermula

Pastikan anda telah menyediakan persekitaran pembangunan seperti yang dijelaskan dalam [SKILL.md](./SKILL.md). Secara ringkas:

- **API key** untuk platform pilihan anda telah diperoleh dan disimpan sebagai pembolehubah persekitaran.
- Runtime yang betul telah dipasang (Python 3.10+ atau Node.js 18+).
- Repositori ini telah diklon (`git clone ...`) dan anda berada dalam direktori yang betul.

> 💡 **Tip:** Jalankan skrip ujian persediaan dalam [SKILL.md](./SKILL.md#✅-semakan-persediaan) untuk mengesahkan semuanya siap.

---

## 2. Pilih Trek Anda

Tidak pasti platform mana yang sesuai? Kembali ke [Matriks Perbandingan](../README.md#📊-platform-comparison-matrix) di README atau gunakan [kalkulator kos interaktif](https://state-of-protocol.github.io/orchestra-website/cost-calculator.html) untuk membuat keputusan. Anda boleh mengikuti trek secara berasingan; tidak perlu mempelajari ketiga‑tiga sekali gus.

---

## 3. Trek Google AI Studio

**Objektif Keseluruhan:** Bina ejen yang berjalan di dalam sandbox Linux terurus, mengakses Google Workspace, dan menyelesaikan tugasan multimodal autonomi.

### Fasa 1G: Integrasi Asas & System Instructions

🎯 **Matlamat:** Hantar permintaan pertama ke Gemini API, fahami penstriman, dan gunakan arahan sistem untuk membentuk tingkah laku ejen.

📂 **Fail Contoh:** `google-ai-studio/boilerplate/agent.py` (versi asas)

🪜 **Langkah-langkah:**

1. Pasang SDK Google GenAI:
   ```bash
   pip install google-genai
   ```

2. Tetapkan pembolehubah persekitaran:
   ```bash
   export GEMINI_API_KEY="your-key-here"
   ```

3. Tulis skrip Python ringkas untuk panggilan pertama:
   ```python
   from google import genai

   client = genai.Client()
   response = client.models.generate_content(
       model="gemini-3.5-flash",
       contents="Apa itu ejen AI? Terangkan dalam 3 ayat."
   )
   print(response.text)
   ```
   Jalankan dan perhatikan output.

4. Tambah *system instruction* untuk menyesuaikan personaliti ejen:
   ```python
   response = client.models.generate_content(
       model="gemini-3.5-flash",
       config=genai.types.GenerateContentConfig(
           system_instruction="Anda adalah tutor AI yang ramah dan sabar. Gunakan analogi mudah."
       ),
       contents="Apa itu ejen AI?"
   )
   ```

5. Aktifkan penstriman untuk melihat token keluar secara langsung:
   ```python
   for chunk in client.models.generate_content_stream(
       model="gemini-3.5-flash",
       contents="Tulis puisi pendek tentang robot."
   ):
       print(chunk.text, end="", flush=True)
   ```

✅ **Tanda Kejayaan:** Anda boleh menghantar mesej, melihat respons distrim, dan ejen mematuhi arahan sistem anda.

🔗 **Rujukan:** [API_SPEC.md – Gemini generateContent](./API_SPEC.md#21-endpoint-utama) | [RULES.md – Safety Settings](./RULES.md#32-safety-settings)

---

### Fasa 2G: Function Calling & Google Workspace

🎯 **Matlamat:** Sambungkan ejen ke alatan luaran – bermula dengan carian web, kemudian integrasikan Gmail dan Google Sheets.

📂 **Fail Contoh:** `google-ai-studio/boilerplate/webhook/main.py`

🪜 **Langkah-langkah:**

1. **Dayakan carian web sebagai alat:**
   ```python
   response = client.models.generate_content(
       model="gemini-3.5-flash",
       config=genai.types.GenerateContentConfig(
           tools=[{"google_search": {}}]
       ),
       contents="Berita terkini tentang AI di Malaysia 2026."
   )
   # Ejen akan mengeluarkan function call untuk carian; SDK mengurus selebihnya.
   ```

2. **Sediakan integrasi Google Workspace (Gmail):**
   - Ikuti panduan OAuth 2.0 Google untuk mendapatkan token penyegar.
   - Gunakan Vertex AI Extensions atau Function Calling yang disesuaikan untuk memanggil Gmail API.
   - Contoh (pseudokod):
     ```python
     # Dalam fungsi handler alat tersuai
     def read_gmail(max_results=5):
         # Gunakan Google API Client Library untuk mengambil emel
         ...
         return ringkasan_emel
     ```

3. **Eksperimen dengan Google Sheets:** Minta ejen membaca data dari helaian dan menjana laporan.
   ```python
   prompt = "Baca data jualan dari helaian 'Sales2026' dan berikan ringkasan trend."
   ```

✅ **Tanda Kejayaan:** Ejen boleh mencari maklumat web secara autonomi dan membaca/menulis data dari Google Workspace apabila diminta.

🔗 **Rujukan:** [API_SPEC.md – Function Calling](./API_SPEC.md#23-function-calling--extensions) | [RULES.md – Google Workspace](./RULES.md#34-google-workspace-integration)

---

### Fasa 3G: Managed Agents & Antigravity Automasi Penuh

🎯 **Matlamat:** Satu panggilan API – ejen menjalankan kod, memanipulasi fail, dan menghasilkan artifak dalam persekitaran Linux terpencil.

📂 **Fail Contoh:** `google-ai-studio/boilerplate/agent.py` (lengkap)

🪜 **Langkah-langkah:**

1. **Gunakan Interactions API untuk Managed Agent:**
   ```python
   from google import genai

   client = genai.Client()
   interaction = client.interactions.create(
       agent="antigravity-preview-05-2026",
       input="Ambil dataset CSV ini, bersihkan data, dan bina carta bar.",
       environment="remote",
       files=[{"uri": "gs://bucket/data.csv", "mime_type": "text/csv"}]
   )
   # Sandbox disediakan automatik, kod Python dijalankan, carta dijana.
   print(interaction.output_text)  # Penerangan teks
   # Carta disimpan dalam sandbox, boleh dimuat turun.
   ```

2. **Sambung semula sesi terdahulu:**
   ```python
   interaction2 = client.interactions.create(
       agent="antigravity-preview-05-2026",
       input="Sekarang analisis carta yang awak hasilkan dan beri cadangan.",
       previous_interaction_id=interaction.id,
       environment_id=interaction.environment_id
   )
   ```

3. **Gunakan Antigravity CLI untuk eksport tempatan:**
   ```bash
   agy export --interaction-id=... --output-dir=./projek-tempatan
   ```
   Ini mencipta `.antigravity/sandbox.yaml` dan kod yang boleh dijalankan semula.

4. **Tambah kemahiran tersuai (AGENTS.md):**
   Cipta fail `skills/workspace-analyst.md` dan rujuk dalam konfigurasi ejen untuk memberikan kepakaran domain.

✅ **Tanda Kejayaan:** Ejen anda beroperasi sepenuhnya dalam sandbox, boleh menjalankan kod, mengakses fail, dan meneruskan kerja merentas sesi.

🔗 **Rujukan:** [API_SPEC.md – Interactions API](./API_SPEC.md#22-interactions-api-managed-agents) | [RULES.md – Managed Agents](./RULES.md#33-managed-agents--sandbox)

---

## 4. Trek Anthropic Claude

**Objektif Keseluruhan:** Bina ejen yang tepat, berdisiplin tinggi, dan boleh mengkoordinasi berbilang sub‑ejen melalui Model Context Protocol (MCP).

### Fasa 1C: Mesej Asas & Adaptive Thinking

🎯 **Matlamat:** Berinteraksi dengan Claude API, memahami blok kandungan (text, tool_use), dan mengaktifkan mod pemikiran lanjutan.

📂 **Fail Contoh:** `anthropic-claude/boilerplate/` (panggilan asas)

🪜 **Langkah-langkah:**

1. Pasang SDK (TypeScript disyorkan):
   ```bash
   npm install @anthropic-ai/sdk
   ```

2. Sediakan klien:
   ```typescript
   import Anthropic from '@anthropic-ai/sdk';
   const anthropic = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY });
   ```

3. Hantar mesej pertama:
   ```typescript
   const msg = await anthropic.messages.create({
       model: "claude-sonnet-5-20260501",
       max_tokens: 1024,
       messages: [{ role: "user", content: "Tulis fungsi Python untuk FizzBuzz." }]
   });
   console.log(msg.content[0].text);
   ```

4. **Aktifkan Adaptive Thinking** untuk masalah logik kompleks:
   ```typescript
   const msg = await anthropic.messages.create({
       model: "claude-opus-4-7-20260501",
       max_tokens: 4096,
       thinking: { type: "adaptive", effort: "medium" },
       messages: [{ role: "user", content: "Optimumkan algoritma carian ini..." }]
   });
   // Respons mengandungi blok "thinking" sebelum jawapan.
   ```

5. Kenal pasti `stop_reason` dan kendalikan `tool_use` jika ada (walaupun belum menggunakan alat, fahami strukturnya).

✅ **Tanda Kejayaan:** Anda boleh menghantar mesej, menerima respons dengan atau tanpa pemikiran, dan membaca `stop_reason`.

🔗 **Rujukan:** [API_SPEC.md – Messages API](./API_SPEC.md#31-messages-api) | [RULES.md – Thinking](./RULES.md#41-extended--adaptive-thinking)

---

### Fasa 2C: MCP Server & Alatan Tempatan

🎯 **Matlamat:** Hubungkan Claude ke sistem fail tempatan atau pangkalan data melalui MCP. Ejen boleh membaca dan menulis fail dengan kebenaran terhad.

📂 **Fail Contoh:** `anthropic-claude/boilerplate/mcp/filesystem-server/`

🪜 **Langkah-langkah:**

1. Fahami `mcp-config.example.json`:
   ```json
   {
     "mcpServers": {
       "filesystem": {
         "command": "npx",
         "args": ["-y", "@anthropic/mcp-server-filesystem", "/home/user/projek"],
         "permissions": {
           "read": ["/home/user/projek/**"],
           "write": ["/home/user/projek/output/**"]
         }
       }
     }
   }
   ```

2. Salin templat ke `mcp-config.json` dan laraskan laluan.

3. Mulakan pelayan MCP dan daftarkan alat ke Claude. (Gunakan kod pembalut dari `boilerplate/` yang melancarkan pelayan dan menyalurkan `tool_use`).

4. Minta Claude melakukan operasi fail:
   ```typescript
   // Prompt: "Baca semua fail .ts dalam src/ dan senaraikan fungsi yang dieksport."
   // Claude akan memanggil alat read_directory dan read_file melalui MCP.
   ```

5. **Eksperimen dengan pelayan pangkalan data:** Sambungkan ke PostgreSQL dan minta Claude menulis pertanyaan SQL.

✅ **Tanda Kejayaan:** Claude boleh berinteraksi dengan sumber tempatan melalui MCP, dengan semua akses dihadkan oleh konfigurasi.

🔗 **Rujukan:** [API_SPEC.md – MCP](./API_SPEC.md#33-model-context-protocol-mcp) | [RULES.md – MCP](./RULES.md#42-model-context-protocol-mcp)

---

### Fasa 3C: Pasukan Ejen (Dev Team)

🎯 **Matlamat:** Orkestrasi multi‑ejen – Coordinator (Opus) memecahkan tugas dan mengagihkan kepada Coder dan Reviewer (Sonnet).

📂 **Fail Contoh:** `anthropic-claude/boilerplate/multi-agent/`

🪜 **Langkah-langkah:**

1. Kaji struktur fail:
   - `coordinator.ts` – menerima tugasan, mengeluarkan pelan, memanggil sub‑ejen.
   - `coder.ts` – menerima sub‑tugasan, melaksanakan kod.
   - `reviewer.ts` – menyemak output coder, mengembalikan `approved` atau `changes_requested`.

2. Jalankan pasukan ejen untuk tugasan refaktor:
   ```bash
   npm run dev-team -- --task "Refaktor modul auth ke JWT"
   ```

3. Perhatikan output dalam terminal: setiap langkah coordinator, output coder, dan keputusan reviewer dipaparkan dengan jelas.

4. Uji dengan tugas lain: "Tambah ujian unit untuk modul X", "Dokumentasikan API".

5. Sesuaikan prompt sistem untuk setiap ejen mengikut keperluan projek anda.

✅ **Tanda Kejayaan:** Tugas kejuruteraan perisian kompleks diselesaikan secara autonomi oleh pasukan ejen, dengan kitaran semakan kualiti.

🔗 **Rujukan:** [ARCHITECTURE.md – Multi‑Ejen](./ARCHITECTURE.md#32-seni-bina-multi-ejen) | [RULES.md – Orkestrasi Multi‑Ejen](./RULES.md#43-orkestrasi-multi-ejen)

---

## 5. Trek DeepSeek

**Objektif Keseluruhan:** Bina ejen berskala besar dengan kos sangat rendah, memanfaatkan MoE, memori Engram, dan rantaian pemikiran R1.

### Fasa 1D: API Serasi OpenAI & Parser CoT

🎯 **Matlamat:** Sambung ke API DeepSeek menggunakan klien OpenAI, dan tulis parser untuk output R1.

📂 **Fail Contoh:** `deepseek/boilerplate/cot-agent.py`

🪜 **Langkah-langkah:**

1. Pasang klien OpenAI:
   ```bash
   pip install openai
   ```

2. Konfigurasi klien:
   ```python
   from openai import OpenAI

   client = OpenAI(
       api_key="sk-your-deepseek-key",
       base_url="https://api.deepseek.com/v1"
   )
   ```

3. Hantar permintaan ke model V4-Pro:
   ```python
   response = client.chat.completions.create(
       model="deepseek-v4-pro",
       messages=[{"role": "user", "content": "Huraikan secara ringkas apa itu Mixture-of-Experts."}]
   )
   print(response.choices[0].message.content)
   ```

4. **Gunakan model R1 dan tulis parser:**
   ```python
   response = client.chat.completions.create(
       model="deepseek-r1",
       messages=[{"role": "user", "content": "Berapa banyak cara untuk menyusun 5 buku di rak?"}]
   )
   raw = response.choices[0].message.content
   # Parse dengan fungsi dari RULES.md atau cot-agent.py
   parsed = parse_r1_output(raw)  # pulangkan dict dengan 'thinking' dan 'answer'
   print("PEMIKIRAN:", parsed["thinking"][:200])
   print("JAWAPAN:", parsed["answer"])
   ```

5. **Uji dengan tugasan kompleks** yang memerlukan langkah pengiraan, pastikan parser berfungsi walaupun output terpotong.

✅ **Tanda Kejayaan:** Anda boleh memanggil kedua‑dua model, dan output R1 dipisahkan dengan betul untuk paparan.

🔗 **Rujukan:** [API_SPEC.md – Chat Completions](./API_SPEC.md#41-chat-completions-openai-compatible) | [RULES.md – Pengurusan Output R1](./RULES.md#51-pengurusan-output-r1-chain-of-thought)

---

### Fasa 2D: Memori Engram & Redis

🎯 **Matlamat:** Sambungkan lapisan memori luaran (Redis) untuk menyimpan dan mengambil semula konteks perbualan panjang secara efisien.

📂 **Fail Contoh:** `deepseek/boilerplate/engram-memory.py` dan `docker-compose.yml`

🪜 **Langkah-langkah:**

1. Hidupkan Redis menggunakan Docker:
   ```bash
   cd deepseek/boilerplate
   docker-compose up -d redis
   ```

2. Fahami modul `engram-memory.py` – ia mengandungi fungsi:
   - `store_context(session_id, text)` – menyimpan teks ke Redis dengan hash.
   - `retrieve_relevant(session_id, query)` – mencari entri yang serupa secara semantik.

3. Integrasikan ke dalam aliran ejen:
   ```python
   # Sebelum hantar prompt ke DeepSeek
   relevant_context = retrieve_relevant(session_id, prompt)
   full_prompt = relevant_context + "\n" + prompt if relevant_context else prompt
   # Hantar full_prompt ke API
   ```

4. Uji dengan dokumen besar (contoh: tampal artikel panjang ke Redis, kemudian tanya soalan spesifik – ejen harus menggunakan konteks tersimpan tanpa memproses semula semua token).

5. Pantau cache hit dan penjimatan token (gunakan widget kos dari DESIGN.md jika ada).

✅ **Tanda Kejayaan:** Ejen mengingati maklumat merentas sesi tanpa kos input yang tinggi. Tetingkap konteks 1M token dapat dimanfaatkan secara ekonomi.

🔗 **Rujukan:** [ARCHITECTURE.md – Engram](./ARCHITECTURE.md#42-sistem-memori-engram) | [RULES.md – Memori Engram](./RULES.md#53-sistem-memori-engram)

---

### Fasa 3D: Automasi Skala Besar dengan Kos Ultra Rendah

🎯 **Matlamat:** Proses volum data besar (contoh: analisis monorepo, pemprosesan dokumen korporat) dengan rangkaian ejen berbilang langkah pada kos minima.

📂 **Fail Contoh:** `deepseek/boilerplate/` (gabungan)

🪜 **Langkah-langkah:**

1. **Reka bentuk aliran kerja berbilang langkah:**
   - Langkah 1 (V4-Pro): Imbas semua fail, ekstrak metadata.
   - Langkah 2 (R1): Kenal pasti kebergantungan dan isu.
   - Langkah 3 (V4-Pro): Jana laporan dan cadangan.

2. **Gunakan teknik pemampatan prompt** untuk mengurangkan token input. Contoh: ringkaskan output langkah sebelumnya sebelum menghantar ke langkah seterusnya.

3. **Laksanakan pemprosesan kelompok (batch):** Bina skrip yang menghantar berpuluh-puluh permintaan secara selari (dengan had kadar) untuk menganalisis fail secara serentak.

4. **Pasang widget pemantauan kos masa nyata** (dari `shared/cost-calculator`). Perhatikan bagaimana kos kekal di bawah $0.10 untuk analisis puluhan ribu baris kod.

5. **Uji dengan senario dunia sebenar:** Muat naik pangkalan kod sumber terbuka besar, suruh ejen mencari kelemahan keselamatan, dan nilai ketepatan serta kos.

✅ **Tanda Kejayaan:** Anda boleh memproses beban kerja yang besar (berjuta token) dengan kos yang sangat rendah, sambil mengekalkan output berkualiti tinggi.

🔗 **Rujukan:** [ARCHITECTURE.md – MoE](./ARCHITECTURE.md#41-mixture-of-experts-moe) | [RULES.md – Prompt Compression](./RULES.md#52-prompt-compression--pengoptimuman-kos)

---

## 6. Selepas Tamat – Ke Mana Seterusnya?

Tahniah! Anda kini boleh membina ejen autonomi menggunakan mana‑mana platform. Berikut adalah cadangan laluan seterusnya:

- **🧪 Cuba Vibe Code Playground:** Uji kod ejen terus dalam pelayar tanpa perlu memasang apa-apa. Buka **[Vibe Code Playground](https://state-of-protocol.github.io/orchestra-website/#vibe-code)** dan mula mengekod.

- **Gabungkan platform:** Gunakan DeepSeek untuk prapemprosesan murah, hantar ringkasan ke Claude untuk pengekodan tepat, dan Google untuk analisis multimodal. Lihat [Pola Orkestrasi Rentas Platform](./ARCHITECTURE.md#6-pola-orkestrasi-rentas-platform-konseptual).

- **Sumbang semula:** Perbaiki tutorial, tambah contoh baharu, atau terjemah ke bahasa lain. Sila lihat [CONTRIBUTING.md](../.github/CONTRIBUTING.md).

- **Sertai komuniti:** Kongsi projek anda di [Discord](https://discord.gg/example) dan dapatkan maklum balas.

- **Pantau kemas kini:** Landskap AI berubah pantas. Repositori ini akan dikemas kini dengan model dan ciri terkini. Tonton (watch) repositori untuk pemberitahuan.

---

_Dokumen ini adalah sebahagian daripada rangka kerja 7-fail Orchestra AI‑Agent Tutorial. Untuk pemahaman menyeluruh, baca kesemua dokumen dalam direktori platform pilihan anda._

_Selamat membina ejen! 🚀_
```

---
