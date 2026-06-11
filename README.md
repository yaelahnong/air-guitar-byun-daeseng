# 🎸 Air Guitar Motion Studio

Mainkan gitar di udara — tanpa gitar. Aplikasi web yang mendeteksi gerakan kedua
tangan lewat webcam: **tangan kiri memilih chord** dengan gerakan _pinch_ (jempol +
telunjuk), **tangan kanan membunyikan chord** dengan gerakan _strumming_ ke bawah.

Seluruh aplikasi ada dalam **satu file `index.html`** — tanpa build, tanpa backend,
tanpa dependensi yang perlu di-_install_. Cukup buka di browser.

> Terinspirasi dari referensi air guitar oleh [@byun_daeseng](https://instagram.com/byun_daeseng).

---

## ✨ Fitur

- 🖐️ **Deteksi 2 tangan real-time** dengan [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands).
- 🎵 **16 chord gitar** dalam grid 2 baris × 8 kolom (C, D, E, F, G, A, E7, B7, C7, Dm, Em, Fm, G7, A7, Am, F).
- 👌 **Pilih chord lewat pinch** — dekatkan ujung jempol & telunjuk tangan kiri di atas sel chord.
- 👇 **Bunyikan lewat downstroke** — gerakkan tangan kanan ke bawah untuk strum.
- 🎸 **Suara gitar akustik asli** (sample dari CDN), dengan _synth_ cadangan agar suara tetap keluar bila sample belum termuat.
- 🎚️ **Kontrol sensitivitas** pinch & strum lewat slider.
- 🔀 **Tombol Tukar Tangan** bila peran tangan terbalik.
- 📊 **Status bar** menampilkan beat, status tiap tangan, dan level audio output.
- 🌑 Antarmuka _dark theme_ dengan overlay grid transparan di atas video.

---

## 🚀 Cara Menjalankan

**Cukup buka `index.html` di Google Chrome** (double-click). Chrome memperlakukan
`file://` sebagai _secure context_, jadi akses webcam tetap diizinkan — tidak perlu
server atau instalasi apa pun.

1. Buka `index.html` di **Google Chrome**.
2. Klik tombol **▶ Mulai** (memberi izin webcam + mengaktifkan audio).
3. Tunggu beberapa detik sampai status audio berubah menjadi **`Sound ON · gitar`** (sample gitar selesai diunduh).
4. Posisikan kedua tangan di depan kamera, lalu mainkan!

> 💡 Butuh koneksi internet, karena pustaka (MediaPipe, Tone.js) dan sample gitar
> dimuat via CDN.

<details>
<summary>Alternatif: lewat server lokal (opsional)</summary>

Diperlukan hanya jika kamera diblokir (konfigurasi Chrome yang ketat) atau jika
memakai browser lain seperti **Firefox**, yang memblokir kamera pada `file://`:

```bash
git clone https://github.com/<username>/air-guitar-byun-daeseng.git
cd air-guitar-byun-daeseng

python3 -m http.server 8000   # atau: npx serve
```

Lalu buka <http://localhost:8000>.
</details>

---

## 🎮 Cara Memainkan

| Aksi | Tangan | Gerakan |
|------|--------|---------|
| **Pilih chord** | Kiri (kotak cyan) | _Pinch_ — sentuhkan ujung jempol & telunjuk di atas sel chord pada grid |
| **Bunyikan chord** | Kanan (kotak merah) | _Downstroke_ — gerakkan tangan ke bawah seperti memetik |

Kontrol di bar atas:

- **Pinch / Strum slider** — atur sensitivitas. Naikkan bila gerakan kecil belum terdeteksi, turunkan bila terlalu sensitif.
- **live: pinch / strum** — nilai sensor langsung, untuk membantu kalibrasi slider.
- **Preview** — bila dicentang, chord berbunyi pelan saat dipilih (sebelum di-strum).
- **🔊 Tes Suara** — uji jalur audio tanpa gestur.
- **⇄ Tukar Tangan** — tukar peran kiri/kanan bila terbalik.

---

## 🛠️ Cara Kerja (Teknis)

| Bagian | Implementasi |
|--------|--------------|
| **Deteksi tangan** | MediaPipe Hands (21 landmark per tangan, via CDN) |
| **Deteksi pinch** | Jarak Euclidean landmark ujung jempol (4) ↔ ujung telunjuk (8) < threshold |
| **Pemetaan chord** | Titik tengah pinch dipetakan ke sel grid berdasarkan koordinat X/Y |
| **Deteksi strum** | Kecepatan vertikal pergelangan (landmark 0) **per detik** melewati threshold (independen frame rate), dengan _cooldown_ 300 ms anti double-trigger |
| **Audio** | [Tone.js](https://tonejs.github.io) — `Tone.Sampler` (sample gitar asli) dengan _fallback_ `PolySynth(MonoSynth)` |
| **Efek strum** | Tiap nada chord dibunyikan berurutan dengan jeda kecil (senar rendah → tinggi) |
| **Visual** | HTML5 Canvas — video _mirror_, grid chord, _bounding box_ + skeleton tangan |

---

## 📦 Teknologi

- [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands) — pelacakan tangan
- [Tone.js](https://tonejs.github.io) — sintesis & pemutaran audio
- Sample gitar dari [nbrosowsky/tonejs-instruments](https://github.com/nbrosowsky/tonejs-instruments)
- HTML5 Canvas + Web Audio API
- Vanilla JavaScript (tanpa framework)

---

## 🧰 Troubleshooting

| Masalah | Solusi |
|---------|--------|
| Kamera tidak muncul | Pastikan dibuka via `http://localhost`, bukan `file://`. Beri izin webcam saat diminta. |
| Tidak ada suara | Klik **🔊 Tes Suara**. Cek tab Chrome tidak di-_mute_ (klik kanan tab). Pastikan output device benar. |
| Suara masih _synth_, bukan gitar | Tunggu hingga status menjadi `Sound ON · gitar` (sample sedang diunduh). Butuh internet. |
| Peran tangan terbalik | Klik **⇄ Tukar Tangan**. |
| Chord susah dipilih / strum tak terdeteksi | Atur slider **Pinch** / **Strum**, sambil melihat nilai `live:`. |

---

## 📄 Lisensi

MIT — bebas digunakan dan dimodifikasi.
