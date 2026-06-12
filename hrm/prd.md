# PRD: Human Resource Management — Aplikasi Manajemen SDM untuk HR Manager

**Project:** Human Resource Management
**Aesthetic:** Modern, professional

---

## 1. Core Layout

Aplikasi HRM menggunakan layout tiga panel responsif:

- **Sidebar Kiri (250px):** Navigasi utama — Dashboard, Rekrutmen, Karyawan, Payroll, Onboarding, Offboarding, Laporan.
- **Panel Tengah (flex):** Konten utama — daftar karyawan, tabel rekrutmen, form input.
- **Panel Kanan (320px, toggle):** Detail panel — profil karyawan, preview dokumen, ringkasan gaji.

Pada layar mobile, panel kanan menjadi full-screen overlay yang bisa di-trigger dari tombol detail.

---

## 2. Data Structure (Mock Only)

```typescript
export interface Karyawan {
  id: string;
  nomorInduk: string;
  namaLengkap: string;
  posisi: string;
  departemen: string;
  tanggalMasuk: string;
  status: 'aktif' | 'cuti' | 'resign' | 'nonaktif';
  email: string;
  telepon: string;
  gajiPokok: number;
  fotoUrl?: string;
  createdAt: string;
  updatedAt: string;
}

export interface Rekrutmen {
  id: string;
  judulPosisi: string;
  departemen: string;
  jumlahDibutuhkan: number;
  jumlahPelamar: number;
  status: 'dibuka' | 'review' | 'interview' | 'offering' | 'ditutup';
  tanggalDibuka: string;
  tanggalDitutup?: string;
  deskripsi: string;
  persyaratan: string[];
  createdAt: string;
  updatedAt: string;
}

export interface PayrollRecord {
  id: string;
  karyawanId: string;
  namaKaryawan: string;
  periode: string;
  gajiPokok: number;
  tunjangan: number;
  potongan: number;
  lembur: number;
  total: number;
  status: 'draft' | 'dikonfirmasi' | 'dibayar';
  tanggalBayar?: string;
  createdAt: string;
  updatedAt: string;
}

export interface OnboardingTask {
  id: string;
  karyawanId: string;
  namaKaryawan: string;
  tugas: string;
  departemen: string;
  pic: string;
  tenggat: string;
  status: 'tertunda' | 'selesai' | 'terlewat';
  createdAt: string;
  updatedAt: string;
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS / CSS Modules
- **Icons:** Lucide React
- **Animation:** Framer Motion
- **Chart:** Recharts
- **UI Library:** shadcn/ui
- **Data:** Mock data — tidak ada backend calls

---

## 4. Styling & Theming

- **Palette:** Primary #8B5CF6 (Purple), dengan aksen ungu muda dan netral abu-abu untuk tampilan profesional.
- **Typography:** Font system-native (Inter/Segoe UI) untuk keterbacaan optimal.
- **Spacing:** Konsisten, gunakan utility classes Tailwind (p-4, gap-4, dll).

---

## 5. UI Components Specification

```text
+------------------+---------------------------+-----------+
|                   |                           |           |
|    SIDEBAR        |     DAFTAR KARYAWAN       |  DETAIL   |
|    Dashboard      |   +-------------------+   |  Karyawan |
|    Rekrutmen      |   | Nama    | Dept     |   |           |
|    Karyawan       |   | Budi   | IT        |   | Foto      |
|    Payroll        |   | Siti   | HR        |   | Nama      |
|    Onboarding     |   | Agus   | Finance   |   | Posisi    |
|    Offboarding    |   +-------------------+   | Gaji      |
|    Laporan        |   [Cari...] [Filter]      | Status    |
|                   |                           |           |
+------------------+---------------------------+-----------+
```

Komponen utama:
- **Sidebar:** NavLink dengan icon Lucide, active state dengan background primary.
- **DataTable:** Tabel dengan sorting, filtering dropdown, pagination (10 per halaman).
- **KaryawanCard:** Kartu ringkas dengan foto, nama, posisi, departemen.
- **DetailPanel:** Slide-over animasi dari kanan untuk informasi detail karyawan.
- **StatusBadge:** Badge warna berdasarkan status (aktif=hijau, cuti=kuning, resign=merah).
- **StatCard:** Kartu statistik untuk dashboard (total karyawan, rekrutmen aktif, payroll bulan ini).
- **FormModal:** Modal untuk form input dengan validasi real-time.

---

## 6. Interactions & States

**States:**
- **Loading:** Skeleton table dengan 5 baris shimmer + spinner di area detail.
- **Empty:** Ilustrasi + teks "Belum ada data karyawan. Tambahkan karyawan pertama Anda."
- **Error:** Alert merah "Gagal memuat data. Silakan muat ulang halaman." dengan tombol retry.
- **Edge cases:** Pencarian tanpa hasil — "Tidak ditemukan karyawan dengan kata kunci '{{keyword}}'."

**Flow Interaksi utama:**
1. User membuka halaman Dashboard → melihat 4 StatCard (total karyawan, rekrutmen aktif, payroll bulan ini, karyawan baru bulan ini) + grafik komposisi departemen.
2. User klik "Karyawan" di sidebar → tabel daftar karyawan muncul. Klik baris → panel kanan slide-in menampilkan detail profil.
3. User klik "Tambah Karyawan" → FormModal muncul, isi data, simpan → toast sukses "Karyawan berhasil ditambahkan", tabel refresh.
4. User buka Rekrutmen → pipeline view dengan kolom status (dibuka, review, interview, offering, ditutup). Drag & drop antar kolom.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- Semua UI labels, tombol, notifikasi, placeholder dalam Bahasa Indonesia.
- Format mata uang Rp, format tanggal DD/MM/YYYY.

---

## 8. Example Mock Data

```typescript
export const mockKaryawan: Karyawan[] = [
  {
    id: 'KRY-001',
    nomorInduk: 'EMP-2024-001',
    namaLengkap: 'Budi Santoso',
    posisi: 'Senior Frontend Developer',
    departemen: 'Teknologi Informasi',
    tanggalMasuk: '2024-01-15',
    status: 'aktif',
    email: 'budi.santoso@company.com',
    telepon: '0812-3456-7890',
    gajiPokok: 15000000,
    createdAt: '2024-01-15T08:00:00Z',
    updatedAt: '2024-06-01T10:30:00Z',
  },
  {
    id: 'KRY-002',
    nomorInduk: 'EMP-2024-002',
    namaLengkap: 'Siti Rahmawati',
    posisi: 'HR Manager',
    departemen: 'Sumber Daya Manusia',
    tanggalMasuk: '2024-02-01',
    status: 'aktif',
    email: 'siti.rahmawati@company.com',
    telepon: '0813-9876-5432',
    gajiPokok: 18000000,
    createdAt: '2024-02-01T08:00:00Z',
    updatedAt: '2024-06-10T09:15:00Z',
  },
  {
    id: 'KRY-003',
    nomorInduk: 'EMP-2024-003',
    namaLengkap: 'Agus Wijaya',
    posisi: 'Finance Analyst',
    departemen: 'Keuangan',
    tanggalMasuk: '2024-03-10',
    status: 'cuti',
    email: 'agus.wijaya@company.com',
    telepon: '0811-222-3333',
    gajiPokok: 12000000,
    createdAt: '2024-03-10T08:00:00Z',
    updatedAt: '2024-06-12T07:45:00Z',
  },
];

export const mockRekrutmen: Rekrutmen[] = [
  {
    id: 'REK-001',
    judulPosisi: 'Backend Developer',
    departemen: 'Teknologi Informasi',
    jumlahDibutuhkan: 2,
    jumlahPelamar: 15,
    status: 'interview',
    tanggalDibuka: '2024-05-01',
    deskripsi: 'Mengembangkan API dan microservices menggunakan Node.js',
    persyaratan: ['Sarjana Ilmu Komputer', '3+ tahun Node.js', 'Mengerti PostgreSQL'],
    createdAt: '2024-05-01T08:00:00Z',
    updatedAt: '2024-06-10T14:00:00Z',
  },
];
```

---

## 9. Success Criteria

- [ ] Semua UI komponen render dengan mock data
- [ ] CRUD operations berfungsi (create, read, update, delete) untuk data karyawan
- [ ] Semantic search / filter berdasarkan nama, departemen, status
- [ ] Responsive design (desktop & mobile) dengan panel detail yang collapse
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
