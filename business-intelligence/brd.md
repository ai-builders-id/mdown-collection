# BRD: Business Intelligence — Business Requirements

**Project:** Business Intelligence
**Version:** 1.0
**Date:** DD/MM/YYYY

---

## 1. Latar Belakang & Konteks Bisnis

Perusahaan modern mengumpulkan data dari berbagai sumber — penjualan, marketing, keuangan, operasional. Namun data mentah tidak berguna tanpa visualisasi dan analisis yang tepat. Tim manajemen sering kesulitan mendapatkan insight cepat karena data tersebar di spreadsheet statis atau laporan cetak yang sudah basi.

Business Intelligence Dashboard hadir untuk mengkonsolidasikan data ke dalam satu platform visual yang interaktif. Dengan drag-and-drop widget, filter dinamis, dan berbagai tipe chart, pengguna bisa mengeksplorasi data secara real-time dan membuat laporan kustom tanpa bantuan tim IT.

---

## 2. Tujuan Bisnis (Business Objectives)

| ID | Objective | Metrik Keberhasilan |
|----|-----------|-------------------|
| BRD-001 | Menyediakan visibilitas real-time kinerja bisnis | Dashboard dengan data < 5 detik dari sumber |
| BRD-002 | Memberdayakan analisis self-service | 80% laporan dibuat oleh user non-teknis tanpa IT |
| BRD-003 | Mengurangi waktu pembuatan laporan | Waktu buat laporan dari 3 hari menjadi 30 menit |
| BRD-004 | Meningkatkan data-driven decision making | 90% rapat manajemen menggunakan data dashboard |

---

## 3. Nilai (Value Proposition)

Apa nilai utama yang diberikan Business Intelligence Dashboard kepada Manajemen dan Analis Bisnis?

- **Visualisasi interaktif:** Grafik dan chart yang bisa di-drag, di-filter, dan di-eksplorasi.
- **Self-service analytics:** User bisnis bisa membuat dashboard sendiri tanpa coding.
- **Real-time insight:** Data terkini dalam hitungan detik, bukan laporan kemarin.
- **Kustomisasi tinggi:** Widget bisa diatur posisi, ukuran, dan sumber data.
- **Satu sumber kebenaran:** Semua metrik dari satu platform terpadu.

---

## 4. Target Pengguna

### Primary Persona: Manajer / Direktur

- **Demografis:** 35-55 tahun, S1/S2, pimpinan departemen atau C-level.
- **Kebutuhan utama:** Melihat KPI bisnis secara cepat, tren, dan area yang perlu perhatian.
- **Pain points:** Laporan datang terlambat; data di spreadsheet tidak konsisten; tidak bisa drill-down.

### Secondary Persona: Analis Bisnis

- **Demografis:** 25-38 tahun, lulusan statistik/manajemen, staf perencanaan atau analis data.
- **Kebutuhan utama:** Membuat laporan kustom, eksplorasi data, ekspor untuk presentasi.
- **Pain points:** Harus request data ke IT; tools BI mahal dan kompleks; sulit kolaborasi laporan.

---

## 5. Fitur Prioritas (MVP)

| Fitur | Prioritas | Deskripsi |
|-------|-----------|-----------|
| Dashboard Grid dengan Widget | P0 | Layout grid responsif dengan widget chart dan metrik yang bisa diatur |
| KPI Metric Cards | P0 | Kartu metrik dengan nilai, tren, ikon, sparkline |
| 3+ Tipe Chart Interaktif | P0 | Bar chart, line chart, pie chart — interaktif dengan tooltip |
| Filter Rentang Waktu | P0 | Date range picker yang memengaruhi semua widget di dashboard |
| Manajemen Laporan | P1 | Buat, edit, simpan laporan kustom dengan pilihan widget |
| Data Table Interaktif | P1 | Tabel data dengan sorting, filtering, pagination, export CSV |
| Dark/Light Mode | P1 | Toggle tema gelap dan terang untuk dashboard |
| Drag & Resize Widget | P2 | Atur posisi dan ukuran widget dengan drag-and-drop |

---

## 6. Model Distribusi

- Static boilerplate frontend — siap clone dan run
- Mock data generator dengan skenario bisnis realistis (sales, marketing, finance)
- Tidak ada backend — semua data hardcoded dalam file TypeScript
- React Grid Layout untuk drag-and-drop widget
- Siap diintegrasikan dengan API data warehouse / BigQuery / database

---

## 7. KPI & Metrik Kesuksesan

- **Adoption:** Jumlah dashboard aktif per departemen (target: 3 dashboard/departemen)
- **Engagement:** Rata-rata sesi per pengguna per hari (target: 4+ sesi)
- **Self-service:** Persentase laporan dibuat tanpa IT (target: 80%)
- **Report creation time:** Waktu rata-rata membuat laporan baru (target: < 15 menit)
- **NPS:** Net Promoter Score dari user dashboard (target: > 50)

---

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | BI Dashboard Advantage |
|------------|-----------|------------|-------------------|
| Tableau | Sangat kuat visualisasi, banyak konektor | Mahal ($70/user/bulan), learning curve tinggi | Gratis, sederhana, fokus ke kebutuhan umum |
| Google Data Studio | Gratis, integrasi Google | Terbatas di ekosistem Google | Open stack, bisa di-deploy sendiri |
| Metabase | Open source, self-hosted | Visualisasi kurang modern | UI modern dengan React, kustomisasi lebih tinggi |
| Power BI | Integrasi Microsoft, DAX | Mahal untuk enterprise, Windows-centric | Cross-platform, web-based, gratis |

---

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Data tidak akurat menyesatkan keputusan | Tinggi — keputusan bisnis salah | Validasi data, tampilkan source metadata |
| User kewalahan dengan terlalu banyak metrik | Sedang — dashboard tidak efektif | Default view dengan KPI essentials, kustomisasi bertahap |
| Kinerja lambat dengan banyak data | Tinggi — pengalaman buruk | Virtual scrolling, data aggregation, caching |

---

## 10. Rencana Implementasi

1. Fase 1 — Prototipe statis dengan mock data (3 minggu): Grid dashboard, chart components, filter
2. Fase 2 — Validasi dengan manajer (1 minggu): User testing, penyempurnaan UX dashboard
3. Fase 3 — Integrasi sumber data (4 minggu): Konektor API, query engine, data caching
4. Fase 4 — Deployment & scaling (2 minggu): Hosting, user training, onboarding departemen
