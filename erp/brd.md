# BRD: ERP Nusantara — Business Requirements

## 1. Latar Belakang & Konteks Bisnis

Perusahaan menengah di Indonesia (skala 20-500 karyawan) menghadapi tantangan operasional yang signifikan:

- **Data terfragmentasi:** Setiap departemen menggunakan sistem berbeda (Finance pakai Accurate, Inventory pakai Excel, HR pakai aplikasi terpisah)
- **Inefisiensi:** Rekonsiliasi data antar departemen memakan waktu 3-5 hari kerja per bulan
- **Visibilitas terbatas:** Manajemen tidak memiliki dashboard real-time untuk pengambilan keputusan
- **Biaya tinggi:** Mengelola 3-4 software berbeda dengan lisensi terpisah

ERP Nusantara hadir sebagai solusi all-in-one yang mengintegrasikan seluruh operasional perusahaan dalam satu platform cloud.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target (12 bulan) | Prioritas |
|--------|--------|-------------------|-----------|
| Adopsi pengguna | Pelanggan aktif | 200 perusahaan | P0 |
| Integrasi data | Modul aktif per pelanggan | Rata-rata 4 modul | P0 |
| Efisiensi operasional | Waktu tutup buku | Turun dari 5 hari ke 1 hari | P0 |
| Retensi | Retention rate tahun-1 | >75% | P0 |
| Revenue | ARR (Annual Recurring Revenue) | Rp 12 Miliar | P1 |
| Customer satisfaction | CSAT Score | >4.2/5.0 | P1 |

## 3. Value Proposition

| Masalah | Solusi ERP Nusantara |
|---------|---------------------|
| 3-4 software terpisah | 1 platform all-in-one |
| Rekonsiliasi manual | Data terintegrasi real-time |
| Laporan butuh 3-5 hari | Dashboard 1 klik |
| Biaya Rp 5-10 juta/bulan | Mulai Rp 2 juta/bulan |
| Tidak ada mobile access | Akses dari mana saja, mobile-ready |
| Data tidak aman di Excel | Cloud dengan enkripsi & backup |

## 4. Target Pengguna

### Persona 1: Hendra — CEO Perusahaan Dagang (50 karyawan)
- **Usia:** 45 tahun
- **Perusahaan:** PT Sinar Jaya Abadi — distributor material bangunan
- **Kebutuhan:** Melihat performa perusahaan real-time, margin per produk
- **Pain point:** "Saya baru tahu laba rugi bulan ini 2 minggu setelah bulan tutup"

### Persona 2: Dewi — Finance Manager
- **Usia:** 35 tahun
- **Latar:** S1 Akuntansi, 10 tahun pengalaman
- **Kebutuhan:** AR/AP management, cash flow forecasting, multi-entity
- **Pain point:** "Setiap bulan stress merekonsiliasi data penjualan dari sales dengan stok gudang"

### Persona 3: Rudi — HR Manager
- **Usia:** 38 tahun
- **Kebutuhan:** Payroll terintegrasi dengan absensi, BPJS, PPh 21
- **Pain point:** "Hitung lembur manual dari 50 karyawan makan waktu 2 hari"

## 5. Fitur Prioritas MVP

| ID | Fitur | Prioritas | Modul |
|----|-------|-----------|-------|
| P0-01 | Master data: Produk, Customer, Supplier | P0 | Core |
| P0-02 | Manajemen stok & inventory | P0 | Inventory |
| P0-03 | Sales Order management | P0 | Sales |
| P0-04 | Purchase Order management | P0 | Purchasing |
| P0-05 | Jurnal akuntansi & COA | P0 | Finance |
| P0-06 | Buku Besar & Trial Balance | P0 | Finance |
| P0-07 | Data karyawan & organisasi | P0 | HR |
| P0-08 | Dashboard utama | P0 | Core |
| P1-09 | Absensi & kehadiran | P1 | HR |
| P1-10 | Piutang & Utang (AR/AP) | P1 | Finance |
| P1-11 | Laporan laba rugi & neraca | P1 | Finance |
| P1-12 | Multi-user & role | P1 | Core |
| P1-13 | Approval workflow | P1 | Core |
| P1-14 | Payroll dasar | P1 | HR |
| P2-15 | Manajemen aset tetap | P2 | Finance |
| P2-16 | Cuti & izin online | P2 | HR |
| P2-17 | Integrasi e-Faktur pajak | P2 | Finance |
| P2-18 | Mobile apps | P2 | Core |

## 6. Model Distribusi

- **Direct Sales:** Tim sales enterprise untuk perusahaan menengah
- **Channel Partner:** Kerjasama dengan konsultan ERP dan akuntan publik
- **Self-Service:** Website landing page dengan trial 14 hari
- **Pricing Tier:**
  - **Starter:** 3 modul, 10 user — Rp 2.000.000/bulan
  - **Business:** 6 modul, 50 user — Rp 5.000.000/bulan
  - **Enterprise:** Unlimited, custom — Rp 10.000.000/bulan
- **Onboarding:** Dedicated CS untuk Business & Enterprise tier

## 7. KPI & Metrik

| KPI | Target | Frekuensi |
|-----|--------|-----------|
| New MRR (Monthly Recurring Revenue) | Rp 500 juta/bulan | Bulanan |
| Churn rate | <5%/bulan | Bulanan |
| Modul aktif per customer | >4 modul | Bulanan |
| Time-to-go-live | <30 hari | Per customer |
| NPS | >45 | Kuartalan |
| Average response time support | <4 jam | Harian |
| Uptime | 99.5% | Bulanan |

## 8. Analisis Kompetitor

| Kompetitor | Kekuatan | Kelemahan | Harga/bulan |
|------------|----------|-----------|-------------|
| **Accurate Online** | Brand kuat, fitur akuntansi lengkap | Kurang kuat di Inventory/HR | Rp150.000-500.000 |
| **Jurnal (Mekari)** | Cloud, integrasi HR + Gaji | Inventory terbatas | Rp130.000-450.000 |
| **SAP Business One** | Enterprise grade, fitur lengkap | Mahal, kompleks, butuh konsultan | $100+ per user |
| **ODOO** | Open source, modular | Butuh technical skill, hosting sendiri | Gratis (self-hosted) |
| **Zahir** | Akuntansi kuat, sudah terkenal | UI outdated, desktop-first | Rp350.000-900.000 |
| **Hashmicro** | All-in-one, lokal | Mahal, kontrak panjang | Rp5-20 juta |

**Differentiator:** UX modern seperti SaaS global, harga terjangkau untuk UKM menengah, fokus pada kemudahan setup, support bahasa Indonesia penuh.

## 9. Risiko & Mitigasi

| Risiko | Dampak | Probabilitas | Mitigasi |
|--------|--------|--------------|----------|
| Implementasi kompleks | Tinggi | Tinggi | Onboarding terstruktur, template data |
| Migrasi data dari sistem lama | Tinggi | Sedang | Tool import Excel/CSV, dedicated CS |
| Kustomisasi berlebihan | Tinggi | Sedang | Batasi kustomisasi, modular config |
| Keamanan data | Tinggi | Rendah | ISO 27001, enkripsi, audit rutin |
| Kompetitor perang harga | Sedang | Sedang | Fokus value & service, bukan harga |
| Integrasi eksternal (bank, pajak) | Sedang | Tinggi | API partnership, priority roadmap |

## 10. Rencana Implementasi

| Fase | Durasi | Deliverable |
|------|--------|-------------|
| **Fase 1: Foundation** | 6 minggu | Core master data, inventory, dashboard |
| **Fase 2: Finance** | 5 minggu | COA, jurnal, buku besar, AR/AP |
| **Fase 3: Sales & Purchasing** | 4 minggu | SO, PO, workflow approval |
| **Fase 4: HR Module** | 4 minggu | Karyawan, absensi, payroll dasar |
| **Fase 5: Reporting** | 3 minggu | Financial reports, export, analytics |
| **Alpha** | 2 minggu | 10 internal testers |
| **Beta** | 4 minggu | 30 pilot customers |
| **Public Launch** | — | GA release + marketing campaign |
