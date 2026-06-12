# FRD: PengadaanCerdas — Functional Requirements

## 1. Scope

Aplikasi procurement management yang mencakup siklus pengadaan dari pembuatan Purchase Order, approval workflow multi-level, manajemen supplier, pelacakan penerimaan barang, hingga pelaporan pengeluaran.

**In Scope:**
- Purchase Order Management (CRUD, status tracking)
- Supplier Management (CRUD, rating, histori)
- Approval Workflow (multi-level, configurable)
- Contract Management
- Dashboard & Analytics
- Receiving (penerimaan barang)
- Reporting & Export

**Out of Scope:**
- Manajemen inventaris / stok
- Modul akuntansi (posting jurnal)
- e-Procurement / lelang publik
- SCM (Supply Chain Management) full
- Manajemen proyek

## 2. Modul & Fitur

| ID | Modul | Fitur | Deskripsi | Prioritas |
|----|-------|-------|-----------|-----------|
| PO-01 | Purchase Order | Buat PO Baru | Form multi-step dengan info umum + items + review | P0 |
| PO-02 | Purchase Order | Daftar PO | Tabel dengan filter status, supplier, departemen, tanggal | P0 |
| PO-03 | Purchase Order | Detail PO | View lengkap + timeline approval + item list | P0 |
| PO-04 | Purchase Order | Edit PO | Edit PO yang masih draft/pending | P0 |
| PO-05 | Purchase Order | Submit PO | Submit untuk masuk approval workflow | P0 |
| PO-06 | Purchase Order | Cetak PO | Export ke PDF format profesional | P1 |
| PO-07 | Purchase Order | Batalkan PO | Cancel dengan alasan | P1 |
| PO-08 | Purchase Order | Revisi PO | Amend PO yang sudah approved (version history) | P2 |
| PO-09 | Purchase Order | Partial Receive | Terima barang sebagian | P1 |
| SUP-01 | Supplier | Daftar Supplier | Tabel dengan filter status, kategori, rating | P0 |
| SUP-02 | Supplier | Tambah Supplier | Form lengkap (identitas, kontak, bank, NPWP) | P0 |
| SUP-03 | Supplier | Detail Supplier | Info lengkap + histori PO + rating | P0 |
| SUP-04 | Supplier | Rating & Review | Rating otomatis dari performa pengiriman | P1 |
| APR-01 | Approval | Approval Queue | Daftar PO menunggu approval per user | P0 |
| APR-02 | Approval | Setujui / Tolak | Aksi approval dengan catatan | P0 |
| APR-03 | Approval | Multi-level | Konfigurasi level approval per nilai/departemen | P2 |
| APR-04 | Approval | Delegasi | Approver bisa delegasi ke user lain | P2 |
| RCV-01 | Receiving | Terima Barang | Input jumlah diterima per item | P1 |
| RCV-02 | Receiving | Riwayat Penerimaan | Log semua penerimaan per PO | P1 |
| CTR-01 | Contract | Daftar Kontrak | Kontrak tahunan/proyek dengan remaining value | P2 |
| CTR-02 | Contract | Buat Kontrak | Form kontrak dengan terms & attachment | P2 |
| RPT-01 | Laporan | Dashboard KPI | Total PO, pending, spending, budget utilization | P0 |
| RPT-02 | Laporan | Spending per Dept | Grafik pengeluaran per departemen | P1 |
| RPT-03 | Laporan | Top Supplier | Supplier dengan nilai PO terbesar | P1 |
| RPT-04 | Laporan | Export Excel | Download laporan ke XLSX | P1 |
| SYS-01 | Sistem | Multi-user role | Admin, procurement staff, manager, finance, director | P2 |
| SYS-02 | Sistem | Budget control | Set anggaran per departemen, warning overbudget | P2 |

## 3. Use Case / Alur Pengguna

### Use Case 1: Membuat dan Mensubmit Purchase Order

**Aktor:** Andi (Procurement Manager)
**Prekondisi:** Supplier sudah terdaftar, Andi sudah login

**Alur Normal:**
1. Andi klik menu "Purchase Orders" -> tombol "+ Buat PO"
2. Sistem tampilkan multi-step form: Step 1 -- Info Umum
3. Andi isi:
   - Judul: "Pembelian perlengkapan IT Q2 2025"
   - Supplier: "PT Sinar Jaya Komputer" (auto-complete)
   - Department: "Teknologi Informasi"
   - Tanggal PO: hari ini
4. Andi klik "Lanjut ke Item"
5. Step 2 -- Items:
   - Tambah item 1: Monitor Dell 27 inch, qty 5, @Rp4.500.000
   - Tambah item 2: Keyboard Mechanical, qty 5, @Rp1.250.000
   - Tambah item 3: Mouse Wireless, qty 5, @Rp950.000
   - Sistem otomatis hitung subtotal (Rp33.250.000), PPN 11% (Rp3.657.500), grand total (Rp37.407.500)
6. Andi klik "Review"
7. Step 3 -- Review: Andi cek semua data, sudah benar
8. Andi klik "Submit PO"
9. Sistem simpan PO dengan status "pending", approval workflow dimulai:
   - PO > Rp10jt -> Level 1: Manager -> Level 2: Director
10. Sistem tampilkan konfirmasi: "PO berhasil disubmit. Menunggu approval Manager."
11. Andi kembali ke daftar PO, PO baru muncul dengan status "Pending"

**Alur Alternatif (Simpan Draft):**
- 8a. Andi klik "Simpan Draft" (belum siap submit)
- 8b. Sistem simpan PO dengan status "draft"
- 8c. Andi bisa lanjutkan nanti dari daftar PO

### Use Case 2: Approval PO Multi-Level

**Aktor:** Siska (Finance Manager) -- Approver Level 1
**Prekondisi:** PO dari Andi sudah masuk approval queue Siska

**Alur Normal:**
1. Siska buka dashboard -> lihat widget "Pending Approval" (3 PO menunggu)
2. Siska klik "Approval Queue"
3. Sistem tampilkan daftar PO menunggu approval
4. Siska klik PO-025 untuk detail
5. Sistem tampilkan panel detail: info PO, items, total, notes
6. Siska setuju -> klik "Setujui"
7. Sistem minta konfirmasi: "Setujui PO-025? (Rp37.407.500)"
8. Siska tambah catatan: "Budget IT masih mencukupi. Lanjut."
9. Siska klik "Konfirmasi Setujui"
10. Sistem catat approval + kirim ke level 2 (Director)

**Alur Alternatif (Ditolak):**
- 6a. Siska cek anggaran IT -- ternyata sudah habis
- 6b. Siska klik "Tolak" -> isi alasan
- 6c. PO status jadi "rejected"
- 6d. Andi dapat notifikasi PO ditolak

## 4. Aturan Bisnis

1. **Nomor PO:** Format PO-{{Tahun}}-{{Bulan}}-{{NoUrut}} (reset bulanan)
2. **Approval Threshold:** PO > Rp10jt = 1 approver (manager). PO > Rp100jt = 2 approver (manager + director)
3. **Edit Window:** PO hanya bisa diedit jika status "draft" atau "pending"
4. **Void:** PO yang sudah "received" tidak bisa di-void, harus return barang manual
5. **Duplicate Check:** Deteksi supplier duplikat berdasarkan NPWP
6. **PPN:** 11% untuk semua transaksi (kecuali non-BKP)
7. **Partial Receive:** Barang bisa diterima sebagian, status PO jadi "partial" sampai full
8. **Currency:** IDR default. USD untuk import (dengan kurs saat PO dibuat)
9. **Budget Control:** PO tidak bisa disubmit jika melebihi sisa anggaran departemen
10. **Audit Trail:** Semua perubahan status PO tercatat (siapa, kapan, aksi)

## 5. Integrasi & Data Flow

```
                    Purchase Order Flow

  [Draft] --> [Pending] --> [Approved] --> [Ordered] --> [Received]
     |            |              |              |
     v            v              v              v
  [Edit]      [Rejected]    [Revision]     [Partial]
                            [Cancelled]

  Data Input                    Data Output
  +---------------+             +---------------+
  | PO Form       |             | Dashboard     |
  | Supplier Form |             | Reports       |
  | Receive Form  |             | Export/PDF    |
  +---------------+             +---------------+

  State Management (Zustand + React Context)
  +----------+  +----------+  +----------+  +--------------+
  |PO Store  |  |Supplier  |  |Approval  |  |Notification  |
  |          |  |Store     |  |Store     |  |Store         |
  +----------+  +----------+  +----------+  +--------------+
```

## 6. Non-Functional Requirements

| Kategori | Requirement |
|----------|-------------|
| **Performance** | Load daftar PO (500 items) < 2 detik |
| **Performance** | Submit PO response < 1 detik |
| **Performance** | Dashboard render < 2 detik |
| **Availability** | 99% uptime (static hosting) |
| **Security** | Role-based access (staff, manager, director) |
| **Usability** | Waktu pembuatan PO < 10 menit |
| **Compliance** | Format PO sesuai standar perusahaan Indonesia |
| **Data Integrity** | Status PO tidak bisa loncat (harus sequential) |
| **Audit** | Semua approval tercatat dengan timestamp |

## 7. Prioritization Matrix

```
          High Value
              |
    P0-01     |  P0-02  P0-03
    P0-04     |  P0-05  SUP-01
    SUP-02    |  SUP-03 APR-01
              |
  ------------+----------- Low Effort
    P1-06     |
    P1-09     |  P1-07  RPT-01
    RPT-02    |  RPT-03 RPT-04
              |
          Low Value
```

**Strategy:** Foundation (P0) = PO CRUD + Supplier + basic approval. Enhancement (P1) = receiving, reports, export. Advanced (P2) = multi-level approval, contracts, budget control.
