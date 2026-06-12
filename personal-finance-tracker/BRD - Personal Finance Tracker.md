# BRD: CatatanDuit — Business Requirements

## 1. Latar Belakang & Konteks Bisnis

Kesadaran finansial masyarakat Indonesia meningkat signifikan, terutama pasca-pandemi. Namun, mayoritas masyarakat masih menggunakan cara manual untuk mencatat keuangan pribadi:

- **60%** masyarakat Indonesia tidak memiliki catatan keuangan terstruktur
- **Hanya 15%** yang menggunakan aplikasi pencatatan keuangan
- **70%** pengguna spreadsheet keuangan berhenti dalam 3 bulan karena tidak konsisten
- **Generasi Milenial & Gen Z** (usia 18-40) sangat membutuhkan tools keuangan yang mobile-friendly

Permasalahan utama: aplikasi existing terlalu kompleks (seperti BukuKas untuk bisnis), terlalu general (excel), atau terlalu ribet (YNAB butuh learning curve tinggi).

CatatanDuit hadir sebagai solusi pencatatan keuangan pribadi yang super simpel, mobile-first, dan menyenangkan untuk digunakan.

## 2. Tujuan Bisnis

| Tujuan | Metrik | Target (12 bulan) | Prioritas |
|--------|--------|-------------------|-----------|
| Pengguna aktif | MAU | 50.000 | P0 |
| Retensi | Retention D+30 | >40% | P0 |
| Engagement | Transaksi/user/bulan | >30 transaksi | P0 |
| Monetisasi | Premium conversion | 5% | P1 |
| Ulasan | Play Store rating | 4.5+ | P1 |
| Virality | Referral rate | 15% pengguna baru via referral | P1 |

## 3. Value Proposition

| Masalah | Solusi CatatanDuit |
|---------|-------------------|
| Males catat keuangan | UI fun, gamification, 5 detik entry |
| Aplikasi terlalu ribet | Minimalist design, 3 tap untuk catat transaksi |
| Tidak konsisten | Reminder harian, streak tracking |
| Gak ngerti laporan | Visual charts yang mudah dipahami |
| Privasi | Data di device (offline-first), cloud opsional |
| Keuangan pribadi vs bisnis | Fokus 100% personal finance |

## 4. Target Pengguna

### Persona 1: Raka — Karyawan Swasta (26 tahun)
- **Status:** Lajang, tinggal di kost Jakarta
- **Pendapatan:** Rp 7.000.000/bulan
- **Kebutuhan:** Mengontrol pengeluaran bulanan, tahu kemana uang habis
- **Pain:** "Gue tiap akhir bulan bingung duit habis buat apa aja"
- **Device:** Xiaomi Redmi Note 12 (Android)
- **Tech skill:** Medium — aktif pakai GoPay, Shopee, Instagram

### Persona 2: Maya — Freelance Designer (31 tahun)
- **Status:** Menikah, 1 anak
- **Pendapatan:** Rp 10-15 juta/bulan (tidak tetap)
- **Kebutuhan:** Mencatat income freelance yang tidak tetap, budgeting
- **Pain:** "Susah bedain duit bisnis sama pribadi, penghasilan naik turun"
- **Device:** iPhone 13
- **Tech skill:** Tinggi — pakai Notion, Google Suite

### Persona 3: Pak RT — Pensunan (60 tahun)
- **Status:** Pensiun, 3 anak sudah mandiri
- **Pendapatan:** Rp 5.000.000/bulan (dana pensiun + kontrakan)
- **Kebutuhan:** Catat pengeluaran harian, pastikan pensiun cukup
- **Pain:** "Kertas catatan sering ilang, kalo pake hape takut salah"
- **Device:** Samsung A14 (budget Android)
- **Tech skill:** Rendah — bisa WA, YouTube doang

## 5. Fitur Prioritas MVP

| ID | Fitur | Prioritas | Kompleksitas |
|----|-------|-----------|--------------|
| P0-01 | Tambah transaksi cepat (income/expense/transfer) | P0 | Medium |
| P0-02 | Multi-akun (tunai, bank, e-wallet) | P0 | Low |
| P0-03 | Kategori transaksi + ikon | P0 | Low |
| P0-04 | Daftar transaksi + filter tanggal | P0 | Medium |
| P0-05 | Dashboard ringkasan saldo | P0 | Medium |
| P0-06 | Grafik pengeluaran per kategori (pie chart) | P0 | Medium |
| P0-07 | Laporan pengeluaran bulanan | P0 | High |
| P1-08 | Anggaran bulanan per kategori | P1 | Medium |
| P1-09 | Transaksi berulang | P1 | Medium |
| P1-10 | Pencarian transaksi | P1 | Low |
| P1-11 | Dark mode | P1 | Low |
| P1-12 | PWA / Installable | P1 | Medium |
| P1-13 | Export data ke CSV | P1 | Low |
| P2-14 | Tujuan finansial / goals | P2 | High |
| P2-15 | Sinkronisasi cloud | P2 | High |
| P2-16 | Multi-mata uang | P2 | Medium |
| P2-17 | Pengingat transaksi rutin | P2 | Low |
| P2-18 | Sharing & kolaborasi (pasangan) | P2 | High |

## 6. Model Distribusi

- **Primary:** Google Play Store (gratis)
- **Secondary:** PWA (install dari browser)
- **iOS:** Apple App Store (TBD — prioritaskan Android dulu karena >80% market share Indonesia)
- **Model:** Freemium
  - **Free:** Unlimited transaksi, 5 akun, standar kategori, laporan dasar
  - **Premium (Rp25.000/bulan atau Rp250.000/tahun):** Unlimited akun, budget, goals, export, dark mode, cloud sync, no ads
- **Monetisasi non-premium:** Iklan non-intrusif di laporan halaman (opsional untuk free user)

## 7. KPI & Metrik

| KPI | Definisi | Target |
|-----|----------|--------|
| MAU | Login dalam 30 hari | 50.000 |
| DAU | Login dalam 24 jam | 10.000 |
| Stickiness | DAU/MAU | >20% |
| Avg. Sessions | Session per user per hari | >2 |
| Transaksi/user/bulan | Total transaksi / user aktif | >30 |
| Retention D+7 | % user masih aktif hari ke-7 | >60% |
| Retention D+30 | % user masih aktif hari ke-30 | >40% |
| Conversion rate | Free → Premium | >5% |
| NPS | Survey dalam app | >40 |
| Crash-free rate | % session tanpa crash | >99.5% |

## 8. Analisis Kompetitor

| Kompetitor | Kekuatan | Kelemahan |
|------------|----------|-----------|
| **Money Lover** | UI bagus, multi-platform | Banyak iklan, fitur premium mahal |
| **BukuKas** | Brand kuat Indonesia | Fokus bisnis, bukan pribadi |
| **Spendee** | UI premium, kategori bagus | Premium mahal ($30/tahun), kurang lokal |
| **Catatan Keuangan (play store)** | Banyak pilihan | UI jelek, banyak malware |
| **Excel / Google Sheets** | Gratis, fleksibel | Tidak mobile-friendly, manual |
| **YNAB (You Need A Budget)** | Metode budgeting bagus | Terlalu kompleks, $99/tahun |

**Differentiator:** CatatanDuit = super simpel (3 tap entry), UI Indonesia banget, offline-first, harga premium terjangkau (Rp25.000), fokus ke pengguna Indonesia.

## 9. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| Adopsi rendah | Tinggi | UI simpel, onboarding cepat, referral program |
| Churn tinggi | Tinggi | Gamification (streak, badges), reminder |
| Data hilang (hp hilang/ganti) | Tinggi | Opsi cloud backup (premium), export rutin |
| Monetisasi sulit | Sedang | Premium murah, value jelas, no ads untuk free |
| Kompetitor copy fitur | Sedang | Bangun brand + komunitas, UX lebih baik |
| Keamanan data finansial | Tinggi | Enkripsi lokal, tidak ada data di server (free tier) |

## 10. Rencana Implementasi

| Fase | Durasi | Milestone |
|------|--------|-----------|
| **Fase 1: Core** | 3 minggu | Transaksi CRUD, akun, kategori, dashboard |
| **Fase 2: Visual** | 2 minggu | Grafik, laporan, filter |
| **Fase 3: Engagement** | 2 minggu | Anggaran, transaksi berulang, reminder |
| **Fase 4: Polish** | 2 minggu | Dark mode, PWA, export CSV, animasi |
| **Beta (Play Store Internal)** | 2 minggu | 100 beta testers |
| **Public Launch** | — | Play Store + marketing |
| **Post-Launch** | Ongoing | Fitur premium, cloud sync, goals |
