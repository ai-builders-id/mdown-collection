# BRD: KasirCerdas — Business Requirements

## 1. Latar Belakang & Konteks Bisnis

Toko retail tradisional di Indonesia (toko kelontong, toko bangunan, toko sembako) masih sangat bergantung pada kalkulator dan nota manual untuk transaksi. Masalah utama:

- **Proses lambat:** Hitung manual dengan kalkulator, tulis nota tangan
- **Rawan kesalahan:** Salah hitung, lupa harga, kembalian salah
- **Tidak ada data:** Pemilik tidak tahu persis omzet harian, produk terlaris, stok
- **Biaya POS mahal:** Software POS enterprise (MES, Kledo) terlalu mahal untuk toko kecil
- **Pelaporan manual:** Rekap penjualan dilakukan dengan buka nota satu-satu

KasirCerdas hadir sebagai solusi POS modern yang terjangkau, cepat, dan mudah digunakan untuk toko retail Indonesia.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target (12 bulan) | Prioritas |
|--------|--------|-------------------|-----------|
| Pengguna aktif | Toko aktif menggunakan POS | 3.000 toko | P0 |
| Transaksi | Rata-rata transaksi/hari/toko | >20 transaksi | P0 |
| Kecepatan kasir | Waktu rata-rata per transaksi | <30 detik | P0 |
| Retensi | Retention bulan ke-3 | >70% | P0 |
| Monetisasi | Paid conversion | 10% | P1 |
| Kepuasan | Rating Play Store | >4.5 | P1 |

## 3. Value Proposition

| Masalah | Solusi KasirCerdas |
|---------|-------------------|
| Hitung manual pakai kalkulator | Scan/tap produk, hitung otomatis |
| Nota tulis tangan | Struk thermal otomatis |
| Tidak tahu omzet harian | Dashboard real-time per shift |
| Harga produk lupa | Pricelist terpusat, scan barcode |
| Stok tidak terkontrol | Update stok otomatis setiap transaksi |
| Biaya POS mahal | Mulai gratis, premium Rp 100.000/bulan |

## 4. Target Pengguna

### Persona 1: Pak Joko — Pemilik Toko Kelontong
- **Usia:** 45 tahun
- **Toko:** Toko Sembako "Joko Makmur" di pasar tradisional
- **Omzet:** Rp 5-10 juta/hari
- **Kebutuhan:** Kasir cepat, tahu omzet harian, kelola stok
- **Pain:** "Anak kasir sering salah hitung, apalagi pas ramai"
- **Device:** Tablet Android 10 inch (dedicated for POS)

### Persona 2: Sari — Kasir Toko Bangunan (23 tahun)
- **Usia:** 23 tahun, lulusan SMK
- **Toko:** Toko Bangunan "Sinar Jaya"
- **Kebutuhan:** Input barang cepat, cetak struk, tanpa ribet
- **Pain:** "Pelanggan banyak yang minta nota, nulis manual pegel"
- **Device:** HP Android + thermal printer Bluetooth

### Persona 3: Hendra — Manajer Toko Retail Chain
- **Usia:** 34 tahun
- **Toko:** 3 cabang toko material
- **Kebutuhan:** Pantau omzet semua cabang, rekap harian
- **Pain:** "Rekap penjualan dari 3 toko manual, makan waktu 2 jam tiap hari"
- **Device:** Laptop untuk admin, tablet untuk kasir

## 5. Fitur Prioritas MVP

| ID | Fitur | Prioritas | Kompleksitas |
|----|-------|-----------|--------------|
| P0-01 | Produk grid dengan kategori | P0 | Medium |
| P0-02 | Scan barcode (kamera/keyboard) | P0 | Medium |
| P0-03 | Cart management (+/-/hapus) | P0 | Medium |
| P0-04 | Pembayaran tunai (hitung kembalian) | P0 | Low |
| P0-05 | Cetak struk thermal (WebUSB) | P0 | High |
| P0-06 | Riwayat transaksi | P0 | Medium |
| P0-07 | Manajemen shift kasir | P0 | Medium |
| P0-08 | Dashboard omzet harian | P0 | Medium |
| P0-09 | Manajemen produk (CRUD) | P0 | Low |
| P1-10 | Pembayaran non-tunai (debit/QRIS) | P1 | High |
| P1-11 | Diskon per item & per transaksi | P1 | Medium |
| P1-12 | Manajemen pelanggan | P1 | Medium |
| P1-13 | Void transaksi (dengan password) | P1 | Low |
| P1-14 | PPN 11% otomatis | P1 | Low |
| P1-15 | Cari produk via nama | P1 | Low |
| P1-16 | Mode offline + sync | P1 | High |
| P2-17 | Multi-toko / cabang | P2 | High |
| P2-18 | Laporan laba kotor per produk | P2 | Medium |
| P2-19 | Integrasi akuntansi | P2 | High |
| P2-20 | Loyalty program poin | P2 | Medium |

## 6. Model Distribusi

- **Primary:** Google Play Store (Android tablet/HP)
- **Web:** Web app untuk admin (lihat laporan, atur produk via laptop)
- **Hybrid POS:** Android app untuk kasir, web untuk manajemen
- **Hardware bundle:** Kerjasama dengan vendor thermal printer (Epson, XPrinter)
- **Pricing:**
  - **Free:** 500 transaksi/bulan, 1 toko, 1 user, cetak struk
  - **Pro (Rp100.000/bulan):** Unlimited transaksi, multi-user, payment non-tunai, laporan lengkap
  - **Enterprise (Rp300.000/bulan):** Multi-cabang, inventory, integrasi akuntansi

## 7. KPI & Metrik

| KPI | Target | Frekuensi |
|-----|--------|-----------|
| MRR (Monthly Recurring Revenue) | Rp 150 juta | Bulanan |
| New stores | 250 toko/bulan | Bulanan |
| Churn rate | <8%/bulan | Bulanan |
| Transaksi/hari/toko | >20 | Harian |
| Time-per-transaction | <30 detik | Harian |
| Offline resilience | >95% transaksi tanpa error | Harian |
| Printer success rate | >98% cetak sukses | Harian |
| Play Store rating | >4.5 | Bulanan |

## 8. Analisis Kompetitor

| Kompetitor | Kekuatan | Kelemahan |
|------------|----------|-----------|
| **MES (Mekari)** | Fitur lengkap, ekosistem besar | Mahal (Rp200rb+), terlalu kompleks untuk toko kecil |
| **Kledo POS** | Cloud-based, akuntansi | On-prem, harga per cabang |
| **Pawoon** | POS mobile, offline mode | Harga perangkat mahal, sewa | 
| **IPos** | Fitur kaya, sudah mature | UI terlalu ramai, harga tinggi |
| **BukuKas (POS)** | Gratis, sudah terkenal | Terlalu simpel, kurang fitur POS |
| **Excel/Nota manual** | Gratis, fleksibel | Lambat, error-prone, tidak ada data historis |

**Differentiator:** KasirCerdas = Simpel (UI seperti makanan cepat saji), Cepat (scan + 2 tap = jadi), harga terjangkau (gratis untuk pemula), offline-first (toko di pelosok tetap bisa), fokus hardware (printer thermal murah).

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Kompatibilitas printer | Tinggi | Support printer ESC/POS standar, test dengan 5 brand utama |
| Offline data loss | Tinggi | Queue mechanism, auto-sync, konfirmasi sebelum clear |
| Adaptasi kasir (tidak tech-savvy) | Sedang | Onboarding interaktif, video tutorial, UI super simpel |
| Device compatibility (Android) | Sedang | Test di 20 device Android populer, minimum Android 9 |
| Koneksi internet tidak stabil | Tinggi | Offline-first architecture, sync queue |
| Transaksi void/fraud | Tinggi | Password supervisor, audit log, batas void per shift |

## 10. Rencana Implementasi

| Fase | Durasi | Milestone |
|------|--------|-----------|
| **Fase 1: Core POS** | 4 minggu | Produk grid, cart, scan barcode, bayar cash + cetak struk |
| **Fase 2: Management** | 2 minggu | Shift, void, riwayat transaksi, dashboard harian |
| **Fase 3: Payment & Customer** | 3 minggu | Non-tunai (QRIS/debit), diskon, pelanggan |
| **Fase 4: Offline & Polish** | 2 minggu | Offline queue, sync engine, responsive admin web |
| **Fase 5: Advanced** | 3 minggu | Multi-toko, loyalty, laporan laba |
| **Beta** | 3 minggu | 50 toko beta |
| **Launch** | — | Play Store + partnership printer |
