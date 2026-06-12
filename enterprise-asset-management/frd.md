# FRD: EAM Pro — Functional Requirements

## 1. Scope
EAM Pro mencakup pencatatan dan manajemen aset fisik perusahaan, penjadwalan dan pencatatan maintenance, perhitungan depresiasi otomatis, serta pelaporan siap audit. Target pengguna adalah perusahaan menengah dengan 100-5000 aset.

## 2. Modul & Fitur

| Modul | Fitur | Deskripsi | Prioritas |
|-------|-------|-----------|-----------|
| **Aset** | CRUD Aset | Tambah, edit, hapus aset dengan detail lengkap | P0 |
| **Aset** | Kategori & Lokasi | Hierarki kategori, lokasi gedung/lantai/ruang | P0 |
| **Aset** | Import/Export Excel | Upload data aset massal via template | P1 |
| **Aset** | QR/Barcode Label | Generate + cetak label untuk fisik aset | P1 |
| **Aset** | Riwayat Mutasi | Catat perpindahan aset antar lokasi/dept | P1 |
| **Maintenance** | Jadwal Preventif | Buat jadwal rutin harian/mingguan/bulanan | P0 |
| **Maintenance** | Catat Maintenance | Input biaya, teknisi, deskripsi, foto | P0 |
| **Maintenance** | Notifikasi | Reminder H-1 dan H-0 maintenance | P1 |
| **Depresiasi** | Hitung Otomatis | Garis lurus, saldo menurun, unit produksi | P1 |
| **Depresiasi** | Proyeksi | Grafik nilai aset di masa depan | P2 |
| **Laporan** | Dashboard | Total aset, nilai, breakdown kondisi | P0 |
| **Laporan** | Export | PDF/Excel laporan aset, maintenance, depresiasi | P1 |

## 3. Use Case

**UC-01: Opname Fisik Aset Tahunan**
1. Admin memulai sesi opname fisik dari menu Aset > Opname
2. Sistem generate daftar aset yang harus diverifikasi
3. User scan QR/barcode aset menggunakan HP
4. Sistem menampilkan data aset: nama, lokasi tercatat, kondisi terakhir
5. User konfirmasi "Ditemukan" atau laporkan "Tidak Ditemukan"
6. Jika kondisi berbeda, user update kondisi aset
7. Sistem tandai aset yang terverifikasi dengan timestamp
8. Setelah sesi selesai, sistem generate laporan selisih

**UC-02: Jadwal Maintenance Preventif Otomatis**
1. Admin masuk ke menu Aset, pilih aset (misal: AC)
2. Atur jadwal preventif: tiap 3 bulan, teknisi "PT Servis Mandiri"
3. Sistem simpan jadwal dan generate task maintenance untuk 12 bulan ke depan
4. H-1 sebelum jadwal, sistem kirim notifikasi ke PIC + teknisi
5. Teknisi login, lihat task, update status "Sedang Dikerjakan"
6. Setelah selesai, input biaya, upload foto hasil, status "Selesai"
7. Sistem update riwayat aset dan jadwal berikutnya

## 4. Aturan Bisnis
- Kode aset otomatis generate: {DEPARTEMEN}/{KATEGORI}/{TAHUN}/{NOMORURUT}
- Nomor seri bersifat unik — duplikasi tidak diperbolehkan
- Aset dengan status "Dihapuskan" tidak bisa diubah/digunakan kembali
- Maintenance preventif otomatis generate ulang setelah satu selesai
- Hanya user role Admin yang bisa menghapus aset
- Perubahan nilai aset harus memiliki approval dari Finance
- Depresiasi dihitung per bulan, dimulai bulan setelah perolehan

## 5. Integrasi & Data Flow
```
[Input Manual] ──────┐
[Import Excel] ──────┤──> EAM Pro API ──> PostgreSQL DB ──> Cache Redis
[QR Scan Mobile] ────┘                          │
                                                 v
                                         [Scheduler Service]
                                                 │
                                          [Maintenance Jobs]
                                                 │
                                                 v
                                         [Notification Service]
                                                 │
                                          [Email / WA Gateway]
```

- **Import Excel**: Template dengan kolom: kode, nama, kategori, merek, seri, lokasi, nilai, tgl_perolehan
- **Integrasi ERP**: REST API untuk sinkronisasi data aset dengan modul akuntansi
- **QR Code**: Generate via `qrcode.react` library, format base64 untuk print

## 6. Non-Functional Requirements
- **Performance**: Dashboard load < 2 detik untuk 5000 aset
- **Concurrency**: Support 30 user input data aset simultan
- **Uptime**: 99% (maintenance window Minggu 02:00-04:00)
- **Data Retention**: Riwayat maintenance minimum 5 tahun
- **Response Time**: API response < 500ms untuk operasi CRUD

## 7. Prioritization Matrix

| Fitur | Value Bisnis | Effort | Priority |
|-------|-------------|--------|----------|
| CRUD Aset | High | Low | P0 |
| Dashboard | High | Low | P0 |
| Jadwal Maintenance | High | Medium | P1 |
| Depresiasi Otomatis | Medium | Medium | P1 |
| QR Label | Medium | Low | P1 |
| Mobile Scan | Medium | High | P2 |
