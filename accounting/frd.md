# FRD: BukuWarung Akuntansi — Functional Requirements

## 1. Scope

Aplikasi akuntansi double-entry berbasis cloud yang mencakup siklus akuntansi penuh: dari pencatatan jurnal, pemrosesan ke buku besar, hingga penyajian laporan keuangan. Didukung oleh manajemen chart of account, periode fiskal, dan multi-entitas.

**In Scope:**
- Chart of Account management
- Jurnal umum (single dan multi-line)
- Buku Besar otomatis
- Neraca Lajur (Trial Balance)
- Laporan Laba Rugi
- Laporan Neraca
- Laporan Arus Kas (metode tidak langsung)
- Manajemen periode fiskal
- Export PDF dan Excel
- Multi-entitas (untuk akuntan dengan banyak klien)

**Out of Scope:**
- Modul penggajian (HR/payroll)
- Manajemen inventaris
- Faktur penjualan dan pembelian
- Perpajakan langsung (PPh, PPN)

## 2. Modul & Fitur

| ID | Modul | Fitur | Deskripsi | Prioritas |
|----|-------|-------|-----------|-----------|
| COA-01 | Chart of Account | Daftar akun | CRUD akun dengan kode, nama, tipe, saldo normal | P0 |
| COA-02 | Chart of Account | Template default | Template akun standar PSAK untuk UKM | P0 |
| COA-03 | Chart of Account | Import/Export | Upload/download COA dari Excel | P1 |
| COA-04 | Chart of Account | Hierarki | Sub-akun dengan indentasi bertingkat | P1 |
| JRN-01 | Jurnal | Entri baru | Form entri dengan debit/kredit validation | P0 |
| JRN-02 | Jurnal | Daftar jurnal | Tabel dengan filter tanggal, akun, status | P0 |
| JRN-03 | Jurnal | Edit jurnal | Edit jurnal draft dan posted (dengan audit) | P0 |
| JRN-04 | Jurnal | Approve jurnal | Workflow approval untuk jurnal posted | P1 |
| JRN-05 | Jurnal | Reverse jurnal | Membalik jurnal dengan koreksi otomatis | P1 |
| JRN-06 | Jurnal | Upload bukti | Lampirkan foto/PDF bukti transaksi | P1 |
| GL-01 | Buku Besar | Generate otomatis | Buku besar dari jurnal yang sudah diposting | P0 |
| GL-02 | Buku Besar | Filter periode | Lihat per akun, per periode | P0 |
| GL-03 | Buku Besar | Saldo berjalan | Kolom saldo yang kumulatif per baris | P0 |
| RPT-01 | Laporan | Laba Rugi | Laporan periodik pendapatan dan beban | P0 |
| RPT-02 | Laporan | Neraca | Laporan posisi keuangan per tanggal | P0 |
| RPT-03 | Laporan | Neraca Lajur | Daftar saldo semua akun sebelum penyesuaian | P1 |
| RPT-04 | Laporan | Arus Kas | Laporan arus kas metode tidak langsung | P2 |
| RPT-05 | Laporan | Export PDF | Download laporan format PDF profesional | P1 |
| RPT-06 | Laporan | Export Excel | Download laporan format XLSX | P1 |
| SYS-01 | Sistem | Periode fiskal | Buka/tutup periode, set aktif | P0 |
| SYS-02 | Sistem | Multi-entitas | Switch antar entitas/perusahaan | P1 |
| SYS-03 | Sistem | Role management | Admin, akuntan, viewer | P2 |

## 3. Use Case / Alur Pengguna

### Use Case 1: Mencatat Transaksi Penjualan Tunai

**Aktor:** Rina (Pemilik Toko)
**Prekondisi:** Rina sudah login, COA sudah ada, periode Maret 2025 aktif

**Alur Normal:**
1. Rina klik menu "Jurnal" → "Entri Baru"
2. Sistem menampilkan form entri jurnal kosong
3. Rina memilih tanggal transaksi (15 Maret 2025)
4. Rina mengisi nomor bukti (BKM-2025-03-0015)
5. Rina mengetik deskripsi: "Penjualan tunai 15 Maret 2025"
6. Pada baris debit, Rina pilih akun "Kas Besar" → isi nominal Rp15.000.000
7. Pada baris kredit, Rina pilih akun "Pendapatan Jasa" → isi nominal Rp15.000.000
8. Sistem validasi: total debit (15.000.000) = total kredit (15.000.000) ✓
9. Rina klik "Simpan & Posting"
10. Sistem simpan jurnal dengan status "posted"
11. Sistem update saldo Kas Besar (+15.000.000) dan Pendapatan (+15.000.000)
12. Sistem tampilkan konfirmasi: "Jurnal berhasil diposting"
13. Rina diarahkan ke daftar jurnal dengan jurnal baru muncul di baris teratas

**Alur Alternatif (Debit ≠ Kredit):**
- 8a. Total debit (15.000.000) ≠ total kredit (14.000.000)
- 8b. Sistem tampilkan error: "Total debit dan kredit harus sama. Selisih: Rp1.000.000"
- 8c. Tombol Simpan tidak aktif sampai balance

**Alur Alternatif (Draft):**
- 9a. Rina klik "Simpan sebagai Draft"
- 9b. Sistem simpan dengan status "draft" (tidak update saldo)
- 9c. Jurnal muncul di daftar dengan label "Draft"

### Use Case 2: Generate Laporan Laba Rugi Bulanan

**Aktor:** Budi (Manajer Keuangan)
**Prekondisi:** Semua jurnal Maret 2025 sudah diposting

**Alur Normal:**
1. Budi klik menu "Laporan" → "Laba Rugi"
2. Sistem menampilkan halaman laporan dengan periode aktif (Maret 2025)
3. Budi bisa ganti periode via dropdown "Periode"
4. Sistem query semua jurnal di periode Maret 2025
5. Sistem kelompokkan per akun pendapatan dan beban
6. Sistem hitung total pendapatan, total beban, laba/rugi bersih
7. Sistem tampilkan laporan dengan format:
   - Header: "PT Maju Jaya — Laporan Laba Rugi — Maret 2025"
   - Section Pendapatan (dengan daftar dan subtotal)
   - Section Beban (dengan daftar dan subtotal)
   - **Laba/Rugi Bersih** (bold)
8. Budi klik "Cetak PDF" untuk download laporan
9. Sistem generate PDF dengan format profesional
10. File terdownload dengan nama "Laba-Rugi-Maret-2025.pdf"

**Alur Alternatif (Tidak Ada Data):**
- 3a. Periode yang dipilih belum ada jurnal
- 3b. Sistem tampilkan: "Belum ada transaksi di periode ini"
- 3c. Tawarkan link untuk ke halaman entri jurnal

## 4. Aturan Bisnis

1. **Double-Entry Principle:** Setiap transaksi harus memiliki minimal 1 debit dan 1 kredit, total debit = total kredit
2. **Saldo Normal:** Aset & Beban = debit normal; Utang, Modal, Pendapatan = kredit normal
3. **Periode Tertutup:** Jurnal tidak bisa ditambah/diedit di periode yang sudah ditutup
4. **Audit Trail:** Setiap perubahan jurnal tercatat (siapa, kapan, apa yang diubah)
5. **Nomor Urut:** No. bukti di-generate otomatis dengan format {{PREFIX}}-{{TAHUN}}-{{BULAN}}-{{NOMOR}}
6. **Jurnal Draft:** Tidak mempengaruhi saldo dan tidak muncul di laporan
7. **Saldo Akun:** Hanya bisa diubah melalui jurnal (tidak bisa diedit langsung)
8. **Akun Induk:** Tidak bisa dihapus jika memiliki sub-akun aktif
9. **Konsistensi:** Semua nilai menggunakan IDR, format 2 desimal

## 5. Integrasi & Data Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  User    │────▶│  React   │────▶│  Zustand │
│  Input   │     │  UI      │     │  (State) │
└──────────┘     └──────────┘     └────┬─────┘
                                       │
                                       ▼
                                ┌──────────────┐
                                │   React Query │
                                │  (Server State)│
                                └──────┬───────┘
                                       │
                                       ▼
                          ┌─────────────────────┐
                          │   REST API / Backend │
                          │   (Express/NestJS)   │
                          ├─────────────────────┤
                          │  ┌───────────────┐  │
                          │  │  PostgreSQL   │  │
                          │  │  Database     │  │
                          │  └───────────────┘  │
                          └─────────────────────┘
```

**Data Flow Jurnal:**
1. User input → Form validation (Zod) → Zustand (draft state)
2. Submit → React Query mutation → POST /api/journals
3. Backend validasi duplikat + balance → INSERT + UPDATE saldo akun
4. Response → React Query invalidate cache → UI update

**Data Flow Laporan:**
1. Pilih periode → GET /api/reports/income-statement?period=2025-03
2. Backend query transactions → aggregate per akun
3. Response JSON → Render komponen Recharts (bar chart) + tabel
4. Export PDF → jsPDF generate dari data yang ada

## 6. Non-Functional Requirements

| Kategori | Requirement |
|----------|-------------|
| **Performance** | Load daftar jurnal 10.000 baris < 2 detik |
| **Performance** | Generate laporan < 3 detik untuk data 1 periode |
| **Availability** | Uptime 99.5% (kecuali maintenance terjadwal) |
| **Security** | Enkripsi data at-rest (AES-256) dan in-transit (TLS 1.3) |
| **Compliance** | Data disimpan di server Indonesia (UU Perlindungan Data) |
| **Usability** | Waktu onboarding (signup → jurnal pertama) < 5 menit |
| **Mobile** | Responsive untuk layar 320px (mobile) hingga 1920px (desktop) |
| **Concurrency** | Support 50 user concurrent per entity |
| **Backup** | Backup otomatis setiap 6 jam, retensi 30 hari |
| **Audit** | Semua operasi write tercatat di audit log |

## 7. Prioritization Matrix

```
          High Value
              │
    P1-09  ●  │  ● P0-01  ● P0-02
    P1-10  ●  │  ● P0-03  ● P0-04
    P1-08  ●  │  ● P0-05  ● P0-06
              │
  ───────────┼─────────── Low Effort
    P2-15  ●  │
    P2-12  ●  │  ● P1-07  ● P1-11
    P2-13  ●  │  ● P2-14
              │
          Low Value
```

- **Kuadran I (High Value, Low Effort):** P0 fitur — foundation akuntansi
- **Kuadran II (High Value, High Effort):** P1 fitur — pelaporan lanjutan
- **Kuadran III (Low Value, Low Effort):** Quick wins (template, export)
- **Kuadran IV (Low Value, High Effort):** P2 fitur — Post-MVP
