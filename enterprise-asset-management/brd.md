# BRD: EAM Pro — Business Requirements

## 1. Latar Belakang & Konteks Bisnis
Perusahaan di Indonesia, khususnya sektor manufaktur dan perkantoran, sering kehilangan track aset fisik mereka — laptop hilang, mesin tidak terawat, dan data pembelian tersebar di invoice kertas. EAM Pro hadir untuk memberikan visibilitas penuh atas siklus hidup aset, dari pengadaan hingga penghapusan, dengan fokus pada perawatan preventif dan akurasi data depresiasi.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target |
|--------|--------|--------|
| Meningkatkan akurasi inventaris aset | % aset terverifikasi secara fisik | > 95% |
| Memperpanjang umur aset | Rata-rata waktu operasional sebelum rusak | Naik 30% |
| Menurunkan biaya maintenance dadakan | % maintenance korektif vs preventif | Preventif > 70% |
| Optimalisasi depresiasi | Selisih nilai aset di sistem vs akuntansi | < 3% |
| Digitalisasi data aset | % aset dengan data digital lengkap | 100% |

## 3. Value Proposition
- **Lifecycle Visibility**: Lacak aset dari pembelian hingga disposal dalam satu platform
- **Maintenance Preventif**: Jadwal otomatis + reminder, kurangi downtime mesin
- **Depresiasi Otomatis**: Hitung penyusutan dengan berbagai metode, siap audit
- **QR/Barcode Label**: Cetak label untuk identifikasi aset di lapangan via scan
- **Laporan Siap Audit**: Semua riwayat aset tercatat, traceable, dan compliance

## 4. Target Pengguna

| Persona | Peran | Kebutuhan Utama |
|---------|-------|-----------------|
| Agus Wijaya | Manajer Fasilitas (45 th) | Tracking aset fisik, jadwal maintenance, vendor |
| Rina Marlina | Staff Asset (28 th) | Input aset baru, scan label, update kondisi |
| Hendra Kusuma | Finance Manager (38 th) | Data depresiasi, nilai aset, laporan audit |
| Pak Winarto | Direktur Operasi (55 th) | Dashboard executive, keputusan replacement |

## 5. Fitur Prioritas MVP

| Prioritas | Fitur |
|-----------|-------|
| P0 | CRUD aset + kategori + lokasi |
| P0 | Dashboard overview total/nilai/kondisi aset |
| P1 | Jadwal maintenance preventif + reminder |
| P1 | Riwayat maintenance + biaya |
| P1 | Hitung depresiasi otomatis (3 metode) |
| P1 | Generate QR/barcode untuk label |
| P2 | Mobile scan untuk opname fisik |
| P2 | Integrasi dengan ERP/akuntansi |
| P2 | Approval workflow untuk disposal |

## 6. Model Distribusi
- **Web App**: Dashboard utama untuk manajemen
- **Mobile PWA**: Scanning aset dan update status di lapangan
- **Deployment**: Cloud (SaaS) multi-tenant
- **Pricing**:
  - Basic: 500 aset, 3 user, maintenance dasar
  - Pro: 5000 aset, 50 user, depresiasi + QR
  - Enterprise: Unlimited, API, dedicated support

## 7. KPI & Metrik
- Asset Utilization Rate: Target > 80%
- Preventive Maintenance Compliance: > 90%
- Asset Data Completeness: 100%
- User Adoption: 70% staf terkait menggunakan sistem dalam 2 bulan
- Audit Readiness: Data siap audit kapan saja tanpa persiapan khusus

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan |
|------------|-----------|------------|
| **SAP EAM** | Komprehensif, enterprise grade | Mahal, kompleks, butuh konsultan |
| **Asset Panda** | Mobile-first, cloud | Bahasa Inggris, harga USD, tidak sesuai akuntansi Indonesia |
| **Spreadsheet Excel** | Gratis, familiar | Rawan error, tidak real-time, tidak ada reminder |
| **SIMPEG/aset pemerintah** | Gratis untuk instansi | Fitur terbatas, UI usang |

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Data aset tidak akurat saat migrasi | Tinggi | Opname fisik sebelum migrasi, validasi ganda |
| User lupa melakukan maintenance rutin | Sedang | Notifikasi otomatis via email/WA |
| Kerusakan aset tidak dilaporkan | Sedang | Wajib lapor via QR scan, insentif pelaporan |
| Duplikasi data aset | Rendah | Validasi nomor seri + barcode unik |

## 10. Rencana Implementasi
- **Phase 1 (Minggu 1-3)**: Aset — CRUD, kategori, lokasi, import Excel
- **Phase 2 (Minggu 4-6)**: Maintenance — Jadwal, riwayat, notifikasi
- **Phase 3 (Minggu 7-9)**: Depresiasi — Perhitungan otomatis, grafik, laporan
- **Phase 4 (Minggu 10-12)**: Label QR + mobile scan + export data
