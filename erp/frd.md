# FRD: ERP Nusantara — Functional Requirements

## 1. Scope

ERP Nusantara adalah sistem ERP all-in-one yang mencakup 6 modul inti: Finance, Inventory, Sales, Purchasing, Human Resources, dan Operational Reporting. Sistem menyediakan integrasi data real-time antar modul, dashboard terpusat, dan approval workflow.

**In Scope:**
- Modul Finance (COA, Jurnal, Buku Besar, AR/AP, Laporan Keuangan)
- Modul Inventory (Produk, Stok, Mutasi Gudang)
- Modul Sales (Sales Order, Customer Management)
- Modul Purchasing (Purchase Order, Supplier Management)
- Modul HR (Karyawan, Absensi, Payroll Dasar, Cuti)
- Dashboard Eksekutif
- Multi-perusahaan & Multi-user
- Approval Workflow

**Out of Scope:**
- Manufacturing / MRP
- Project Management
- CRM (Customer Relationship)
- E-commerce integration
- Business Intelligence advanced

## 2. Modul & Fitur

### Modul Finance
| ID | Fitur | Deskripsi | Prioritas |
|----|-------|-----------|-----------|
| FIN-01 | Chart of Account | CRUD akun standar PSAK | P0 |
| FIN-02 | Jurnal Umum | Entri jurnal multi-line | P0 |
| FIN-03 | Buku Besar | Otomatis dari jurnal | P0 |
| FIN-04 | Neraca Lajur | Trial balance per periode | P0 |
| FIN-05 | Piutang (AR) | Aging, tagihan customer | P1 |
| FIN-06 | Utang (AP) | Aging, tagihan supplier | P1 |
| FIN-07 | Laporan Laba Rugi | Periodik | P0 |
| FIN-08 | Laporan Neraca | Posisi keuangan | P0 |
| FIN-09 | Laporan Arus Kas | Metode tidak langsung | P2 |
| FIN-10 | Rekonsiliasi Bank | Cocokkan dengan bank statement | P2 |

### Modul Inventory
| ID | Fitur | Deskripsi | Prioritas |
|----|-------|-----------|-----------|
| INV-01 | Master Produk | SKU, barcode, kategori, harga | P0 |
| INV-02 | Stok Masuk | Penerimaan barang (PO → stok) | P0 |
| INV-03 | Stok Keluar | Pengeluaran barang (SO → stok) | P0 |
| INV-04 | Mutasi Gudang | Transfer antar gudang | P1 |
| INV-05 | Opname Stok | Stock opname periodik | P1 |
| INV-06 | Peringatan Stok | Min/max stock alerts | P0 |
| INV-07 | Kartu Stok | Riwayat pergerakan per produk | P0 |

### Modul Sales
| ID | Fitur | Deskripsi | Prioritas |
|----|-------|-----------|-----------|
| SAL-01 | Master Customer | Data pelanggan | P0 |
| SAL-02 | Sales Order | Buat, edit, approve SO | P0 |
| SAL-03 | Invoice | Generate invoice dari SO | P1 |
| SAL-04 | Penerimaan Pembayaran | Catat pembayaran customer | P1 |
| SAL-05 | Riwayat Penjualan | Laporan penjualan | P0 |
| SAL-06 | Price List | Harga per produk per customer | P1 |

### Modul Purchasing
| ID | Fitur | Deskripsi | Prioritas |
|----|-------|-----------|-----------|
| PUR-01 | Master Supplier | Data supplier | P0 |
| PUR-02 | Purchase Order | Buat, approve, kirim PO | P0 |
| PUR-03 | Penerimaan Barang | Receive barang dari PO | P0 |
| PUR-04 | Pembayaran | Catat pembayaran ke supplier | P1 |
| PUR-05 | Riwayat Pembelian | Laporan pembelian | P0 |

### Modul HR
| ID | Fitur | Deskripsi | Prioritas |
|----|-------|-----------|-----------|
| HR-01 | Master Karyawan | Data pribadi, kontrak, dokumen | P0 |
| HR-02 | Struktur Organisasi | Department, posisi, level | P1 |
| HR-03 | Absensi | Check-in/out, izin, lembur | P1 |
| HR-04 | Cuti | Pengajuan & approval cuti | P1 |
| HR-05 | Payroll Dasar | Hitung gaji + potongan | P1 |
| HR-06 | BPJS | Kelola nomor & iuran BPJS | P2 |
| HR-07 | PPh 21 | Hitung PPh 21 karyawan | P2 |

## 3. Use Case / Alur Pengguna

### Use Case 1: Proses Penjualan dari SO hingga Pembayaran

**Aktor:** Dewi (Finance Manager) & Staff Sales
**Prekondisi:** Data produk dan customer sudah ada

**Alur Normal:**
1. Staff Sales membuat Sales Order baru untuk PT Bangun Sejahtera
2. Pilih produk (Semen 50 sak, Pasir 2 truk), sistem hitung total otomatis
3. SO disimpan sebagai "draft" → dikirim untuk approval ke supervisor
4. Supervisor approve SO → status "confirmed"
5. Sistem kurangi stok otomatis (Inventory menyesuaikan)
6. Sistem generate jurnal otomatis (Piutang + Pendapatan)
7. Barang dikirim → status SO "delivered"
8. Finance generate invoice dari SO → kirim ke customer
9. Customer bayar → Finance catat penerimaan pembayaran
10. Sistem update AR Aging, update jurnal (Kas + Piutang)

**Alur Alternatif (Stok Tidak Mencukupi):**
- 5a. Stok Semen hanya 30 sak, permintaan 50 sak
- 5b. Sistem bloque: "Stok tidak mencukupi. Stok tersedia: 30"
- 5c. User bisa: (1) kurangi qty, (2) buat PO untuk restock, (3) partial delivery

### Use Case 2: Proses Penggajian Bulanan

**Aktor:** Rudi (HR Manager)
**Prekondisi:** Data karyawan lengkap, absensi bulan berjalan sudah di-approve

**Alur Normal:**
1. Rudi buka modul HR → Payroll → "Generate Payroll Bulan Ini"
2. Sistem hitung otomatis:
   - Gaji pokok (masing-masing karyawan)
   - Tunjangan tetap
   - Lembur (dari data absensi: total jam lembur × rate)
   - Potongan BPJS (4% dari gaji untuk pensiun)
   - PPh 21 (jika sudah > PTKP)
3. Rudi review perhitungan per karyawan
4. Rudi klik "Approve All" → status payroll "approved"
5. Sistem generate slip gaji PDF per karyawan
6. Data payroll dikirim ke modul Finance:
   - Jurnal biaya gaji (Beban Gaji + Utang Gaji)
   - Jurnal potongan BPJS
7. Finance proses pembayaran gaji via bank transfer

## 4. Aturan Bisnis

1. **Integrasi SO-Inventory:** SO confirmed = stok otomatis terkurangi (real-time)
2. **Integrasi PO-Inventory:** PO received = stok otomatis bertambah
3. **SO-PO Link:** Jika stok tidak cukup, sistem rekomendasi buat PO terkait
4. **Approval Routing:** SO > Rp50 juta perlu approval manager, > Rp100 juta perlu director
5. **Harga Jual:** Minimal = purchase price + 15% margin
6. **Diskon:** Maks diskon 30% (kecuali ada approval khusus)
7. **PPN:** Otomatis 11% untuk semua transaksi B2B, bisa di-override
8. **Cuti:** Maks 12 hari/tahun, minimal pengajuan H-3
9. **Lembur:** Max 4 jam/hari, rate 1.5× di hari biasa, 2× di hari libur
10. **Tutup Buku:** Semua transaksi harus selesai H+5 setelah bulan tutup

## 5. Integrasi & Data Flow

```
                    ┌──────────────────────────────┐
                    │      ERP Nusantara Core        │
                    │  ┌──────┐ ┌──────┐ ┌──────┐  │
                    │  │Finance│ │Inventory│ │ Sales│  │
                    │  └──┬───┘ └──┬───┘ └──┬───┘  │
                    │     │         │         │      │
                    │  ┌──▼───┐ ┌──▼───┐ ┌──▼───┐  │
                    │  │  HR  │ │Purch. │ │Repo. │  │
                    │  └──────┘ └──────┘ └──────┘  │
                    └──────────────┬───────────────┘
                                   │
        ┌──────────────────────────┼──────────────────────────┐
        │                          │                          │
   ┌────▼────┐              ┌──────▼──────┐           ┌──────▼──────┐
   │  Bank   │              │  E-Faktur   │           │   BPJS      │
   │  API    │              │  DJP Online │           │   Ketenaga-  │
   └─────────┘              └─────────────┘           │   kerjaan   │
                                                      └─────────────┘
```

**Data Flow Antar Modul:**
- **Sales → Inventory:** SO confirmed → stok berkurang
- **Purchasing → Inventory:** PO received → stok bertambah
- **Sales → Finance:** SO → Piutang (AR) + Pendapatan
- **Purchasing → Finance:** PO → Utang (AP) + Persediaan
- **HR → Finance:** Payroll → Beban Gaji + Utang Gaji
- **Inventory → Finance:** COGS calculation

## 6. Non-Functional Requirements

| Kategori | Requirement |
|----------|-------------|
| **Performance** | Dashboard muat < 3 detik untuk data 12 bulan |
| **Performance** | Query laporan < 5 detik untuk 50.000 transaksi |
| **Availability** | 99.5% uptime (kecuali maintenance) |
| **Scalability** | Support 500 concurrent users |
| **Security** | Role-based access, audit log, enkripsi data |
| **Data Integrity** | Transactional integrity antar modul |
| **Backup** | Automated backup setiap 4 jam, point-in-time recovery |
| **Compliance** | UU ITE, UU PDP (Perlindungan Data Pribadi) |
| **Multi-tenancy** | Full data isolation antar perusahaan |
| **Mobile** | Responsive view untuk tablet dan smartphone |

## 7. Prioritization Matrix

```
          High Value
              │
    P0-01 ●   │  ● P0-02  ● P0-03
    P0-05 ●   │  ● P0-04  ● P0-06
    P0-08 ●   │  ● P0-07
              │
  ───────────┼─────────── Low Effort
    P1-09 ●  │
    P1-11 ●  │  ● P1-10  ● P1-12
    P1-14 ●  │  ● P1-13
              │
          Low Value
```

**Strategy:** Deliver Foundation (P0) first = Sales → Inventory → Finance core loop. Then add HR module and advanced reporting (P1).
