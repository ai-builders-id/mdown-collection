# PRD: CV Extractor — Aplikasi Ekstraksi Data CV ke Structured Data untuk HR Recruiter

**Project:** CV Extractor
**Aesthetic:** Modern, clean, data-focused

---

## 1. Core Layout

Aplikasi CV Extractor menggunakan layout dua panel horizontal:

- **Panel Kiri (50%):** Area upload/paste CV — drag & drop file, paste teks, atau pilih dari mock CV library.
- **Panel Kanan (50%):** Hasil ekstraksi — structured data dalam bentuk form terisi otomatis, JSON preview, tombol export.

Pada layar mobile (< 768px), panel ditumpuk vertikal: upload di atas, hasil ekstraksi di bawah.

---

## 2. Data Structure (Mock Only)

```typescript
export interface DataPribadi {
  namaLengkap: string;
  email: string;
  telepon: string;
  alamat: string;
  linkedinUrl?: string;
  portfolioUrl?: string;
  fotoUrl?: string;
}

export interface PengalamanKerja {
  id: string;
  perusahaan: string;
  posisi: string;
  lokasi: string;
  tanggalMulai: string;
  tanggalSelesai: string | 'sekarang';
  deskripsi: string;
  pencapaian: string[];
}

export interface Pendidikan {
  id: string;
  institusi: string;
  gelar: string;
  jurusan: string;
  ipk?: number;
  tanggalMulai: string;
  tanggalSelesai: string;
}

export interface CVData {
  id: string;
  dataPribadi: DataPribadi;
  ringkasan: string;
  pengalamanKerja: PengalamanKerja[];
  pendidikan: Pendidikan[];
  keahlian: string[];
  sertifikasi: string[];
  bahasa: { nama: string; tingkat: 'pemula' | 'menengah' | 'mahir' | 'native' }[];
  tanggalEkstraksi: string;
  status: 'berhasil' | 'gagal' | 'diproses';
}
```

---

## 3. Implementation Details

- **Frontend:** Vite + React + TypeScript + Tailwind CSS
- **Styling:** Tailwind CSS / CSS Modules
- **Icons:** Lucide React
- **Animation:** Framer Motion
- **Chart:** Recharts (untuk skill radar chart)
- **UI Library:** shadcn/ui
- **Data:** Mock data — tidak ada backend calls, simulasi parsing di frontend

---

## 4. Styling & Theming

- **Palette:** Primary #7C3AED (Violet), dengan aksen ungu dan netral untuk tampilan data-centric.
- **Typography:** Font monospace untuk output JSON, font system untuk UI utama.
- **Spacing:** Konsisten, gunakan utility classes Tailwind.

---

## 5. UI Components Specification

```text
+-------------------------------+------------------------------------+
|                               |                                    |
|        UPLOAD AREA            |         HASIL EKSTRAKSI            |
|   +------------------------+  |   +----------------------------+  |
|   |                        |  |   | Data Pribadi               |  |
|   |   [Drag & Drop File]   |  |   | Nama: Budi Santoso         |  |
|   |   atau klik untuk      |  |   | Email: budi@email.com      |  |
|   |   upload PDF/DOCX/TXT  |  |   +----------------------------+  |
|   |                        |  |   | Pengalaman (3)             |  |
|   +------------------------+  |   | PT Maju Jaya - Senior Dev  |  |
|                               |   | 2020 - sekarang             |  |
|   [Pilih Mock CV] v          |   +----------------------------+  |
|                               |   | Pendidikan                 |  |
|   [Proses Ekstraksi]         |   | UI - S.Kom - 2016-2020     |  |
|                               |   +----------------------------+  |
|                               |   | Keahlian: React, TS, Node  |  |
|                               |   | [Export JSON] [Export PDF] |  |
+-------------------------------+------------------------------------+
```

Komponen utama:
- **DropZone:** Area drag & drop dengan animasi hover, icon upload, validasi tipe file.
- **MockCVSelector:** Dropdown untuk memilih CV dari library mock (5-10 CV siap pakai).
- **ExtractedForm:** Form read-only yang menampilkan hasil parsing terstruktur.
- **SkillBadge:** Tag/chip untuk skill dengan warna berdasarkan kategori.
- **JSONPreview:** Collapsible panel yang menampilkan raw JSON hasil ekstraksi.
- **TimelineView:** Komponen timeline untuk pengalaman kerja dan pendidikan.
- **ExportDropdown:** Menu export (JSON, PDF, TXT).

---

## 6. Interactions & States

**States:**
- **Loading (parsing):** Overlay dengan spinner + teks "Menganalisis CV..." dengan progress bar simulasi.
- **Empty (belum upload):** Ilustrasi upload + teks "Upload CV untuk memulai ekstraksi data."
- **Error (parsing gagal):** Alert merah "Gagal mengekstrak CV. Pastikan format file didukung." + tombol upload ulang.
- **Edge cases:** File terlalu besar > 10MB — peringatan. Format tidak didukung — error spesifik. CV dengan data minim — notifikasi "Beberapa field tidak terdeteksi."

**Flow Interaksi utama:**
1. User membuka halaman → panel kiri menampilkan drop zone, panel kanan placeholder "Upload CV untuk melihat hasil."
2. User drag & drop file PDF → animasi drop zone, muncul nama file, tombol "Proses" aktif.
3. User klik "Proses" → loading animation 2-3 detik (simulasi parsing), progress bar bergerak.
4. Hasil parsing muncul di panel kanan — form terisi, ringkasan muncul.
5. User klik "Export JSON" → download file .json, toast sukses.
6. User memilih mock CV dari dropdown → langsung populate hasil tanpa loading.

---

## 7. Language & Localization

- **Bahasa:** Bahasa Indonesia (id)
- Semua UI labels, tombol, notifikasi, placeholder dalam Bahasa Indonesia.

---

## 8. Example Mock Data

```typescript
export const mockCVLibrary: CVData[] = [
  {
    id: 'CV-001',
    dataPribadi: {
      namaLengkap: 'Budi Santoso',
      email: 'budi.santoso@email.com',
      telepon: '0812-3456-7890',
      alamat: 'Jl. Merdeka No. 45, Jakarta Selatan',
      linkedinUrl: 'https://linkedin.com/in/budisantoso',
      portfolioUrl: 'https://budisantoso.dev',
    },
    ringkasan: 'Senior Frontend Developer dengan 5+ tahun pengalaman di React dan TypeScript. Berpengalaman memimpin tim frontend dan mengembangkan aplikasi enterprise.',
    pengalamanKerja: [
      {
        id: 'EXP-001',
        perusahaan: 'PT Teknologi Maju',
        posisi: 'Senior Frontend Developer',
        lokasi: 'Jakarta',
        tanggalMulai: '2021-03-01',
        tanggalSelesai: 'sekarang',
        deskripsi: 'Memimpin pengembangan frontend untuk 3 produk utama perusahaan.',
        pencapaian: ['Meningkatkan performa aplikasi 40%', 'Mengimplementasikan micro-frontend architecture'],
      },
    ],
    pendidikan: [
      {
        id: 'EDU-001',
        institusi: 'Universitas Indonesia',
        gelar: 'Sarjana Komputer',
        jurusan: 'Ilmu Komputer',
        ipk: 3.75,
        tanggalMulai: '2016-08-01',
        tanggalSelesai: '2020-06-30',
      },
    ],
    keahlian: ['React', 'TypeScript', 'Node.js', 'Tailwind CSS', 'GraphQL', 'Docker'],
    sertifikasi: ['AWS Certified Developer', 'Google Associate Android Developer'],
    bahasa: [
      { nama: 'Bahasa Indonesia', tingkat: 'native' },
      { nama: 'Inggris', tingkat: 'mahir' },
    ],
    tanggalEkstraksi: '2024-06-13T10:30:00Z',
    status: 'berhasil',
  },
];
```

---

## 9. Success Criteria

- [ ] Semua UI komponen render dengan mock data
- [ ] Drag & drop upload area berfungsi (simulasi)
- [ ] Parsing results tampil di panel kanan dengan struktur lengkap
- [ ] Mock CV selector langsung populate data tanpa loading
- [ ] Export JSON button menghasilkan file download
- [ ] Semua state handling (loading, empty, error) terimplementasi
- [ ] Responsive design (desktop & mobile)
- [ ] Bahasa Indonesia digunakan untuk semua label UI
- [ ] TIDAK ADA backend calls, TIDAK ADA API integration
