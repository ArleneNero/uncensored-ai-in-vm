<div align="center">

# 🔓 Uncensored AI in VM

**Jalankan Model AI Uncensored (Gemma 31B, Qwen 14B, Qwen 7B) di Cloud GPU (Kaggle 2x T4 16GB) Secara Gratis & Sambungkan ke OpenCode / Client AI Lokal.**

Trik Symlink 0 MB Disk · Dual T4 VRAM Offload · Ngrok Tunnel Bebas Timeout · In-Notebook Streaming Chat

<br/>

[![Kaggle Dual T4](https://img.shields.io/badge/GPU-Kaggle%202x%20T4%20(30GB)-blue?style=flat-square&logo=kaggle)](https://kaggle.com)
[![Engine Ollama](https://img.shields.io/badge/Engine-Ollama-white?style=flat-square&logo=ollama)](https://ollama.com)
[![Tunnel Ngrok](https://img.shields.io/badge/Tunnel-Ngrok-1f1e33?style=flat-square&logo=ngrok)](https://ngrok.com)
[![Client OpenCode](https://img.shields.io/badge/Client-OpenCode-black?style=flat-square)](https://opencode.ai)
[![Disk Usage](https://img.shields.io/badge/Disk%20Usage-0%20MB%20(Symlink)-brightgreen?style=flat-square)](#-trik-utama-symlink-0-mb-anti-crash)

<br/>

[Overview](#-overview) • [Pilihan Model](#-pilihan-model-dataset) • [Panduan Setting Konteks](#-panduan-setting-konteks-token) • [Installation](#-installation--prerequisites) • [Cara Pakai](#-cara-pakai-3-langkah-instan) • [Konfigurasi OpenCode](#-konfigurasi-opencode) • [Troubleshooting](#-troubleshooting)

<hr/>

</div>

> [!IMPORTANT]
> **Disclaimer & Responsible Use**: Proyek ini dibuat untuk tujuan riset, edukasi, dan eksperimen pribadi dalam menjalankan model open-weights di lingkungan cloud terbatas. Pengguna bertanggung jawab penuh atas segala prompt, konten, dan output yang dihasilkan dari model LLM yang dijalankan melalui pipeline ini.

## ✨ Overview

Repository ini menyediakan notebook siap pakai (`uncensored-ai-in-vm.ipynb`) untuk menjalankan model AI Uncensored menggunakan hardware gratis dari **Kaggle (2x Nvidia T4 = 30 GB VRAM)**, lalu menghubungkannya langsung ke aplikasi coding desktop seperti **OpenCode** atau client berbasis OpenAI-compatible.

### 🔥 Keunggulan Pipeline Ini:
- **Trik Symlink 0 MB Disk:** Mengatasi batas storage writable Kaggle (20 GB) dengan menghubungkan file bobot model langsung dari input dataset tanpa memakan kuota disk.
- **Bebas Limit Timeout 100 Detik:** Menggunakan Ngrok tunnel sehingga bebas dari pemutusan paksa Cloudflare `HTTP 524`.
- **Anti-Banned / Anti-Kill:** Menggunakan HTTPS tunnel biasa (bukan VPN/WireGuard seperti Tailscale) yang aman dari scanner IDS Kaggle.
- **Konteks Luas (Hingga 32k/64k Token):** Mendukung percakapan panjang dan analisis file source code ratusan baris.
- **Dukungan Chat 2 Arah:** Bisa diajak ngobrol langsung di browser Kaggle (streaming) atau dari OpenCode di komputer lokal.

---

## 💾 Pilihan Model (Dataset)

Pilih salah satu dataset model di bawah ini sesuai kebutuhan tugas Anda:

| Kasta Model | Ukuran | Karakteristik Utama | Rekomendasi Penggunaan | Link Dataset Kaggle |
|---|---|---|---|---|
| **Gemma 4 31B Heretic** *(Model Raksasa)* | ~18.7 GB | **Deep Knowledge & Penalaran Kompleks.** Otak terbesar dan wawasan paling mendalam untuk instruksi rumit tanpa sensor. | Arsitektur sistem, riset mendalam, diskusi filosofis tingkat tinggi. | https://www.kaggle.com/datasets/muhammadariefsyah/gemma-31b-heretic-ollama |
| **Qwen 2.5 14B Instruct (Abliterated)** *(Model Menengah)* | ~9.0 GB | ⚖️ **Sweet Spot Sempurna.** Sangat cerdas, bahasa Indonesia sangat luwes/nyambung, dan tetap gesit di GPU T4. | **Diskusi harian**, tanya jawab hacking/keamanan, analisis konsep, chat cerdas. | https://www.kaggle.com/datasets/muhammadariefsyah/qwen-2-5-14b-instruct-abliterated-ollama |
| **Qwen 2.5 Coder 7B (Abliterated)** *(Model Ringan)* | ~4.7 GB | ⚡ **Super Cepat & Presisi Coding.** Ngetik kilat (40-50 token/s), ahli membuat script (Python, Bash, C, PowerShell) dan payload. | Task coding cepat, nulis script/tools, refactoring, bug fixing. | https://www.kaggle.com/datasets/muhammadariefsyah/qwen-2-5-coder-7b-abliterated-ollama |

> [!NOTE]
> **Atribusi Model & Alasan Penggunaan Fitur Dataset:**  
> - **Bukan Pelatihan Mandiri:** Seluruh bobot model di atas merupakan karya riset dari organisasi pengembang aslinya (Google DeepMind, Alibaba Qwen, dsb.) dengan teknik kuantisasi GGUF dan abliterasi oleh komunitas open-source (*mradermacher, failspy, huihui-ai*).
> - **Mengapa Dipublikasikan sebagai Dataset?** Fitur *Kaggle Models* dirancang untuk distribusi arsitektur machine learning standar (PyTorch/Transformers). Sementara itu, pipeline pada repositori ini membutuhkan akses langsung ke struktur direktori internal runtime Ollama (`blobs` dan `manifests`). Dengan menyimpannya sebagai **Dataset Kaggle**, file cache dapat dimount secara instan (0 detik download) dan dihubungkan menggunakan teknik symlink 0 MB tanpa mengubah struktur file biner aslinya.

> [!TIP]
> **1 Notebook untuk Semua Model:** Notebook [`uncensored-ai-in-vm.ipynb`](./uncensored-ai-in-vm.ipynb) secara otomatis mendeteksi model mana pun yang Anda pasang di `/kaggle/input`. Anda tidak perlu mengubah kode apa pun di dalam notebook!

---

## 🎛️ Panduan Setting Konteks Token

Batas konteks (*Context Window*) menentukan seberapa panjang ingatan percakapan dan file kode yang bisa dibaca oleh AI dalam 1 waktu:

| Model | Konteks Bawaan | Rekomendasi Konteks (`context`) | Rekomendasi Output (`output`) | Catatan VRAM GPU T4 |
|---|---|---|---|---|
| **Qwen 2.5 Coder 7B** | 8.192 | **32.768 (32k)** / **65.536 (64k)** | **8.192** / **16.384** | Sangat enteng. VRAM T4 hanya terpakai ~6-8 GB. |
| **Qwen 2.5 14B Instruct** | 8.192 | **32.768 (32k)** | **8.192** / **16.384** | Stabil & responsif. VRAM T4 terpakai ~11-13 GB. |
| **Gemma 4 31B Heretic** | 8.192 | **16.384 (16k)** | **8.192** | Maksimal 16k agar VRAM dual T4 (30 GB) tidak meluap. |

> [!IMPORTANT]
> - Konfigurasi di notebook (`uncensored-ai-in-vm.ipynb`) pada Sel 2 sudah diset default ke:  
>   `os.environ["OLLAMA_CONTEXT_LENGTH"] = "32768"`
> - Pastikan pengaturan `limit.context` di `opencode.json` tidak melebihi kapasitas `OLLAMA_CONTEXT_LENGTH` di notebook agar memori tetap optimal.

---

## 📦 Installation & Prerequisites

Sebelum mulai, pastikan Anda telah menyiapkan hal-hal berikut:

### 1. Akun Cloud (Gratis)
- **Akun Kaggle Terverifikasi (No. HP):** Wajib agar bisa mengaktifkan **GPU T4 x2** dan fitur **Internet on**. Daftar di [kaggle.com](https://www.kaggle.com).
- **Akun Ngrok (Gratis):** Untuk mendapatkan tunnel publik tanpa batas timeout. Salin token gratis Anda di [dashboard.ngrok.com/get-started/your-authtoken](https://dashboard.ngrok.com/get-started/your-authtoken).

### 2. Client di Komputer Lokal
- Pasang **[OpenCode Desktop](https://opencode.ai)** (atau client AI lokal lainnya seperti Continue.dev / LibreChat).
- Clone atau unduh file notebook dari repository ini:
  ```bash
  git clone https://github.com/ArleneNero/uncensored-ai-in-vm.git
  cd uncensored-ai-in-vm
  ```
  *(Atau cukup download langsung file [`uncensored-ai-in-vm.ipynb`](./uncensored-ai-in-vm.ipynb)).*

---

## 🚀 Cara Pakai (3 Langkah Instan)

### Langkah 1: Buat Notebook Baru dari Dataset Pilihan Anda
1. Buka salah satu link dataset di atas:
   - https://www.kaggle.com/datasets/muhammadariefsyah/gemma-31b-heretic-ollama
   - https://www.kaggle.com/datasets/muhammadariefsyah/qwen-2-5-14b-instruct-abliterated-ollama
   - https://www.kaggle.com/datasets/muhammadariefsyah/qwen-2-5-coder-7b-abliterated-ollama
2. Klik tombol **New Notebook** di pojok kanan atas halaman dataset. Dataset otomatis terpasang di `/kaggle/input/`.
3. Di menu bar atas notebook Kaggle, klik menu **Settings**:
   - Klik **Accelerator** $\to$ pilih **GPU T4 x2**.
   - Pastikan internet aktif (jika tulisannya **Turn on internet**, klik agar berubah menjadi aktif / *Turn off internet*).

### Langkah 2: Upload / Import Notebook Ini ke Kaggle
1. Di menu atas notebook Kaggle, klik **File** $\to$ **Upload Notebook** / **Import Notebook**.
2. Pilih file [`uncensored-ai-in-vm.ipynb`](./uncensored-ai-in-vm.ipynb) dari repository ini.

### Langkah 3: Masukkan Token Ngrok & Jalankan
1. Masukkan token Ngrok Anda pada **Sel 3**:
   ```python
   NGROK_TOKEN = "MASUKKAN_AUTHTOKEN_ANDA_DI_SINI"
   ```
2. Klik tombol **Run All** (atau jalankan Sel 1, 2, 3 berurutan). Sel 2 otomatis mendeteksi model apa pun yang Anda pasang!
3. Di output **Sel 3**, salin URL yang muncul di samping tulisan:  
   `🔥 URL UNTUK OPENCODE: https://xxxx-xx-xx.ngrok-free.dev/v1`

### Langkah 4: Pasang URL ke Konfigurasi OpenCode
1. Buka file konfigurasi OpenCode di komputer Anda:
   - **Windows:** `%USERPROFILE%\.config\opencode\opencode.json`
   - **Linux / macOS:** `~/.config/opencode/opencode.json`
2. Tempel URL yang Anda dapat dari Langkah 3 ke bagian `"baseURL"`:
   ```json
   "baseURL": "https://xxxx-xx-xx.ngrok-free.dev/v1"
   ```
   *(Pastikan akhiran `/v1` tetap ada).*
3. Simpan file dan buka **OpenCode Desktop**. Pilih model yang diinginkan dan Anda siap coding!

---

## 🔌 Konfigurasi OpenCode

Buka file konfigurasi OpenCode di komputer Anda (`~/.config/opencode/opencode.json` atau `%USERPROFILE%\.config\opencode\opencode.json`), lalu masukkan konfigurasi provider berikut:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "kaggle": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama Kaggle (T4 x2)",
      "options": {
        "baseURL": "https://xxxx-xx-xx.ngrok-free.dev/v1",
        "apiKey": "ollama"
      },
      "models": {
        "hf.co/mradermacher/gemma-4-31B-it-heretic-GGUF:Q4_K_M": {
          "name": "Gemma 31B Heretic (Raksasa / Riset)",
          "limit": {
            "context": 16384,
            "output": 8192
          }
        },
        "hf.co/mradermacher/Qwen2.5-14B-Instruct-abliterated-v2-GGUF:Q4_K_M": {
          "name": "Qwen 2.5 14B Instruct (Menengah / Chat Cerdas)",
          "limit": {
            "context": 32768,
            "output": 16384
          }
        },
        "hf.co/mradermacher/Qwen2.5-Coder-7B-Instruct-abliterated-GGUF:Q4_K_M": {
          "name": "Qwen 2.5 Coder 7B (Ringan / Cepat & Coding)",
          "limit": {
            "context": 32768,
            "output": 16384
          }
        }
      }
    }
  }
}
```

> [!NOTE]
> - Pastikan `baseURL` selalu berakhiran **`/v1`**.
> - Di dropdown model OpenCode, pilih opsi **`Default`** (jangan pilih *Low/Medium/High* karena model GGUF ini tidak menggunakan parameter thinking OpenAI).
> - Nilai `output: 8192` atau `16384` mencegah error *"Response still truncated after two output token limit continuations"*.

---

## 🧠 Trik Utama: Symlink 0 MB (Anti-Crash)

Model AI memiliki ukuran besar (5 GB s/d 19 GB), sementara batas disk writable Kaggle hanya **20 GB**. Jika file di-download atau di-copy biasa, disk langsung penuh dan container dimatikan paksa (`Exit code: 137`).

* **`blobs`** (file bobot GGUF) **hanya di-symlink** (shortcut) dari `/kaggle/input/` ke cache Ollama. Memakan waktu 0 detik dan 0 MB kuota disk!
* **`manifests`** (metadata JSON beberapa KB) **di-copy fisik** menggunakan `shutil.copy2` (wajib karena Ollama menolak jika manifests berupa symlink).

---

## 🛠️ Troubleshooting

| Kendala | Penyebab | Solusi |
|---|---|---|
| **Exit code 137 / SIGKILL** | Melakukan copy file besar hingga storage Kaggle 20 GB habis. | Gunakan symlink untuk `blobs` (hanya `manifests` yang di-copy). |
| **`manifest symlink target outside model directory`** | Ollama menolak jika folder `manifests` berupa symlink. | Copy file manifest dengan `shutil.copy2` seperti pada Sel 2. |
| **HTTP 524 Timeout** | Cloudflare Tunnel memutus koneksi jika model mikir >100 detik. | Gunakan **Ngrok** (Sel 3) yang bebas dari limit timeout 100s. |
| **HTTP 404 Not Found** | URL di `opencode.json` tidak memiliki akhiran `/v1`. | Tambahkan `/v1` di akhir `baseURL`. |
| **Does not support thinking** | Mengatur dropdown reasoning di OpenCode ke *Low/Medium/High*. | Kembalikan dropdown reasoning ke **`Default`**. |
| **Response still truncated** | Jawaban panjang terpotong batas token bawaan. | Naikkan batas `"limit": { "context": 32768, "output": 16384 }` di `opencode.json`. |
