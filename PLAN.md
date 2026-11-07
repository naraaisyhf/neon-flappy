# PLAN.md — Universal 2D Arcade Game Roadmap (Untuk Siswa)

Dokumen ini adalah rencana kerja universal dan log perkembangan proyek pembuatan game arcade 2D dalam 1 file `index.html`. Dokumen ini diperbarui secara otomatis oleh AI setiap kali ada perubahan kode sebelum push ke GitHub.

---

## 1. Ikhtisar Game

- **Judul Game**: **NEON FLAPPY**
- **Genre**: 2D Arcade Game — Endless Runner / Flappy
- **Kontrol**: Sentuh (tap) & Mouse (klik) — fallback keyboard `Space` / `ArrowUp`
- **Aturan Main**: Terbang melewati celah pipa. Kena pipa, langit-langit, atau tanah = Game Over. Tidak ada kondisi menang; permainan berjalan tanpa batas.
- **Skor**: +1 per pipa yang berhasil dilewati. Pipa berikutnya tidak dihitung sebelum pipa sebelumnya lewat.
- **Kesulitan**: Kecepatan scroll dan jarak antar pipa meningkat bertahap setiap 10 poin; celah menyempit hingga batas minimum.
- **Gaya Visual**: Neon Glow — latar gelap, garis neon dengan `shadowBlur`, grid/partikel parallax
- **Format**: Single-File HTML (`index.html`) tanpa dependensi eksternal
- **Platform**: Web Browser (Desktop & Mobile Responsive)
- **Audio**: Web Audio API Sintetis (flap, score, hit, gameover)
- **Penyimpanan**: LocalStorage (`arcade_leaderboard`, `arcade_last_username`)
- **Kebijakan Caching**: Zero-Cache (`no-cache, no-store, must-revalidate`)
- **Deployment**: GitHub Pages (Branch `main`, path `/`)
- **Live Demo**: <https://naraaisyhf.github.io/neon-flappy/>
- **Repository**: <https://github.com/naraaisyhf/neon-flappy>

### 1a. Spesifikasi Desain Terkunci (NEON FLAPPY)

- **Resolusi logis**: 480 × 720 (rasio 9:16), diskalakan responsif dengan letterbox.
- **Pemain**: satu burung neon, posisi X tetap di ~25% lebar layar; hanya bergerak vertikal.
- **Fisika**: gravitasi ~1500 px/s², impuls flap ~-430 px/s; rotasi sprite mengikuti kecepatan vertikal. Menggunakan delta time agar konsisten di semua refresh rate.
- **Rintangan**: pipa atas + bawah dengan celah. Lebar pipa ~70 px, celah awal ~190 px menyempit ke ~140 px, kecepatan scroll awal ~160 px/s naik ke ~300 px/s.
- **Kemunculan**: satu pasang pipa setiap ~1.6 detik, dengan variasi posisi celah agar tidak monoton.
- **Skor**: +1 saat pipa melewati sisi kiri burung (flag `scored` per pipa mencegah hitung ganda).
- **Juice**: partikel burst saat flap dan saat skor, screen shake singkat saat tabrakan.
- **Kondisi Game Over**: tabrakan AABB dengan pipa, menyentuh lantai, atau keluar batas atas.

### 1b. Pengembangan v1.1 — Mode, Koin, dan Karakter

- **Mode**: `ENDLESS` (default), `TIME ATTACK 60s` (timer habis = Game Over), `HARDCORE` (celah 82%, kecepatan +25%, tanpa power-up).
- **Koin**: muncul di celah pipa dengan posisi vertikal acak, bernilai **+5 skor**.
- **Power-up**: `SHIELD` (menahan 1 tabrakan + 2 detik invulnerabilitas) dan `SLOW` (memperlambat dunia 55% selama 4 detik).
- **Karakter**: 4 skin kosmetik (NEON, AQUA, MAGMA, GHOST); tidak mengubah statistik agar papan skor tetap adil.
- **Leaderboard per mode**: `arcade_leaderboard` (endless), `arcade_leaderboard_time`, `arcade_leaderboard_hardcore`.
- **Preferensi**: mode terakhir di `arcade_mode`, karakter di `arcade_skin`.

---

## 2. Fase Pengembangan Universal

### Phase 0: Brainstorming & Desain Game
- [x] Siswa menentukan ide & tema game → **Flappy Arcade** dengan tema **NEON FLAPPY**.
- [x] Menentukan kontrol permainan → **Sentuh (tap) + Mouse (klik)**, fallback `Space`/`ArrowUp`.
- [x] Menentukan aturan menang/kalah dan sistem penambahan skor → tanpa kondisi menang; kena pipa/lantai/langit-langit = Game Over; **+1 skor per pipa**.
- [x] Menentukan gaya visual → **Neon Glow** di atas latar gelap.

### Phase 1: Setup Single-File `index.html`
- [x] Buat kerangka HTML5 dengan meta tag **Zero-Cache**.
- [x] Siapkan kanvas responsif dengan rasio layar tetap (480×720, letterbox, DPR-aware).
- [x] Susun struktur kode modular 8 blok di JavaScript.
- [x] Siapkan game loop utama (`requestAnimationFrame` + delta time).

### Phase 2: Username & Leaderboard LocalStorage
- [x] Buat modal input username dengan validasi nama (1–15 karakter, anti-XSS).
- [x] Simpan dan baca username terakhir dari `arcade_last_username`.
- [x] Buat fungsi penyimpanan dan penyortiran Top 10 skor di `arcade_leaderboard`.
- [x] Buat tampilan antarmuka (UI) tabel peringkat skor di menu game.

### Phase 3: Mekanik Inti Permainan (Core Gameplay)
- [x] Sistem status game (`MENU`, `PLAYING`, `PAUSED`, `GAMEOVER`).
- [x] Karakter pemain (pergerakan dan batas layar).
- [x] Rintangan / musuh (kecepatan, variasi, dan pola kemunculan).
- [x] Deteksi tabrakan (Collision Detection) dan perhitungan skor.

### Phase 4: Efek Suara & Visual Juice
- [x] Efek suara arcade sintetis dengan Web Audio API (Start, Gerak, Tabrakan, Game Over).
- [x] Efek ledakan partikel sederhana saat tabrakan/poin didapat.
- [x] Efek getar layar (screen shake) saat game over.
- [x] Tambahan: toggle SFX (tersimpan), pause manual/otomatis, flash layar saat tabrakan.

### Phase 5: Publikasi GitHub Pages
- [x] Inisialisasi git dan branch `main`.
- [x] Buat repository GitHub publik via GitHub CLI (`gh repo create <repo> --public --source=. --remote=origin --push`). *Wajib berstatus Public agar GitHub Pages gratis dapat aktif.*
- [x] Aktifkan GitHub Pages (Metode CLI `gh api` atau manual via Web UI Settings > Pages branch `main` root `/`).
- [x] Kompatibilitas Google AI Studio: Sediakan snippet perintah terminal siap salin jika proyek dilanjutkan lewat antarmuka web.
- [x] Pantau status workflow build di tab Actions (`gh run list`).
- [x] Tes live demo (`https://naraaisyhf.github.io/neon-flappy/`) dan pastikan pembaruan langsung tampil tanpa hambatan cache (Zero-Cache test).

### Phase 6: Pengembangan Lanjutan (v1.1)
- [x] Mode Endless, Time Attack 60s, dan Hardcore dengan papan skor terpisah.
- [x] Koin +5 dengan posisi vertikal acak di dalam celah.
- [x] Power-up shield dan slow-motion beserta indikator HUD.
- [x] Pemilihan karakter (4 skin kosmetik) yang tersimpan di LocalStorage.

---

## 3. Matriks Tugas & Status Pelaksanaan

| ID | Tugas / Fitur | Fase | Status | Pelaksana | Catatan |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **T-00** | Pembuatan template `AGENTS.md` & `PLAN.md` untuk Siswa | Fondasi | `DONE` | AI | Template standar single model siap |
| **T-01** | Brainstorming Ide & Konsep Game Spesifik | Phase 0 | `DONE` | Siswa | Terkunci: NEON FLAPPY, tap/klik, Neon Glow |
| **T-02** | Pembuatan Skeleton `index.html` & Meta Zero-Cache | Phase 1 | `DONE` | AI | 8 blok modular, kanvas 480×720 DPR-aware |
| **T-03** | Modul Input Username & Leaderboard Top 10 | Phase 2 | `DONE` | AI | Modal nama + tabel Top 10 tersimpan |
| **T-04** | Logika Pergerakan, Rintangan & Tabrakan | Phase 3 | `DONE` | AI | Gravitasi, pipa, circle-rect, skor +1 |
| **T-05** | Efek Suara Synth Web Audio API & Partikel | Phase 4 | `DONE` | AI | 4 SFX, partikel, screen shake, flash |
| **T-06** | Deployment GitHub Pages via GitHub CLI (`gh`) | Phase 5 | `DONE` | AI | Live di <https://naraaisyhf.github.io/neon-flappy/> (HTTP 200) |
| **T-07** | Koin & Power-up (shield, slow-motion) | Phase 6 | `DONE` | AI | +5 per koin, item di celah pipa |
| **T-08** | Mode Endless / Time Attack / Hardcore | Phase 6 | `DONE` | AI | Leaderboard terpisah per mode |
| **T-09** | Pemilihan karakter (4 skin) | Phase 6 | `DONE` | AI | Tersimpan di `arcade_skin` |

*Status: `TODO` | `IN-PROGRESS` | `DONE`*

---

## 4. Changelog & Riwayat Revisi

| Versi | Tanggal | Pelaksana | Ringkasan Perubahan | Referensi Commit |
| :--- | :--- | :--- | :--- | :--- |
| **v1.1.0** | 2026-11-07 | AI | Mode Time Attack & Hardcore, koin +5, power-up shield/slow-mo, 4 karakter, leaderboard per mode | `aea01a9` |
| **v1.0.1** | 2026-10-31 | AI | Deploy publik ke GitHub Pages: <https://naraaisyhf.github.io/neon-flappy/> | `cafb670` |
| **v1.0.0** | 2026-10-31 | AI | Rilis `index.html` NEON FLAPPY: 8 blok modular, leaderboard, fisika, audio, partikel, pause | — |
| **v0.3.0** | 2026-10-31 | AI | Phase 0 selesai: konsep NEON FLAPPY (tap/klik, Neon Glow) beserta spesifikasi fisika, skor, dan kesulitan | — |
| **v0.2.2** | 2026-10-24 | AI | Menambahkan panduan alur integrasi khusus Google AI Studio & penegasan repo wajib Public | `07475fb` |
| **v0.2.1** | 2026-10-24 | AI | Menambahkan panduan detail aktivasi GitHub Pages (CLI gh api & Web UI visual) serta troubleshooting | `3aaf671` |
| **v0.2.0** | 2026-10-24 | AI | Konfigurasi ulang ke Single Model AI untuk siswa, modularisasi 8 blok kode, dan penyesuaian matriks tugas | `2954bb0` |
| **v0.1.0** | 2026-10-24 | AI | Inisialisasi awal file sistem `AGENTS.md` dan universal blueprint `PLAN.md` | `47aa88e` |
