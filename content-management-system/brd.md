# BRD: NusantaraCMS — Business Requirements
**Project:** NusantaraCMS

## 1. Latar Belakang & Konteks Bisnis
Pemerintah daerah, desa, organisasi nirlaba, dan UMKM di Indonesia menghadapi tantangan besar dalam mengelola konten digital mereka. Banyak institusi masih menggunakan media sosial sebagai satu-satunya saluran komunikasi, atau bergantung pada pengembang web untuk mengupdate website — yang memakan biaya dan waktu.

Berdasarkan survei internal terhadap 120 instansi pemerintah desa/kota dan 50 organisasi nirlaba di Indonesia:
- 78% masih menggunakan website statis yang jarang diupdate (update < 1x/bulan)
- 65% tidak memiliki staf dengan kemampuan teknis untuk mengelola CMS
- 82% ingin memiliki blog/halaman berita tetapi terkendala biaya pengembangan
- 70% mengalami kesulitan dalam memoderasi komentar dan konten pengguna

NusantaraCMS hadir sebagai solusi CMS tanpa coding yang terjangkau, berbahasa Indonesia, dan dirancang khusus untuk kebutuhan organisasi di Indonesia — dengan fitur manajemen konten, media, komentar, dan pengguna yang lengkap.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target (12 bulan) | Cara Ukur |
|--------|--------|--------------------|-----------|
| Meningkatkan adopsi CMS di instansi pemerintah desa | Jumlah pengguna aktif bulanan (MAU) | 5.000 pengguna terdaftar, 1.500 MAU | Dashboard admin |
| Mempercepat publikasi konten | Rata-rata waktu dari login ke terbit | < 10 menit | Log aktivitas sistem |
| Mengurangi ketergantungan pada pengembang | % konten yang diterbitkan tanpa bantuan teknis | > 90% | Survey pengguna |
| Meningkatkan frekuensi update konten | Rata-rata publikasi per instansi/bulan | 15 artikel/bulan | Analytics sistem |
| Mendapatkan revenue berkelanjutan | Monthly Recurring Revenue (MRR) | Rp 150.000.000 | Sistem billing |
| Menjangkau 30 provinsi | Sebaran geografis pengguna | 30 provinsi | Data registrasi |

## 3. Value Proposition

**Untuk Pemerintah Desa/Kota:**
- Buat dan publikasi konten tanpa keahlian teknis — cukup bisa mengetik dan upload foto
- Template halaman yang sesuai standar pemerintahan Indonesia
- Fitur SEO otomatis membantu konten ditemukan warga
- Manajemen konten terpusat untuk seluruh dinas/unit kerja
- Biaya lebih hemat dibanding menyewa pengembang web

**Untuk Organisasi Nirlaba:**
- Platform donasi dan campaign terintegrasi dengan konten
- Manajemen relawan dan pengumuman dalam satu tempat
- Laporan dampak yang bisa dipublikasikan langsung

**Untuk UMKM:**
- Toko online sederhana terintegrasi dengan blog
- Galeri produk dengan katalog digital
- Update promo dan berita usaha dengan cepat

## 4. Target Pengguna

### Primary Persona: Pak Budi — Kepala Dinas Kominfo Kabupaten
- **Usia:** 45 tahun
- **Pendidikan:** S1 Ilmu Pemerintahan
- **Pekerjaan:** Kepala Dinas Komunikasi dan Informatika
- **Kemampuan teknis:** Dasar — bisa menggunakan Microsoft Office, WhatsApp, media sosial
- **Kebutuhan:** Ingin website dinasnya aktif dengan berita dan informasi layanan publik, tetapi staf IT hanya 2 orang dan sibuk dengan infrastruktur
- **Pain points:** Harus menunggu staf IT untuk update konten, proses birokrasi untuk perubahan konten, biaya maintenance website mahal
- **Goal:** Website dinas bisa update setiap hari dengan konten dari seluruh bidang

### Secondary Persona: Ibu Sari — Staf Humas Desa
- **Usia:** 28 tahun
- **Pendidikan:** D3 Komunikasi
- **Pekerjaan:** Staf Hubungan Masyarakat Desa
- **Kemampuan teknis:** Menengah — aktif di media sosial, pernah menggunakan WordPress
- **Kebutuhan:** Ingin membuat website desa yang informatif dengan berita kegiatan desa, pengumuman, dan galeri foto
- **Pain points:** Anggaran desa terbatas untuk website, sulit mencari vendor yang paham kebutuhan desa, takut typo karena tidak ada editor
- **Goal:** Website desa menjadi sumber informasi utama warga dan meningkatkan citra desa

## 5. Fitur Prioritas MVP

| Prioritas | Fitur | Keterangan |
|-----------|-------|------------|
| **P0** | Manajemen Blog Post | Buat, edit, hapus, publikasi, jadwalkan artikel dengan rich text editor |
| **P0** | Manajemen Halaman | Buat dan kelola halaman statis dengan template pilihan |
| **P0** | Kategori & Tag | Organisasi konten dengan kategori bertingkat dan tag |
| **P0** | Media Library | Upload, kelola, dan sisipkan gambar/dokumen ke konten |
| **P0** | Autosave | Simpan otomatis setiap 30 detik saat menulis |
| **P0** | Role-based Access | Admin, Editor, Penulis, Kontributor |
| **P1** | Manajemen Komentar | Moderasi komentar (setuju/tolak/spam) |
| **P1** | SEO Management | Meta title, description, Open Graph, sitemap |
| **P1** | Kalender Publikasi | Jadwalkan konten dengan kalender visual |
| **P1** | Dashboard Statistik | Total post, views, komentar, grafik tren |
| **P1** | Export/Import Konten | Backup dan migrasi konten (JSON/CSV) |
| **P2** | Multi-bahasa | Dukungan bahasa daerah (inggris, jawa, sunda) |
| **P2** | Template Landing Page | Template khusus halaman muka |
| **P2** | Integrasi Media Sosial | Auto-post ke Facebook/Twitter saat publikasi |
| **P2** | Analytic Terintegrasi | Google Analytics, statistik pengunjung |

## 6. Model Distribusi
NusantaraCMS akan didistribusikan sebagai **Software-as-a-Service (SaaS)** berbasis web dengan model freemium:

**Tier Gratis:**
- 1 pengguna admin
- 5 halaman/blog post
- 50 MB penyimpanan media
- Domain subdomain (*.nusantaracms.go.id)
- Template dasar

**Tier Pro (Rp 150.000/bulan):**
- Hingga 5 pengguna
- Blog post tak terbatas
- 1 GB penyimpanan media
- Kustom domain
- Semua template + kustomisasi CSS
- SEO tools lengkap
- Prioritas support

**Tier Enterprise (Rp 500.000/bulan):**
- Pengguna tak terbatas
- 10 GB penyimpanan media
- Multi-domain
- API akses
- Integrasi SSO
- Pelatihan tim
- SLA 99.9%

Selain SaaS, juga tersedia opsi **self-hosted** untuk instansi yang memerlukan data on-premise dengan lisensi tahunan.

## 7. KPI & Metrik

| KPI | Definisi | Target Q1 | Target Q2 | Target Q3 | Target Q4 |
|-----|----------|-----------|-----------|-----------|-----------|
| Customer Acquisition Cost (CAC) | Biaya pemasaran / jumlah pelanggan baru | Rp 50.000 | Rp 40.000 | Rp 35.000 | Rp 25.000 |
| Monthly Active Users (MAU) | Pengguna login dalam 30 hari | 500 | 1.200 | 2.500 | 5.000 |
| Churn Rate | % pelanggan berhenti berlangganan | < 8% | < 6% | < 5% | < 4% |
| Net Promoter Score (NPS) | Survey kepuasan pengguna | 40 | 50 | 55 | 60 |
| Time to Publish | Rata-rata waktu dari login ke terbit | 15 menit | 12 menit | 10 menit | 8 menit |
| Konten per Pengguna | Rata-rata konten/bulan per instansi | 5 | 8 | 12 | 15 |
| Page Load Time | Waktu muat dashboard | < 3 detik | < 2 detik | < 1.5 detik | < 1 detik |

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | Posisi Kami |
|------------|-----------|------------|-------------|
| **WordPress** | Ekosistem plugin besar, fleksibel | Butuh hosting sendiri, kompleks, tidak fokus Indonesia, biaya plugin mahal | Lebih sederhana, siap pakai, bahasa Indonesia, harga tetap |
| **Wix** | Drag-and-drop mudah, template cantik | Mahal, tidak optimasi SEO Indonesia, pelan di akses Indonesia | Lebih cepat, harga lebih murah, konten lokal |
| **Ghost** | Fokus blogging, clean UI | Fitur terbatas, tidak ada halaman statis, bayar per publikasi | Fitur lebih lengkap, model harga lebih cocok institusi |
| **Blogger** | Gratis, terintegrasi Google | Terbatas, tidak profesional, tidak support kustom domain gratis | Lebih profesional, fitur lengkap, cocok untuk instansi |
| **Website desa statis** | Sesuai kebutuhan dasar | Tidak bisa update mandiri, tergantung pengembang | CMS tanpa coding, update mandiri 100% |

**Keunggulan kompetitif NusantaraCMS:**
- 100% Bahasa Indonesia — dari UI hingga dokumentasi
- Disain khusus untuk institusi Indonesia (pemerintahan, desa, organisasi nirlaba)
- Autosave dan kolaborasi tim yang seamless
- Harga yang terjangkau untuk anggaran pemerintah desa
- Dukungan pelanggan via WhatsApp dan telepon

## 9. Risiko & Mitigasi

| Risiko | Dampak | Probabilitas | Mitigasi |
|--------|--------|--------------|----------|
| Adopsi rendah karena kebiasaan pakai WordPress | Tinggi | Sedang | Campaign edukasi, migration tool dari WordPress, webinar gratis |
| Koneksi internet lambat di daerah | Sedang | Tinggi | Optimasi performa, mode offline, progressive web app |
| Keamanan data konten pemerintah | Tinggi | Sedang | Sertifikasi keamanan, encryption, backup harian, compliance Kominfo |
| Kompetitor meniru fitur | Sedang | Tinggi | Fokus pada konten lokal dan kualitas dukungan, bangun komunitas |
| Churn karena keterbatasan fitur | Sedang | Sedang | Roadmap transparan, feedback loop dengan pengguna, rilis fitur tiap 2 minggu |
| Biaya infrastruktur membengkak | Tinggi | Rendah | Arsitektur cloud efisien, auto-scaling, monitoring biaya real-time |

## 10. Rencana Implementasi

### Fase 1 — Foundation (Bulan 1-2)
- Setup project Vite + React + TypeScript + Tailwind
- Implementasi layout dasar: sidebar, topbar, routing
- Modul Blog: CRUD artikel dengan rich text editor sederhana
- Modul Halaman: CRUD halaman statis
- Autentikasi dan role-based access
- Dark mode tidak termasuk MVP

### Fase 2 — Core Features (Bulan 3-4)
- Media Library dengan upload drag-and-drop
- Kategori dan Tag management
- Autosave dan version history
- Manajemen komentar dan moderasi
- Dashboard statistik dasar
- Kalender publikasi

### Fase 3 — Polish & Advanced (Bulan 5-6)
- SEO tools: meta, sitemap, Open Graph
- Export/Import konten
- Multi-bahasa untuk UI (Indonesia/Inggris)
- Integrasi media sosial (auto-post)
- Template landing page
- Performance optimization

### Fase 4 — Scale (Bulan 7-12)
- Sistem billing dan subscription management
- Enterprise features: SSO, API, multi-domain
- Self-hosted deployment package
- Mobile app (React Native) untuk menulis konten
- Marketplace template
- Komunitas pengguna dan forum
