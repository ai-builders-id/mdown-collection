# FRD: NusantaraCMS — Functional Requirements
**Project:** NusantaraCMS

## 1. Scope
NusantaraCMS adalah sistem manajemen konten berbasis web yang memungkinkan pengguna non-teknis untuk membuat, mengelola, dan mempublikasikan konten digital (blog, halaman statis, media) tanpa perlu kemampuan coding. Sistem mencakup manajemen pengguna dengan role-based access, moderasi komentar, optimasi SEO, dan dashboard analitik.

**In Scope:**
- Manajemen blog post (CRUD, rich text editor, scheduling)
- Manajemen halaman statis dengan multiple template
- Media library dengan upload dan organisasi file
- Manajemen kategori dan tag
- Manajemen komentar dengan moderasi
- Dashboard statistik dan analitik dasar
- Manajemen pengguna dengan 4 role (admin, editor, penulis, kontributor)
- SEO tools (meta title, description, Open Graph)
- Autosave dan version history

**Out of Scope:**
- E-commerce / toko online (akan dirilis di fase terpisah)
- Email marketing campaign builder
- Forum atau komunitas
- Aplikasi mobile native
- CRM atau fitur helpdesk

## 2. Modul & Fitur

### Modul A — Autentikasi & Manajemen Pengguna

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-A-001 | Registrasi pengguna baru dengan verifikasi email | P0 |
| FRD-A-002 | Login dengan email dan password | P0 |
| FRD-A-003 | Login dengan Google SSO | P1 |
| FRD-A-004 | Logout dan session management | P0 |
| FRD-A-005 | Reset password via email | P0 |
| FRD-A-006 | Manajemen profil pengguna (nama, bio, avatar, jabatan) | P0 |
| FRD-A-007 | Role management: Admin, Editor, Penulis, Kontributor | P0 |
| FRD-A-008 | CRUD pengguna (hanya admin) dengan form lengkap | P0 |
| FRD-A-009 | Aktivitas log pengguna (riwayat login, aksi) | P1 |
| FRD-A-010 | Two-factor authentication (2FA) opsional | P2 |

### Modul B — Manajemen Blog

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-B-001 | Membuat artikel baru dengan rich text editor (TipTap) | P0 |
| FRD-B-002 | Mengedit artikel yang sudah ada | P0 |
| FRD-B-003 | Menghapus artikel (soft delete) | P0 |
| FRD-B-004 | Daftar semua artikel dengan tabel (filter, search, sort) | P0 |
| FRD-B-005 | Mengatur status artikel: draf, terjadwal, terbit, arsip | P0 |
| FRD-B-006 | Menjadwalkan publikasi artikel pada tanggal tertentu | P0 |
| FRD-B-007 | Autosave setiap 30 detik dengan indikator visual | P0 |
| FRD-B-008 | Version history — lihat dan restore versi sebelumnya | P1 |
| FRD-B-009 | Preview artikel sebelum terbit | P0 |
| FRD-B-010 | Mengatur kategori dan tag untuk artikel | P0 |
| FRD-B-011 | Featured image dengan crop dan alt text | P0 |
| FRD-B-012 | Setting SEO: meta title, meta description, slug | P0 |
| FRD-B-013 | Bulk action: pilih banyak artikel untuk publish/arsip/hapus | P1 |
| FRD-B-014 | Export artikel ke format PDF | P2 |
| FRD-B-015 | Duplikasi artikel (clone post) | P1 |

### Modul C — Manajemen Halaman

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-C-001 | Membuat halaman statis baru dengan editor | P0 |
| FRD-C-002 | Mengedit dan menghapus halaman | P0 |
| FRD-C-003 | Daftar halaman dengan tabel | P0 |
| FRD-C-004 | Template halaman: default, full-width, landing, kontak | P0 |
| FRD-C-005 | Hierarki halaman (parent-child) untuk sub-halaman | P0 |
| FRD-C-006 | Pengaturan menu: urutan, tampil/sembunyi di navigasi | P0 |
| FRD-C-007 | SEO settings untuk halaman | P0 |
| FRD-C-008 | Preview halaman | P1 |

### Modul D — Media Library

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-D-001 | Upload file via drag-and-drop atau tombol pilih file | P0 |
| FRD-D-002 | Gallery view (grid thumbnail) dan list view | P0 |
| FRD-D-003 | Filter media berdasarkan tipe (gambar, dokumen, video, audio) | P0 |
| FRD-D-004 | Search media berdasarkan nama file | P0 |
| FRD-D-005 | Detail media: preview, info, edit alt text dan caption | P0 |
| FRD-D-006 | Hapus media dengan konfirmasi | P0 |
| FRD-D-007 | Deteksi file duplikat saat upload | P1 |
| FRD-D-008 | Crop dan resize gambar built-in | P2 |
| FRD-D-009 | Batch upload (multiple files sekaligus) | P1 |
| FRD-D-010 | Progress bar upload | P0 |

### Modul E — Komentar

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-E-001 | Daftar komentar dari semua artikel | P0 |
| FRD-E-002 | Moderasi: setujui, tolak, tandai spam | P0 |
| FRD-E-003 | Balas komentar langsung dari dashboard | P1 |
| FRD-E-004 | Notifikasi komentar baru (badge di sidebar) | P0 |
| FRD-E-005 | Filter komentar berdasarkan status | P0 |
| FRD-E-006 | Edit komentar (admin only) | P2 |
| FRD-E-007 | Bulk action komentar | P1 |

### Modul F — Dashboard & Statistik

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-F-001 | Ringkasan: total artikel, total view, total komentar | P0 |
| FRD-F-002 | Grafik artikel yang terbit per bulan (bar chart) | P0 |
| FRD-F-003 | Grafik views per hari (line chart) | P0 |
| FRD-F-004 | Artikel terpopuler (top 5 berdasarkan views) | P0 |
| FRD-F-005 | Aktivitas terbaru (riwayat aksi pengguna) | P1 |
| FRD-F-006 | Kalender publikasi dengan postingan terjadwal | P1 |
| FRD-F-007 | Quick actions: Tulis artikel, Upload media, Lihat situs | P0 |

**Total Requirements: 48 requirements across 6 modul**

## 3. Use Case / Alur Pengguna

### Use Case 1: Menerbitkan Artikel Baru

**Aktor:** Penulis (Budi)
**Prasyarat:** Budi sudah login ke dashboard NusantaraCMS
**Alur Normal:**
1. Budi mengklik tombol "Tulis Konten Baru" di sidebar.
2. Sistem menampilkan halaman editor dengan form kosong.
3. Budi memasukkan judul artikel: "Inovasi Pelayanan Publik di Era Digital".
4. Sistem otomatis menggenerate slug: "inovasi-pelayanan-publik-di-era-digital". Budi bisa mengeditnya.
5. Budi menulis konten menggunakan rich text editor. Sistem mulai autosave setiap 30 detik dengan indikator "Tersimpan" di pojok kanan.
6. Budi mengklik tombol "Featured Image" dan memilih gambar dari Media Library. Sistem menampilkan preview.
7. Budi memilih kategori "Transformasi Digital" dan menambahkan tag: "pelayanan-publik", "digitalisasi", "inovasi".
8. Budi mengisi SEO title dan meta description di panel samping.
9. Budi mengklik tombol "Terbitkan".
10. Sistem menampilkan konfirmasi: "Apakah Anda yakin ingin menerbitkan artikel ini?"
11. Budi mengklik "Ya, Terbitkan".
12. Sistem menyimpan artikel dengan status "terbit", mencatat timestamp publishedAt, dan menampilkan notifikasi sukses: "Artikel berhasil diterbitkan!"
13. Sistem mengarahkan Budi kembali ke daftar artikel dengan artikel baru muncul di urutan pertama.

**Alur Alternatif A — Menjadwalkan:**
- Pada langkah 9, Budi memilih "Jadwalkan" dan memilih tanggal 1 Desember 2025 pukul 08:00 WIB.
- Sistem menyimpan artikel dengan status "terjadwal" dan menampilkan pesan: "Artikel akan terbit pada 1 Desember 2025 pukul 08:00 WIB."

**Alur Alternatif B — Menyimpan Draf:**
- Pada langkah 9, Budi memilih "Simpan Draf".
- Sistem menyimpan artikel dengan status "draf" dan menampilkan pesan: "Artikel disimpan sebagai draf."

**Alur Error:**
- Jika koneksi terputus saat autosave, sistem menampilkan banner: "Koneksi terputus. Perubahan akan disimpan saat koneksi kembali."
- Jika judul melebihi 200 karakter, tombol terbitkan menjadi disabled dan muncul pesan error: "Judul maksimal 200 karakter."

### Use Case 2: Memoderasi Komentar

**Aktor:** Editor (Sari)
**Prasyarat:** Sari login sebagai Editor. Terdapat 5 komentar baru yang menunggu moderasi.
**Alur Normal:**
1. Sari melihat badge "5" di menu "Komentar" pada sidebar.
2. Sari mengklik menu "Komentar".
3. Sistem menampilkan tabel komentar dengan filter default "Menunggu".
4. Sari membaca komentar pertama: "Terima kasih informasinya" dari Agus Wijaya.
5. Sari mengklik tombol centang (setujui) di samping komentar tersebut.
6. Sistem langsung mengubah status komentar menjadi "Disetujui" tanpa reload halaman.
7. Sari membaca komentar kedua yang berisi link mencurigakan.
8. Sari mengklik tombol "Tandai Spam".
9. Sistem memindahkan komentar ke tab "Spam" dan menampilkan toast: "Komentar ditandai sebagai spam."
10. Sari mengklik komentar ketiga dan membalasnya.
11. Sistem menampilkan form balasan di bawah komentar.
12. Sari mengetik balasan dan mengklik "Kirim".
13. Sistem menambahkan balasan dan menampilkan status "Dibalas oleh Sari Indah Dewi".

**Alur Alternatif — Bulk Action:**
- Sari mencentang beberapa komentar, lalu memilih aksi "Setujui Semua" atau "Hapus Semua" dari dropdown bulk action.

**Alur Error:**
- Jika ada komentar dengan kata-kata terlarang, sistem menandai otomatis sebagai "spam" dan menampilkan peringatan: "Komentar terdeteksi mengandung konten yang tidak pantas dan telah dipindahkan ke spam."

## 4. Aturan Bisnis

| ID | Aturan Bisnis | Deskripsi |
|----|---------------|-----------|
| BR-001 | Slug unik per konten | Slug artikel dan halaman harus unik dalam satu domain. Jika duplikat, sistem menambahkan angka (-1, -2, dst) |
| BR-002 | Autosave interval | Autosave berjalan setiap 30 detik hanya jika ada perubahan. Jika tidak ada perubahan, autosave dilewati |
| BR-003 | Role hierarchy | Admin > Editor > Penulis > Kontributor. Role yang lebih tinggi bisa mengedit konten role di bawahnya |
| BR-004 | Pembatasan kontributor | Kontributor hanya bisa membuat draf. Editor/admin harus menyetujui sebelum terbit |
| BR-005 | Soft delete | Semua konten yang dihapus dipindahkan ke "Tempat Sampah" selama 30 hari sebelum permanent delete |
| BR-006 | Batas upload media | File maksimal 50 MB per file. Total penyimpanan tergantung tier pengguna |
| BR-007 | Moderasi komentar | Semua komentar baru otomatis berstatus "menunggu" dan harus disetujui admin/editor |
| BR-008 | Jadwal publikasi | Publikasi terjadwal memeriksa status setiap menit. Jika terjadi error publikasi, admin mendapat notifikasi |
| BR-009 | SEO length limit | Meta title maksimal 60 karakter, meta description maksimal 160 karakter |
| BR-010 | Featured image wajib | Setiap artikel wajib memiliki featured image untuk tampilan grid/listing yang optimal |

## 5. Integrasi & Data Flow

```
                           ┌─────────────────────────────┐
                           │      Frontend (React)        │
                           │  ┌───────┐ ┌───────┐        │
                           │  │  Blog  │ │Halaman│        │
                           │  │  Module│ │Module │        │
                           │  └───┬───┘ └───┬───┘        │
                           │      │         │            │
                           │  ┌───▼─────────▼───┐        │
                           │  │  Zustand Store   │        │
                           │  │  (Client State)  │        │
                           │  └────────┬─────────┘        │
                           │           │                  │
                           │  ┌────────▼─────────┐        │
                           │  │  React Query      │        │
                           │  │  (Server State)   │        │
                           │  └────────┬─────────┘        │
                           └───────────┼──────────────────┘
                                       │
                           ┌───────────▼──────────────────┐
                           │       API Layer              │
                           │  ┌──────────────────────┐   │
                           │  │  REST API Endpoints   │   │
                           │  │  /api/v1/*            │   │
                           │  └──────────┬───────────┘   │
                           │             │               │
                           │  ┌──────────▼───────────┐   │
                           │  │  Authentication       │   │
                           │  │  JWT Token Validation │   │
                           │  └──────────────────────┘   │
                           └───────────┬──────────────────┘
                                       │
              ┌────────────────────────┼────────────────────┐
              │                        │                     │
   ┌──────────▼────────────┐ ┌────────▼──────────┐ ┌───────▼──────────┐
   │  Database (PostgreSQL) │ │  Storage (S3)     │ │  Search (Elastic)│
   │  - users              │ │  - images         │ │  - fulltext       │
   │  - posts              │ │  - documents      │ │  search           │
   │  - pages              │ │  - videos         │ │                   │
   │  - media              │ │                   │ │                   │
   │  - comments           │ └───────────────────┘ └───────────────────┘
   │  - categories         │
   └───────────────────────┘

**Alur Data Publikasi Artikel:**
1. User menulis konten di Editor → autosave mengirim PATCH /api/v1/posts/{id} setiap 30 detik
2. User klik "Terbitkan" → POST /api/v1/posts/{id}/publish mengubah status
3. Sistem menyimpan data post, category relations, dan tag di PostgreSQL
4. Featured image dan media disimpan di S3-compatible storage
5. React Query invalidate cache "posts" sehingga daftar artikel terupdate
6. Untuk scheduled post, cron job internal menjalankan publish pada waktu yang ditentukan
7. Data untuk front-end (website publik) di-cache dengan Redis

## 6. Non-Functional Requirements

| Kategori | Requirement | Target |
|----------|-------------|--------|
| Performance | Waktu muat halaman dashboard | < 2 detik (FCP) |
| Performance | Waktu render daftar artikel (100 item) | < 500 ms |
| Performance | Ukuran bundle JavaScript | < 300 KB (gzipped) |
| Performance | Upload file 10 MB | < 5 detik |
| Scalability | Mendukung pengguna konkuren | 500 concurrent users |
| Scalability | Growth data per bulan | 50 GB media, 10.000 post |
| Availability | Uptime SLA | 99.5% (monthly) |
| Security | Enkripsi data di transit | TLS 1.3 |
| Security | Enkripsi data di penyimpanan | AES-256 |
| Security | Proteksi XSS, CSRF, SQL Injection | OWASP Top 10 compliance |
| Security | Password policy | Min 8 karakter, kombinasi huruf/angka |
| Usability | Waktu belajar pengguna baru | < 30 menit untuk publish pertama |
| Usability | Persentase task completion | > 90% tanpa bantuan |
| Accessibility | WCAG compliance | Level AA |
| Browser Support | Browser modern | Chrome, Firefox, Safari, Edge (2 versi terakhir) |

## 7. Prioritization Matrix

| Fitur | Value (1-5) | Effort (1-5) | Priority |
|-------|-------------|--------------|----------|
| Rich text editor blog | 5 | 3 | P0 |
| CRUD artikel + status | 5 | 2 | P0 |
| Media library | 5 | 3 | P0 |
| Autentikasi + roles | 5 | 3 | P0 |
| Autosave | 4 | 2 | P0 |
| Manajemen kategori | 4 | 1 | P0 |
| Dashboard statistik | 4 | 3 | P1 |
| Komentar + moderasi | 4 | 2 | P1 |
| SEO tools | 4 | 2 | P1 |
| Version history | 3 | 3 | P1 |
| Export/Import konten | 3 | 2 | P1 |
| Multi-bahasa | 3 | 4 | P2 |
| Integrasi media sosial | 3 | 3 | P2 |
| Template landing page | 4 | 2 | P2 |
| Kalender publikasi | 3 | 2 | P1 |
| 2FA | 3 | 3 | P2 |
| Bulk actions | 3 | 1 | P1 |
| Duplikasi artikel | 2 | 1 | P1 |

Value: 1 (rendah) - 5 (sangat tinggi) | Effort: 1 (kecil) - 5 (besar). Prioritas P0 = value >= 4 dan effort <= 3.
