# BRD: PengadaanCerdas — Business Requirements

## 1. Latar Belakang & Konteks Bisnis

Proses pengadaan barang dan jasa di perusahaan Indonesia masih banyak dilakukan secara manual — menggunakan email, spreadsheet, dan dokumen fisik. Masalah utama yang dihadapi:

- **Proses approval lambat:** PO dikirim via email, menunggu approval berhari-hari tanpa tracking yang jelas
- **Data supplier tidak terpusat:** Informasi vendor tersebar di berbagai file dan kontak pribadi
- **Risiko kesalahan:** Double order, salah harga, atau PO kadaluwarsa karena tidak ada sistem monitoring
- **Tidak ada audit trail:** Sulit melacak siapa yang menyetujui apa dan kapan
- **Rendahnya visibilitas:** Manajemen tidak bisa melihat total pengeluaran dan status PO secara real-time

PengadaanCerdas hadir sebagai solusi procurement digital yang terpusat, transparan, dan efisien untuk membantu perusahaan mengelola siklus pengadaan dari awal hingga akhir.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target (12 bulan) | Prioritas |
|--------|--------|-------------------|-----------|
| Mempercepat siklus approval PO | Waktu dari submit ke approve | < 2 hari kerja | P0 |
| Meningkatkan visibilitas pengadaan | % PO yang ter-track dalam sistem | > 95% | P0 |
| Mengurangi kesalahan pemesanan | Tingkat error PO (salah item/harga) | < 1% | P0 |
| Memusatkan data supplier | Supplier aktif terdaftar | 100% | P0 |
| Meningkatkan efisiensi pembuatan PO | Waktu pembuatan PO | < 15 menit | P1 |
| Transparansi anggaran | Pengeluaran tercatat real-time | 100% | P1 |

## 3. Value Proposition

| Masalah Bisnis | Solusi PengadaanCerdas |
|----------------|----------------------|
| Proses manual lambat dan tidak terstruktur | Sistem PO digital dengan workflow approval otomatis |
| Data supplier tersebar dan tidak terpusat | Database supplier terintegrasi dengan histori lengkap |
| Approval tidak terlihat dan tidak teraudit | Timeline approval transparan dengan catatan setiap langkah |
| Tidak ada kontrol anggaran real-time | Dashboard pengeluaran dan utilisasi budget |
| Risiko duplikasi dan kesalahan pesanan | Auto-generate nomor PO, validasi otomatis, anti-duplikasi |
| Sulit tracking status pengadaan | Status visual dari Draft hingga Received di setiap PO |

## 4. Target Pengguna

### Persona 1: Andi — Procurement Manager (35 tahun)
- **Latar:** S1 Manajemen Logistik, 8 tahun pengalaman procurement
- **Tanggung jawab:** Membuat dan mengelola PO, memilih supplier, memastikan barang tiba tepat waktu
- **Kebutuhan:** Dashboard real-time status semua PO, form PO cepat, tracking approval, riwayat per supplier
- **Pain:** "Report PO dari email dan spreadsheet memakan waktu 3-4 jam per minggu"
- **Device:** Laptop Windows (kantor), Smartphone Android

### Persona 2: Siska — Finance Manager (42 tahun)
- **Latar:** S1 Akuntansi, Certified Financial Analyst
- **Tanggung jawab:** Menyetujui PO di atas nilai tertentu, memonitor anggaran departemen
- **Kebutuhan:** Approval queue dengan info lengkap, dashboard pengeluaran per departemen, export data
- **Pain:** "Saya perlu lihat pengeluaran per departemen real-time, bukan nunggu akhir bulan"
- **Device:** Laptop macOS

### Persona 3: Bambang — Supplier/Vendor PIC (38 tahun)
- **Latar:** Sales Manager PT Sinar Jaya Komputer
- **Kebutuhan:** Menerima PO yang sudah di-order, melihat status pembayaran, riwayat transaksi
- **Pain:** "Sering PO dikirim telat atau salah alamat karena komunikasi via WA"
- **Device:** Smartphone, laptop

## 5. Fitur Prioritas MVP

| ID | Fitur | Prioritas | Kompleksitas | Deskripsi |
|----|-------|-----------|--------------|-----------|
| P0-01 | Dashboard Procurement | P0 | Medium | Kartu statistik, grafik pengeluaran, queue approval |
| P0-02 | Daftar Purchase Order | P0 | High | Tabel PO dengan filter, search, sorting |
| P0-03 | Form Buat/Edit PO | P0 | High | Form lengkap dengan items, supplier select, validasi |
| P0-04 | Status Workflow PO | P0 | High | Draft -> Submit -> Approve -> Order -> Receive |
| P0-05 | Manajemen Supplier | P0 | High | CRUD supplier, detail kontak, histori transaksi |
| P0-06 | Approval Queue | P0 | Medium | Daftar PO menunggu approval dengan aksi setujui/tolak |
| P0-07 | Approval Timeline | P0 | Medium | Visual timeline setiap langkah approval |
| P1-08 | Filter & Pencarian PO | P1 | Low | Filter by status, supplier, tanggal, range nominal |
| P1-09 | Export Data PO | P1 | Low | Export daftar PO ke CSV/Excel |
| P1-10 | Dashboard Detail | P1 | Medium | Grafik pengeluaran per supplier, per bulan |
| P1-11 | Panel Detail Supplier | P1 | Low | Preview supplier di panel kanan saat PO dipilih |
| P2-12 | Multi-level Approval | P2 | High | Workflow approval bertingkat (manager -> finance -> director) |
| P2-13 | Notifikasi Real-time | P2 | Medium | Notifikasi saat PO perlu diapprove atau ditolak |
| P2-14 | Manajemen Anggaran | P2 | High | Alokasi budget per departemen, peringatan overbudget |
| P2-15 | Kontrak Management | P2 | High | Kontrak tahunan/proyek dengan remaining value |

## 6. Model Distribusi

- **Cloud SaaS:** Hosting terpusat, akses via browser, subscription bulanan
- **Self-hosted:** Perusahaan bisa deploy sendiri di server internal
- **Pricing:**
  - **Starter (Free):** 50 PO/bulan, 1 user, data lokal
  - **Pro (Rp 150.000/bulan):** Unlimited PO, 10 user, approval workflow, export
  - **Enterprise (Rp 500.000/bulan):** Unlimited users, multi-level approval, budget management, API access
- **Trial:** 14 hari gratis untuk semua fitur Pro

## 7. KPI & Metrik

| KPI | Definisi | Target | Frekuensi |
|-----|----------|--------|-----------|
| PO Processing Time | Waktu dari draft ke PO diterbitkan | < 2 hari | Per PO |
| Approval Cycle Time | Waktu dari submit ke approve | < 1 hari | Per approval |
| Data Completeness | % PO dengan data supplier & item lengkap | > 98% | Bulanan |
| User Adoption | % procurement staff yang menggunakan sistem | > 90% | Bulanan |
| PO Accuracy | % PO tanpa revisi karena error | > 99% | Bulanan |
| Supplier Coverage | % transaksi yang tercatat di sistem | > 95% | Bulanan |
| Report Generation Time | Waktu buat laporan pengadaan | < 5 menit | Per report |

## 8. Analisis Kompetitor

| Kompetitor | Kekuatan | Kelemahan | Harga |
|------------|----------|-----------|-------|
| **SAP Ariba** | Enterprise, global, fitur lengkap | Mahal, kompleks, butuh tim IT | $$$$ |
| **Oracle Procurement** | Integrasi ERP, robust | Enterprise-only, sulit kustomisasi | $$$$ |
| **TenderWell** | Platform tender Indonesia | Fokus tender publik, bukan PO internal | $$$ |
| **Procurement (Mekari)** | Ekosistem Mekari, cloud | Mahal untuk UKM | Rp 200rb+ |
| **Spreadsheet / Email** | Gratis, fleksibel | Rawan error, tidak ada audit trail | Gratis |
| **Paper-based** | Tidak perlu IT | Sangat lambat, rawan hilang | Variatif |

**Differentiator:** PengadaanCerdas = cost-effective (mulai gratis), UI sederhana (tidak perlu training), workflow approval fleksibel (bisa diatur per perusahaan), bahasa Indonesia penuh.

## 9. Risiko & Mitigasi

| Risiko | Dampak | Probabilitas | Mitigasi |
|--------|--------|--------------|----------|
| Adopsi rendah karena tim terbiasa manual | Tinggi | Sedang | UI intuitif, onboarding cepat, champion user |
| Data supplier tidak lengkap saat migrasi | Sedang | Tinggi | Import tools, validasi mandatory fields |
| Kesalahan workflow approval (salah approve) | Tinggi | Rendah | Konfirmasi 2 langkah, recall capability |
| Kehilangan data PO | Tinggi | Rendah | Auto-save draft, cloud backup, version history |
| Tidak ada koneksi internet | Sedang | Rendah | Offline queue, sync saat online |
| Perubahan kebijakan procurement | Sedang | Sedang | Konfigurasi workflow fleksibel |

## 10. Rencana Implementasi

| Fase | Durasi | Milestone | Deliverables |
|------|--------|-----------|-------------|
| **Fase 1: Foundation** | 3 minggu | Dashboard + PO List + PO Form | Layout 3 panel, halaman PO, form create/edit |
| **Fase 2: Core Workflow** | 2 minggu | Status Workflow + Approval Queue | Flow Draft -> Approve -> Receive, timeline visual |
| **Fase 3: Supplier Module** | 2 minggu | Manajemen Supplier + Panel Detail | CRUD supplier, panel detail, histori transaksi |
| **Fase 4: Enhancement** | 2 minggu | Filter, Search, Export, Dashboard | Filtering & search, export CSV, grafik lengkap |
| **Fase 5: Finalization** | 1 minggu | Polish, Testing, Dokumentasi | State handling, responsive, code cleanup |
| **Total MVP** | 10 minggu | Rilis v1.0 | Semua fitur P0 dan P1 selesai |
