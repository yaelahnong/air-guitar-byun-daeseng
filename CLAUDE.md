Saya ingin membuat aplikasi web "Air Guitar Motion Studio" dalam satu file HTML.

Ini screenshot dari referensi app yang ingin saya buat:
@screenshots/instagram_byun_daeseng.png  

Dari screenshot tersebut, berikut yang saya pahami tentang cara kerjanya:

- Tangan kiri mendeteksi "pinch" (jempol + telunjuk menyentuh) untuk memilih chord dari grid
- Tangan kanan melakukan gerakan "downstroke/strumming" ke bawah untuk membunyikan chord yang dipilih
- Ada grid chord 2 baris × 8 kolom yang muncul sebagai overlay transparan di atas video kamera
- Baris atas: C, D, E, F, G, A, E7, B7
- Baris bawah: C7, Dm, Em, Fm, G7, A7, Am, F
- Tangan kiri divisualisasikan dengan bounding box berwarna cyan/teal
- Tangan kanan divisualisasikan dengan bounding box berwarna merah/orange
- Ada status bar di bawah yang menampilkan: beat saat ini, status tangan kanan (pick grip), status tangan kiri (pinch detected), dan audio output
- Ada kontrol di atas: display chord yang dipilih, slider pinch/strum threshold, checkbox preview, dan tombol swap hands
- UI dark theme dengan font bold putih, judul besar "Air Guitar Motion Studio"

Tolong buat aplikasi ini sebagai SATU FILE HTML yang bisa langsung dibuka di browser Chrome, menggunakan:
- MediaPipe Hands (via CDN) untuk deteksi tangan real-time dari webcam
- Web Audio API + Tone.js (via CDN) untuk audio synthesis chord gitar
- HTML5 Canvas untuk overlay visual di atas video
- Vanilla JavaScript, tanpa framework, tanpa backend

Detail implementasi yang diperlukan:
1. Minta izin webcam saat halaman dibuka
2. Deteksi pinch tangan kiri: hitung jarak landmark ujung jempol (4) dan ujung telunjuk (8), jika < threshold = chord dipilih sesuai posisi X/Y di grid
3. Deteksi strumming tangan kanan: bandingkan posisi Y pergelangan tangan frame ini vs sebelumnya, jika deltaY > threshold ke bawah = strum
4. Tambahkan cooldown 300ms setelah setiap strum agar tidak double-trigger
5. Bunyikan chord sebagai beberapa nada sekaligus dengan slight delay antar string (efek strum natural)
6. Gunakan synth dengan tone yang menyerupai gitar akustik (attack cepat, decay medium)
7. Highlight chord yang sedang dipilih di grid secara real-time

Pastikan kode rapi, terstruktur, dan diberi komentar yang jelas di setiap bagian penting.