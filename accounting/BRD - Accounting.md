# BRD: BukuWarung Akuntansi — Business Requirements

## 1. Latar Belakang & Konteks Bisnis

UMKM di Indonesia (total ±65 juta unit usaha) masih sangat bergantung pada pencatatan manual atau spreadsheet untuk pembukuan keuangan. Kurang dari 20% UMKM menggunakan software akuntansi dikarenakan:

- **Biaya tinggi:** Software akuntansi enterprise (seperti Accurate, Jurnal) terlalu mahal untuk UMKM mikro
- **Kompleksitas:** UI yang rumit dan tidak intuitif untuk pemilik usaha non-akuntan
- **Bahasa:** Mayoritas software berbahasa Inggris atau tidak menggunakan istilah akuntansi Indonesia yang sesuai PSAK
- **Mobile-first:** Kebutuhan akses dari smartphone untuk pemilik usaha yang mobile

BukuWarung Akuntansi hadir sebagai solusi akuntansi double-entry berbasis cloud yang terjangkau, mudah digunakan, dan berbahasa Indonesia.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target (12 bulan) | Prioritas |
|--------|--------|-------------------|-----------|
| Mendapatkan pengguna aktif | MAU (Monthly Active Users) | 5.000 pengguna | P0 |
| Meningkatkan retensi | Retention rate bulan-3 | >60% | P0 |
| Monetisasi | Konversi free-to-paid | 8% | P1 |
| Kepuasan pengguna | NPS Score | >40 | P1 |
| Akurasi data | Error rate jurnal | <0.5% | P0 |
| Waktu onboarding | Time-to-first-journal | <5 menit | P0 |

## 3. Value Proposition

| Masalah | Solusi BukuWarung |
|---------|-------------------|
| Pencatatan manual rawan error | Sistem double-entry dengan validasi otomatis |
| Spreadsheet tidak terstruktur | Chart of Account standar PSAK dengan kustomisasi |
| Laporan butuh waktu lama | Generate laporan keuangan 1 klik |
| Istilah asing sulit dipahami | Bahasa Indonesia dengan istilah PSAK |
| Biaya software mahal | Pricing terjangkau mulai dari Rp50.000/bulan |
| Tidak ada backup data | Cloud sync otomatis |

## 4. Target Pengguna

### Persona 1: Rina — Pemilik Toko Kelontong
- **Usia:** 35 tahun
- **Latar belakang:** Lulusan SMA, tidak punya latar akuntansi
- **Usaha:** Toko kelontong di pasar tradisional, omzet Rp30-50 juta/bulan
- **Kebutuhan:** Mencatat pemasukan/pengeluaran, tahu untung-rugi tiap bulan
- **Pain point:** Bingung dengan istilah akuntansi, takut salah catat
- **Device:** Smartphone Android, kadang laptop pinjaman

### Persona 2: Dimas — Akuntan Freelance
- **Usia:** 28 tahun
- **Latar belakang:** D3 Akuntansi, melayani 5-10 klien UKM
- **Kebutuhan:** Platform multi-entity, bisa generate laporan untuk klien
- **Pain point:** Repot manage banyak spreadsheet untuk setiap klien
- **Device:** Laptop Windows, tablet

### Persona 3: Budi — Manajer Keuangan Startup
- **Usia:** 40 tahun
- **Latar belakang:** S1 Manajemen, mengelola keuangan startup 20 karyawan
- **Kebutuhan:** Laporan real-time, approval workflow, integrasi bank
- **Pain point:** Butuh laporan akurat untuk investor dan pajak
- **Device:** Laptop macOS, smartphone

## 5. Fitur Prioritas MVP

| ID | Fitur | Prioritas | Kompleksitas | Timeline |
|----|-------|-----------|--------------|----------|
| P0-01 | Chart of Account management | P0 | Medium | Sprint 1 |
| P0-02 | Jurnal umum (single & double entry) | P0 | High | Sprint 1-2 |
| P0-03 | Buku Besar otomatis | P0 | High | Sprint 2 |
| P0-04 | Laporan Laba Rugi | P0 | Medium | Sprint 3 |
| P0-05 | Laporan Neraca | P0 | Medium | Sprint 3 |
| P0-06 | Multi-periode akuntansi | P0 | Medium | Sprint 3 |
| P1-07 | Neraca Lajur (Trial Balance) | P1 | Medium | Sprint 4 |
| P1-08 | Export PDF/Excel | P1 | Low | Sprint 4 |
| P1-09 | Template akun default | P1 | Low | Sprint 1 |
| P1-10 | Pencarian & filter transaksi | P1 | Medium | Sprint 2 |
| P1-11 | Upload bukti transaksi (foto) | P1 | Medium | Sprint 4 |
| P2-12 | Multi-user & role | P2 | High | Sprint 5 |
| P2-13 | Laporan Arus Kas | P2 | High | Sprint 5 |
| P2-14 | Integrasi rekening bank | P2 | High | Sprint 6 |
| P2-15 | Approval workflow | P2 | Medium | Sprint 5 |

## 6. Model Distribusi

- **Mobile App:** Android (Play Store) — primary distribution channel
- **Web App:** PWA (Progressive Web App) — secondary
- **Marketplace:** Listing di Google Play, website landing page
- **Partnership:** Integrasi dengan penyedia jasa pajak (Klikpajak, OnlinePajak)
- **Komunitas:** WhatsApp group UKM, forum pengusaha
- **Freemium Model:** Gratis untuk 1 entitas + 50 transaksi/bulan, berbayar untuk unlimited

## 7. KPI & Metrik

| KPI | Definisi | Target | Frekuensi |
|-----|----------|--------|-----------|
| MAU | Pengguna unik yang login dalam 30 hari | 5.000 | Bulanan |
| Transaksi/bulan | Rata-rata jurnal per user | >50 | Bulanan |
| Retention D+30 | % user aktif 30 hari setelah signup | >70% | Mingguan |
| Time-to-value | Waktu dari signup ke jurnal pertama | <5 menit | Harian |
| NPS | Net Promoter Score | >40 | Kuartalan |
| Churn rate | % user berhenti berlangganan | <8%/bulan | Bulanan |
| Report generation | Jumlah laporan digenerate/user | >3/bulan | Bulanan |

## 8. Analisis Kompetitor

| Kompetitor | Kekuatan | Kelemahan | Harga/bulan |
|------------|----------|-----------|-------------|
| **Accurate Online** | Fitur lengkap, sudah trusted | Mahal, UI kompleks, terlalu enterprise | Rp150.000+ |
| **Jurnal (Mekari)** | Cloud-based, fitur HR/Gaji | Mahal untuk UKM kecil, overkill | Rp99.000+ |
| **BukuWarung (existing)** | UI sederhana, gratis | Bukan double-entry, hanya cash basis | Gratis |
| **Zahir** | Fitur standar akuntansi | Kurang mobile-friendly, desktop-first | Rp175.000+ |
| **Excel/Spreadsheet** | Gratis, fleksibel | Rawan error, tidak real-time, tidak aman | Gratis |
| **BukuKas** | Mobile-first, gratis | Fokus pencatatan, bukan akuntansi penuh | Gratis |

**Differentiator BukuWarung Akuntansi:** Double-entry penuh, UI dioptimalkan untuk mobile, bahasa Indonesia murni, harga terjangkau untuk UKM.

## 9. Risiko & Mitigasi

| Risiko | Dampak | Probabilitas | Mitigasi |
|--------|--------|--------------|----------|
| Kesalahan perhitungan akuntansi | Tinggi | Rendah | Automated testing, peer review kode |
| Data hilang/rusak | Tinggi | Rendah | Backup harian, point-in-time recovery |
| Adopsi rendah | Tinggi | Sedang | Onboarding interaktif, tutorial video |
| Kompetitor tiru fitur | Sedang | Sedang | Fokus pada kualitas UX, brand loyalty |
| Regulasi pajak berubah | Sedang | Rendah | Modular tax engine, update berkala |
| Keamanan data finansial | Tinggi | Rendah | Enkripsi end-to-end, sertifikasi ISO |

## 10. Rencana Implementasi

| Fase | Durasi | Milestone |
|------|--------|-----------|
| **Fase 1: Foundation** | 4 minggu | COA, Jurnal, Buku Besar |
| **Fase 2: Pelaporan** | 3 minggu | Laba Rugi, Neraca, Trial Balance |
| **Fase 3: Quality & Export** | 2 minggu | Export PDF/Excel, search |
| **Fase 4: Multi-user** | 3 minggu | Role management, approval |
| **Fase 5: Advanced** | 4 minggu | Arus Kas, integrasi bank |
| **Beta Closed** | 2 minggu | 50 user beta |
| **Launch Publik** | — | Play Store & Web |
