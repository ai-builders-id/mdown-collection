# BRD: EstateOS — Business Requirements

## 1. Latar Belakang & Konteks Bisnis
Bisnis properti di Indonesia — khususnya sewa apartemen, ruko, dan rumah tinggal — masih banyak dikelola secara manual dengan Excel, buku catatan, dan bahkan ingatan. Pemilik properti kesulitan melacak penyewa, jadwal pembayaran, dan kontrak yang akan berakhir. EstateOS memberikan solusi manajemen properti yang terstruktur, mulai dari listing, penyewa, kontrak, hingga laporan keuangan.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target |
|--------|--------|--------|
| Meningkatkan okupansi properti | % unit terisi | > 90% |
| Mengurangi pembayaran telat | % pembayaran tepat waktu | > 95% |
| Mempercepat penggantian penyewa | Waktu re-let setelah kontrak berakhir | < 14 hari |
| Digitalisasi kontrak | % kontrak digital | 100% |
| Transparansi pendapatan | % pendapatan tercatat di sistem | 100% |

## 3. Value Proposition
- **Property Portfolio View**: Semua properti dalam satu dashboard dengan foto dan status
- **Otomasi Tagihan**: Generate tagihan otomatis setiap bulan, reminder via WhatsApp
- **Tenant Rating**: Rating pembayaran membantu seleksi penyewa berikutnya
- **Kontrak Management**: Template kontrak, tracking perpanjangan, notifikasi H-30
- **Laporan Keuangan**: Pendapatan sewa, piutang, deposit per properti dan per periode

## 4. Target Pengguna

| Persona | Peran | Kebutuhan Utama |
|---------|-------|-----------------|
| Haryanto Tanuwijaya | Pemilik Properti (48 th) | Lihat portfolio, pendapatan, okupansi |
| Dewi Lestari | Manajer Properti (34 th) | Manajemen penyewa, kontrak, pembayaran |
| Andika Pratama | Admin Properti (26 th) | Input data, upload foto, follow-up pembayaran |
| Sari Dewi | Finance (32 th) | Laporan pendapatan, rekap pajak |

## 5. Fitur Prioritas MVP

| Prioritas | Fitur |
|-----------|-------|
| P0 | CRUD properti + upload foto |
| P0 | CRUD penyewa + NIK validasi |
| P0 | Buat kontrak sewa (manual + template) |
| P0 | Dashboard overview (unit, okupansi, pendapatan) |
| P1 | Generate tagihan + status pembayaran |
| P1 | Notifikasi kontrak akan berakhir (< 30 hari) |
| P1 | Gallery + lightbox foto properti |
| P1 | Laporan pendapatan |
| P2 | Map view properti |
| P2 | Template kontrak downloadable |
| P2 | Integrasi WA reminder pembayaran |

## 6. Model Distribusi
- **Web App**: Dashboard manajemen properti
- **Mobile Responsive**: Akses via HP untuk pengecekan di lapangan
- **Deployment**: Cloud (SaaS)
- **Pricing**:
  - Starter: 10 properti, 50 penyewa, 3 user
  - Pro: 100 properti, unlimited penyewa, 50 user, WA reminder
  - Enterprise: Unlimited, custom contract template, API

## 7. KPI & Metrik
- Occupancy Rate: Target > 90%
- On-time Payment Rate: > 95%
- Average Days on Market: < 30 hari untuk properti kosong
- Tenant Retention: > 60% penyewa perpanjang kontrak
- Net Operating Income per properti: Naik 10% per tahun

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan |
|------------|-----------|------------|
| **Travelio** | Listing + sewa online, okupansi tinggi | Fee tinggi, kontrol terbatas, hanya apartemen |
| **Mamik** | Manajemen properti Indonesia | UI kurang modern, fitur terbatas |
| **Excel/Google Sheets** | Gratis, fleksibel | Rentan error, tidak otomatis, tidak ada notifikasi |
| **Buildium** | Fitur properti AS lengkap | Harga USD, tidak support aturan Indonesia, Bahasa Inggris |

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Data penyewa fiktif | Tinggi | Verifikasi NIK via API Dukcapil (Phase 2) |
| Kontrak tidak mengikat hukum | Sedang | Template kontrak direview notaris, disclaimer |
| Pembayaran tidak tercatat | Sedang | Validasi via konfirmasi tenant, bukti transfer wajib |
| Keterlambatan notifikasi | Rendah | Backup notifikasi via email + WhatsApp Gateway |

## 10. Rencana Implementasi
- **Phase 1 (Minggu 1-3)**: Properti — CRUD, foto, gallery grid, search filter
- **Phase 2 (Minggu 4-6)**: Penyewa & Kontrak — CRUD tenant, buat kontrak, durasi
- **Phase 3 (Minggu 7-9)**: Pembayaran — Generate tagihan, status, dashboard income
- **Phase 4 (Minggu 10-12)**: Laporan & Map — Laporan keuangan, map view, export data
