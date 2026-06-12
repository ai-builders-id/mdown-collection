# FRD: KolabPro — Functional Requirements
**Project:** KolabPro

## 1. Scope
KolabPro adalah platform manajemen proyek dan kolaborasi tim berbasis web yang memungkinkan tim untuk mengelola tugas, melacak progress, dan berkolaborasi dalam satu workspace terpadu. Sistem mendukung multiple view (Kanban Board, List, Timeline, Calendar) dan fitur kolaborasi real-time.

**In Scope:**
- Manajemen workspace, proyek, dan task
- Kanban Board dengan drag-and-drop
- List, Timeline (Gantt), dan Calendar view
- Task assignment, prioritas, label, checklist
- Komentar, mention, dan file attachment
- Filter, search, dan sorting
- Dashboard proyek dengan grafik progress
- Activity log
- Manajemen anggota tim dengan role
- Notifikasi in-app

**Out of Scope:**
- Integrasi dengan alat eksternal (Google Calendar, Slack, GitHub) — fase 2
- Time tracking / timesheet — fitur terpisah
- Invoice dan billing untuk klien
- CRM atau pipeline penjualan
- Real-time collaborative editing (seperti Google Docs)
- Aplikasi mobile native — fase 2

## 2. Modul & Fitur

### Modul A — Workspace & Proyek

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-A-001 | Membuat workspace baru (personal/tim/perusahaan) | P0 |
| FRD-A-002 | Mengedit profil workspace (nama, deskripsi, logo, warna) | P0 |
| FRD-A-003 | Switch antar workspace | P0 |
| FRD-A-004 | Hapus workspace dengan konfirmasi berlapis | P0 |
| FRD-A-005 | Membuat proyek baru dalam workspace | P0 |
| FRD-A-006 | Mengedit proyek (nama, deskripsi, prioritas, tanggal) | P0 |
| FRD-A-007 | Arsip dan restore proyek | P0 |
| FRD-A-008 | Hapus proyek (soft delete, restore dalam 30 hari) | P1 |
| FRD-A-009 | Menandai proyek sebagai favorit | P1 |
| FRD-A-010 | Mengatur status proyek (aktif/dijeda/selesai/diarsipkan) | P0 |
| FRD-A-011 | Progress bar proyek otomatis dari task completion | P0 |
| FRD-A-012 | Tag proyek untuk kategorisasi | P1 |

### Modul B — Task Management

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-B-001 | Membuat task baru dengan form cepat (inline di kolom) | P0 |
| FRD-B-002 | Membuat task dengan form lengkap (modal) | P0 |
| FRD-B-003 | Mengedit task (inline edit judul, modal untuk detail) | P0 |
| FRD-B-004 | Menghapus task (soft delete) | P0 |
| FRD-B-005 | Drag-and-drop task antar status kolom | P0 |
| FRD-B-006 | Drag-and-drop untuk reorder dalam kolom | P1 |
| FRD-B-007 | Assign task ke anggota tim (single assignee) | P0 |
| FRD-B-008 | Set prioritas: rendah, sedang, tinggi, urgent | P0 |
| FRD-B-009 | Set deadline dengan date picker | P0 |
| FRD-B-010 | Label/tag task (multiple, dengan warna) | P1 |
| FRD-B-011 | Checklist / subtask dalam task | P1 |
| FRD-B-012 | Duplikasi task | P1 |
| FRD-B-013 | Task dengan parent (parent task ID untuk subtask) | P1 |
| FRD-B-014 | Estimasi jam pengerjaan | P1 |
| FRD-B-015 | Bulk action: pilih banyak task untuk ubah status/priority/hapus | P2 |

### Modul C — View & Visualization

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-C-001 | Board View: Kanban dengan 4 kolom default (Belum, Dikerjakan, Tinjau, Selesai) | P0 |
| FRD-C-002 | Kolom kanban bisa di-collapse/expand | P1 |
| FRD-C-003 | List View: tabel dengan kolom status, judul, prioritas, assignee, deadline, labels | P1 |
| FRD-C-004 | List View: sorting per kolom (asc/desc) | P1 |
| FRD-C-005 | List View: inline edit untuk judul dan status | P1 |
| FRD-C-006 | Timeline View: Gantt chart dengan task bar berdasarkan deadline | P2 |
| FRD-C-007 | Timeline View: zoom level (hari/minggu/bulan) | P2 |
| FRD-C-008 | Timeline View: indikator today (garis vertikal) | P2 |
| FRD-C-009 | Calendar View: month grid dengan task card per tanggal deadline | P2 |
| FRD-C-010 | Calendar View: drag task untuk ubah deadline | P2 |
| FRD-C-011 | View switcher di topbar untuk ganti view | P0 |

### Modul D — Kolaborasi

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-D-001 | Komentar per task (input teks) | P1 |
| FRD-D-002 | Mention anggota tim dengan @nama | P1 |
| FRD-D-003 | File attachment ke task (upload dari lokal) | P1 |
| FRD-D-004 | Preview attachment (gambar, dokumen, link) | P1 |
| FRD-D-005 | Hapus attachment | P1 |
| FRD-D-006 | Activity log (riwayat perubahan task dan proyek) | P1 |
| FRD-D-007 | Undang anggota baru ke workspace via email | P0 |
| FRD-D-008 | Role anggota: pemilik, admin, anggota, pengamat | P0 |
| FRD-D-009 | Hapus anggota dari workspace | P0 |

### Modul E — Filter, Search & Notifikasi

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-E-001 | Filter task berdasarkan assignee | P1 |
| FRD-E-002 | Filter task berdasarkan prioritas | P1 |
| FRD-E-003 | Filter task berdasarkan label | P1 |
| FRD-E-004 | Filter task berdasarkan status | P0 |
| FRD-E-005 | Search global: cari task, proyek, anggota | P1 |
| FRD-E-006 | Notifikasi in-app: mention, assign, deadline approaching | P2 |
| FRD-E-007 | Notifikasi deadline: H-1, H-3, overdue | P2 |

### Modul F — Dashboard & Laporan

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-F-001 | Dashboard overview workspace: total task, completed, overdue | P1 |
| FRD-F-002 | Grafik task distribution per status (pie/donut) | P1 |
| FRD-F-003 | Task per anggota (bar chart beban kerja) | P2 |
| FRD-F-004 | Burndown chart (proyek dengan deadline) | P2 |
| FRD-F-005 | Export summary proyek ke PDF | P2 |

**Total Requirements: 50 requirements across 6 modul**

## 3. Use Case / Alur Pengguna

### Use Case 1: Mengelola Task dalam Project Menggunakan Kanban Board

**Aktor:** Andi — Project Manager
**Prasyarat:** Andi sudah login, workspace "PT Maju Jaya" sudah dibuat, proyek "Redesign Website" sudah ada
**Alur Normal:**
1. Andi membuka KolabPro dan memilih workspace "PT Maju Jaya".
2. Sistem menampilkan daftar proyek. Andi mengklik "Redesign Website".
3. Sistem menampilkan Kanban Board dengan 4 kolom: Belum (3 task), Dikerjakan (2 task), Tinjau (1 task), Selesai (1 task).
4. Andi melihat task "Integrasi API autentikasi" di kolom Belum — sudah 3 hari sejak dibuat.
5. Andi mengklik task tersebut. Panel detail muncul di sebelah kanan.
6. Di panel detail, Andi melihat deskripsi task, checklist (1/4 selesai), dan komentar.
7. Andi mengganti assignee dari "Dewi Lestari" menjadi "Ahmad Rizki" karena Dewi sedang sibuk.
8. Andi mengubah deadline dari 1 Juli menjadi 5 Juli.
9. Andi menambahkan komentar: "@ahmad tolong prioritaskan task ini ya, deadline dimajukan."
10. Andi menutup panel detail. Task tetap di kolom Belum dengan assignee baru.
11. Andi melihat task "Implementasi navbar responsif" — seharusnya sudah dikerjakan.
12. Andi men-drag task "Integrasi API autentikasi" dari kolom Belum ke kolom Dikerjakan untuk menandai bahwa Ahmad sudah mulai.
13. Sistem memperbarui status task. Counter di header kolom berubah: Belum (2), Dikerjakan (3).
14. Progress bar proyek di topbar berubah dari 65% menjadi 68%.

**Alur Alternatif A — Buat Task Cepat:**
- Di langkah 13, Andi mengklik tombol "+ Baru" di footer kolom Belum.
- Sistem menampilkan input inline di bagian atas kolom.
- Andi mengetik "Buat halaman kontak" dan tekan Enter.
- Task baru muncul di kolom Belum dengan prioritas default (sedang) dan tanpa assignee.

**Alur Alternatif B — Task dengan Checklist:**
- Andi membuka task "Implementasi navbar responsif".
- Checklist menunjukkan 2 dari 4 item selesai.
- Andi mencentang item "Responsive mobile".
- Checklist progress berubah menjadi 3/4. Status task tetap "Dikerjakan".

**Alur Error:**
- Jika Andi mencoba men-drag task dan koneksi terputus, task kembali ke posisi semula dengan toast: "Gagal memperbarui status. Coba lagi."
- Jika Andi menambahkan komentar dengan mention ke user yang tidak ada di workspace, muncul tooltip: "Anggota tidak ditemukan."
- Jika deadline yang dimasukkan sudah lewat, muncul konfirmasi: "Tanggal yang dipilih sudah lewat. Tetap simpan?"

### Use Case 2: Melihat Timeline Proyek dan Mengecek Progress

**Aktor:** Andi — Project Manager
**Prasyarat:** Proyek "Redesign Website" memiliki task dengan deadline, sudah ada beberapa task selesai
**Alur Normal:**
1. Andi berada di proyek "Redesign Website". View saat ini: Board.
2. Andi mengklik icon "Timeline" di view switcher pada topbar.
3. Sistem beralih ke Gantt chart view. Sumbu horizontal menampilkan timeline dari Mei hingga Agustus 2025.
4. Setiap task direpresentasikan sebagai bar horizontal dari tanggal mulai hingga deadline.
5. Task selesai berwarna hijau dengan centang. Task aktif berwarna biru. Task overdue berwarna merah.
6. Andi melihat bahwa "Integrasi API autentikasi" mulai 20 Mei dan deadline 1 Juli.
7. Andi melihat garis vertikal merah "Today" menunjukkan posisi hari ini.
8. Di sebelah kanan, summary panel menampilkan: "5 task selesai", "3 dalam progress", "2 belum mulai", "1 overdue".
9. Andi mengecek task "Pengujian UX" — belum dimulai. Deadline: 25 Juni. Ini 3 hari lagi.
10. Andi men-drag bar task "Pengujian UX" ke kanan, memperpanjang deadline menjadi 30 Juni.
11. Sistem memperbarui deadline dan menampilkan toast: "Deadline berhasil diubah."

**Alur Alternatif — Zoom Level:**
- Andi mengklik tombol "Minggu" untuk melihat timeline per minggu. Bar task menyesuaikan ukuran.
- Andi mengklik "Bulan" untuk melihat keseluruhan proyek dalam gambaran besar.

**Alur Error:**
- Jika ada task tanpa deadline, task tidak muncul di Timeline dan ada notifikasi: "3 task tanpa deadline tidak ditampilkan di Timeline. Atur deadline untuk melihatnya."

## 4. Aturan Bisnis

| ID | Aturan Bisnis | Deskripsi |
|----|---------------|-----------|
| BR-001 | Status flow default | Task bergerak: belum -> dikerjakan -> tinjau -> selesai. Bisa loncat (belum langsung ke selesai) |
| BR-002 | Progress otomatis | Progress proyek = (total task selesai / total task) x 100 |
| BR-003 | Overdue detection | Task dengan deadline < hari ini dan status != selesai dianggap "Terlambat" |
| BR-004 | Single assignee | Satu task hanya bisa memiliki satu penanggung jawab utama |
| BR-005 | Role hierarchy | Pemilik > Admin > Anggota > Pengamat. Pengamat tidak bisa membuat/mengedit task |
| BR-006 | Soft delete | Proyek/task yang dihapus masuk ke Trash dan bisa direstore dalam 30 hari |
| BR-007 | Attachment limit | Maks 10 file per task, total ukuran 10 MB per file |
| BR-008 | Label uniqueness | Label bersifat global dalam satu workspace (bisa dipakai di banyak proyek) |
| BR-009 | Deadline cascade | Jika task parent berubah deadline, subtask mengikuti perubahan |
| BR-010 | Batas anggota | Tier Gratis: 10 anggota, Tier Pro: 30 anggota, Tier Business: unlimited |
| BR-011 | Nested subtask | Subtask hanya 1 level (tidak ada sub-subtask) |

## 5. Integrasi & Data Flow

```
                         ┌─────────────────────────────────┐
                         │     Frontend SPA (React)        │
                         │  ┌──────┐ ┌────┐ ┌──────────┐  │
                         │  │Board │ │List│ │ Timeline │  │
                         │  │View  │ │View│ │  View     │  │
                         │  └──┬───┘ └─┬──┘ └────┬─────┘  │
                         │     │       │          │        │
                         │  ┌──▼───────▼──────────▼─────┐  │
                         │  │    Zustand + React Query   │  │
                         │  │    State Management        │  │
                         │  └──────────────┬─────────────┘  │
                         └─────────────────┼────────────────┘
                                           │
                         ┌─────────────────▼────────────────┐
                         │        API Layer                  │
                         │  ┌───────────────────────────┐   │
                         │  │  /api/v1/workspace/*       │   │
                         │  │  /api/v1/projects/*        │   │
                         │  │  /api/v1/tasks/*           │   │
                         │  │  /api/v1/comments/*        │   │
                         │  │  /api/v1/members/*         │   │
                         │  └──────────┬────────────────┘   │
                         └─────────────────┼────────────────┘
                                           │
                         ┌─────────────────▼────────────────┐
                         │        Database (PostgreSQL)      │
                         │  tables:                          │
                         │    - workspaces                   │
                         │    - projects                     │
                         │    - tasks                        │
                         │    - task_checklist_items         │
                         │    - task_labels                  │
                         │    - comments                     │
                         │    - attachments                  │
                         │    - activity_log                 │
                         │    - members                      │
                         └──────────────────────────────────┘

**Alur Data Task Drag-and-Drop:**
1. User mulai drag task dari kolom "Belum"
2. dnd-kit menangkap event drag, komponen memberikan feedback visual (opacity, posisi)
3. User drop task di kolom "Dikerjakan"
4. Zustand store update status task secara optimis (langsung tampil di UI)
5. React Query mutation mengirim PATCH /api/v1/tasks/{id} dengan status baru + posisi
6. Jika request sukses: query di-invalidate, UI sudah terupdate
7. Jika request gagal: Zustand store di-rollback ke state sebelumnya, toast error

**Alur Komentar:**
1. User mengetik komentar + mention @nama
2. Mention di-parse ke format internal ({userId: "usr-003", name: "Ahmad Rizki"})
3. POST /api/v1/comments dengan taskId, konten, mentions
4. Response berisi komentar dengan ID dan timestamp
5. React Query menambahkan komentar ke cache (tanpa invalidasi penuh)
6. Notifikasi mention dikirim ke user yang disebut (via polling atau WebSocket)

## 6. Non-Functional Requirements

| Kategori | Requirement | Target |
|----------|-------------|--------|
| Performance | Waktu muat halaman kanban (50 task) | < 1.5 detik |
| Performance | Drag-and-drop response time | < 50 ms |
| Performance | Render timeline dengan 100 task | < 2 detik |
| Performance | Ukuran bundle JS (initial) | < 150 KB gzipped |
| Performance | Search global hasil | < 500 ms |
| Scalability | Task per proyek | Maks 500 task |
| Scalability | Proyek per workspace | Maks 50 proyek |
| Scalability | Anggota per workspace | Maks 100 anggota |
| Availability | Uptime web app | 99.5% (monthly) |
| Security | Data isolation | Multi-tenant strict |
| Usability | Waktu belajar pengguna baru | < 10 menit untuk buat task pertama |
| Usability | Bahasa Indonesia | 100% UI, notifikasi, tooltip |
| Compatibility | Browser | Chrome, Firefox, Safari, Edge (2 versi terakhir) |
| Compatibility | Responsive | Desktop (1920x1080), tablet (768x1024), mobile (< 640px via PWA) |

## 7. Prioritization Matrix

| Fitur | Value (1-5) | Effort (1-5) | Priority |
|-------|-------------|--------------|----------|
| Kanban Board drag-and-drop | 5 | 3 | P0 |
| CRUD Task (judul, assignee, deadline, prioritas) | 5 | 2 | P0 |
| CRUD Proyek | 5 | 2 | P0 |
| Workspace management | 5 | 2 | P0 |
| Anggota tim & role | 5 | 2 | P0 |
| Progress otomatis | 4 | 1 | P0 |
| Task filter by status | 4 | 1 | P0 |
| Priority badges | 4 | 1 | P0 |
| Checklist dalam task | 4 | 2 | P1 |
| Komentar + mention | 4 | 3 | P1 |
| List View | 4 | 2 | P1 |
| Filter & Search | 4 | 2 | P1 |
| Labels/Tags | 3 | 2 | P1 |
| File attachment | 3 | 2 | P1 |
| Timeline / Gantt View | 4 | 4 | P2 |
| Calendar View | 3 | 3 | P2 |
| Dashboard grafik | 3 | 3 | P2 |
| Notifikasi in-app | 3 | 3 | P2 |
| Activity log | 3 | 2 | P1 |
| Export PDF | 2 | 3 | P2 |
| Bulk actions | 2 | 1 | P1 |

Value: 1 (rendah) - 5 (sangat tinggi) | Effort: 1 (kecil) - 5 (besar). Prioritas P0 = value >= 4 dan effort <= 3.
