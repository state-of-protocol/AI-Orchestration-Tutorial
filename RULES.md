# ⚖️ RULES.md — Piawaian Pengekodan & Kekangan Sistem

**Peraturan Ketat untuk Pembangunan Ejen Autonomi**  
_Versi 1.0 · Mei 2026_

Dokumen ini mentakrifkan **peraturan, kekangan, dan amaran** yang mesti dipatuhi semasa membangunkan ejen di dalam ekosistem Orchestra AI‑Agent. Kegagalan mematuhi peraturan ini boleh menyebabkan kos tidak terkawal, kebocoran data, kegagalan ejen, atau pelanggaran syarat perkhidmatan platform.

---

## 📖 Isi Kandungan

- [1. Prinsip Asas](#1-prinsip-asas)
- [2. Peraturan Sejagat (Semua Platform)](#2-peraturan-sejagat-semua-platform)
- [3. Peraturan Khusus Google AI Studio](#3-peraturan-khusus-google-ai-studio)
- [4. Peraturan Khusus Anthropic Claude](#4-peraturan-khusus-anthropic-claude)
- [5. Peraturan Khusus DeepSeek](#5-peraturan-khusus-deepseek)
- [6. Gaya Pengekodan & Linting](#6-gaya-pengekodan--linting)
- [7. Peraturan Pengujian & Eksperimen](#7-peraturan-pengujian--eksperimen)
- [8. Peraturan Pengerahan (Deployment)](#8-peraturan-pengerahan-deployment)
- [9. Ringkasan "Jangan Sesekali"](#9-ringkasan-jangan-sesekali)

---

## 1. Prinsip Asas

> **Falsafah kami:** Ejen yang tidak terkawal adalah liabiliti, bukan aset. Setiap peraturan di sini wujud untuk melindungi pembangun, pengguna akhir, dan integriti sistem.

| Prinsip | Huraian |
|---------|----------|
| **Keselamatan Diutamakan** | Tiada pengecualian untuk peraturan keselamatan. Ejen tidak boleh mempunyai akses tanpa had. |
| **Kos Terkawal** | Setiap panggilan API mesti mempunyai had token dan bajet. Ejen tidak boleh beroperasi dalam gelung tanpa had. |
| **Ketelusan** | Semua tindakan ejen mesti dilog dan boleh diaudit. Tiada kotak hitam. |
| **Prinsip Keengganan Minimum** | Ejen hanya boleh melakukan apa yang diminta, bukan lebih. Jangan beri kuasa berlebihan. |
| **Pengasingan Persekitaran** | Uji dalam persekitaran terpencil sebelum pengerahan. Jangan uji ejen pada data pengeluaran. |

---

## 2. Peraturan Sejagat (Semua Platform)

Peraturan ini terpakai tanpa mengira platform yang digunakan.

### 2.1 Pengurusan API Key & Rahsia

| Peraturan | Keutamaan |
|-----------|-----------|
| **Rahsia mesti disimpan dalam pembolehubah persekitaran**, bukan dalam kod sumber. | 🔴 WAJIB |
| **Fail `.env` mesti disenaraikan dalam `.gitignore`**. Jangan commit fail `.env` ke repositori. | 🔴 WAJIB |
| **Gunakan `.env.example`** sebagai templat dengan nilai palsu. | 🟡 DISYORKAN |
| **API key tidak boleh dikongsi** dalam tangkapan skrin, log, atau forum. | 🔴 WAJIB |
| **Gunakan perkhidmatan pengurusan rahsia** (AWS Secrets Manager, Google Secret Manager, Vault) untuk pengerahan pengeluaran. | 🟡 DISYORKAN |

### 2.2 Had & Kawalan Ejen

```python
# ❌ SALAH: Ejen tanpa had
agent.run(prompt="Selesaikan masalah ini", max_iterations=None)

# ✅ BETUL: Ejen dengan had yang jelas
agent.run(prompt="Selesaikan masalah ini", max_iterations=10, max_tokens=10000, timeout_seconds=60)
```

| Peraturan | Keutamaan |
|-----------|-----------|
| **Sentiasa tetapkan `max_tokens`** dalam setiap panggilan API. | 🔴 WAJIB |
| **Sentiasa tetapkan had lelaran** untuk ejen yang memanggil alatan secara autonomi. | 🔴 WAJIB |
| **Sentiasa tetapkan masa tamat (timeout)** untuk mengelakkan ejen tergantung. | 🔴 WAJIB |
| **Gunakan `max_cost` atau bajet token** untuk mengelakkan bil mengejut. | 🟡 DISYORKAN |

### 2.3 Pengendalian Ralat & Log

| Peraturan | Keutamaan |
|-----------|-----------|
| **Semua panggilan API mesti dibalut dalam blok `try-catch`** dengan pengendalian ralat spesifik. | 🔴 WAJIB |
| **Log semua tindakan ejen** – termasuk tool call, parameter, dan hasil. | 🟡 DISYORKAN |
| **Jangan dedahkan data sensitif dalam log** – tapis API key, token, dan data peribadi. | 🔴 WAJIB |
| **Gunakan exponential backoff** untuk ralat `429 Rate Limit`. | 🟡 DISYORKAN |

### 2.4 Etika & Kandungan

| Peraturan | Keutamaan |
|-----------|-----------|
| **Jangan bina ejen untuk menjana kandungan berbahaya**, penipuan, spam, atau pelanggaran privasi. | 🔴 WAJIB |
| **Patuhi syarat perkhidmatan** setiap platform. | 🔴 WAJIB |
| **Ejen yang berinteraksi dengan manusia mesti mendedahkan** bahawa ia adalah AI. | 🔴 WAJIB |

---

## 3. Peraturan Khusus Google AI Studio

### 3.1 Context Caching

| Peraturan | Keutamaan |
|-----------|-----------|
| **Wajib menggunakan Context Caching** untuk sebarang permintaan dengan konteks melebihi 100,000 token. | 🔴 WAJIB |
| **Tetapkan TTL (Time-To-Live) cache dengan munasabah** – jangan biarkan cache tamat terlalu cepat atau terlalu lama. | 🟡 DISYORKAN |

```python
# ✅ BETUL: Menggunakan cache untuk dokumen besar
cache = client.cachedContents.create(
    model="models/gemini-3.5-flash",
    contents=[document_content],
    ttl="3600s"  # 1 jam
)
response = client.models.generate_content(
    model="models/gemini-3.5-flash",
    cachedContent=cache.name,
    contents=[user_query]
)
# Menjimatkan ~80% kos token input
```

### 3.2 Safety Settings

| Peraturan | Keutamaan |
|-----------|-----------|
| **Jangan lumpuhkan penapis keselamatan sepenuhnya** (`OFF`) melainkan benar‑benar diperlukan dan difahami risikonya. | 🔴 WAJIB |
| **Konfigurasi safety thresholds secara eksplisit** dalam kod – jangan bergantung pada tetapan lalai. | 🟡 DISYORKAN |
| **Pantau penolakan keselamatan** – jika ejen kerap dihalang, semak semula prompt atau konteks. | 🟡 DISYORKAN |

```python
# ✅ BETUL: Tetapan keselamatan yang eksplisit
safety_settings = [
    {"category": "HARM_CATEGORY_DANGEROUS_CONTENT", "threshold": "BLOCK_ONLY_HIGH"},
    {"category": "HARM_CATEGORY_HATE_SPEECH", "threshold": "BLOCK_MEDIUM_AND_ABOVE"},
]
# ❌ SALAH: Melumpuhkan semua penapis tanpa pertimbangan
safety_settings = [{"category": "...", "threshold": "OFF"}]
```

### 3.3 Managed Agents & Sandbox

| Peraturan | Keutamaan |
|-----------|-----------|
| **Jangan hantar data sensitif ke sandbox** melainkan anda pasti tentang pengasingannya. | 🔴 WAJIB |
| **Pantau penggunaan fail dalam sandbox** – fail yang dijana boleh mendedahkan maklumat. | 🟡 DISYORKAN |
| **Gunakan `environment_id` dan `previous_interaction_id` dengan berhati‑hati** – jangan sambung semula sesi yang tidak diketahui. | 🟡 DISYORKAN |
| **Jangan bergantung pada keadaan sandbox kekal selama‑lamanya** – sandbox boleh ditamatkan oleh Google pada bila‑bila masa. | 🟡 DISYORKAN |

### 3.4 Google Workspace Integration

| Peraturan | Keutamaan |
|-----------|-----------|
| **Minta skop OAuth minimum yang diperlukan** – jangan minta akses penuh jika hanya perlu baca. | 🔴 WAJIB |
| **Jangan baca atau tulis dokumen pengguna tanpa kebenaran eksplisit** dalam UI. | 🔴 WAJIB |

---

## 4. Peraturan Khusus Anthropic Claude

### 4.1 Extended & Adaptive Thinking

| Peraturan | Keutamaan |
|-----------|-----------|
| **Sentiasa tetapkan `budget_tokens`** apabila mengaktifkan mod thinking. Tanpa had, kos boleh melambung tanpa kawalan. | 🔴 WAJIB |
| **Jangan aktifkan thinking untuk tugas mudah** – ia membazir token tanpa faedah. | 🟡 DISYORKAN |
| **Gunakan Adaptive Thinking dengan tahap usaha (`effort`) yang sesuai** – `"high"` hanya untuk masalah benar‑benar kompleks. | 🟡 DISYORKAN |

```typescript
// ✅ BETUL: Thinking dengan bajet yang ditetapkan
const response = await anthropic.messages.create({
    model: "claude-opus-4-7-20260501",
    thinking: { type: "enabled", budget_tokens: 4000 },
    max_tokens: 8192,
    messages: [...]
});

// ❌ SALAH: Thinking tanpa bajet – kos tidak terkawal
const response = await anthropic.messages.create({
    model: "claude-opus-4-7-20260501",
    thinking: { type: "enabled" },  // TIADA budget_tokens!
    max_tokens: 8192,
    messages: [...]
});
```

### 4.2 Model Context Protocol (MCP)

| Peraturan | Keutamaan |
|-----------|-----------|
| **Konfigurasi `mcp-config.json` mesti menyenarai putih laluan fail yang boleh diakses.** Jangan beri akses kepada `/` (root). | 🔴 WAJIB |
| **Jangan beri keizinan tulis kepada direktori sistem** atau fail kritikal. | 🔴 WAJIB |
| **Semua input daripada MCP Server mesti disanitasi** sebelum digunakan dalam konteks ejen. | 🔴 WAJIB |
| **Pelayan MCP yang berjalan sebagai proses tempatan mesti dihadkan keizinan OS** (contoh: jalankan sebagai pengguna terhad). | 🟡 DISYORKAN |

```json
// ✅ BETUL: Keizinan terhad dalam mcp-config.json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-filesystem", "/home/user/project"],
      "permissions": {
        "read": ["/home/user/project/src/**"],
        "write": ["/home/user/project/output/**"]
      }
    }
  }
}

// ❌ SALAH: Akses tanpa had ke keseluruhan sistem fail
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-filesystem", "/"],
      "permissions": {
        "read": ["/**"],
        "write": ["/**"]
      }
    }
  }
}
```

### 4.3 Orkestrasi Multi‑Ejen

| Peraturan | Keutamaan |
|-----------|-----------|
| **Sub‑ejen mesti mempunyai peranan dan had yang jelas** – jangan beri akses penuh kepada semua sub‑ejen. | 🔴 WAJIB |
| **Ejen reviewer mesti diwajibkan** untuk sebarang perubahan kod sebelum digabungkan. | 🟡 DISYORKAN |
| **Log komunikasi antara ejen** untuk tujuan audit dan nyahpepijat. | 🟡 DISYORKAN |

### 4.4 Claude Code (Terminal Agency)

| Peraturan | Keutamaan |
|-----------|-----------|
| **Jangan jalankan Claude Code dengan keizinan `sudo` atau root.** | 🔴 WAJIB |
| **Sentiasa semak perubahan yang dicadangkan** sebelum meluluskan pelaksanaan automatik. | 🔴 WAJIB |
| **Gunakan mod `--dry-run`** terlebih dahulu untuk melihat apa yang akan dilakukan oleh ejen. | 🟡 DISYORKAN |

---

## 5. Peraturan Khusus DeepSeek

### 5.1 Pengurusan Output R1 (Chain‑of‑Thought)

| Peraturan | Keutamaan |
|-----------|-----------|
| **Wajib menghuraikan (parse) dan mengasingkan tag `<think>...</think>`** daripada output sebelum menghantar ke fungsi hiliran. | 🔴 WAJIB |
| **Jangan hantar blok `<think>` ke pangkalan data, API luaran, atau UI pengguna akhir** tanpa penapisan. | 🔴 WAJIB |
| **Parser mesti mengendalikan kes di mana tag `<think>` tidak lengkap** (output terpotong akibat had token). | 🟡 DISYORKAN |

```python
# ✅ BETUL: Parser yang selamat
import re

def parse_r1_output(text: str) -> dict:
    """
    Mengasingkan pemikiran dan jawapan daripada output DeepSeek-R1.
    """
    think_pattern = r'<think>(.*?)</think>'
    match = re.search(think_pattern, text, re.DOTALL)
    
    if match:
        thinking = match.group(1).strip()
        answer = re.sub(think_pattern, '', text, flags=re.DOTALL).strip()
    else:
        # Jika tiada tag, anggap semua sebagai jawapan
        # (mungkin output terpotong)
        thinking = ""
        answer = text.strip()
    
    return {"thinking": thinking, "answer": answer}

# ❌ SALAH: Menghantar output mentah termasuk <think> ke downstream
save_to_database(response.choices[0].message.content)  # Bahaya!
```

### 5.2 Prompt Compression & Pengoptimuman Kos

| Peraturan | Keutamaan |
|-----------|-----------|
| **Gunakan teknik pemampatan prompt** untuk mengurangkan token input – buang perbualan tidak relevan, ringkaskan konteks lama. | 🟡 DISYORKAN |
| **Manfaatkan cache hit DeepSeek** – ulang guna konteks yang sama untuk mengurangkan kos input kepada $0.0036/1M token. | 🟡 DISYORKAN |
| **Pantau penggunaan token melalui endpoint `/v1/usage`** secara berkala. | 🟡 DISYORKAN |

### 5.3 Sistem Memori Engram

| Peraturan | Keutamaan |
|-----------|-----------|
| **Data yang disimpan dalam Redis/Engram mesti mempunyai TTL** – jangan simpan data selama‑lamanya tanpa sebab. | 🔴 WAJIB |
| **Jangan simpan data peribadi atau sensitif dalam cache Engram** tanpa penyulitan. | 🔴 WAJIB |
| **Sahkan integriti data yang diambil dari cache** sebelum digunakan sebagai konteks. | 🟡 DISYORKAN |

### 5.4 Penggunaan Model Tempatan (vLLM/Ollama)

| Peraturan | Keutamaan |
|-----------|-----------|
| **Jangan dedahkan endpoint vLLM/Ollama ke rangkaian awam** tanpa pengesahan. | 🔴 WAJIB |
| **Pantau penggunaan GPU/CPU** – model tempatan boleh menggunakan sumber yang besar. | 🟡 DISYORKAN |
| **Gunakan Docker dengan had sumber** (`--memory`, `--cpus`) untuk mengelakkan gangguan pada sistem hos. | 🟡 DISYORKAN |

---

## 6. Gaya Pengekodan & Linting

### 6.1 Python (Google AI Studio & DeepSeek)

| Peraturan | Keutamaan |
|-----------|-----------|
| **Ikut PEP 8** – gunakan `black` dan `ruff` untuk pemformatan automatik. | 🔴 WAJIB |
| **Gunakan type hints** pada semua fungsi awam. | 🟡 DISYORKAN |
| **Gunakan `pathlib`** untuk pengendalian laluan fail, bukan rentetan mentah. | 🟡 DISYORKAN |
| **Fail `requirements.txt` atau `pyproject.toml` mesti mengandungi versi spesifik** pergantungan. | 🔴 WAJIB |

```python
# ✅ BETUL: Type hints + pathlib
from pathlib import Path

def read_agent_config(config_path: Path) -> dict:
    """Membaca fail konfigurasi ejen."""
    return json.loads(config_path.read_text())

# ❌ SALAH: Tiada type hints, guna rentetan untuk laluan
def read_agent_config(config_path):
    return json.loads(open(config_path).read())
```

### 6.2 TypeScript/JavaScript (Anthropic Claude)

| Peraturan | Keutamaan |
|-----------|-----------|
| **Gunakan TypeScript** – bukan JavaScript biasa – untuk semua kod MCP dan ejen. | 🔴 WAJIB |
| **Ikut ESLint + Prettier** dengan konfigurasi yang disediakan dalam boilerplate. | 🔴 WAJIB |
| **Gunakan `zod`** untuk pengesahan skema data input/output. | 🟡 DISYORKAN |
| **Fail `package.json` mesti menyenaraikan versi spesifik** pergantungan. | 🔴 WAJIB |

```typescript
// ✅ BETUL: TypeScript dengan pengesahan zod
import { z } from 'zod';

const MCPConfigSchema = z.object({
  mcpServers: z.record(z.object({
    command: z.string(),
    args: z.array(z.string()),
    permissions: z.object({
      read: z.array(z.string()),
      write: z.array(z.string()),
    }).optional(),
  })),
});

type MCPConfig = z.infer<typeof MCPConfigSchema>;
```

### 6.3 Dokumentasi Kod

| Peraturan | Keutamaan |
|-----------|-----------|
| **Setiap fungsi awam mesti mempunyai docstring** yang menerangkan tujuan, parameter, dan nilai pulangan. | 🟡 DISYORKAN |
| **Gunakan ulasan `TODO` dan `FIXME`** untuk menandakan isu yang diketahui – tetapi sertakan tarikh dan nama. | 🟡 DISYORKAN |
| **Jangan tinggalkan kod yang dikomen (commented-out code)** dalam pengeluaran. | 🔴 WAJIB |

---

## 7. Peraturan Pengujian & Eksperimen

| Peraturan | Keutamaan |
|-----------|-----------|
| **Uji ejen dalam persekitaran pembangunan terpencil** sebelum pengerahan. | 🔴 WAJIB |
| **Gunakan data sintetik atau data ujian** – bukan data pengguna sebenar – semasa fasa pembangunan. | 🔴 WAJIB |
| **Rakam dan semak log ejen** selepas setiap ujian untuk mengesan tingkah laku tidak dijangka. | 🟡 DISYORKAN |
| **Uji ejen dengan input yang sengaja salah** (contoh: prompt kosong, aksara unicode luar biasa) untuk memastikan keteguhan. | 🟡 DISYORKAN |
| **Lakukan ujian kebocoran data** – pastikan ejen tidak mendedahkan maklumat konteks sistem. | 🟡 DISYORKAN |

---

## 8. Peraturan Pengerahan (Deployment)

| Peraturan | Keutamaan |
|-----------|-----------|
| **Jangan gunakan API key peribadi dalam persekitaran pengeluaran** – gunakan akaun perkhidmatan atau kunci pengeluaran khusus. | 🔴 WAJIB |
| **Pantau penggunaan API secara masa nyata** dengan amaran (alert) jika melebihi ambang. | 🟡 DISYORKAN |
| **Sediakan pelan kontingensi** jika perkhidmatan API platform tidak tersedia. | 🟡 DISYORKAN |
| **Ejen yang dihadapi pengguna mesti mempunyai lapisan keselamatan tambahan** – pengesahan input, had kadar, dan penapisan output. | 🔴 WAJIB |

---

## 9. Ringkasan "Jangan Sesekali"

Berikut adalah senarai mutlak perkara yang **dilarang sama sekali** dalam mana‑mana projek tutorial ini:

| # | Larangan | Akibat Jika Dilanggar |
|---|----------|----------------------|
| 1 | **Jangan commit API key atau rahsia ke repositori.** | Kebocoran data, bil tidak sah, penggantungan akaun. |
| 2 | **Jangan jalankan ejen tanpa had token/lelaran/masa.** | Kos melambung, gelung tak terhingga, sumber habis. |
| 3 | **Jangan beri ejen akses tulis tanpa had ke sistem fail.** | Kehilangan data, kerosakan sistem, suntikan kod. |
| 4 | **Jangan hantar output mentah R1 (dengan `<think>`) ke sistem hiliran.** | Ralat sintaks, pendedahan logik dalaman yang tidak diingini. |
| 5 | **Jangan lumpuhkan penapis keselamatan tanpa justifikasi kukuh.** | Penjanaan kandungan berbahaya, pelanggaran syarat platform. |
| 6 | **Jangan gunakan data pengguna sebenar untuk ujian.** | Pelanggaran privasi, ketidakpatuhan GDPR/PDPA. |
| 7 | **Jangan dedahkan endpoint model tempatan ke internet awam.** | Penyalahgunaan sumber, akses tanpa kebenaran. |
| 8 | **Jangan abaikan ralat API – sentiasa kendalikan dengan baik.** | Ejen gagal secara senyap, data tidak konsisten. |

---

_Dokumen ini diselenggara oleh komuniti State of Protocol.  
Pelanggaran peraturan yang disengajakan dalam sumbangan kod akan menyebabkan penolakan Pull Request. Sila lihat [CONTRIBUTING.md](../.github/CONTRIBUTING.md)._
```

---

`RULES.md` ini kini menjadi benteng disiplin teknikal yang melindungi pelajar dan pembangun daripada perangkap biasa dalam pembangunan ejen autonomi. Ia melengkapkan set 7 dokumen spesifikasi utama yang telah kita bina bersama-sama.
