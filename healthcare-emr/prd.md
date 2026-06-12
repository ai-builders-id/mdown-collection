# PRD: Healthcare EMR — Aplikasi Rekam Medis Elektronik untuk Dokter

**Project:** Healthcare EMR
**Aesthetic:** Clean, professional, medical-grade

---

## 1. Core Layout

Aplikasi Healthcare EMR menggunakan layout tiga panel yang terinspirasi dari sistem rekam medis modern:

- **Sidebar Kiri (260px):** Navigasi — Dashboard, Pasien, Jadwal, Rekam Medis, Resep, Laporan. Juga daftar pasien terbaru.
- **Panel Tengah (flex):** Konten utama — daftar pasien, detail rekam medis, formulir diagnosis.
- **Panel Kanan (340px, toggle):** Detail pasien aktif — profil, riwayat kunjungan, alergi, obat-obatan.

---

## 2. Data Structure (Mock Only)

```typescript
export interface Pasien {
  id: string;
  nomorRM: string;
  namaLengkap: string;
  tanggalLahir: string;
  jenisKelamin: 'L' | 'P';
  alamat: string;
  telepon: string;
  email?: string;
  golonganDarah: 'A' | 'B' | 'AB' | 'O';
  alergi: string[];
  penyakitBawaan: string[];
  asuransi: string;
  status: 'aktif' | 'tidak_aktif';
  createdAt: string;
  updatedAt: string;
}

export interface Kunjungan {
  id: string;
  pasienId: string;
  tanggal: string;
  keluhan: string;
  diagnosis: string;
  icdCode: string;
  tekananDarah: string;
  suhuTubuh: number;
  denyutNadi: number;
  beratBadan: number;
  tinggiBadan: number;
  catatanDokter: string;
  status: 'selesai' | 'menunggu' | 'dalam_perawatan';
  createdAt: string;
}

export interface ResepObat {
  id: string;
  kunjunganId: string;
  pasienId: string;
  namaObat: string;
  dosis: string;
  frekuensi: string;
  durasi: string;
  jumlah: number;
  catatan?: string;
  status: 'aktif' | 'selesai' | 'dihentikan';
  createdAt: string;
}

export interface JadwalDokter {
  id: string;
  pasienId: string;
  namaPasien: string;
  tanggal: string;
  waktuMulai: string;
  waktuSelesai: string;
  jenis: 'konsultasi' | 'kontrol' | 'tindakan';
  status: 'dijadwalkan' | 'selesai' | 'batal';
  catatan?: string;
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS / CSS Modules
- **Icons:** Lucide React
- **Animation:** Framer Motion
- **Chart:** Recharts (untuk grafik tekanan darah, suhu, riwayat)
- **UI Library:** shadcn/ui
- **Data:** Mock data — tidak ada backend calls

---

## 4. Styling & Theming

- **Palette:** Primary #0D9488 (Teal), dengan aksen hijau toska bersih dan putih steril untuk nuansa medis.
- **Typography:** Font system-native (Inter/Segoe UI) untuk keterbacaan maksimal.
- **Spacing:** Konsisten, gunakan utility classes Tailwind.

---

## 5. UI Components Specification

```text
+------------------+---------------------------+---------------------+
|                   |                           |                     |
|    SIDEBAR        |     REKAM MEDIS           |   DETAIL PASIEN     |
|    Dashboard      |   Pasien: Budi Santoso    |                     |
|    Pasien         |   No RM: RM-2024-001      |   Nama: Budi        |
|    Jadwal         |   +--------------------+  |   Umur: 45 th       |
|    Rekam Medis    |   | Kunjungan 12/06    |  |   Gol.Darah: O      |
|    Resep          |   | Keluhan: demam      |  |   Alergi: Penisilin |
|    Laporan        |   | Diagnosis: ISPA     |  |   Riwayat: Hipertensi|
|                   |   | TD: 120/80, Suhu:38|  |                     |
|    Pasien Aktif:  |   +--------------------+  |   [Resep Aktif]     |
|    - Budi S.      |   | Kunjungan 01/05    |  |   Amoxicillin       |
|    - Siti R.      |   | ...                 |  |   3x1 sehari        |
|    - Agus W.      |   +--------------------+  |                     |
+------------------+---------------------------+---------------------+
```

Komponen utama:
- **PatientSearch:** Search bar dengan auto-suggest berdasarkan nama/no RM.
- **PatientCard:** Ringkasan pasien untuk daftar/sidebar.
- **VitalSignsCard:** Kartu untuk tanda-tanda vital (TD, suhu, nadi, BB, TB).
- **KunjunganTimeline:** Timeline kronologis riwayat kunjungan pasien.
- **DiagnosisForm:** Form input diagnosis dengan ICD-10 code lookup (mock).
- **ResepForm:** Form input resep obat dengan dosis, frekuensi, durasi.
- **CalendarView:** Kalender jadwal praktik dengan event pasien.
- **AlergiBadge:** Badge merah untuk alergi obat.

---

## 6. Interactions & States

**States:**
- **Loading:** Skeleton untuk timeline kunjungan, shimmer untuk detail pasien.
- **Empty (belum ada pasien):** "Belum ada data pasien. Daftarkan pasien pertama Anda." dengan tombol "Daftarkan Pasien".
- **Error:** "Gagal memuat data rekam medis. Periksa koneksi dan muat ulang."
- **Edge cases:** Pasien dengan banyak alergi — ditampilkan dalam scrollable tag list. Riwayat kunjungan kosong — "Belum ada kunjungan tercatat."

**Flow Interaksi utama:**
1. Dokter login → Dashboard menampilkan jadwal hari ini, jumlah pasien, dan notifikasi.
2. Dokter klik "Pasien" → daftar pasien, bisa search by nama/no RM.
3. Dokter klik pasien → panel kiri tetap, panel tengah menampilkan profil dan riwayat, panel kanan detail.
4. Dokter klik "Kunjungan Baru" → form keluhan, diagnosis, vital signs, resep.
5. Dokter simpan kunjungan → otomatis tercatat di timeline, status pasien terupdate.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- Semua UI labels, tombol, notifikasi, placeholder dalam Bahasa Indonesia.
- Istilah medis tetap dalam istilah baku (ICD-10, diagnosis, resep).

---

## 8. Example Mock Data

```typescript
export const mockPasien: Pasien[] = [
  {
    id: 'PSN-001',
    nomorRM: 'RM-2024-001',
    namaLengkap: 'Budi Santoso',
    tanggalLahir: '1979-05-12',
    jenisKelamin: 'L',
    alamat: 'Jl. Merdeka No. 45, Jakarta Pusat',
    telepon: '0812-3456-7890',
    email: 'budi.s@email.com',
    golonganDarah: 'O',
    alergi: ['Penisilin', 'Aspirin'],
    penyakitBawaan: ['Hipertensi'],
    asuransi: 'BPJS Kesehatan',
    status: 'aktif',
    createdAt: '2024-01-15T08:00:00Z',
    updatedAt: '2024-06-12T14:30:00Z',
  },
  {
    id: 'PSN-002',
    nomorRM: 'RM-2024-002',
    namaLengkap: 'Siti Rahmawati',
    tanggalLahir: '1985-11-23',
    jenisKelamin: 'P',
    alamat: 'Jl. Sudirman No. 10, Jakarta Selatan',
    telepon: '0813-9876-5432',
    golonganDarah: 'A',
    alergi: [],
    penyakitBawaan: ['Diabetes Tipe 2'],
    asuransi: 'Asuransi Swasta',
    status: 'aktif',
    createdAt: '2024-02-01T08:00:00Z',
    updatedAt: '2024-06-10T09:15:00Z',
  },
];

export const mockKunjungan: Kunjungan[] = [
  {
    id: 'KUN-001',
    pasienId: 'PSN-001',
    tanggal: '2024-06-12T10:30:00Z',
    keluhan: 'Demam tinggi sejak 2 hari, batuk berdahak, pilek',
    diagnosis: 'Infeksi Saluran Pernapasan Akut (ISPA)',
    icdCode: 'J06.9',
    tekananDarah: '120/80',
    suhuTubuh: 38.5,
    denyutNadi: 88,
    beratBadan: 72,
    tinggiBadan: 170,
    catatanDokter: 'Pasien diberikan antibiotik dan antipiretik. Kontrol kembali jika demam tidak turun dalam 3 hari.',
    status: 'selesai',
    createdAt: '2024-06-12T11:15:00Z',
  },
];
```

---

## 9. Success Criteria

- [ ] Semua UI komponen render dengan mock data
- [ ] CRUD data pasien berfungsi
- [ ] Timeline kunjungan per pasien tampil kronologis
- [ ] Form diagnosis & resep berfungsi dengan validasi
- [ ] Kalender jadwal menampilkan events
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Responsive design (desktop & mobile)
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
