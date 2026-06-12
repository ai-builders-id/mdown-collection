# FRD: FleetPro — Functional Requirements

## 1. Scope
FleetPro mencakup manajemen armada kendaraan: pencatatan kendaraan dan driver, manajemen perjalanan (trip), pencatatan BBM, jadwal perawatan, dan pelaporan biaya operasional. Target pengguna adalah perusahaan dengan armada 5-100 kendaraan.

## 2. Modul & Fitur

| Modul | Fitur | Deskripsi | Prioritas |
|-------|-------|-----------|-----------|
| **Kendaraan** | CRUD Kendaraan | Tambah/edit/hapus kendaraan + spesifikasi | P0 |
| **Kendaraan** | Dokumen | Upload foto STNK, KIR, asuransi | P1 |
| **Kendaraan** | Reminder Pajak/KIR | Notifikasi otomatis H-30, H-7, H-1 | P1 |
| **Driver** | CRUD Driver | Data personal, SIM, kontak, rating | P0 |
| **Driver** | Penugasan | Assign driver ke kendaraan | P0 |
| **Trip** | Mulai Trip | Check-in kendaraan, driver, KM awal, tujuan | P0 |
| **Trip** | Selesai Trip | Check-out: KM akhir, biaya, catatan | P0 |
| **Trip** | Riwayat Trip | Table riwayat per kendaraan/driver/periode | P1 |
| **BBM** | Catat Isi BBM | Input liter, harga, SPBU, KM, upload foto | P1 |
| **BBM** | Laporan Konsumsi | Grafik km/L per kendaraan, tren, anomali | P1 |
| **Service** | Jadwal Service | Input jadwal rutin, catat riwayat service | P1 |
| **Service** | Reminder | Notifikasi service due, pajak, KIR, SIM | P1 |
| **Map** | Live Tracking | Peta dengan marker kendaraan (mock) | P2 |
| **Laporan** | Dashboard Executive | Ringkasan biaya, utilisasi, efisiensi | P1 |

## 3. Use Case

**UC-01: Memulai dan Menyelesaikan Perjalanan (Trip)**
1. Driver login ke mobile app / supervisor buka web
2. Pilih kendaraan dan driver, input KM awal (foto odometer)
3. Input tujuan, muatan, dan catatan
4. Sistem simpan trip dengan status "Berlangsung"
5. Saat sampai tujuan: input KM akhir, total biaya tol + BBM + lain
6. Upload foto bukti (struk tol, struk BBM)
7. Sistem hitung jarak tempuh, konsumsi BBM, biaya total
8. Status trip berubah menjadi "Selesai"
9. Dashboard update: kendaraan tersedia, statistik trip terbaru

**UC-02: Monitoring Konsumsi BBM dan Deteksi Anomali**
1. Setiap kali driver isi BBM, input jumlah liter, harga, KM, SPBU
2. Sistem hitung konsumsi BBM = (KM sekarang - KM sebelumnya) / liter
3. Bandingkan dengan rata-rata historis kendaraan
4. Jika konsumsi turun >20% dari rata-rata, sistem generate alert
5. Alert muncul di dashboard: "Konsumsi BBM Colt D 1234 FG turun drastis. Cek mesin."
6. Supervisor dapat melihat detail perbandingan per bulan di grafik
7. Data dapat diexport untuk evaluasi penggantian kendaraan

## 4. Aturan Bisnis
- Satu kendaraan hanya bisa memiliki satu trip aktif dalam satu waktu
- Driver tidak bisa ditugaskan ke dua trip bersamaan
- KM akhir trip baru harus > KM awal
- Konsumsi BBM dihitung otomatis dari selisih KM dibagi liter
- Jika konsumsi BBM < 50% dari rata-rata, sistem flagged sebagai anomali
- Pajak/KIR/SIM yang kedaluwarsa > 30 hari: kendaraan otomatis dinonaktifkan
- Rating driver dihitung dari rata-rata review supervisor
- Biaya perjalanan harus diisi untuk menyelesaikan trip

## 5. Integrasi & Data Flow
```
[Web Dashboard] ──┐
[Driver Mobile] ──┤──> FleetPro API ──> PostgreSQL ──> Redis Cache
[GPS Tracker] ────┘                        │
                                            v
                                     [Scheduler Service]
                                            │
                                     [Reminder Jobs]
                                            │
                                            v
                                     [Notification: Email / WA]
```

- **GPS Integration**: REST API untuk menerima data GPS dari device pihak ketiga (Phase 2)
- **Photo Upload**: Cloudinary / local storage untuk upload foto odometer, struk
- **Export**: PDF laporan bulanan, Excel raw data

## 6. Non-Functional Requirements
- **Performance**: Dashboard load < 3 detik untuk 100 kendaraan
- **Concurrency**: 50 user + 100 driver mobile simultan
- **Offline Support**: Driver mobile bisa input trip offline, queue sync
- **Data Retention**: Riwayat trip min 3 tahun
- **Uptime**: 99.5% (kecuali maintenance)
- **API Response**: < 500ms

## 7. Prioritization Matrix

| Fitur | Value Bisnis | Effort | Priority |
|-------|-------------|--------|----------|
| CRUD Kendaraan | High | Low | P0 |
| Trip Check-in/out | High | Medium | P0 |
| Catat BBM | High | Low | P1 |
| Reminder Service | Medium | Low | P1 |
| Map Tracking | Medium | High | P2 |
| Laporan | Medium | Medium | P1 |
