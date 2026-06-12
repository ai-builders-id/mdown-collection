# BRD: CRM — Business Requirements

**Project:** CRM
**Version:** 1.0
**Date:** DD/MM/YYYY

---

## 1. Latar Belakang & Konteks Bisnis

Tim penjualan di banyak perusahaan masih menggunakan spreadsheet, email, dan catatan pribadi untuk mengelola hubungan dengan pelanggan. Data kontak tercecer, follow-up terlewat, pipeline penjualan tidak terlihat, dan prediksi pendapatan hanya berdasarkan feeling.

CRM (Customer Relationship Management) hadir sebagai platform terpusat untuk mengelola seluruh siklus hubungan pelanggan — dari prospek awal hingga pasca-penjualan. Dengan kontak terintegrasi, pipeline visual, dan aktivitas yang tercatat, tim penjualan dapat bekerja lebih terorganisir, transparan, dan produktif.

---

## 2. Tujuan Bisnis (Business Objectives)

| ID | Objective | Metrik Keberhasilan |
|----|-----------|-------------------|
| BRD-001 | Meningkatkan visibilitas pipeline penjualan | 100% deal tercatat dengan stage dan nilai yang jelas |
| BRD-002 | Meningkatkan konversi prospek ke pelanggan | Conversion rate meningkat 25% dalam 6 bulan |
| BRD-003 | Mengurangi waktu follow-up | Rata-rata waktu follow-up < 24 jam sejak aktivitas terakhir |
| BRD-004 | Meningkatkan akurasi forecast pendapatan | Forecast error < 15% dari realisasi |

---

## 3. Nilai (Value Proposition)

Apa nilai utama yang diberikan CRM kepada Tim Sales dan Manajemen?

- **Pipeline visual:** Semua deal terlihat dalam kanban board — siapa, berapa nilai, di stage mana.
- **Data kontak terpusat:** Tidak ada lagi kontak hilang di email atau spreadsheet.
- **Activity tracking:** Setiap interaksi dengan prospek tercatat — call, email, meeting, demo.
- **Kolaborasi tim:** Semua orang tahu siapa meng-handle kontak/deal apa.
- **Forecast akurat:** Data real-time untuk prediksi pendapatan.

---

## 4. Target Pengguna

### Primary Persona: Sales Representative

- **Demografis:** 24-40 tahun, lulusan D3/S1, staf penjualan atau account executive.
- **Kebutuhan utama:** Mencatat kontak, mengelola pipeline, follow-up aktivitas, melihat target.
- **Pain points:** Data kontak di Excel tidak terpusat; lupa follow-up karena tidak ada reminder.

### Secondary Persona: Sales Manager

- **Demografis:** 35-55 tahun, kepala tim penjualan, butuh laporan performa tim.
- **Kebutuhan utama:** Melihat pipeline tim, forecast revenue, evaluasi performa sales.
- **Pain points:** Forecast hanya tebak-tebakan; tidak tahu aktivitas harian tim.

---

## 5. Fitur Prioritas (MVP)

| Fitur | Prioritas | Deskripsi |
|-------|-----------|-----------|
| Manajemen Kontak | P0 | CRUD kontak dengan detail lengkap (info, tags, sumber lead) |
| Manajemen Perusahaan | P0 | CRUD perusahaan dengan relasi kontak dan deal |
| Pipeline Kanban | P0 | Visual pipeline dengan drag-and-drop antar stage penjualan |
| Manajemen Deal | P0 | CRUD deal dengan nilai, probabilitas, expected close date |
| Catatan Aktivitas | P0 | Timeline aktivitas per kontak/deal (call, email, meeting, demo, note) |
| Dashboard Sales | P1 | Ringkasan pipeline, total value, win rate, aktivitas terbaru |
| Filter & Search | P1 | Filter kontak/deal berdasarkan status, stage, tags, owner |
| Laporan Sederhana | P2 | Laporan pipeline value, aktivitas per sales, konversi stage |

---

## 6. Model Distribusi

- Static boilerplate frontend — clone dan jalankan
- Mock data dengan skenario penjualan B2B realistis
- Pipeline dengan kanban board yang siap digunakan
- Tidak ada backend — data tersimpan di memori (state React)
- Siap diintegrasikan dengan backend API CRM

---

## 7. KPI & Metrik Kesuksesan

- **Adoption:** Jumlah deal yang tercatat per sales per bulan (target: 20+)
- **Pipeline Coverage:** Total nilai pipeline dibanding target (target: 3x target)
- **Win Rate:** Persentase deal won dari total deal (target: > 30%)
- **Activity Compliance:** Rata-rata aktivitas tercatat per deal per minggu (target: 3+)
- **Sales Cycle:** Rata-rata waktu dari lead ke closed-won (target: < 60 hari)

---

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | CRM Advantage |
|------------|-----------|------------|---------------|
| Salesforce | Paling komplit, ekosistem luas | Sangat mahal, implementasi kompleks | Gratis, sederhana, cocok untuk UKM |
| HubSpot CRM | Gratis untuk fitur dasar, mudah digunakan | Fitur lanjutan mahal, data di cloud pihak ketiga | Full fitur gratis, self-hosted |
| Pipedrive | Pipeline visual yang excellent | Terbatas di custom field, mahal per user | UI modern, kanban premium, open source |
| Zoho CRM | Fitur lengkap, harga terjangkau | UI kurang modern, support lambat | Desain modern dengan React, performa tinggi |

---

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Sales malas mencatat aktivitas | Tinggi — data tidak berguna | Buat input semudah mungkin, integrasikan dengan workflow harian |
| Data duplikasi kontak/perusahaan | Sedang — data kotor | Deteksi duplikasi otomatis saat input |
| Pipeline tidak akurat karena sales over-optimistic | Sedang — forecast salah | Gunakan probabilitas berbasis stage, historis win rate |

---

## 10. Rencana Implementasi

1. Fase 1 — Prototipe statis dengan mock data (2 minggu): Kontak, pipeline, deal, aktivitas UI
2. Fase 2 — Validasi dengan tim sales (2 minggu): UAT dengan 5 sales rep, iterasi UX
3. Fase 3 — Integrasi backend / API (4 minggu): REST API, autentikasi, role management
4. Fase 4 — Deployment & scaling (2 minggu): Hosting, import data, training tim sales
