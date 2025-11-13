---
name: asmr-video-transition-desktop
description: Workflow untuk merancang aplikasi desktop pembuat transisi video ASMR bertema alam yang panjang, mulus, dan natural tanpa proses editing yang rumit.
---

# Aplikasi Desktop Transisi Video ASMR Alam

Skill ini membantu Anda (atau Claude) merancang dan membangun aplikasi desktop yang secara otomatis membuat transisi video ASMR bertema alam dengan durasi panjang, perpindahan mulus, dan nuansa natural—tanpa perlu alur kerja editing yang ribet.

## Kapan Menggunakan Skill Ini

- Saat ingin membuat prototipe aplikasi editing video ringan fokus pada transisi ASMR
- Ketika perlu otomatisasi penyusunan montase footage alam dengan perpindahan halus
- Untuk creator YouTube/Instagram/TikTok ASMR yang ingin efisiensi produksi
- Ketika merancang tool desktop offline yang sederhana tapi powerful
- Saat perlu panduan UI/UX dan pipeline audio-visual untuk video relaksasi

## Sasaran Pengguna

1. **Pembuat konten ASMR** yang ingin konsistensi transisi tanpa software kompleks
2. **Videografer alam** yang butuh perapian footage cepat
3. **Developer indie** yang ingin membangun produk editing niche
4. **Tim marketing** yang menyiapkan demo aplikasi relaksasi/meditasi

## Fitur Inti yang Direkomendasikan

1. **Manajemen Footage Alam**
   - Drag & drop banyak klip (4K/1080p)
   - Metadata otomatis: lokasi, mood, jenis ambience
2. **Generator Transisi Otomatis**
   - Crossfade adaptif dengan durasi 3–8 detik
   - Blend exposure & white balance antar klip
   - Motion match ringan untuk footage handheld
3. **Layer Audio ASMR**
   - Crossfade ambience (rain, forest, ocean)
   - Volume automation agar tetap halus
   - Library loop ambience dengan tag mood
4. **Preview Real-time**
   - Timeline minimalis dengan waveform
   - Tombol “Relax Preview” layar penuh
5. **Ekspor Tanpa Ribet**
   - Preset 4K Ultra Smooth, 1080p Streaming, Audio Only
   - Normalisasi loudness -23 LUFS untuk ASMR
6. **Automation Quality of Life**
   - Template sequence (Sunrise, Forest Walk, Ocean Night)
   - Penstabil halus (mild stabilization)
   - Penyesuaian warna sinematik ringan

## Arsitektur & Teknologi yang Disarankan

| Komponen | Rekomendasi | Catatan |
| --- | --- | --- |
| UI Desktop | **Electron + React** atau **Tauri + Svelte** | Cross-platform, mudah dipaketkan |
| Rendering Video | **FFmpeg** via wrapper (ffmpeg.wasm untuk preview, ffmpeg CLI untuk ekspor) | Gunakan script pipeline JSON |
| Audio Mixing | **librosa** (analisis), **SoX**/**FFmpeg** (render) | Loudness normalization, fade |
| Storage | **SQLite** untuk preset & metadata | Simpan template transisi |
| State Mgmt | Zustand/Redux (React) atau Store bawaan Svelte | Sinkron antar panel |
| Styling | TailwindCSS + komponen gelap (relax vibes) | Dominasi warna hijau tua & krem |

## Alur Kerja Pipeline

1. **Import**
   - Analisis metadata: frame rate, exposure, audio RMS
   - Deteksi warna dominan untuk transisi tone-matching
2. **Susun Timeline**
   - Algoritma `SmoothOrder` mengurutkan klip berdasarkan kesamaan warna & ambience
   - Tambahkan node transisi 5 detik default (dapat disesuaikan pengguna)
3. **Generate Transisi**
   - Render preview resolusi rendah (720p) secara incremental
   - Terapkan exposure ramp & color LUT ringan
   - Crossfade audio dengan kurva S-curve
4. **Preview & Fine-tune**
   - UI slider untuk durasi transisi, intensitas color match, kekuatan stabilizer
   - Panel audio untuk atur timing ambience tambahan
5. **Ekspor**
   - Jalankan pipeline FFmpeg: stitching + filter_complex (xfade, afade, eq)
   - Tambahkan watermark opsional atau teks judul ASMR

## Detail Implementasi Penting

- **Preset Transisi**: Simpan dalam JSON, contoh:
  ```json
  {
    "name": "Forest Breeze",
    "video": {"transition": "crossfade", "duration": 6, "colorMatch": 0.7},
    "audio": {"fade": "s_curve", "padTail": 3},
    "stabilization": {"enable": true, "strength": 0.35}
  }
  ```
- **Algoritma Pengurutan**: gunakan skor `similarity = 0.5 * colorDistance + 0.3 * brightnessDiff + 0.2 * audioTexture`
- **UI Inspiration**: mood board dari aplikasi meditasi (Calm, Headspace) dan editing (Descript, VN)
- **Hotkeys**: Space (play/pause), J/K/L (scrub), Shift+Scroll (zoom timeline)
- **Relax Mode**: Fullscreen preview dengan efek bokeh background dan timer

## Contoh Prompt Penggunaan

### 1. Mendesain Fitur
```
Gunakan skill asmr-video-transition-desktop untuk membuat spesifikasi fitur onboarding pengguna baru.
```

### 2. Membuat Component UI
```
Dengan skill asmr-video-transition-desktop, buat layout React + Tailwind untuk panel timeline dan preview.
```

### 3. Menyiapkan Pipeline FFmpeg
```
Aktifkan skill asmr-video-transition-desktop dan tulis script Node.js yang memanggil FFmpeg filter_complex untuk transisi crossfade natural 6 detik.
```

### 4. Skenario QA
```
Jalankan skill asmr-video-transition-desktop untuk daftar test case memastikan crossfade audio tetap mulus walau input berbeda durasi.
```

## Deliverable yang Diharapkan dari Claude

- Rencana produk & roadmap MVP
- Wireframe UI/UX (dalam bentuk deskripsi atau kode JSX/Svelte)
- Snippet kode untuk modul transisi & audio mixing
- Skrip automasi FFmpeg / Node.js / Python
- Template dokumentasi fitur untuk developer dan QA
- Copywriting marketing dalam bahasa Indonesia dan Inggris

## Tips Optimal

- Sertakan sample pack ambience (hujan, ombak, angin) dalam folder `assets/ambience`
- Sediakan opsi “Zen Randomizer” untuk membuat playlist transisi otomatis
- Gunakan easing `cosine` untuk durasi crossfade agar terasa organik
- Pastikan UI mendukung dark mode default dan toggle ke mode terang hangat
- Tambahkan modul “Breathing Reminder” agar pengguna tidak stres saat editing

Selamat membangun aplikasi desktop transisi video ASMR alam yang menenangkan! 🌿
