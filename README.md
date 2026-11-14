<div align="center">

# 🔓 Uncensored AI in VM

**Jalankan LLM Uncensored Raksasa (Gemma 31B Heretic) di Dual GPU Kaggle (2x T4 16GB) Secara Gratis & Sambungkan ke OpenCode / Client AI Lokal.**

Trik Symlink 0 MB Disk · Dual T4 VRAM Offload · Ngrok Tunnel Bebas Timeout · In-Notebook Streaming Chat

<br/>

[![Kaggle Dual T4](https://img.shields.io/badge/GPU-Kaggle%202x%20T4%20(30GB)-blue?style=flat-square&logo=kaggle)](https://kaggle.com)
[![Engine Ollama](https://img.shields.io/badge/Engine-Ollama-white?style=flat-square&logo=ollama)](https://ollama.com)
[![Tunnel Ngrok](https://img.shields.io/badge/Tunnel-Ngrok-1f1e33?style=flat-square&logo=ngrok)](https://ngrok.com)
[![Client OpenCode](https://img.shields.io/badge/Client-OpenCode-black?style=flat-square)](https://opencode.ai)
[![Disk Usage](https://img.shields.io/badge/Disk%20Usage-0%20MB%20(Symlink)-brightgreen?style=flat-square)](#-trik-utama-symlink-0-mb-anti-crash)

<br/>

[Overview](#-overview) • [Installation & Prerequisites](#-installation--prerequisites) • [Dataset Model](#-dataset-model-wajib) • [Cara Pakai](#-cara-pakai-3-langkah-instan) • [Konfigurasi OpenCode](#-konfigurasi-opencode) • [Troubleshooting](#-troubleshooting)

<hr/>

</div>

> [!IMPORTANT]
> **Disclaimer & Responsible Use**: Proyek ini dibuat untuk tujuan riset, edukasi, dan eksperimen pribadi dalam menjalankan model open-weights besar di lingkungan cloud terbatas. Pengguna bertanggung jawab penuh atas segala prompt, konten, dan output yang dihasilkan dari model LLM yang dijalankan melalui pipeline ini.

## ✨ Overview

Repository ini menyediakan notebook siap pakai (`uncensored-ai-in-vm.ipynb`) untuk menjalankan model LLM Uncensored berukuran besar (**Gemma 4 31B Heretic**) menggunakan hardware gratis dari **Kaggle (2x Nvidia T4 = 30 GB VRAM)**, lalu menghubungkannya langsung ke aplikasi coding desktop seperti **OpenCode** atau client berbasis OpenAI-compatible.

### 🔥 Keunggulan Pipeline Ini:
- **Trik Symlink 0 MB Disk:** Mengatasi keterbatasan storage writable Kaggle (20 GB) dengan symlink file bobot model 19 GB langsung dari dataset input tanpa memakan kuota disk.
- **Bebas Limit Timeout 100 Detik:** Menggunakan Ngrok tunnel sehingga bebas dari pemutusan paksa Cloudflare `HTTP 524`.
- **Anti-Banned / Anti-Kill:** Menggunakan HTTPS tunnel biasa (bukan VPN/WireGuard seperti Tailscale) yang aman dari deteksi pemindaian otomatis container Kaggle.
- **Dukungan Chat 2 Arah:** Bisa langsung diajak ngobrol secara streaming di browser Kaggle atau dari OpenCode di komputer lokal.

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

## 💾 Dataset Model (Wajib)

Model yang digunakan sudah dipersiapkan dalam bentuk dataset Kaggle cache Ollama:

👉 **[Kaggle Dataset: gemma-31b-heretic-ollama](https://www.kaggle.com/datasets/muhammadariefsyah/gemma-31b-heretic-ollama)**

*(Dataset ini berisi file bobot GGUF Q4_K_M ~19 GB dan manifests Ollama yang siap dimount).*

---

## 🚀 Cara Pakai (3 Langkah Instan)

### Langkah 1: Buat Notebook Baru dari Dataset
1. Buka link dataset: [gemma-31b-heretic-ollama](https://www.kaggle.com/datasets/muhammadariefsyah/gemma-31b-heretic-ollama).
2. Klik tombol **New Notebook** di pojok kanan atas halaman dataset. Dataset otomatis langsung terpasang di `/kaggle/input/`.
3. Di panel kanan (*Notebook options*):
   - **Accelerator**: Pilih **GPU T4 x2**.
   - **Internet**: Pastikan **Internet on** (centang aktif).

### Langkah 2: Upload Notebook Ini ke Kaggle
1. Di menu atas notebook Kaggle, klik **File** $\to$ **Upload Notebook**.
2. Pilih file [`uncensored-ai-in-vm.ipynb`](./uncensored-ai-in-vm.ipynb) dari repository ini.

### Langkah 3: Masukkan Token Ngrok & Jalankan
1. Masukkan token Ngrok Anda pada **Sel 3**:
   ```python
   NGROK_TOKEN = "MASUKKAN_AUTHTOKEN_ANDA_DI_SINI"
   ```
2. Klik tombol **Run All** (atau jalankan Sel 1, 2, 3 berurutan).
3. Di output **Sel 3**, salin URL yang muncul di samping tulisan:  
   `🔥 URL UNTUK OPENCODE: https://xxxx-xx-xx.ngrok-free.dev/v1`

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
          "name": "Gemma 31B Heretic",
          "limit": {
            "context": 16384,
            "output": 8192
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
> - Nilai `output: 8192` mencegah error *"Response still truncated after two output token limit continuations"*.

---

## 🧠 Trik Utama: Symlink 0 MB (Anti-Crash)

Ukuran model 31B adalah **18.7 GB**, sementara batas disk writable Kaggle hanya **20 GB**. Jika file di-download atau di-copy biasa, disk langsung 100% penuh dan container dimatikan paksa (`Exit code: 137`).

* **`blobs`** (berisi file GGUF 18.7 GB) **hanya di-symlink** (shortcut) dari `/kaggle/input/` ke cache Ollama. Memakan waktu 0 detik dan 0 MB kuota disk!
* **`manifests`** (metadata JSON beberapa KB) **di-copy fisik** menggunakan `shutil.copy2` (wajib karena Ollama menolak jika manifests berupa symlink).

---

## 🛠️ Troubleshooting

| Kendala | Penyebab | Solusi |
|---|---|---|
| **Exit code 137 / SIGKILL** | Melakukan copy file 19 GB hingga storage Kaggle 20 GB habis. | Gunakan symlink untuk `blobs` (hanya `manifests` yang di-copy). |
| **`manifest symlink target outside model directory`** | Ollama menolak jika folder `manifests` berupa symlink. | Copy file manifest dengan `shutil.copy2` seperti pada Sel 2. |
| **HTTP 524 Timeout** | Cloudflare Tunnel memutus koneksi jika model mikir >100 detik. | Gunakan **Ngrok** (Sel 3) yang bebas dari limit timeout 100s. |
| **HTTP 404 Not Found** | URL di `opencode.json` tidak memiliki akhiran `/v1`. | Tambahkan `/v1` di akhir `baseURL`. |
| **Does not support thinking** | Mengatur dropdown reasoning di OpenCode ke *Low/Medium/High*. | Kembalikan dropdown reasoning ke **`Default`**. |
| **Response still truncated** | Jawaban panjang terpotong batas token bawaan. | Atur `"limit": { "context": 16384, "output": 8192 }` di `opencode.json`. |
