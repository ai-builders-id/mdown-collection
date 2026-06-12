# PRD: Learning Management System — Aplikasi Manajemen Kursus Online untuk Siswa/Pelatih

**Project:** Learning Management System
**Aesthetic:** Modern, engaging, educational

---

## 1. Core Layout

Aplikasi LMS menggunakan layout dua panel dengan header sticky:

- **Header Atas (64px):** Navigasi utama — Dashboard, Kursus Saya, Katalog, Sertifikasi, Profile. Search bar di tengah.
- **Sidebar Kiri (280px, collapsible):** Navigasi kursus — daftar modul, progress per modul, bookmark.
- **Panel Utama (flex):** Konten kursus — video embed, materi bacaan, quiz, forum diskusi.
- **Bottom Bar:** Progress bar keseluruhan kursus, tombol Next/Previous.

---

## 2. Data Structure (Mock Only)

```typescript
export interface Kursus {
  id: string;
  judul: string;
  deskripsi: string;
  instruktur: string;
  kategori: string;
  tingkat: 'pemula' | 'menengah' | 'mahir';
  durasi: number;
  totalModul: number;
  rating: number;
  jumlahSiswa: number;
  thumbnailUrl: string;
  harga: number;
  status: 'draft' | 'terbit' | 'arsip';
  createdAt: string;
  updatedAt: string;
}

export interface Modul {
  id: string;
  kursusId: string;
  judul: string;
  tipe: 'video' | 'bacaan' | 'quiz' | 'tugas';
  durasi: number;
  urutan: number;
  kontenUrl?: string;
  statusProgress: 'belum_dimulai' | 'sedang_dipelajari' | 'selesai';
}

export interface Siswa {
  id: string;
  namaLengkap: string;
  email: string;
  avatarUrl?: string;
  kursusTerdaftar: string[];
  progressKeseluruhan: number;
  tanggalBergabung: string;
  sertifikatDiperoleh: string[];
}

export interface Quiz {
  id: string;
  modulId: string;
  pertanyaan: string;
  opsi: string[];
  jawabanBenar: number;
  penjelasan: string;
}

export interface Sertifikat {
  id: string;
  siswaId: string;
  namaSiswa: string;
  kursusId: string;
  judulKursus: string;
  tanggalTerbit: string;
  nomorSertifikat: string;
  persentaseNilai: number;
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS / CSS Modules
- **Icons:** Lucide React
- **Animation:** Framer Motion
- **Chart:** Recharts (untuk progress chart, quiz analytics)
- **UI Library:** shadcn/ui
- **Data:** Mock data — tidak ada backend calls

---

## 4. Styling & Theming

- **Palette:** Primary #F59E0B (Amber), dengan aksen kuning hangat dan netral putih/abu-abu.
- **Typography:** Font system-native dengan heading yang bold dan readable.
- **Spacing:** Konsisten, gunakan utility classes Tailwind.

---

## 5. UI Components Specification

```text
+------------------------------------------------------------------+
| [Logo] [Kursus Saya] [Katalog] [Sertifikasi]  [Search...] [Profil]|
+------------------------------------------------------------------+
| +--------+ +----------------------------------------------------+ |
| | MODUL  | |                                                    | |
| | 1.Pend | |   [Video Player / Konten Bacaan / Quiz]            | |
| | 2.Dasar | |                                                    | |
| | 3.Lanjut| |   Judul Modul                                     | |
| | 4.Quiz  | |   Deskripsi Materi                                | |
| |         | |                                                    | |
| |         | |   [Previous]                         [Next]       | |
| +--------+ +----------------------------------------------------+ |
| Progress: [==========--------] 65%                                |
+------------------------------------------------------------------+
```

Komponen utama:
- **CourseCard:** Kartu kursus dengan thumbnail, rating, progress bar.
- **ModuleSidebar:** Daftar modul dengan status icon (check, play, lock).
- **VideoPlayer:** Embed video dengan controls (mock player).
- **QuizComponent:** Soal pilihan ganda dengan feedback jawaban.
- **ProgressBar:** Progress bar horizontal dengan persentase.
- **CertificateCard:** Template sertifikat dengan animasi confetti.
- **RatingStars:** Rating bintang interaktif.
- **DiscussionThread:** Forum diskusi per modul.

---

## 6. Interactions & States

**States:**
- **Loading:** Skeleton untuk course card grid, shimmer untuk sidebar modul.
- **Empty (belum terdaftar):** "Anda belum mendaftar kursus apapun. Jelajahi katalog kursus kami!" dengan tombol "Jelajahi Katalog".
- **Error:** Alert merah "Gagal memuat data kursus. Coba muat ulang halaman."
- **Edge cases:** Quiz nilai 0 — tampilan "Jangan menyerah! Coba lagi." Video tidak bisa diputar — fallback text transcript.

**Flow Interaksi utama:**
1. User login → Dashboard menampilkan kursus yang sedang diambil, progress, rekomendasi.
2. User klik kursus → Sidebar modul muncul, konten modul pertama dimuat.
3. User selesaikan modul → centang hijau, progress bar update, next modul aktif.
4. User buka quiz → pilih jawaban, submit, feedback langsung benar/salah + penjelasan.
5. User selesaikan semua modul → Sertifikat otomatis tersedia, modal confetti.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- Semua UI labels, tombol, notifikasi, placeholder dalam Bahasa Indonesia.

---

## 8. Example Mock Data

```typescript
export const mockKursus: Kursus[] = [
  {
    id: 'KRS-001',
    judul: 'Fundamental React & TypeScript',
    deskripsi: 'Pelajari React dari dasar hingga mahir dengan TypeScript. Cocok untuk pemula yang ingin menjadi frontend developer.',
    instruktur: 'Budi Prasetyo',
    kategori: 'Pengembangan Web',
    tingkat: 'pemula',
    durasi: 2400,
    totalModul: 12,
    rating: 4.8,
    jumlahSiswa: 1240,
    thumbnailUrl: '/thumbnails/react-ts.jpg',
    harga: 0,
    status: 'terbit',
    createdAt: '2024-01-15T08:00:00Z',
    updatedAt: '2024-05-20T10:30:00Z',
  },
  {
    id: 'KRS-002',
    judul: 'Backend Engineering with Node.js',
    deskripsi: 'Kursus komprehensif untuk membangun REST API dan microservices menggunakan Node.js dan Express.',
    instruktur: 'Dewi Sartika',
    kategori: 'Pengembangan Web',
    tingkat: 'menengah',
    durasi: 3600,
    totalModul: 16,
    rating: 4.6,
    jumlahSiswa: 890,
    thumbnailUrl: '/thumbnails/nodejs.jpg',
    harga: 250000,
    status: 'terbit',
    createdAt: '2024-02-01T08:00:00Z',
    updatedAt: '2024-06-10T09:15:00Z',
  },
];

export const mockModul: Modul[] = [
  {
    id: 'MDL-001',
    kursusId: 'KRS-001',
    judul: 'Pengantar React',
    tipe: 'video',
    durasi: 900,
    urutan: 1,
    statusProgress: 'selesai',
  },
  {
    id: 'MDL-002',
    kursusId: 'KRS-001',
    judul: 'JSX dan Components',
    tipe: 'bacaan',
    durasi: 600,
    urutan: 2,
    statusProgress: 'sedang_dipelajari',
  },
  {
    id: 'MDL-003',
    kursusId: 'KRS-001',
    judul: 'Quiz Module 1',
    tipe: 'quiz',
    durasi: 300,
    urutan: 3,
    statusProgress: 'belum_dimulai',
  },
];
```

---

## 9. Success Criteria

- [ ] Semua UI komponen render dengan mock data
- [ ] Navigasi kursus (modul side bar + konten utama) berfungsi
- [ ] Quiz component dengan feedback jawaban berfungsi
- [ ] Progress tracking update real-time
- [ ] Sertifikat otomatis saat semua modul selesai
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Responsive design (desktop & mobile)
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
