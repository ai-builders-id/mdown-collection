# FRD: SCM Pro — Functional Requirements

## 1. Scope
SCM Pro mencakup manajemen inventory multi-gudang, tracking pengiriman end-to-end, manajemen supplier dan purchase order, serta pelaporan supply chain. Sistem ini dirancang untuk UKM dan perusahaan menengah di Indonesia dengan volume transaksi 100-5000 SKU.

## 2. Modul & Fitur

| Modul | Fitur | Deskripsi | Prioritas |
|-------|-------|-----------|-----------|
| **Inventory** | CRUD Produk | Tambah, edit, hapus, nonaktifkan produk | P0 |
| **Inventory** | Manajemen Stok | Update stok masuk/keluar, adjustment, opname | P0 |
| **Inventory** | Multi-Gudang | Atur stok per lokasi gudang | P1 |
| **Inventory** | Reorder Alert | Notifikasi otomatis saat stok di bawah minimum | P0 |
| **Inventory** | Barcode Scan | Scan barcode untuk penerimaan/pengeluaran | P2 |
| **Pengiriman** | Buat Pengiriman | Input data pengiriman, pilih produk & qty | P0 |
| **Pengiriman** | Status Kanban | Drag-drop card antar status | P0 |
| **Pengiriman** | Tracking Map | Visual posisi pengiriman (jika integrasi GPS) | P1 |
| **Pengiriman** | Dokumen | Upload surat jalan, invoice, foto barang | P1 |
| **Supplier** | CRUD Supplier | Data profil, kontak, alamat, rating | P0 |
| **Supplier** | Purchase Order | Buat & manage PO, track status approval | P1 |
| **Supplier** | Evaluasi | Rating, lead time, on-time delivery % | P1 |
| **Laporan** | Dashboard | Ringkasan stok, pengiriman, supplier | P0 |
| **Laporan** | Ekspor | Export data ke PDF/Excel/CSV | P1 |

## 3. Use Case

**UC-01: Melakukan Penerimaan Barang**
1. User membuka menu Pengiriman > pilih shipment dengan status "Dalam Perjalanan"
2. Sistem menampilkan detail PO dan daftar item yang dikirim
3. User scan barcode atau input manual qty yang diterima
4. Sistem memvalidasi: qty diterima tidak boleh melebihi qty dikirim
5. Jika ada selisih, user dapat menambah notes (claim/shortage)
6. Sistem update stok gudang tujuan secara real-time
7. Status shipment berubah menjadi "Diterima Sebagian" atau "Selesai"
8. Sistem mengirim notifikasi ke purchasing manager

**UC-02: Konfigurasi Reorder Point & Generate Alert**
1. User membuka detail produk
2. Input nilai "Stok Minimum" dan "Stok Maksimum"
3. Sistem menyimpan konfigurasi
4. Setiap kali transaksi keluar terjadi, sistem cek: jika stok_saat_ini <= stok_minimum
5. Sistem generate alert reorder di dashboard + notifikasi push
6. Alert menampilkan: nama produk, stok saat ini, rekomendasi qty order (max - current)
7. User dapat langsung membuat PO dari alert tersebut

## 4. Aturan Bisnis
- Stok tidak boleh negatif: validasi saat transaksi keluar
- Satu SKU unik per produk — tidak boleh duplikat
- Qty penerimaan barang tidak boleh melebihi qty di PO (kecuali dengan approval)
- Status shipment hanya bisa maju: Dijadwalkan > Dalam Perjalanan > Diterima > Selesai (tidak bisa mundur, kecuali admin)
- Hanya user dengan role Admin Gudang yang bisa melakukan adjustment stok
- Setiap perubahan stok harus tercatat di history/log
- Harga beli produk tidak bisa diubah jika sudah ada transaksi pembelian terkait

## 5. Integrasi & Data Flow
```
[Input Manual] ──────┐
[Barcode Scan] ──────┤
[Import CSV] ────────┤───> SCM Pro API ───> Database ───> Cache Redis
                     │                          │
[Ekspedisi API] ─────┘                          │
                                                v
                                         [Notification Service]
                                                │
                                                v
                                        [Email/Push/In-App]
```

- **Import/Export**: CSV dengan template header yang sudah ditentukan
- **Integrasi Ekspedisi**: REST API ke RajaOngkir, JNE, SiCepat (Phase 2)
- **Email Notifikasi**: SMTP untuk alert reorder dan status pengiriman

## 6. Non-Functional Requirements
- **Performance**: Dashboard inventory load < 2 detik untuk 5000 SKU
- **Availability**: 99.5% uptime (kecuali maintenance terjadwal)
- **Concurrency**: Support 50 user bersamaan melakukan transaksi
- **Data Retention**: Riwayat transaksi disimpan minimal 3 tahun
- **Backup**: Backup database otomatis setiap 6 jam

## 7. Prioritization Matrix

| Fitur | Value Bisnis | Effort | Priority |
|-------|-------------|--------|----------|
| CRUD Produk | High | Low | P0 |
| Reorder Alert | High | Low | P0 |
| Tracking Kanban | High | Medium | P0 |
| Multi-Gudang | Medium | High | P1 |
| Barcode Scan | Medium | Medium | P2 |
| Forecast Demand | Low | High | P2 |
