# FRD: KasirCerdas — Functional Requirements

## 1. Scope

Aplikasi Point of Sales (POS) yang berfokus pada transaksi penjualan cepat untuk toko retail. Mencakup manajemen produk, transaksi dengan berbagai metode pembayaran, manajemen shift kasir, cetak struk, dan pelaporan omzet.

**In Scope:**
- POS utama: produk grid, cart, barcode scan, pembayaran
- Manajemen produk & kategori
- Manajemen shift kasir
- Void & refund transaksi
- Cetak struk thermal (ESC/POS)
- Riwayat transaksi & dashboard
- Multi-metode pembayaran
- Manajemen pelanggan
- Diskon dan PPN

**Out of Scope:**
- Manajemen stok kompleks (purchase order, mutasi gudang)
- Akuntansi double-entry (posting jurnal otomatis)
- E-commerce integration
- Toko online / marketplace
- CRM / marketing automation

## 2. Modul & Fitur

| ID | Modul | Fitur | Deskripsi | Prioritas |
|----|-------|-------|-----------|-----------|
| POS-01 | Transaksi | Grid produk | Tampilan grid dengan thumbnail, nama, harga | P0 |
| POS-02 | Transaksi | Kategori filter | Filter produk by kategori | P0 |
| POS-03 | Transaksi | Cari produk | Search by nama/SKU/barcode | P0 |
| POS-04 | Transaksi | Scan barcode | Kamera + keyboard wedge input | P0 |
| POS-05 | Transaksi | Tambah ke cart | Tap grid → tambah qty 1 ke cart | P0 |
| POS-06 | Transaksi | Atur qty | +/- qty, input manual qty | P0 |
| POS-07 | Transaksi | Hapus item | Hapus item dari cart | P0 |
| POS-08 | Transaksi | Diskon item | Diskon per item (nominal/%) | P1 |
| POS-09 | Transaksi | Diskon transaksi | Diskon total transaksi | P1 |
| POS-10 | Transaksi | Subtotal | Hitung subtotal otomatis | P0 |
| POS-11 | Transaksi | PPN | PPN 11% otomatis (toggle) | P1 |
| POS-12 | Transaksi | Pilih pelanggan | Assign customer ke transaksi | P1 |
| PAY-01 | Pembayaran | Metode tunai | Input cash, hitung kembalian | P0 |
| PAY-02 | Pembayaran | Metode debit | Debit card (Midtrans) | P1 |
| PAY-03 | Pembayaran | Metode QRIS | QR code generation | P1 |
| PAY-04 | Pembayaran | Mixed payment | Bayar dengan 2 metode | P1 |
| PAY-05 | Pembayaran | Cetak struk | Struk thermal ESC/POS | P0 |
| PAY-06 | Pembayaran | Kirim struk digital | Struk via WA/email | P2 |
| SFT-01 | Shift | Buka shift | Set saldo awal, mulai shift | P0 |
| SFT-02 | Shift | Tutup shift | Hitung total, setoran, selisih | P0 |
| SFT-03 | Shift | Riwayat shift | Laporan per shift | P0 |
| SFT-04 | Shift | Setoran | Pisah uang setor dan modal awal | P1 |
| RPT-01 | Laporan | Omzet harian | Total penjualan hari ini | P0 |
| RPT-02 | Laporan | Laporan shift | Rekap per shift | P0 |
| RPT-03 | Laporan | Produk terlaris | Top 10 produk | P1 |
| RPT-04 | Laporan | Laporan per periode | Filter tanggal | P1 |
| CUS-01 | Pelanggan | Daftar pelanggan | CRUD data pelanggan | P1 |
| CUS-02 | Pelanggan | Riwayat belanja | Transaksi per pelanggan | P1 |
| CUS-03 | Pelanggan | Poin loyalitas | Accumulate & redeem points | P2 |
| VOID-01 | Void | Void transaksi | Batalkan transaksi (password) | P1 |
| VOID-02 | Void | Alasan void | Catat alasan pembatalan | P1 |
| VOID-03 | Void | Audit log | Log semua void | P1 |

## 3. Use Case / Alur Pengguna

### Use Case 1: Transaksi Penjualan Tunai Cepat

**Aktor:** Sari (Kasir Toko Bangunan)
**Prekondisi:** Shift sudah dibuka, produk sudah ada di sistem

**Alur Normal:**
1. Pelanggan datang dengan 5 sak semen + 2 palu
2. Sari scan barcode semen via kamera → "BIP" suara sukses → otomatis masuk ke cart (qty 1)
3. Sari tap qty semen di cart → ubah ke "5"
4. Sari scan palu → masuk cart → ubah qty ke "2"
5. Sari cek total di cart: Rp 275.000 + Rp 17.000 = Rp 292.000
6. Sari tap "Bayar"
7. Sistem tampilkan modal pembayaran dengan total Rp 292.000
8. Sari pilih "Tunai" (default)
9. Pelanggan bayar Rp 300.000 → Sari input "300000" di field "Dibayar"
10. Sistem otomatis hitung kembalian: Rp 8.000
11. Sari tap "Konfirmasi Bayar"
12. Sistem simpan transaksi, kurangi stok, cetak struk otomatis
13. Struk tercetak, Sari serahkan ke pelanggan
14. Otomatis kembali ke POS screen siap untuk transaksi berikutnya

**Alur Alternatif (Produk Tidak Ditemukan):**
- 4a. Sari scan produk, barcode tidak ditemukan
- 4b. Sistem beep error (suara berbeda) + toast "Produk tidak ditemukan"
- 4c. Sari bisa: input manual via cari nama, atau cancel

**Alur Alternatif (Pembayaran Kurang):**
- 9a. Sari input "250000" (kurang Rp 42.000)
- 9b. Sistem tampilkan: "Pembayaran kurang Rp 42.000"
- 9c. Tombol "Konfirmasi Bayar" tidak aktif
- 9d. Sari bisa: minta pelanggan tambah, atau pindah ke mixed payment

### Use Case 2: Void Transaksi

**Aktor:** Sari (Kasir) + Supervisor
**Prekondisi:** Ada transaksi yang sudah selesai

**Alur Normal:**
1. Pelanggan kembali, minta refund karena barang cacat
2. Sari buka menu Riwayat Transaksi → cari transaksi INV-20250315-001
3. Sari tap transaksi → detail → "Void"
4. Sistem minta password supervisor
5. Sari panggil supervisor, supervisor input password
6. Sistem tampilkan form alasan void
7. Sari pilih alasan: "Barang Cacat" + catatan opsional
8. Sari tap "Konfirmasi Void"
9. Sistem void transaksi:
   - Status transaksi → "voided"
   - Stok produk dikembalikan (stok + lagi)
   - Struk void tercetak (tulisan "VOID" besar)
   - Audit log: user Sari, supervisor verify, timestamp

## 4. Aturan Bisnis

1. **Harga:** Default dari master produk. Override price butuh password supervisor.
2. **Stok:** Setiap transaksi completed = stok produk berkurang otomatis
3. **Void:** Hanya bisa void transaksi hari yang sama (H+0). Void H+1 perlu refund manual.
4. **Diskon:** Maks diskon per item 30% (tanpa supervisor). >30% perlu approval.
5. **PPN:** Otomatis 11% untuk semua produk, bisa di-non-aktifkan (untuk non-BKP).
6. **Shift:** Hanya bisa transaksi jika shift aktif. Satu user hanya bisa buka 1 shift.
7. **Setoran:** Setor tunai minimal sama dengan total penjualan tunai - modal awal.
8. **Nomor Nota:** Format INV-{{Tahun}}{{Bulan}}{{Tanggal}}-{{NoUrut}}. Reset harian.
9. **Cetak Struk:** Wajib cetak struk untuk setiap transaksi. Jika printer error, simpan draft.
10. **Offline Queue:** Transaksi di-cache lokal, push ke server saat online. Antrian FIFO.

## 5. Integrasi & Data Flow

```
┌────────────────────────────────────────────────────────────────┐
│                        KasirCerdas                              │
│                                                                │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────────────┐  │
│  │ POS Screen   │   │ Admin Web    │   │ Mobile Kasir     │  │
│  │ (React + POS)│   │ (React Admin)│   │ (PWA + Camera)   │  │
│  └──────┬───────┘   └──────┬───────┘   └────────┬─────────┘  │
│         │                  │                     │            │
│  ┌──────▼──────────────────▼─────────────────────▼──────────┐ │
│  │                    Data Layer                             │ │
│  │  ┌─────────────┐  ┌─────────────┐  ┌───────────────┐   │ │
│  │  │ IndexedDB   │  │ Zustand     │  │ Sync Queue    │   │ │
│  │  │ (Offline)   │  │ (UI State)  │  │ (Pending TXs) │   │ │
│  │  └─────────────┘  └─────────────┘  └───────┬───────┘   │ │
│  └─────────────────────────────────────────────┼─────────────┘ │
│                                                 │              │
│  ┌──────────────────────────────────────────────▼────────────┐ │
│  │  Sync Engine (Online)                                     │ │
│  │  ┌────────────┐  ┌────────────┐  ┌──────────────┐       │ │
│  │  │ REST API   │  │ WebSocket  │  │ Auth Service │       │ │
│  │  └──────┬─────┘  └────────────┘  └──────────────┘       │ │
│  └─────────┼───────────────────────────────────────────────────┘ │
└────────────┼─────────────────────────────────────────────────┘
             │
    ┌────────▼────────┐  ┌──────────────┐  ┌──────────────────┐
    │   Printer USB   │  │  Scanner     │  │  Payment Gateway  │
    │   (WebUSB)      │  │  (Camera)    │  │  (Midtrans)       │
    └─────────────────┘  └──────────────┘  └──────────────────┘
```

**External Integrations:**
- **Printer:** WebUSB API untuk Epson TM series / XPrinter (ESC/POS protocol)
- **Scanner:** Kamera (html5-qrcode) + keyboard wedge (barcode scanner read as keyboard input)
- **Payment:** Midtrans API untuk debit, credit, QRIS
- **Sync:** REST API ke backend server untuk aggregasi data multi-toko

## 6. Non-Functional Requirements

| Kategori | Requirement |
|----------|-------------|
| **Performance** | Scan → cart response < 100ms |
| **Performance** | Payment process < 3 detik |
| **Performance** | Struk cetak < 5 detik |
| **Offline** | Full POS functionality tanpa internet |
| **Sync** | Sinkronisasi otomatis saat online, max delay 5 detik |
| **Reliability** | Tidak ada data loss saat crash (transactional) |
| **Device** | Support Android 9+, layar 6 inch - 10 inch |
| **Printer** | Support Epson TM series, XPrinter, generic ESC/POS |
| **Security** | Password supervisor untuk void & override |
| **Sound** | Audio feedback untuk scan sukses/gagal, payment |

## 7. Prioritization Matrix

```
          High Value
              │
    P0-01 ●   │  ● P0-02  ● P0-03
    P0-04 ●   │  ● P0-05  ● P0-06
    P0-07 ●   │  ● P0-08  ● P0-09
              │
  ───────────┼─────────── Low Effort
    P1-10 ●  │
    P1-12 ●  │  ● P1-11  ● P1-13
    P1-14 ●  │  ● P1-15  ● P1-16
              │
          Low Value
```

**Strategy:** Phase 1 = minimal viable POS (scan → cart → cash → print). Phase 2 = payment variety + management features. Phase 3 = offline resilience + multi-store.
