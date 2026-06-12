# BRD: KolabPro — Business Requirements
**Project:** KolabPro

## 1. Latar Belakang & Konteks Bisnis
Manajemen proyek yang efektif menjadi tantangan besar bagi tim dan perusahaan di Indonesia. Banyak tim — terutama UKM dan startup — masih mengandalkan alat seadanya seperti WhatsApp group, spreadsheet, dan email untuk mengelola tugas dan proyek. Approach ini menyebabkan:

- **73% proyek** mengalami keterlambatan karena tracking manual
- **68% anggota tim** merasa tidak tahu tugas apa yang harus dikerjakan selanjutnya
- **62% project manager** kesulitan memonitor progress secara real-time
- **55%** tugas terlewat deadline karena tidak ada sistem pengingat yang terpusat

Sementara itu, tools manajemen proyek global seperti Asana, Trello, ClickUp, dan Monday.com menawarkan solusi, tetapi:
- Harga dalam USD — mahal untuk UKM Indonesia (Asana $10.99/user/bulan)
- Antarmuka Bahasa Inggris — menjadi hambatan adopsi
- Fitur terlalu kompleks untuk kebutuhan tim kecil
- Tidak ada template yang sesuai dengan budaya kerja Indonesia

KolabPro hadir sebagai platform manajemen proyek dan kolaborasi tim berbahasa Indonesia yang terjangkau, dengan fitur kanban, timeline, dan kolaborasi yang lengkap namun tetap mudah digunakan.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target (12 bulan) | Cara Ukur |
|--------|--------|--------------------|-----------|
| Mendapatkan pengguna aktif | Jumlah workspace aktif | 2.000 workspace | Database sistem |
| Meningkatkan produktivitas tim | Task completion rate | > 80% per proyek | Analytics sistem |
| Mengurangi missed deadline | % task selesai tepat waktu | > 90% | Sistem tracking |
| Monetisasi platform | Monthly Recurring Revenue (MRR) | Rp 100.000.000 | Sistem billing |
| Adopsi fitur kolaborasi | Komentar per task rata-rata | > 5 komentar/task | Database |
| Retensi pengguna | Churn rate bulanan | < 6% | Database pelanggan |
| Kepuasan pengguna | NPS Score | > 50 | Survey triwulanan |

## 3. Value Proposition

**Untuk Project Manager:**
- Pantau semua proyek dalam satu dashboard — progress, deadline, beban kerja tim
- Multiple view: Kanban untuk visual workflow, Timeline untuk perencanaan, List untuk detail
- Otomatisasi progress tracking — tidak perlu update manual
- Laporan progress siap presentasi dalam 1 klik

**Untuk Anggota Tim:**
- Tahu persis tugas apa yang harus dikerjakan hari ini
- Kolaborasi dalam satu tempat — komentar, mention, attachment
- Notifikasi deadline dan perubahan status
- Akses dari mana saja, kapan saja

**Untuk Pemilik Bisnis:**
- Visibilitas penuh terhadap semua proyek perusahaan
- Resource management — lihat beban kerja setiap anggota tim
- Data untuk pengambilan keputusan: proyek mana yang on-track dan mana yang butuh intervensi

## 4. Target Pengguna

### Primary Persona: Andi — Project Manager di Startup Teknologi
- **Usia:** 30 tahun
- **Pendidikan:** S1 Teknik Informatika
- **Pekerjaan:** Project Manager di startup fintech
- **Tanggung jawab:** Mengelola 3-5 proyek bersamaan, mengoordinasi 12 anggota tim (developer, designer, marketing)
- **Pain points:** Repot ngecek progress satu per satu ke anggota tim, susah lihat siapa yang overload, deadline sering terlewat karena komunikasi tercecer di WhatsApp
- **Goal:** Satu platform yang bisa lihat semua proyek, task, deadline, dan progress secara real-time

### Secondary Persona: Maya — Freelance Graphic Designer
- **Usia:** 26 tahun
- **Pendidikan:** D3 Desain Komunikasi Visual
- **Pekerjaan:** Freelance designer dengan 3-5 klien aktif
- **Kebutuhan:** Mengelola proyek dari setiap klien, deadline, feedback, dan revisi
- **Pain points:** Pusing tracking revisi di email, klien lupa ngasih feedback, file tercecer di berbagai tempat
- **Goal:** Simple project management untuk freelance — track task per klien, lampirkan file, dan komunikasi dalam satu tempat

## 5. Fitur Prioritas MVP

| Prioritas | Fitur | Keterangan |
|-----------|-------|------------|
| **P0** | Workspace management | Buat & kelola workspace (personal/tim/perusahaan) |
| **P0** | Proyek CRUD | Buat, edit, arsip, hapus proyek |
| **P0** | Kanban Board | 4 kolom status, drag-and-drop task, add task cepat |
| **P0** | Task CRUD | Buat, edit, hapus task dengan judul, assignee, deadline, prioritas |
| **P0** | Task assignment | Assign task ke anggota tim |
| **P0** | Undangan anggota | Tambah/hapus anggota workspace, role management |
| **P1** | Priority & Labels | Priority badge (4 level) dan label/tag task |
| **P1** | List View | Tabel view dengan sorting dan filtering |
| **P1** | Checklist dalam Task | Subtask/checklist dengan progress |
| **P1** | Komentar per Task | Diskusi dalam task, mention anggota |
| **P1** | File Attachment | Upload dan preview file |
| **P1** | Filter & Search | Filter by assignee, status, priority, label; search global |
| **P2** | Timeline / Gantt View | Visual timeline dengan dependency |
| **P2** | Calendar View | Kalender dengan task berdasarkan deadline |
| **P2** | Dashboard Proyek | Overview progress, task distribution, burndown chart |
| **P2** | Activity Log | Riwayat perubahan setiap task dan proyek |
| **P2** | Notifikasi | Notifikasi in-app dan email untuk deadline, mention, perubahan |

## 6. Model Distribusi
KolabPro adalah **Software-as-a-Service (SaaS)** berbasis web dengan model freemium:

**Tier Gratis:**
- 1 workspace
- 5 proyek aktif
- 10 anggota per workspace
- Kanban + List view
- Task dasar (tanpa checklist, labels)
- 100 MB penyimpanan file

**Tier Pro (Rp 79.000/bulan per workspace):**
- 20 proyek aktif
- 30 anggota per workspace
- Semua view: Board, List, Timeline, Calendar
- Checklist, labels, priority
- Filter & search lanjutan
- 2 GB penyimpanan file
- Export laporan

**Tier Business (Rp 199.000/bulan per workspace):**
- Proyek tak terbatas
- Anggota tak terbatas
- Semua fitur Pro
- Dashboard analitik
- 10 GB penyimpanan file
- Prioritas support (WA + telepon)
- Custom branding
- API akses

**Tier Enterprise (Rp 599.000/bulan):**
- Unlimited workspace
- SLA 99.9%
- Dedicated account manager
- On-premise / private cloud option
- Integrasi kustom
- Pelatihan tim

## 7. KPI & Metrik

| KPI | Definisi | Target Q1 | Target Q2 | Target Q3 | Target Q4 |
|-----|----------|-----------|-----------|-----------|-----------|
| Customer Acquisition Cost (CAC) | Biaya marketing / pelanggan baru | Rp 50.000 | Rp 40.000 | Rp 35.000 | Rp 25.000 |
| Monthly Active Workspaces | Workspace dengan aktivitas dalam 30 hari | 200 | 500 | 1.000 | 2.000 |
| Trial to Paid | % pengguna gratis ke berbayar | 10% | 15% | 20% | 25% |
| Task Created per User | Rata-rata task dibuat per user/bulan | 15 | 20 | 25 | 30 |
| Task Completion Rate | % task selesai tepat waktu | 70% | 75% | 80% | 85% |
| Churn Rate | % pelanggan berhenti | < 10% | < 8% | < 6% | < 5% |
| NPS | Survey kepuasan pengguna | 30 | 40 | 48 | 55 |

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | Posisi Kami |
|------------|-----------|------------|-------------|
| **Trello** | Simple, free tier murah, power-ups | Tidak ada timeline, reporting lemah, terbatas untuk proyek kompleks | Timeline built-in, view lebih lengkap, harga lokal |
| **Asana** | Fitur lengkap, timeline, reporting | Mahal ($10.99/user/bulan), UI kompleks, Inggris saja | Harga flat per workspace, UI sederhana, bahasa Indonesia |
| **ClickUp** | Fitur sangat lengkap, customizable | Learning curve tinggi, overload fitur, performa lambat | Fokus pada fitur esensial yang benar-benar dipakai, lebih ringan |
| **Monday.com** | Visual, automasi, integrations | Sangat mahal, bayar per user, Inggris | 10x lebih murah, bayar per workspace, tim besar tetap hemat |
| **Notion** | Fleksibel, database, docs | Bukan dedicated PM tool, tidak ada timeline/Gantt, offline lemah | Dedicated PM dengan views lengkap, lebih fokus |
| **WhatsApp + Spreadsheet** | Gratis, semua punya | Tidak terstruktur, tidak ada tracking, mudah hilang | Platform terstruktur, tracking real-time, kolaborasi terpusat |

**Keunggulan kompetitif KolabPro:**
- 100% Bahasa Indonesia — UI, notifikasi, dokumentasi
- Harga flat per workspace — tidak dihitung per user, lebih hemat untuk tim besar
- Multiple view (Board, List, Timeline, Calendar) dalam satu platform
- Checklist dalam task untuk breakdown pekerjaan
- File attachment built-in tanpa perlu integrasi tambahan

## 9. Risiko & Mitigasi

| Risiko | Dampak | Probabilitas | Mitigasi |
|--------|--------|--------------|----------|
| Adopsi rendah karena kebiasaan pakai Trello/Asana | Tinggi | Sedang | Campaign edukasi, migration tool, webinar perbandingan harga |
| Kompleksitas fitur membingungkan pengguna baru | Sedang | Sedang | Onboarding interaktif, tooltips, template proyek siap pakai |
| Performa drag-and-drop tidak optimal di browser tertentu | Sedang | Rendah | Testing multi-browser, fallback ke dropdown status |
| Keamanan data proyek klien | Tinggi | Sedang | Encryption data, backup harian, compliance sertifikasi |
| Churn karena keterbatasan fitur gratis | Sedang | Tinggi | Roadmap publik, feedback loop, rilis fitur baru tiap bulan |
| Kompetitor besar turun harga | Tinggi | Rendah | Fokus pada kualitas dan dukungan lokal, bangun komunitas setia |

## 10. Rencana Implementasi

### Fase 1 — Core Platform (Bulan 1-2)
- Setup Vite + React + TypeScript + Tailwind
- Layout utama: sidebar, topbar, main panel
- Workspace CRUD + switcher
- Proyek CRUD + anggota
- Kanban Board: 4 kolom, drag-and-drop
- Task CRUD: judul, assignee, deadline, prioritas

### Fase 2 — Views & Features (Bulan 3-4)
- List View dengan sorting dan filtering
- Checklist/subtask dalam task
- Komentar dan mention
- File attachment + preview
- Label/tag management
- Filter dan search global
- Task detail panel (right panel)

### Fase 3 — Advanced Features (Bulan 5-6)
- Timeline / Gantt Chart view
- Calendar view
- Dashboard proyek dengan grafik
- Activity log
- Notifikasi in-app
- Export laporan PDF

### Fase 4 — Scale & Monetize (Bulan 7-12)
- Subscription & billing system
- Team management (invite, roles)
- Template proyek siap pakai
- API publik
- Mobile app (React Native)
- Integrasi dengan alat lain (Google Calendar, Slack, email)
- Marketplace template project
