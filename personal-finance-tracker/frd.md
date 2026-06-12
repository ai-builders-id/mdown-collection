# FRD: CatatanDuit — Functional Requirements

## 1. Scope

Aplikasi pencatatan keuangan pribadi mobile-first yang memungkinkan pengguna mencatat pemasukan dan pengeluaran, mengelola beberapa akun, melihat laporan visual, dan mengatur anggaran bulanan.

**In Scope:**
- Manajemen transaksi (CRUD) untuk income, expense, transfer
- Multi-akun (bank, e-wallet, cash)
- Kategori dengan ikon dan warna
- Dashboard saldo & ringkasan
- Grafik pengeluaran pie & bar chart
- Laporan bulanan
- Anggaran bulanan per kategori
- Transaksi berulang
- Pencarian & filter
- Dark mode
- PWA (offline-capable)

**Out of Scope:**
- Sinkronisasi cloud (MVP — post-launch)
- Multi-pengguna / kolaborasi
- Pembayaran tagihan otomatis
- Investasi tracker
- AI-powered insights (future)
- Koneksi API bank

## 2. Modul & Fitur

| ID | Modul | Fitur | Deskripsi | Prioritas |
|----|-------|-------|-----------|-----------|
| TRX-01 | Transaksi | Entry cepat | Input nominal, kategori, akun dalam 3 tap | P0 |
| TRX-02 | Transaksi | Riwayat | Daftar transaksi kronologis | P0 |
| TRX-03 | Transaksi | Edit/Hapus | Ubah atau hapus transaksi | P0 |
| TRX-04 | Transaksi | Filter | Filter by tanggal, kategori, akun, tipe | P0 |
| TRX-05 | Transaksi | Pencarian | Cari berdasarkan deskripsi | P1 |
| TRX-06 | Transaksi | Transaksi berulang | Auto-create transaksi periodik | P1 |
| ACC-01 | Akun | CRUD akun | Tambah/ubah/hapus akun keuangan | P0 |
| ACC-02 | Akun | Saldo | Update saldo otomatis saat transaksi | P0 |
| ACC-03 | Akun | Urutan | Drag-and-drop urutan akun | P1 |
| CAT-01 | Kategori | CRUD kategori | Atur kategori income & expense | P0 |
| CAT-02 | Kategori | Template default | 15 kategori bawaan | P0 |
| CAT-03 | Kategori | Subkategori | Grup kategori bertingkat | P1 |
| DSH-01 | Dashboard | Ringkasan saldo | Total saldo semua akun | P0 |
| DSH-02 | Dashboard | Grafik pengeluaran | Pie chart kategori | P0 |
| DSH-03 | Dashboard | Transaksi terbaru | 5 transaksi terakhir | P0 |
| RPT-01 | Laporan | Bulanan | Income vs expense bulan ini | P0 |
| RPT-02 | Laporan | Tren | Bar chart 6 bulan | P1 |
| RPT-03 | Laporan | Export CSV | Download data transaksi | P1 |
| BGT-01 | Anggaran | Budget per kategori | Target bulanan | P1 |
| BGT-02 | Anggaran | Progress bar | Visual sisa budget | P1 |
| BGT-03 | Anggaran | Notifikasi | Alert saat >80% terpakai | P1 |
| SYS-01 | Sistem | Dark mode | Toggle tema gelap | P1 |
| SYS-02 | Sistem | PWA | Install prompt, offline cache | P1 |
| SYS-03 | Sistem | Pin/Passcode | Keamanan aplikasi | P1 |

## 3. Use Case / Alur Pengguna

### Use Case 1: Mencatat Pengeluaran Cepat

**Aktor:** Raka (Karyawan Swasta)
**Prekondisi:** Aplikasi terinstall, akun BCA dan kategori sudah ada

**Alur Normal:**
1. Raka selesai makan siang, buka CatatanDuit
2. Tap FAB "+" → muncul 3 pilihan: Pemasukan / Pengeluaran / Transfer
3. Raka tap "Pengeluaran"
4. Nominal: Raka tap keypad numerik, ketik "45000"
5. Kategori: Tap "Makanan & Minuman" (paling sering, ada di urutan atas)
6. Akun: Tap "BCA Syariah" (default akun dari transaksi sebelumnya)
7. (Opsional) Tambah catatan: "Makan siang di Warteg Budi"
8. Tap "Simpan"
9. System simpan transaksi + update saldo BCA (-45.000)
10. Toast: "Transaksi berhasil disimpan" (auto-dismiss 2 detik)
11. Dashboard update: saldo turun, chart pengeluaran update

**Alur Alternatif (Input Nominal Cepat via Kalkulator):**
- 4a. Raka ingin input "45.000" → cukup tap "4-5-0-0-0" atau "4-5-.-"
- 4b. Keyboard numerik dengan separator otomatis

### Use Case 2: Review Anggaran Bulanan

**Aktor:** Maya (Freelance Designer)
**Prekondisi:** Transaksi bulan Maret sudah tercatat, budget sudah di-set

**Alur Normal:**
1. Maya tap tab "Anggaran" di bottom navigation
2. Sistem menampilkan daftar budget per kategori:
   - Makanan: Rp900.000 / Rp1.200.000 (75%) 🟢
   - Transport: Rp650.000 / Rp800.000 (81%) 🟡
   - Belanja: Rp1.200.000 / Rp1.000.000 (120%) 🔴
   - Tagihan: Rp1.350.000 / Rp1.500.000 (90%) 🟠
3. Maya tap kategori "Belanja" untuk detail
4. Sistem tampilkan daftar transaksi belanja bulan ini
5. Maya sadar sudah overspent, putuskan kurangi belanja sisa bulan
6. Maya tap ikon filter → "Lihat transaksi lain" untuk cek pengeluaran

**Alur Alternatif (Notifikasi Over Budget):**
- 2a. Saat Maya menambah transaksi belanja
- 2b. Setelah simpan, sistem deteksi budget Belanja > 100%
- 2c. Toast: "Anggaran Belanja sudah terlampaui! (Rp1.200.000 / Rp1.000.000)"

## 4. Aturan Bisnis

1. **Saldo Otomatis:** Setiap transaksi expense = saldo akun berkurang, income = saldo bertambah
2. **Transfer:** Transfer antar akun = saldo sumber berkurang, saldo tujuan bertambah (kategori tidak diperlukan)
3. **Balance Check:** Saldo akun tidak boleh negatif (dengan opsi override)
4. **Kategori:** Wajib untuk income/expense, tidak untuk transfer
5. **Default Akun:** Akun yang terakhir dipakai jadi default untuk transaksi berikutnya
6. **Tanggal:** Default ke hari ini, bisa ubah ke tanggal sebelumnya (tidak boleh > hari ini)
7. **Transaksi Berulang:** Generate otomatis di hari yang sama setiap bulan/minggu
8. **Budget Reset:** Budget di-reset setiap awal bulan (kecuali rollover enabled)
9. **Hapus Akun:** Tidak bisa hapus akun yang masih punya transaksi
10. **Data Lokal:** Semua data disimpan di IndexedDB lokal (cloud sync adalah premium feature)

## 5. Integrasi & Data Flow

```
┌──────────────────────────────────────────────────────────────┐
│                    CatatanDuit App                            │
│                                                              │
│  ┌─────────┐   ┌──────────┐   ┌──────────────────┐         │
│  │ UI Layer│──▶│ Zustand  │──▶│ TanStack Query   │         │
│  │ (React) │   │ (UI State)│  │ (Optional)       │         │
│  └─────────┘   └──────────┘   └────────┬─────────┘         │
│                                        │                    │
│  ┌─────────────────────────────────────▼──────────────────┐ │
│  │              Data Layer                                 │ │
│  │  ┌──────────────┐  ┌──────────────┐                   │ │
│  │  │ Dexie.js     │  │ Service      │                   │ │
│  │  │ (IndexedDB)  │  │ Worker Cache │                   │ │
│  │  └──────────────┘  └──────────────┘                   │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                              │
│  ┌──────────────────────────────────────────────────────────┐│
│  │  Cloud Sync (Premium) — future                           ││
│  │  ┌─────────────────┐  ┌─────────────────┐               ││
│  │  │ REST API        │  │ PostgreSQL      │               ││
│  │  └─────────────────┘  └─────────────────┘               ││
│  └──────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────┘
```

**Data Flow Transaksi:**
1. User input → Validasi (Zod) → Zustand (UI feedback)
2. Simpan → Dexie.js (IndexedDB) → Update saldo akun
3. Re-render → Update dashboard, chart, daftar transaksi
4. (Jika online + premium) → Sync ke cloud via TanStack Query mutation

**Offline-First:**
- Semua operasi baca/tulis ke IndexedDB langsung
- TanStack Query hanya untuk cloud sync (premium)
- Service Worker cache app shell untuk offline loading

## 6. Non-Functional Requirements

| Kategori | Requirement |
|----------|-------------|
| **Performance** | App launch < 2 detik (cold start) |
| **Performance** | Transaksi entry response < 200ms |
| **Performance** | Daftar transaksi 10.000 item render < 1 detik |
| **Offline** | Full functionality tanpa internet |
| **Storage** | < 50MB data 10.000 transaksi |
| **Battery** | Minimal background processing |
| **Security** | IndexedDB encryption untuk data sensitif |
| **PWA** | Installable, works offline, push notifications |
| **Accessibility** | Support screen reader, contrast ratio WCAG AA |
| **Mobile** | 320px - 428px width optimized (mobile-first) |

## 7. Prioritization Matrix

```
          High Value
              │
    P0-01 ●   │  ● P0-02  ● P0-03
    P0-04 ●   │  ● P0-05  ● P0-06
    P0-07 ●   │
              │
  ───────────┼─────────── Low Effort
    P1-08 ●  │
    P1-09 ●  │  ● P1-10  ● P1-11
    P1-12 ●  │  ● P1-13
              │
          Low Value
```

**Strategy:** Core transaction recording first (P0) → make it delightful with charts (P0) → add engagement features (P1) → premium monetization (P1/P2).
