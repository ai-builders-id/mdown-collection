# BRD: FleetPro — Business Requirements

## 1. Latar Belakang & Konteks Bisnis
Perusahaan distribusi dan logistik di Indonesia mengelola armada kendaraan dengan biaya operasional yang tinggi — BBM, tol, perawatan, dan gaji driver. Tanpa sistem terpadu, biaya operasional bocor, jadwal service terlewat, dan utilisasi kendaraan tidak optimal. FleetPro memberikan visibilitas penuh atas operasional armada untuk menekan biaya dan meningkatkan efisiensi.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target |
|--------|--------|--------|
| Menekan biaya BBM | Konsumsi BBM rata-rata per km | Turun 10% |
| Meningkatkan utilisasi kendaraan | % kendaraan aktif digunakan | > 85% |
| Mengurangi downtime | % kendaraan dalam perbaikan | < 10% |
| Mencegah pajak/SIM mati | % kepatuhan pajak & KIR tepat waktu | 100% |
| Transparansi biaya operasional | % biaya perjalanan tercatat | 100% |

## 3. Value Proposition
- **Live Fleet Tracking**: Pantau posisi dan status semua kendaraan real-time
- **BBM Intelligence**: Analisis konsumsi BBM per kendaraan, deteksi anomali (boros)
- **Service Reminder**: Otomatis ingatkan jadwal service, pajak, KIR, SIM
- **Trip Cost Calculator**: Hitung total biaya per perjalanan (BBM + tol + lain-lain)
- **Driver Performance**: Rating, pelanggaran, efisiensi driving behavior

## 4. Target Pengguna

| Persona | Peran | Kebutuhan Utama |
|---------|-------|-----------------|
| Haryanto Suwandi | Manajer Armada (40 th) | Tracking kendaraan, laporan operasional, biaya |
| Dedi Kurniawan | Supervisor Logistik (32 th) | Penugasan driver, jadwal trip, cek kelengkapan |
| Maya Anggraini | Finance (35 th) | Rekap biaya BBM, tol, service per bulan |
| Supardi | Driver (45 th) | Laporan trip via mobile, scan barcode BBM |

## 5. Fitur Prioritas MVP

| Prioritas | Fitur |
|-----------|-------|
| P0 | CRUD kendaraan + detail lengkap |
| P0 | CRUD driver + SIM |
| P0 | Check-in/check-out trip (start-end, KM, biaya) |
| P1 | Manajemen BBM (pencatatan isi bensin) |
| P1 | Jadwal service + reminder pajak/KIR/SIM |
| P1 | Dashboard operasional harian |
| P2 | Map tracking (real-time / mock) |
| P2 | Driver mobile app untuk laporan trip |
| P2 | Integrasi GPS device (GPS tracker IoT) |

## 6. Model Distribusi
- **Web App**: Dashboard manajer armada
- **Mobile PWA**: Driver untuk check-in/check-out trip
- **Deployment**: Cloud (SaaS), support multi-perusahaan
- **Pricing**:
  - Starter: 5 kendaraan, 10 driver, basic tracking
  - Business: 20 kendaraan, 50 driver, + map + reports
  - Enterprise: Unlimited, GPS integration, custom API

## 7. KPI & Metrik
- Fleet Utilization Rate: Target > 85%
- Average Fuel Cost per km: Target < Rp 1.500/km
- Service Compliance: 100% tepat waktu
- Trip Documentation Rate: 100% trip tercatat
- Driver Retention: Turnover driver < 15%/tahun

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan |
|------------|-----------|------------|
| **GPS Trackit** | Tracking GPS real-time, hardware included | Harga mahal, tidak ada manajemen BBM |
| **Fleetio** | Fitur lengkap, UI modern | Bahasa Inggris, harga USD, tidak support pajak Indonesia |
| **Manual (Buku/Catatan)** | Gratis | Rawan hilang, tidak analitik, tidak real-time |
| **WhatsApp group** | Mudah, semua pakai | Tidak terstruktur, data berserakan |

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Driver tidak mengisi data trip | Tinggi | Wajib input untuk gaji, reward kepatuhan |
| Koneksi internet di perjalanan terbatas | Sedang | Mode offline, sync saat online |
| Data BBM fiktif (mark-up) | Tinggi | Validasi via foto struk + geolocation SPBU |
| Plat nomor duplikat | Rendah | Validasi unik + format standar |

## 10. Rencana Implementasi
- **Phase 1 (Minggu 1-3)**: Kendaraan & Driver — CRUD, import data
- **Phase 2 (Minggu 4-6)**: Trip Management — check-in/out, biaya, map
- **Phase 3 (Minggu 7-9)**: BBM & Service — pencatatan, reminder, grafik
- **Phase 4 (Minggu 10-12)**: Laporan + Ekspor — dashboard eksekutif, export data
