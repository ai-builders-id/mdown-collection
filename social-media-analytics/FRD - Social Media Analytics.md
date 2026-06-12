# FRD: SosmedInsight — Functional Requirements
**Project:** SosmedInsight

## 1. Scope
SosmedInsight adalah dashboard analitik media sosial multi-platform yang menyajikan data follower growth, engagement metrics, reach, dan performa konten dari berbagai platform (Instagram, TikTok, YouTube, Facebook, Twitter/X, LinkedIn) dalam satu antarmuka terpadu. Sistem mencakup analisis audiens, perbandingan kompetitor, dan generate laporan otomatis.

**In Scope:**
- Koneksi multi-platform via API resmi
- Dashboard overview dengan metrik kunci dan grafik tren
- Analisis performa postingan individu
- Insight demografi audiens (usia, gender, lokasi, jam aktif)
- Perbandingan kompetitor
- Laporan PDF siap presentasi
- Analisis hashtag dan konten
- Filter periode waktu dan perbandingan antar periode

**Out of Scope:**
- Scheduling dan auto-posting konten (akan dirilis fase 2)
- Social inbox / moderasi komentar terpadu
- Manajemen konten (content creation tools)
- Integrasi dengan iklan berbayar (Facebook Ads, TikTok Ads)
- CRM atau lead generation dari sosial media

## 2. Modul & Fitur

### Modul A — Manajemen Akun & Autentikasi

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-A-001 | Registrasi dan login pengguna | P0 |
| FRD-A-002 | Hubungkan akun Instagram (via IG Basic Display API / Graph API) | P0 |
| FRD-A-003 | Hubungkan akun TikTok (via TikTok Business API) | P0 |
| FRD-A-004 | Hubungkan akun YouTube (via YouTube Data API) | P1 |
| FRD-A-005 | Hubungkan akun Facebook Page | P1 |
| FRD-A-006 | Hubungkan akun Twitter/X | P1 |
| FRD-A-007 | Hubungkan akun LinkedIn Page | P2 |
| FRD-A-008 | Refresh token otomatis untuk setiap platform | P0 |
| FRD-A-009 | Status koneksi akun (terhubung/error/expired) | P0 |
| FRD-A-010 | Hapus koneksi akun dengan konfirmasi | P0 |
| FRD-A-011 | Pilih akun default untuk dashboard utama | P1 |

### Modul B — Dashboard Overview

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-B-001 | Stat card: Total Followers dengan sparkline | P0 |
| FRD-B-002 | Stat card: Follower Growth (% dan angka absolut) | P0 |
| FRD-B-003 | Stat card: Engagement Rate dengan tren | P0 |
| FRD-B-004 | Stat card: Total Reach periode ini | P0 |
| FRD-B-005 | Stat card: Total Impressions | P0 |
| FRD-B-006 | Grafik line: Followers Trend (harian/mingguan/bulanan) | P0 |
| FRD-B-007 | Grafik area: Reach & Impressions overlay | P0 |
| FRD-B-008 | Grafik bar: Engagement per hari | P0 |
| FRD-B-009 | Grafik: Perbandingan metrik antar platform | P1 |
| FRD-B-010 | Top 5 postingan dengan engagement tertinggi | P0 |
| FRD-B-011 | Ringkasan insight: "Followers naik 12% minggu ini" | P1 |
| FRD-B-012 | Filter platform: pilih satu atau semua platform | P0 |
| FRD-B-013 | Filter periode: 7H, 30H, 3B, 12B, kustom | P0 |
| FRD-B-014 | Perbandingan periode sebelumnya (MoM/WoW/YoY) | P1 |
| FRD-B-015 | Kalender mini dengan indikator hari posting | P1 |

### Modul C — Content Analytics

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-C-001 | Daftar semua postingan dengan tabel (thumbnail, caption, tanggal, likes, comments, shares, saves, engagement rate) | P0 |
| FRD-C-002 | Sorting dan filter postingan (platform, tipe konten, periode, engagement range) | P0 |
| FRD-C-003 | Detail postingan: semua metrik, grafik performa per jam, komentar teratas | P0 |
| FRD-C-004 | Identifikasi tipe konten terbaik (foto vs video vs carousel) | P1 |
| FRD-C-005 | Analisis performa hashtag: engagement per hashtag | P1 |
| FRD-C-006 | Grafik performa postingan berdasarkan jam tayang | P1 |
| FRD-C-007 | Best time to post recommendation | P1 |
| FRD-C-008 | Analisis caption length vs engagement | P2 |
| FRD-C-009 | Filter konten berbayar (paid vs organic) | P1 |
| FRD-C-010 | Export data postingan ke CSV | P1 |
| FRD-C-011 | Bookmark postingan untuk referensi | P2 |

### Modul D — Audience Insights

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-D-001 | Demografi usia: bar chart distribusi usia pengikut | P0 |
| FRD-D-002 | Demografi gender: pie chart persentase pria/wanita | P0 |
| FRD-D-003 | Demografi lokasi: top 10 kota pengikut | P0 |
| FRD-D-004 | Jam aktif audiens: heatmap per jam dalam seminggu | P0 |
| FRD-D-005 | Hari aktif audiens: bar chart per hari | P1 |
| FRD-D-006 | Tipe device: mobile vs desktop vs tablet | P1 |
| FRD-D-007 | Bahasa audiens: distribusi bahasa pengikut | P2 |
| FRD-D-008 | Akun serupa: rekomendasi akun mirip | P1 |
| DRD-D-009 | Follower growth source: dari mana followers datang | P2 |
| FRD-D-010 | Waktu aktif terbaik untuk posting | P1 |

### Modul E — Kompetitor

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-E-001 | Tambah akun kompetitor (input username/platform) | P0 |
| FRD-E-002 | Tabel perbandingan: Followers, Growth, Engagement Rate, Post Frequency | P0 |
| FRD-E-003 | Grafik perbandingan followers trend (kita vs kompetitor) | P1 |
| FRD-E-004 | Grafik perbandingan engagement rate | P1 |
| FRD-E-005 | Ranking otomatis kompetitor berdasarkan metrik | P1 |
| FRD-E-006 | Hapus/edit kompetitor | P0 |
| FRD-E-007 | Notifikasi jika kompetitor mengalami lonjakan followers | P2 |
| FRD-E-008 | Top post kompetitor (publik) | P2 |

### Modul F — Laporan & Export

| ID | Fitur | Prioritas |
|----|-------|-----------|
| FRD-F-001 | Generate laporan PDF otomatis | P1 |
| FRD-F-002 | Template laporan: Ringkasan Esktutif, Metrik Utama, Grafik Tren, Top Posts, Demografi | P1 |
| FRD-F-003 | Preview laporan sebelum export | P1 |
| FRD-F-004 | Export data mentah ke CSV | P1 |
| FRD-F-005 | Export grafik sebagai PNG | P2 |
| FRD-F-006 | Laporanterjadwal (email otomatis mingguan/bulanan) | P2 |
| FRD-F-007 | Kustomisasi brand pada laporan (logo, warna) | P2 |
| FRD-F-008 | Download riwayat laporan | P1 |
| FRD-F-009 | Hapus laporan lama | P1 |

**Total Requirements: 54 requirements across 6 modul**

## 3. Use Case / Alur Pengguna

### Use Case 1: Monitoring Performa Bulanan dan Membuat Laporan

**Aktor:** Rina — Social Media Manager
**Prasyarat:** Rina sudah login, akun Instagram @ptmajujaya sudah terhubung dan tersinkronisasi
**Alur Normal:**
1. Rina membuka dashboard SosmedInsight. Sistem menampilkan overview dengan akun default @ptmajujaya, periode 30 hari terakhir.
2. Rina melihat 4 stat card: Followers 45.230 (+5,2%), Engagement Rate 3,8%, Reach 185.000, Impressions 320.000.
3. Rina menggeser ke bawah melihat grafik Followers Trend — garis menunjukkan kenaikan stabil.
4. Rina mengklik tab "Top Posts". Sistem menampilkan 5 postingan dengan engagement tertinggi — postingan promo diskon memimpin dengan 9,2% engagement.
5. Rina mengklik salah satu postingan. Panel detail muncul menampilkan: likes 2.103, comments 167, shares 890, saves 234, reach 32.100.
6. Rina berkata "Data ini bagus untuk laporan bulanan". Ia mengklik tombol "Buat Laporan".
7. Sistem menampilkan form: pilih template "Laporan Bulanan Eksekutif", periode 1-30 November 2025.
8. Rina mengklik "Generate". Sistem memproses dan menampilkan preview PDF dalam 5 detik.
9. Rina memeriksa preview: halaman cover dengan brand, ringkasan eksekutif, grafik tren, tabel top posts, demografi audiens.
10. Rina mengklik "Download PDF". File laporan terunduh.
11. Sistem menampilkan toast: "Laporan berhasil diunduh."

**Alur Alternatif A — Data Tidak Lengkap:**
- Di langkah 2, jika data hanya 15 hari (karena baru connect), sistem menampilkan notifikasi: "Data tersedia sejak 15 November 2025. Metrik mungkin tidak merepresentasikan tren penuh."

**Alur Alternatif B — Perbandingan Periode:**
- Di langkah 2, Rina mengaktifkan toggle "Bandingkan dengan periode sebelumnya". Sistem overlay grafik bulan sebelumnya dengan warna berbeda (abu-abu) dan menampilkan delta: "Reach naik 23% dibanding bulan lalu."

**Alur Error:**
- Jika token Instagram expired, sistem menampilkan banner kuning: "Koneksi Instagram @ptmajujaya perlu diperbarui. Klik di sini untuk menghubungkan ulang."
- Grafik gagal render: fallback ke stat angka saja dengan pesan "Grafik tidak dapat dimuat."

### Use Case 2: Analisis Audiens dan Perbandingan Kompetitor

**Aktor:** Dimas — Pemilik UKM Fashion
**Prasyarat:** Dimas sudah login dan menghubungkan akun Instagram @busananusantara
**Alur Normal:**
1. Dimas membuka menu "Audiens" di sidebar.
2. Sistem menampilkan demografi: 58% pria, 38% usia 25-34 tahun, lokasi terbanyak Jakarta (28%) dan Bandung (15%).
3. Dimas melihat heatmap jam aktif — ternyata pengikutnya paling aktif hari Jumat jam 19.00-21.00 WIB.
4. Sistem menampilkan rekomendasi: "Waktu terbaik untuk posting: Jumat pukul 19.00 WIB."
5. Dimas puas dan pindah ke menu "Kompetitor".
6. Dimas mengklik "Tambah Kompetitor" dan memasukkan username Instagram @sejahteraabadi.
7. Sistem memproses dan menampilkan data: followers 52.300, growth 4,8%, engagement 3,2%.
8. Tabel perbandingan menampilkan posisi Dimas vs kompetitor. Sistem menandai dengan warna hijau jika lebih unggul, merah jika kalah.
9. Dimas melihat bahwa akunnya unggul di engagement rate (4,5% vs 3,2%) tetapi kalah di jumlah followers.
10. Dimas mengklik grafik perbandingan followers trend. Grafik line dual-axis menampilkan pertumbuhan kedua akun.

**Alur Alternatif — Kompetitor Tidak Ditemukan:**
- Di langkah 6, jika username tidak valid, sistem menampilkan: "Akun @sejahteraabadi tidak ditemukan. Periksa kembali username atau platform yang dipilih."

**Alur Error:**
- Jika data kompetitor gagal diambil karena akun private, sistem menampilkan: "Data kompetitor tidak tersedia. Akun mungkin diatur sebagai private."

## 4. Aturan Bisnis

| ID | Aturan Bisnis | Deskripsi |
|----|---------------|-----------|
| BR-001 | Normalisasi metrik antar platform | Setiap platform memiliki metrik berbeda. Sistem menormalisasi: engagement = likes + comments + shares + saves untuk semua platform. Untuk YouTube: likes + comments + shares |
| BR-002 | Engagement rate formula | ER = (total engagement / reach) x 100. Jika reach tidak tersedia, gunakan impressions sebagai fallback |
| BR-003 | Follower growth rate | Growth % = ((followers_akhir - followers_awal) / followers_awal) x 100 |
| BR-004 | Periode default | Dashboard default menampilkan 30 hari terakhir. Cache data maksimal 6 jam |
| BR-005 | Rate limit platform | Setiap platform memiliki batas API. Sistem mengatur jadwal sync untuk menghindari rate limit: Instagram 4x/hari, TikTok 6x/hari, YouTube 2x/hari |
| BR-006 | Batas kompetitor | Tier Gratis: 0 kompetitor. Tier Pro: maks 3 kompetitor. Tier Business: maks 10 kompetitor |
| BR-007 | Batas history data | Tier Gratis: 7 hari. Tier Pro: 12 bulan. Tier Business: unlimited |
| BR-008 | Data freshness | Data dianggap "real-time" jika < 1 jam. "Hari ini" jika < 24 jam. "Kemarin" jika > 24 jam |
| BR-009 | Anomali detection threshold | Perubahan > 50% dalam 7 hari dianggap anomali dan trigger notifikasi |
| BR-010 | Postingan terbaik dihitung | Postingan diurutkan berdasarkan engagement rate, bukan engagement absolut |

## 5. Integrasi & Data Flow

```
                           ┌──────────────────────────────────┐
                           │     Frontend SPA (React)         │
                           │  ┌────┐ ┌──────┐ ┌──────────┐   │
                           │  │Dash│ │Content│ │ Audience  │   │
                           │  │board│ │Analytics│ │Insights  │   │
                           │  └─┬──┘ └──┬───┘ └────┬─────┘   │
                           │    │       │           │         │
                           │  ┌─▼───────▼───────────▼──────┐  │
                           │  │    Zustand + TanStack Query│  │
                           │  │    (Client + Server State) │  │
                           │  └──────────────┬─────────────┘  │
                           └─────────────────┼────────────────┘
                                             │
                           ┌─────────────────▼────────────────┐
                           │        API Gateway                │
                           │  ┌───────────────────────────┐   │
                           │  │  /api/v1/analytics/*       │   │
                           │  │  /api/v1/accounts/*        │   │
                           │  │  /api/v1/reports/*         │   │
                           │  └──────────┬────────────────┘   │
                           │             │                    │
                           │  ┌──────────▼────────────────┐   │
                           │  │  Data Aggregation Layer    │   │
                           │  │  (Normalisasi metrik)      │   │
                           │  └──────────┬────────────────┘   │
                           └─────────────────┼────────────────┘
                                             │
        ┌────────────────────────────────────┼────────────────────────┐
        │              Platform Connectors   │                        │
        │                                    │                        │
   ┌────▼─────┐  ┌────▼─────┐  ┌────▼─────┐  ┌────▼─────┐  ┌───────▼───┐
   │ Instagram │  │  TikTok  │  │ YouTube  │  │ Facebook │  │ Twitter/X │
   │ Graph API │  │Business  │  │ Data API │  │Graph API │  │  API v2   │
   └───────────┘  └──────────┘  └──────────┘  └──────────┘  └───────────┘
```

**Alur Data:**
1. Platform Connectors mengambil data dari API masing-masing platform menggunakan token akses pengguna
2. Data aggregation layer menormalisasi format data (misal: Instagram "follower_count" -> SosmedInsight "followers")
3. Data disimpan di database internal (PostgreSQL) untuk history dan caching
4. Frontend memanggil API Gateway yang membaca dari database
5. TanStack Query di frontend melakukan caching client-side (staleTime: 5 menit)
6. Saat user request sync manual, sistem memanggil API platform dan memperbarui database
7. Automated sync berjalan setiap 6 jam via cron job backend

**Mock Flow (Development):**
- MSW mencegat semua panggilan API di browser
- Data mock dihasilkan oleh generator dengan seed tetap untuk konsistensi
- Delay artifisial 200-800ms mensimulasikan latency API nyata
- State mutation menggunakan Map in-memory agar CRUD kompetitor dan postingan realistis

## 6. Non-Functional Requirements

| Kategori | Requirement | Target |
|----------|-------------|--------|
| Performance | Waktu muat dashboard overview | < 2 detik (FCP) |
| Performance | Render grafik dengan 365 data points | < 500 ms |
| Performance | Ukuran bundle JS (initial) | < 200 KB gzipped |
| Performance | Export PDF laporan 10 halaman | < 10 detik |
| Scalability | Jumlah akun per pengguna | Maks 50 akun |
| Scalability | Jumlah data points per grafik | Maks 1.000 titik data |
| Availability | Uptime web app | 99% (monthly) |
| Security | Enkripsi token API | AES-256 di database |
| Security | Token tidak pernah terekspos ke frontend | Hanya refresh token via backend proxy |
| Usability | Waktu belajar pengguna baru | < 15 menit untuk baca dashboard |
| Usability | Bahasa Indonesia baku di seluruh UI | 100% |
| Compatibility | Browser support | Chrome, Firefox, Safari, Edge (2 versi terakhir) |
| Compatibility | Responsive | Desktop (1920x1080) hingga tablet (768x1024) |

## 7. Prioritization Matrix

| Fitur | Value (1-5) | Effort (1-5) | Priority |
|-------|-------------|--------------|----------|
| Dashboard overview metrik | 5 | 2 | P0 |
| Grafik followers trend | 5 | 2 | P0 |
| Post performance table | 5 | 3 | P0 |
| Multi-platform connection (IG, TikTok) | 5 | 4 | P0 |
| Stat card dengan sparkline | 4 | 1 | P0 |
| Filter periode waktu | 4 | 1 | P0 |
| Engagement trend chart | 4 | 2 | P0 |
| Top posts identification | 4 | 2 | P0 |
| Audience demographics | 4 | 3 | P1 |
| Kompetitor comparison | 4 | 3 | P1 |
| Period comparison | 3 | 2 | P1 |
| Report export PDF | 3 | 3 | P1 |
| Hashtag analysis | 3 | 2 | P1 |
| Best time to post | 3 | 2 | P1 |
| Content calendar | 3 | 3 | P1 |
| Automated insight | 3 | 4 | P2 |
| Sentiment analysis | 2 | 4 | P2 |
| Export CSV | 2 | 1 | P1 |
| White-label report | 3 | 3 | P2 |
| Team collaboration | 4 | 4 | P2 |

Value: 1 (rendah) - 5 (sangat tinggi) | Effort: 1 (kecil) - 5 (besar). Prioritas P0 = value >= 4 dan effort <= 3, atau value = 5.
