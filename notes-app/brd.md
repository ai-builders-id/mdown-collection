# BRD: Catat.in — Business Requirements

## 1. Latar Belakang & Konteks Bisnis
Masyarakat Indonesia, terutama pekerja pengetahuan, pelajar, dan kreator, membutuhkan aplikasi catatan yang simpel, cepat, dan mendukung markdown. Banyak aplikasi catatan yang ada terlalu kompleks (Notion), terlalu terbatas (Google Keep), atau tidak mendukung organisasi folder (Apple Notes). Catat.in hadir sebagai aplikasi catatan yang menggabungkan kemudahan markdown, organisasi folder hierarkis, dan tag system — semuanya offline-first dengan Bahasa Indonesia.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target |
|--------|--------|--------|
| Memudahkan pencatatan cepat | Waktu dari buka apps → mulai nulis | < 3 detik |
| Organisasi catatan rapi | % catatan yang masuk folder/tag | > 80% |
| Mendorong kebiasaan menulis | Rata-rata catatan per user per minggu | > 5 note |
| Retensi pengguna | % user aktif dalam 30 hari | > 40% |
| Kepuasan pengguna | Rating di store/feedback | > 4.5/5 |

## 3. Value Proposition
- **Markdown-native**: Tulis dalam markdown, preview otomatis, tanpa perlu tombol format berantakan
- **Folder + Tags**: Dua dimensi organisasi — struktural (folder) dan fleksibel (tag)
- **Offline-first**: Semua catatan tersimpan lokal, akses tanpa internet, sync cloud opsional
- **Minimal & Cepat**: Loading instan, UI bersih, fokus ke tulisan
- **Bahasa Indonesia**: Antarmuka dan konten default dalam Bahasa Indonesia

## 4. Target Pengguna

| Persona | Peran | Kebutuhan Utama |
|---------|-------|-----------------|
| Rizki Pratama | Developer (28 th) | Markdown notes, code snippets, daily journal |
| Dina Amalia | Mahasiswa (21 th) | Catatan kuliah, organisasi folder per matkul, search |
| Pak Agus | Penulis Blog (45 th) | Draft artikel, referensi, version history |
| Sari Dewi | Project Manager (34 th) | Notulensi rapat, todo list, tagging per topic |

## 5. Fitur Prioritas MVP

| Prioritas | Fitur |
|-----------|-------|
| P0 | Create, edit, delete note dengan markdown |
| P0 | Live preview markdown |
| P0 | Folder tree + nested folder |
| P0 | Search (judul + konten) |
| P1 | Tag management |
| P1 | Pin, favorite, archive |
| P1 | Auto-save + save indicator |
| P1 | Dark mode |
| P1 | Keyboard shortcuts |
| P2 | Export markdown / PDF |
| P2 | Version history |
| P2 | Cloud sync (optional) |

## 6. Model Distribusi
- **Web App (PWA)**: Akses via browser, bisa install ke desktop/HP
- **Chrome Extension (Phase 2)**: Quick capture dari browser
- **Deployment**: Static site di Vercel/Netlify
- **Pricing**:
  - Free: Unlimited local notes, 5 folder, basic features
  - Pro: Unlimited folder, cloud sync, export PDF, version history
  - (Catat.in fokus ke free dulu untuk adopsi)

## 7. KPI & Metrik
- Daily Active Users (DAU): Target growth 20% month-over-month
- Notes created per user per week: > 5
- Session duration: Rata-rata 8-15 menit
- Search usage: > 30% user menggunakan search setiap minggu
- PWA install rate: > 15% dari total pengunjung

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan |
|------------|-----------|------------|
| **Notion** | Super fleksibel, database, kolaborasi | Berat, lambat, kompleks untuk catatan simpel |
| **Obsidian** | Markdown-first, graph view, plugin | Butuh belajar, tidak ada cloud bawaan gratis |
| **Google Keep** | Cepat, simple, reminder | Tanpa folder, tanpa markdown, organisasi terbatas |
| **Apple Notes** | Sync sempurna di ekosistem Apple | Hanya Apple, markdown terbatas |
| **Simplenote** | Minimal, cepat, sync | Tanpa folder, markdown terbatas, UI kuno |

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Data hilang (local storage corrupt) | Tinggi | Auto-backup ke IndexedDB, export reminder |
| Kompetisi tinggi (Notion, Obsidian) | Tinggi | Fokus ke simplicity + Bahasa Indonesia |
| User bingung markdown syntax | Sedang | Toolbar minimal, cheatsheet built-in |
| Sync conflict (multi device) | Sedang | Last-write-wins + version history sebagai fallback |

## 10. Rencana Implementasi
- **Phase 1 (Minggu 1-2)**: Foundation — CRUD notes, markdown editor + preview, autosave
- **Phase 2 (Minggu 3-4)**: Organisasi — Folder tree, tags, pin/favorite/archive
- **Phase 3 (Minggu 5-6)**: Pencarian — Search full-text, highlight, filter by folder/tag
- **Phase 4 (Minggu 7-8)**: PWA — Install prompt, offline mode, dark mode, keyboard shortcuts
