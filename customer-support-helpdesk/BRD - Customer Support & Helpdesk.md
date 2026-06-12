# BRD: Customer Support & Helpdesk — Business Requirements

**Project:** Customer Support & Helpdesk
**Version:** 1.0
**Date:** DD/MM/YYYY

---

## 1. Latar Belakang & Konteks Bisnis

Bisnis modern menerima puluhan hingga ratusan pertanyaan pelanggan setiap hari melalui berbagai saluran — email, chat, telepon, media sosial. Tanpa sistem terpusat, tiket hilang, prioritas tidak jelas, dan respons lambat menyebabkan kepuasan pelanggan menurun.

Customer Support & Helpdesk menyediakan platform terpusat untuk mengelola semua interaksi support. Dengan sistem tiket, prioritas otomatis, workflow status, dan knowledge base internal, tim support dapat merespons lebih cepat, konsisten, dan terukur.

---

## 2. Tujuan Bisnis (Business Objectives)

| ID | Objective | Metrik Keberhasilan |
|----|-----------|-------------------|
| BRD-001 | Meningkatkan kecepatan respons support | Rata-rata first response time < 30 menit |
| BRD-002 | Meningkatkan resolusi tiket | Resolution rate > 85% dalam 24 jam |
| BRD-003 | Kepuasan pelanggan | Customer satisfaction score > 4.5 / 5.0 |
| BRD-004 | Efisiensi tim support | Satu agent handle 20+ tiket per hari |

---

## 3. Nilai (Value Proposition)

Apa nilai utama yang diberikan Customer Support & Helpdesk kepada Tim Support dan Pelanggan?

- **Sentralisasi:** Semua tiket dari berbagai saluran dalam satu dashboard.
- **Prioritas cerdas:** Tiket darurat otomatis mendapat prioritas tertinggi.
- **Kolaborasi tim:** Internal notes, assignment, dan status tracking.
- **Knowledge base:** Solusi cepat untuk pertanyaan umum, mengurangi beban agent.
- **Transparansi:** Pelanggan bisa tracking status tiket.

---

## 4. Target Pengguna

### Primary Persona: Agent Support

- **Demografis:** 22-40 tahun, lulusan D3/S1, staf customer service atau technical support.
- **Kebutuhan utama:** Melihat dan merespons tiket, mengubah status, kolaborasi dengan tim.
- **Pain points:** Kebingungan karena banyaknya saluran komunikasi; tiket tidak terprioritaskan; data pelanggan terpisah-pisah.

### Secondary Persona: Manajer Support

- **Demografis:** 30-50 tahun, kepala tim customer service, butuh laporan dan insight.
- **Kebutuhan utama:** Melihat performa tim, SLA compliance, tren tiket, laporan bulanan.
- **Pain points:** Tidak ada data performa real-time; sulit mengidentifikasi masalah berulang.

---

## 5. Fitur Prioritas (MVP)

| Fitur | Prioritas | Deskripsi |
|-------|-----------|-----------|
| Manajemen Tiket (CRUD) | P0 | Buat, lihat, edit, hapus tiket support dengan detail lengkap |
| Status & Prioritas Workflow | P0 | Ubah status tiket: Open → In Progress → Resolved → Closed |
| Filter & Pencarian Tiket | P0 | Filter berdasarkan status, prioritas, kategori, assignee |
| Dashboard Statistik | P1 | Kartu metrik dengan total tiket, open, resolved, tren harian |
| Message Thread & Balasan | P1 | Thread pesan antara customer dan agent dengan internal notes |
| Knowledge Base Internal | P1 | Artikel solusi yang bisa dirujuk agent saat membalas tiket |
| Assignment Tiket | P1 | Assign tiket ke agent tertentu dengan notifikasi |
| Laporan & Ekspor CSV | P2 | Generate laporan tiket per periode dan ekspor data |

---

## 6. Model Distribusi

- Static boilerplate frontend — clone dan jalankan dengan Vite
- Mock data dengan skenario realistis support ticket
- Tidak ada backend — simulasi data dan delay buatan
- Siap diintegrasikan dengan REST API backend dan webhook email

---

## 7. KPI & Metrik Kesuksesan

- **Adoption:** Jumlah agent aktif menggunakan sistem setiap hari (target: 100% tim support)
- **First Response Time:** Waktu rata-rata balasan pertama (target: < 30 menit)
- **Resolution Time:** Waktu rata-rata penyelesaian tiket (target: < 4 jam)
- **CSAT:** Customer satisfaction score dari survey setelah tiket resolved (target: > 85%)
- **Backlog:** Jumlah tiket open > 48 jam (target: < 5% dari total)

---

## 8. Analisis Kompetitor

| Kompetitor | Kelebihan | Kekurangan | Helpdesk Advantage |
|------------|-----------|------------|-------------------|
| Zendesk | Fitur lengkap, enterprise-grade | Sangat mahal, kompleks untuk UKM | Gratis, lightweight, cocok untuk UKM |
| Freshdesk | Mudah digunakan, harga terjangkau | Fitur terbatas di paket murah | Full fitur tanpa biaya berlangganan |
| Trello (disalahgunakan) | Gratis, familiar | Bukan untuk support ticket, tidak ada fitur SLA | Dibangun khusus untuk workflow support |

---

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Agent tidak konsisten update status tiket | Sedang — data tidak akurat | Notifikasi reminder, validasi workflow |
| Prioritas tidak sesuai ekspektasi pelanggan | Sedang — komplain | Berikan SLA guideline ke pelanggan di awal |
| Duplikasi tiket dari pelanggan yang sama | Rendah — kebingungan | Deteksi duplikasi otomatis oleh sistem |

---

## 10. Rencana Implementasi

1. Fase 1 — Prototipe statis dengan mock data (2 minggu): UI tiket, filter, dashboard
2. Fase 2 — Validasi dengan tim support (1 minggu): UAT dengan agent support, refine UX
3. Fase 3 — Integrasi backend / API (4 minggu): REST API tiket, autentikasi, notifikasi email
4. Fase 4 — Deployment & scaling (2 minggu): Hosting, training agent, migrasi data
