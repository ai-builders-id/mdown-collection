# FRD: EstateOS — Functional Requirements

## 1. Scope
EstateOS adalah sistem manajemen properti untuk pemilik dan manajer properti di Indonesia. Mencakup manajemen properti, penyewa, kontrak sewa, pembayaran, dan laporan. Target pengguna adalah pemilik properti dengan 5-200 unit properti dan tim manajemen properti.

## 2. Modul & Fitur

| Modul | Fitur | Deskripsi | Prioritas |
|-------|-------|-----------|-----------|
| **Properti** | CRUD Properti | Tambah/edit/hapus properti + detail + foto | P0 |
| **Properti** | Gallery | Grid masonry, lightbox, upload multiple foto | P1 |
| **Properti** | Map View | Peta marker lokasi properti | P2 |
| **Properti** | Fasilitas | Checklist fasilitas, listrik, AC, parkir, dll | P0 |
| **Penyewa** | CRUD Penyewa | Data pribadi, NIK, kontak, pekerjaan | P0 |
| **Penyewa** | Dokumen | Upload KTP, kontrak kerja, dokumen lain | P1 |
| **Penyewa** | Rating | Rating pembayaran otomatis + manual review | P1 |
| **Kontrak** | Buat Kontrak | Pilih properti + penyewa, atur biaya, deposit, denda | P0 |
| **Kontrak** | Perpanjang | Perpanjang kontrak dengan update biaya | P1 |
| **Kontrak** | Akhiri Kontrak | Catat pengembalian deposit, handover properti | P1 |
| **Pembayaran** | Generate Tagihan | Buat tagihan bulanan otomatis dari kontrak | P1 |
| **Pembayaran** | Catat Pembayaran | Input pembayaran, upload bukti transfer | P1 |
| **Pembayaran** | Status | Lunas, pending, telat, gagal | P0 |
| **Laporan** | Dashboard | Total unit, okupansi, income, piutang | P0 |
| **Laporan** | Income Report | Grafik pendapatan per bulan/per properti | P1 |
| **Laporan** | Export Excel | Download data properti, penyewa, pembayaran | P1 |

## 3. Use Case

**UC-01: Proses Penyewaan Properti Baru**
1. User masuk ke menu Properti, pilih properti dengan status "Tersedia"
2. Ubah status ke "Dalam Proses" (optional)
3. Buka menu Kontrak > "Buat Kontrak Baru"
4. Sistem tampilkan form: pilih properti (pre-filled), pilih penyewa (dari daftar atau tambah baru)
5. Isi: mulai sewa, durasi, biaya sewa, biaya service, deposit, denda telat
6. Upload dokumen kontrak (PDF)
7. Sistem generate nomor kontrak otomatis: KTR/YYYY/MM/TIPE/XXX
8. Simpan → kontrak aktif, status properti berubah jadi "Disewa"
9. Sistem generate tagihan pertama dengan status "Pending"
10. Dashboard update okupansi dan pendapatan

**UC-02: Pelacakan Pembayaran Sewa**
1. Setiap bulan (sesuai jadwal kontrak), sistem generate tagihan otomatis
2. Tagihan muncul di menu Pembayaran dengan status "Pending"
3. User (admin) atau penyewa bayar via transfer/tunai
4. Admin input pembayaran: pilih tagihan, masukkan jumlah, upload bukti
5. Sistem validasi: jumlah >= tagihan (jika lebih → catat sebagai deposit kelebihan)
6. Status tagihan berubah jadi "Lunas", tanggal bayar tercatat
7. Jika H+3 belum bayar → status "Telat", sistem hitung denda otomatis
8. Dashboard piutang terupdate, notifikasi ke admin dan penyewa

## 4. Aturan Bisnis
- NIK penyewa harus unique — tidak boleh duplikat
- Satu properti hanya bisa memiliki 1 kontrak aktif dalam satu waktu
- Kontrak tidak bisa tumpang tindih (overlap) untuk properti yang sama
- Tagihan generate otomatis setiap tanggal {{tanggalJatuhTempo}} setiap bulan
- Jika pembayaran > tanggal jatuh tempo, denda otomatis {{dendaTelat}} per hari
- Deposit dikembalikan saat kontrak berakhir, setelah dikurangi kerusakan (jika ada)
- Nomor kontrak auto-format: KTR/(TAHUN)/(BULAN)/(TIPE)/(XXXX)
- Hanya properti dengan status "Tersedia" atau "Disewa" yang bisa dipilih di form kontrak

## 5. Integrasi & Data Flow
```
[Web Dashboard] ─────┐
[Input Manual] ──────┤──> EstateOS API ──> PostgreSQL ──> Redis Cache
                      │                        │
                      │                        v
                      │                 [Scheduler Service]
                      │                        │
                      │                 [Generate Tagihan Bulanan]
                      │                        │
                      │                        v
                      │                 [Notification Service]
                      │                        │
                      └────────────────── [Email / WhatsApp]
```

- **WhatsApp Gateway**: Integrasi dengan layanan WA API (WATI / Twilio) untuk reminder pembayaran
- **Google Maps**: Embed maps untuk tampilan lokasi properti
- **Upload File**: Cloudinary untuk foto properti dan dokumen
- **Export**: Excel via xlsx library untuk laporan properti, penyewa, pembayaran

## 6. Non-Functional Requirements
- **Performance**: Dashboard load < 2 detik untuk 200 properti
- **Concurrency**: 20 user mengelola properti secara bersamaan
- **Storage**: Foto properti maks 5MB/file, support format JPEG/PNG/WebP
- **Uptime**: 99% (maintenance window)
- **Data Retention**: Riwayat kontrak dan pembayaran min 5 tahun
- **Response Time**: API < 500ms

## 7. Prioritization Matrix

| Fitur | Value Bisnis | Effort | Priority |
|-------|-------------|--------|----------|
| CRUD Properti | High | Low | P0 |
| CRUD Penyewa | High | Low | P0 |
| Buat Kontrak | High | Medium | P0 |
| Dashboard | High | Low | P0 |
| Generate Tagihan | High | Medium | P1 |
| Notifikasi | Medium | Medium | P1 |
| Map View | Low | High | P2 |
