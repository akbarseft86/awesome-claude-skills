---
name: asmr-video-transition-web
description: Workflow untuk merancang aplikasi web pembuat transisi video ASMR bertema alam dengan preview interaktif yang panjang, mulus, dan natural tanpa proses editing yang rumit.
---

# Aplikasi Web Transisi Video ASMR Alam

Skill ini membantu Anda (atau Claude) merancang dan membangun aplikasi web yang secara otomatis membuat transisi video ASMR bertema alam lengkap dengan preview real-time. Pengguna dapat mengunggah footage, mengatur perpindahan, dan melihat hasilnya langsung di browser—tanpa perlu alur kerja editing yang ribet.

## Kapan Menggunakan Skill Ini

- Saat ingin membuat prototipe aplikasi editing video ringan berbasis web dengan fokus transisi ASMR
- Ketika perlu otomatisasi penyusunan montase footage alam dengan perpindahan halus dan dapat dipreview instan
- Untuk creator YouTube/Instagram/TikTok ASMR yang ingin efisiensi produksi via browser
- Ketika merancang progressive web app (PWA) yang dapat diakses lintas perangkat
- Saat perlu panduan UI/UX dan pipeline audio-visual untuk video relaksasi dengan preview layar penuh

## Sasaran Pengguna

1. **Pembuat konten ASMR** yang menginginkan preview cepat tanpa install software
2. **Videografer alam** yang butuh perapian footage cepat dari mana saja
3. **Developer indie** yang ingin membangun produk editing niche berbasis web
4. **Tim marketing** yang menyiapkan demo aplikasi relaksasi/meditasi langsung di browser

## Fitur Inti yang Direkomendasikan

1. **Manajemen Footage Alam**
   - Drag & drop banyak klip (4K/1080p) langsung ke canvas web
   - Metadata otomatis: lokasi, mood, jenis ambience, exposure, frame rate
2. **Generator Transisi Otomatis**
   - Crossfade adaptif dengan durasi 3–8 detik dan easing cosine
   - Color tone matching antar klip dengan WebGL shader ringan
   - Motion match berbasis optical flow WebAssembly untuk footage handheld
3. **Layer Audio ASMR**
   - Crossfade ambience (rain, forest, ocean) dengan kurva S
   - Volume automation agar tetap halus, dilihat di waveform
   - Library loop ambience dengan tag mood dan preview audio instan
4. **Preview Interaktif**
   - Timeline minimalis dengan waveform dan thumbnail scene
   - Panel preview 720p dengan tombol “Relax Preview” layar penuh dan mode picture-in-picture
   - Mode perbandingan sebelum/sesudah (split view) untuk koreksi warna
5. **Ekspor Tanpa Ribet**
   - Rendering server-side (FFmpeg worker) atau client-side (ffmpeg.wasm) dengan preset 4K Ultra Smooth, 1080p Streaming, Audio Only
   - Normalisasi loudness -23 LUFS untuk ASMR sebelum ekspor
6. **Automation Quality of Life**
   - Template sequence (Sunrise, Forest Walk, Ocean Night) yang dapat dipreview cepat
   - Penstabil halus (mild stabilization) dengan parameter slider
   - Penyesuaian warna sinematik ringan + LUT bawaan

## Arsitektur & Teknologi yang Disarankan

| Komponen | Rekomendasi | Catatan |
| --- | --- | --- |
| Frontend | **Next.js / Remix** + **React** + **TypeScript** | SSR/ISR untuk landing, SPA mode di editor |
| Rendering Video | **ffmpeg.wasm** untuk preview, **FFmpeg** di server worker/Edge function untuk ekspor | Simpan pipeline dalam JSON |
| Audio Processing | **Web Audio API** + **Tone.js** untuk mixing preview, **FFmpeg**/**SoX** untuk rendering akhir | Pastikan sinkronisasi audio-video |
| State Mgmt | Zustand/Redux Toolkit atau Recoil | Sinkron antar panel timeline & preview |
| Storage | IndexedDB (cache klip lokal), Supabase/Firestore untuk preset cloud | Mendukung autosave |
| Styling | TailwindCSS + komponen glassmorphism | Dominasi warna hijau tua & krem, dukung dark/light |
| Auth & Sharing | Magic link / OAuth + shareable preview link | Live preview dapat di-embed |

## Alur Kerja Pipeline

1. **Import**
   - Unggah klip (drag & drop) → service worker menyimpan di IndexedDB
   - Analisis metadata: frame rate, exposure, audio RMS, warna dominan via OffscreenCanvas
2. **Susun Timeline**
   - Algoritma `SmoothOrder` mengurutkan klip berdasarkan kesamaan warna & ambience
   - Tambahkan node transisi 5 detik default (dapat diubah slider)
   - Preview timeline digenerate dengan thumbnail otomatis dan waveform audio
3. **Generate Transisi**
   - Render preview resolusi 720p secara incremental memakai WebCodecs/WebGL
   - Terapkan exposure ramp & color LUT ringan
   - Crossfade audio dengan kurva S-curve, divisualisasikan di waveform
4. **Preview & Fine-tune**
   - UI slider untuk durasi transisi, intensitas color match, kekuatan stabilizer
   - Panel audio untuk atur timing ambience tambahan
   - Mode “Relax Preview” layar penuh dengan latar bokeh dan overlay timer napas
5. **Ekspor**
   - Jalankan pipeline FFmpeg (serverless worker atau backend Node) dengan filter_complex (xfade, afade, eq)
   - Tambahkan watermark opsional atau teks judul ASMR sebelum rendering final
   - Kirim hasil ke CDN / cloud storage dan sediakan link unduhan

## Modul Preview Interaktif

- **Video Preview Player**: menggunakan `<video>` + WebGL overlay untuk LUT dan split view
- **Timeline React Component**: virtualization untuk klip panjang, keyboard shortcut (Space play/pause, J/K/L scrub, Shift+Scroll zoom)
- **Audio Waveform Drawer**: gunakan Web Audio API + Canvas untuk waveform real-time
- **Feedback Panel**: indikator kualitas transisi (warna, eksposur, loudness) dengan badge hijau/kuning/merah

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
- **Preview Cache**: simpan buffer preview di IndexedDB dengan key `projectId:clipId:quality`
- **Progressive Enhancement**: fallback ke rendering server-side jika device tidak mendukung WebCodecs/WebGL
- **PWA Support**: tambahkan service worker untuk offline preview & caching ambience loop

## Contoh Prompt Penggunaan

### 1. Mendesain Fitur
```
Gunakan skill asmr-video-transition-web untuk membuat spesifikasi onboarding pengguna baru.
```

### 2. Membuat Komponen UI
```
Dengan skill asmr-video-transition-web, buat layout React + Tailwind untuk panel timeline dan preview.
```

### 3. Menyiapkan Pipeline FFmpeg
```
Aktifkan skill asmr-video-transition-web dan tulis script API route Node.js yang memanggil FFmpeg filter_complex untuk transisi crossfade natural 6 detik.
```

### 4. Skenario QA
```
Jalankan skill asmr-video-transition-web untuk daftar test case memastikan crossfade audio tetap mulus walau input berbeda durasi.
```

## Deliverable yang Diharapkan dari Claude

- Rencana produk & roadmap MVP berbasis web
- Wireframe UI/UX (dalam bentuk deskripsi atau kode JSX)
- Snippet kode untuk modul transisi & audio mixing (frontend + serverless)
- Skrip automasi FFmpeg / Node.js / serverless worker
- Template dokumentasi fitur untuk developer dan QA
- Copywriting marketing dalam bahasa Indonesia dan Inggris

## Tips Optimal

- Sertakan sample pack ambience (hujan, ombak, angin) dalam bucket `assets/ambience`
- Sediakan opsi “Zen Randomizer” untuk membuat playlist transisi otomatis lengkap dengan preview cepat
- Gunakan easing `cosine` untuk durasi crossfade agar terasa organik
- Pastikan UI mendukung dark mode default dan toggle ke mode terang hangat
- Tambahkan modul “Breathing Reminder” agar pengguna tetap rileks saat mengedit

Selamat membangun aplikasi web transisi video ASMR alam dengan preview menenangkan! 🌿
